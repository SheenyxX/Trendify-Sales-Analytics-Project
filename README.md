
[![Power BI](https://img.shields.io/badge/Power_BI-Microsoft-blue.svg)](https://powerbi.microsoft.com/)

# Sales & Customer Insights: Diagnosing Profitability Gaps in E-Commerce  
*Prepared for: Trendify (Growing E-Commerce Retailer)*  

## 🔍 Executive Summary  

### The Growth Paradox  
Trendify achieved **25.81% order growth in 2023** while struggling with inconsistent profitability across product categories. Analysis of **9,994+ transactions** revealed:  

**Critical Business Findings**:  

1. **Category Profitability Extremes**:  
   - Copiers: **+136.54% profit growth** ($83.1M → $196.6M)  
   - Machines: **-130.8% profit decline** ($53.7M → -$16.5M)  

2. **Customer Concentration**  
   - **Top 10 customers** contribute **30% of total profit**  
   - California dominates with **1,189 orders** (vs. NY's 672)  

3. **Seasonal Volatility**  
   - December peak: **462 orders**  
   - February slump: **107 orders** (lowest of year)  

**Solution**: Power BI dashboard suite with 12 critical DAX measures tracking:  
✔️ Real-time profitability by product category  
✔️ Customer lifetime value alerts  
✔️ Seasonal demand forecasting  

---

## 🌐 Data Organization & Structure  

### 1. **Product Master Data** (`Products.xlsx`)  
![Products Table](https://github.com/SheenyxX/Trendify-Sales-Analytics-Project/blob/main/tb1.png)  
*1892 products with hierarchical categorization*  

- **Classification Structure**:  
  ```plaintext
  Level 1: Category (e.g., Office Supplies)  
  Level 2: Sub-Category (e.g., Binders)  
  Level 3: Product Name (e.g., Cardinal Hold-it Binder)  
  ```  

### 2. **Transactional Core** (`Orders.xlsx`)  
![Orders Table](https://github.com/SheenyxX/Trendify-Sales-Analytics-Project/blob/main/tb2.png)  
*9,994 transactions with profit/sales metadata*  

- **Key Fields**:  
  - `Order Date` (temporal analysis)  
  - `Sales` (transaction value)  
  - `Profit` (color-coded for losses)  
  - `Quantity` (demand tracking)  

### 3. **Dimensional Data**  

#### Customer Table  
![Customer Table](https://github.com/SheenyxX/Trendify-Sales-Analytics-Project/blob/main/tb3.png)  
- **793 unique customers** with purchase history  

#### Location Table  
![Location Table](https://github.com/SheenyxX/Trendify-Sales-Analytics-Project/blob/main/tb4.png)  
- **Geographic hierarchy**: Postal Code → City → State → Region  

### 4. **Relational Model**  
![Data Model](https://github.com/SheenyxX/Trendify-Sales-Analytics-Project/blob/main/ttb6.png)  
*Star schema optimized for retail analytics*  

- Central fact table (`Orders`) connected to all dimensions  
- One-to-many relationships from dimensions to transactions  
- Optimized for both executive summaries and drill-down analysis  

---

## 📊 Dashboard Walkthrough  

### 1. Sales Performance Dashboard  
![Sales Dashboard](https://github.com/SheenyxX/Trendify-Sales-Analytics-Project/blob/main/d6.png)  

**Components**:  
- Year-over-year sales/profit comparison (2022 vs 2023)  
- Profitability heatmap by product category  
- Loss-making transaction flagging  

**Key Metrics**:  
```dax
Profit Growth % = 
VAR CurrentProfit = [Current Year Profit]  // 2023: $641.4M
VAR PreviousProfit = [Previous Year Profit]  // 2022: $492.3M
RETURN DIVIDE((CurrentProfit - PreviousProfit), PreviousProfit)  // +30.3%
```  

**Insights**:  
- Machines category shows **-$16.5M loss** despite $53.7M 2022 profit  
- February is weakest month (**$9.4M sales** vs December peak of $35.2M)  
- Copiers deliver **68% gross margin** (highest of all categories)  

---

### 2. Customer Behavior Dashboard  
![Customer Dashboard](https://github.com/SheenyxX/Trendify-Sales-Analytics-Project/blob/main/d7.png)  

**Components**:  
- Customer segmentation by order frequency  
- Geographic profit contribution map  
- Repeat purchase probability model  

**Key Metrics**:  
```dax
Top Customers Profit = 
TOPN(10, SUMMARIZE(Orders, Customers[Customer Name], 
    "TotalProfit", SUM(Orders[Profit])), [TotalProfit])  // 30% of total
```  

**Insights**:  
- **California + New York** = 38% of total orders  
- Top customer generates **$2.1M profit** annually  
- 22% of customers make **repeat orders within 90 days**  

---

## 🔬 Technical Backbone  

### Core DAX Measures  
```dax
// Profitability Suite
Current Year Profit = SUM(Orders[Profit])  // Absolute performance
Profit Margin % = DIVIDE([Current Year Profit], [Total Sales Current Year])  // Efficiency

// Growth Analytics
Sales Growth % = 
VAR CurrentSales = [Total Sales Current Year]  // 2023: $355.7M
VAR PreviousSales = [Total Sales Previous Year]  // 2022: $309.3M
RETURN DIVIDE((CurrentSales - PreviousSales), PreviousSales)  // +15.01%

// Customer Intelligence
Repeat Purchase Rate = 
DIVIDE(
    COUNTROWS(FILTER(Customers, [Order Count] > 1)),
    COUNTROWS(Customers)
)  // Loyalty metric
```

---

## 🎯 Strategic Recommendations  

1. **Profit Recovery Plan**  
   - Machines category: Renegotiate supplier contracts (target: **50% loss reduction**)  
   - Implement minimum order quantities for low-margin items  

2. **Seasonal Boost**  
   - February promotion: Bundle slow-moving inventory with copiers  
   - Target **20% sales increase** vs 2023 ($9.4M → $11.3M)  

3. **Customer Goldmine**  
   - Launch tiered loyalty program for top 10% customers  
   - Geographic focus: California + NY (**1,861 order opportunity**)  

```dax
// Implementation KPI
February_Sales_Target = 
CALCULATE([Total Sales Current Year], 
    MONTH('Date'[Date]) = 2) * 1.2  // 20% growth goal
```  

--- 

*Dashboard design and DAX measures optimized for Power BI Service deployment with automatic refresh capabilities.*
