# Budget Variance Analysis Dashboard — Power BI

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat&logo=linkedin)](www.linkedin.com/in/kaif-mahaldar-18300b333)

<img width="1374" height="755" alt="image" src="https://github.com/user-attachments/assets/f5df61f3-5720-4253-a4f3-59d90d5b0656" />


The question this dashboard answers is simple: we know we're ahead of budget, but where exactly? A single percentage doesn't help anyone act on anything. This breaks it down by department, by month, by account. Built on 40,000 rows of general ledger data across five departments and twelve months.

---

## What I found

- The business finished FY 2023 at +3.5% above budget net profit. That's +$5.7M. Sounds clean until you look at the departments.
- Engineering overdelivered by 19.1%. Sales and HR both came in around +8%. Marketing hit almost exactly on budget.
- Executive was the only department to miss. At -14.1%, it dragged $4.9M off the total. Without it, the company number would've been closer to +7%.
- January and July were the two strongest months on both revenue and profit.
- March and April were the soft spots. Both fell short of budget on revenue and net profit.

---

## What's in this repo

```
budget-variance-analysis-powerbi/
│
├── Dataset/
│   ├── Fact_General_Ledger.csv
│   ├── Dim_Chart_of_Accounts.csv
│   └── Dim_Departments.csv
│
├── Report/
│   └── Budget_Variance_Dashboard_FY2023.pbix
│
└── README.md
```

---

## The two pages

###  Executive KPI Summary

Four KPI cards at the top. Monthly trend chart and department variance chart side by side. Full P&L table at the bottom. Both slicers filter everything on the page at the same time. It's meant to be the view you keep open in a meeting.

<img width="728" height="258" alt="image" src="https://github.com/user-attachments/assets/560f02cd-2bf2-471f-bdcc-426a815a02a0" />


---

### Departmental Drill-Down

Same numbers, different layout. This one is for going deeper. Pick a month or a range of months on the left. Click a department in the bar chart and the whole page filters to just that department. Two clicks from "Executive missed" to "here's which accounts."

<img width="248" height="259" alt="image" src="https://github.com/user-attachments/assets/4a5196d9-4050-4a48-a484-0903b04ae694" />


---

## The numbers

| Metric | Value |
|---|---|
| Actual Revenue | $361,381,293 |
| Actual Expenses | $194,246,928 |
| Actual Net Profit | $167,134,365 |
| Budget Net Profit | $161,446,627 |
| Variance $ | +$5,687,738 |
| Variance % | +3.5% |

46 cents of every revenue dollar ends up as net profit. That's a healthy number for a services business.

---

## Revenue vs Budget by Month

```mermaid
xychart-beta
    title "Actual vs Budget Revenue by Month ($K)"
    x-axis ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
    y-axis "Revenue ($K)" 25000 --> 35000
    bar [32222, 26461, 29372, 28580, 28839, 29828, 33740, 31523, 30117, 29188, 29474, 32038]
    line [28093, 26991, 30370, 30257, 29178, 30397, 32653, 30689, 29923, 27800, 29464, 32351]
```

Bars = Actual. Line = Budget.

<img width="555" height="274" alt="image" src="https://github.com/user-attachments/assets/9fabdc52-321e-483f-b2ce-131527bfe1a3" />


---

## Department Variance

```mermaid
xychart-beta horizontal
    title "Net Profit Variance % by Department"
    x-axis ["Engineering", "Sales", "Human Resources", "Marketing", "Executive"]
    y-axis "Variance %" -20 --> 25
    bar [19.1, 8.6, 7.8, 0.1, -14.1]
```

| Department | Actual Net Profit | Budget Net Profit | Variance $ | Variance % |
|---|---|---|---|---|
| Engineering | $32,966,779 | $27,670,252 | +$5,296,527 | +19.1% |
| Sales | $35,272,985 | $32,475,661 | +$2,797,324 | +8.6% |
| Human Resources | $34,596,732 | $32,105,951 | +$2,490,781 | +7.8% |
| Marketing | $34,084,545 | $34,035,272 | +$49,273 | +0.1% |
| Executive | $30,213,324 | $35,159,491 | -$4,946,167 | -14.1% |
| **Total** | **$167,134,365** | **$161,446,627** | **+$5,687,738** | **+3.5%** |

