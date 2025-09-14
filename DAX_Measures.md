# DAX Measures
Total Revenue := SUMX(FILTER(fact_Transactions, fact_Transactions[Type]="Revenue"), fact_Transactions[Amount])
Total Expense := SUMX(FILTER(fact_Transactions, fact_Transactions[Type]="Expense"), -fact_Transactions[Amount])
Net Profit := [Total Revenue] - [Total Expense]
Gross Margin := VAR SalesCOGS := [Total Revenue] - CALCULATE(SUMX(FILTER(fact_Transactions, fact_Transactions[AccountID]=200), -fact_Transactions[Amount])) RETURN SalesCOGS
Gross Margin % := DIVIDE([Gross Margin],[Total Revenue])
Budget Amount := SUM(fact_Budget[BudgetAmount])
Actual vs Budget := [Total Revenue] - [Total Expense] - [Budget Amount]
Variance % := DIVIDE([Actual vs Budget],[Budget Amount])
Total Revenue PY := CALCULATE([Total Revenue], DATEADD(dim_Date[Date], -1, YEAR))
Revenue YoY % := DIVIDE([Total Revenue]-[Total Revenue PY],[Total Revenue PY])
Net Profit PY := CALCULATE([Net Profit], DATEADD(dim_Date[Date], -1, YEAR))
Net Profit YoY % := DIVIDE([Net Profit]-[Net Profit PY],[Net Profit PY])
Avg FX Rate (Selected) := AVERAGE(fx_Rates[RateToCAD])
Revenue (CAD) := DIVIDE([Total Revenue],[Avg FX Rate (Selected)])
Expense (CAD) := DIVIDE([Total Expense],[Avg FX Rate (Selected)])
Net Profit (CAD) := [Revenue (CAD)] - [Expense (CAD)]
