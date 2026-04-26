# Payment & Arrears Analysis

## Business Problem
Companies need to track customer payments to identify:
- Late payments
- Outstanding balances
- Risky customers

## Solution
Developed SQL logic to classify payments into:
- On Time
- Late
- In Arrears

## Sample Logic
Used CASE statements to determine payment status based on due date and payment date.

## Next Steps
- Add KPI calculations
- Build Power BI dashboard

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