<img width="550" height="201" alt="image" src="https://github.com/user-attachments/assets/fc7a2e95-81a1-42eb-a1d5-1d0c2458cfb1" />


---

## Net Profit vs Budget by Month

```mermaid
xychart-beta
    title "Monthly Net Profit — Actual vs Budget ($)"
    x-axis ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
    y-axis "Net Profit ($)" 10000000 --> 18000000
    bar [16058514, 11586281, 12909445, 11966748, 12498358, 13303590, 17164926, 15326235, 13834022, 12454966, 15173266, 14858013]
    line [12043927, 11023610, 14866730, 14186909, 11803837, 14257979, 15890148, 13325575, 13177569, 11574125, 13450531, 15845686]
```

Bars = Actual. Line = Budget.

January was the best month. $16M actual against a $12M budget. March, April, and June were the three months that came in under plan.

---

## Where the revenue goes

```mermaid
pie title FY 2023 — Revenue Split
    "Net Profit" : 167134365
    "Total Expenses" : 194246928
```

<img width="563" height="361" alt="image" src="https://github.com/user-attachments/assets/3e18bcee-0caa-4822-a3d6-c0779370893f" />


---

## How the data model works

Three tables. One fact, two dimensions.

```mermaid
flowchart TD
    A[Dim_Departments\nDepartment_ID · Department_Name]
    B[Dim_Chart_of_Accounts\nAccount_ID · Account_Name\nSub_Category · Master_Category]
    C[Fact_General_Ledger\nDate · Account_ID · Department_ID\nScenario · Amount\n\n40,000 rows · FY 2023]

    A -- Department_ID --> C
    B -- Account_ID --> C
```

The `Scenario` column does most of the work. Both Actual and Budget numbers live in the same fact table, just tagged differently. Every measure has an Actual version and a Budget version. The variance is just the gap between them.

---

## DAX measures

```
Actual Net Profit   = Actual Revenue - Actual Expenses
Budget Net Profit   = Budget Revenue - Budget Expenses
Net Profit Var $    = Actual Net Profit - Budget Net Profit
Net Profit Var %    = DIVIDE([Net Profit Var $], ABS([Budget Net Profit]))
```

---

## How the interactivity works

```mermaid
flowchart LR
    S1[Department Slicer] --> K[KPI Cards]
    S1 --> CH[Combo Chart]
    S1 --> B[Variance Bar Chart]
    S1 --> P1[P&L Pivot]
    S1 --> P2[Budget vs Actual Pivot]

    S2[Month Slicer] --> K
    S2 --> CH
    S2 --> B
    S2 --> P1
    S2 --> P2

    B -- cross-filter --> K
    B -- cross-filter --> P1
```

Both slicers hit everything. On Page 2, clicking a bar in the department chart also cross-filters the table and cards. So the whole page becomes about that one department.

---

## What's coming next

- **Executive deep dive page** — Pages 1 and 2 show that Executive missed by -14.1%. The next page would show exactly which sub-categories and accounts caused it. That's the question this data raises and doesn't fully answer yet.
- **Bookmarks** — saved views for All Departments, Under-Budget Only, and H2 only. So anyone using the report can jump between states without touching the slicers.

---

## Assumptions

- Budget and Actual are in the same fact table, separated by the Scenario column
- Net Profit is calculated. It's not a standalone column in the source data.
- The P&L hierarchy goes Master Category to Sub-Category to Account Name
- All figures in USD, full calendar year 2023

---

## Tools

Power BI Desktop · DAX · Power Query


**Role: Data Analyst ** 

 
 **Others Project -**
 
 
1) https://github.com/Kaif39211/pharmaceutical-inventory-analysis-SQL 

                                              
                                              
2) https://github.com/Kaif39211/KM_Logistics_2022_Analysis.xlsx
                                             
                                              
    
     
 3) https://github.com/Kaif39211/zepto_inventory_analysis.sql

                      


## **Contact: [LinkedIn](http://www.linkedin.com/in/kaif-mahaldar-18300b333) | [Email](mailto:kaifmahaldar5@gmail.com)**

