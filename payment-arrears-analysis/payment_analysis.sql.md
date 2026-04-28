# 📊 Payment Arrears & Revenue Leakage Analysis

## The Business Problem
Organizations that rely on recurring payments (contracts, deeds, subscriptions) often face challenges tracking:
- Customers in arrears
- Partial or underpayments
- Duplicate or incorrect renewals

These issues can lead to **revenue leakage, inaccurate reporting, and missed collection opportunities**.

This project analyzes customer payment behavior to identify **outstanding balances, inconsistencies, and financial risk**.

---

##  Objectives
- Identify customers with overdue balances
- Compare **total billed vs total paid vs total owed**
- Detect **partial payments and underpayments**
- Flag anomalies in renewal behavior (e.g., duplicate renewals)
- Quantify **revenue at risk**

---

## Data Overview
The analysis is based on relational datasets including:

- **Customers** – Account and demographic data  
- **Invoices** – Billing records and expected payments  
- **Payments** – Transactions made by customers  
- **Contracts/Deeds** – Renewal terms and durations  

---

##  Approach

### 1. Data Preparation
- Standardized null/empty values
- Cleaned inconsistent fields
- Ensured proper joins across datasets

### 2. Transformation
- Aggregated invoice totals per customer
- Aggregated payment totals per customer
- Calculated:
  - `Total_Billed`
  - `Total_Paid`
  - `Total_Owed`

### 3. Analysis Logic
- Compared billed vs paid amounts to identify arrears
- Used joins and grouping to avoid duplicate records
- Implemented logic to handle:
  - Multiple payments per invoice
  - Split transactions
  - Misaligned joins causing duplication

---
CREATE TABLE CustomerPayments (
    CustomerID INT,
    CustomerName VARCHAR(100),
    InvoiceID INT,
    InvoiceDate DATE,
    DueDate DATE,
    PaymentDate DATE,
    Amount DECIMAL(10,2)
);

INSERT INTO CustomerPayments VALUES
(1, 'John Smith', 1001, '2024-06-01', '2024-06-15', '2024-06-14', 500.00),
(2, 'Maria Lopez', 1002, '2024-06-05', '2024-06-20', '2024-06-25', 300.00),
(3, 'David Chen', 1003, '2024-06-10', '2024-06-25', NULL, 450.00),
(4, 'Sarah Johnson', 1004, '2024-06-12', '2024-06-27', '2024-06-27', 700.00),
(5, 'James Brown', 1005, '2024-06-15', '2024-06-30', '2024-07-10', 650.00);

-- Calculate outstanding balances and classify payment behavior
-- This logic simulates real-world accounts receivable tracking

SELECT 
    CustomerID,
    CustomerName,
    InvoiceID,
    DueDate,
    PaymentDate,

    CASE 
        WHEN PaymentDate IS NULL THEN 'In Arrears'
        WHEN PaymentDate <= DueDate THEN 'On Time'
        WHEN PaymentDate > DueDate THEN 'Late'
    END AS PaymentStatus

FROM CustomerPayments;
---

## 📈 Example Output

| CustomerID | CustomerName | InvoiceID | DueDate   | PaymentDate | PaymentStatus |
|------------|--------------|-----------|-----------|-------------|---------------|
| 310967     | John Doe     | INV001    | 2024-01-15| NULL        | In Arrears    |
| 310968     | Jane Smith   | INV002    | 2024-01-10| 2024-01-08  | On Time       |
| 310969     | Mike Brown   | INV003    | 2024-01-05| 2024-01-12  | Late          |
---

## -- Key Challenges & Solutions

### 1.Duplicate Records from Joins
**Problem:** Joining invoices and payments created multiple rows per customer  
**Solution:** Aggregated payments separately before joining  

---

### 2.Incorrect Payment Totals
**Problem:** Payment totals inflated due to multiple joins  
**Solution:** Used grouped subqueries to ensure accurate totals  

---

### 3. Revenue Leakage Scenario
**Problem:** Renewals extended contract duration without proper billing  
**Example:**  
- Contract renewed for 2 years  
- Removed and re-added  
- Renewed again → total duration extended 4 years  
- Customer charged for only 2  

---

**Impact:** Hidden revenue loss  

---
## 1.Key Insights

- A portion of customers have **outstanding balances despite active contracts**
- Payment discrepancies exist due to **data structure and renewal logic**
- Revenue leakage can occur when **contract logic is not tightly controlled**

---

## 2.Recommendations

- Implement validation for renewal actions  
- Track contract history changes (audit trail)  
- Flag discrepancies between expected vs actual billing  
- Automate arrears monitoring dashboards  

---

## 3.Tools & Skills Demonstrated

- SQL Server
- Complex Joins & Aggregations
- Data Cleaning & Transformation
- Financial Data Analysis
- Business Logic Implementation

---

## 4.Future Enhancements

- Power BI dashboard for arrears tracking  
- Automated alerts for overdue accounts  
- Python integration for deeper analysis  
- API pipeline for real-time data ingestion  

---

## 📌 Author
Eric Guzman  
Business & Data Analyst  




