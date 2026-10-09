# ADS23702 – Advanced Data Visualization Using Power BI Laboratory

Global Academy of Technology, Bengaluru  
Department of Artificial Intelligence and Data Science  
Academic Year: 2026–27

This README contains all **5 lab programs** from the ADS23702 lab manual. Programs 1–3 use Tableau (including TabPy); Programs 4–5 use Power BI. Copy each code block as needed. These are formulas/scripts to enter into the relevant application—not programs that can all be run directly in a terminal.

---

## Program 1: Connect Multiple Relational Datasets in Tableau and Build Interactive Visualizations

```shell

**Objective:** Connect Orders, Returns, and People tables from `Sample Superstore.xlsx`, create relationships, and build interactive visualizations.

### Relationships
- `Orders[Order ID]` ↔ `Returns[Order ID]`
- `Orders[Region]` ↔ `People[Region]`

### Tableau calculated fields
No calculated field is required for this program.

### Visualizations to create
1. **Sales by Region:** Columns = `Region`; Rows = `SUM(Sales)`; choose Bar Chart.
2. **Sales Trend by Year:** Columns = `YEAR(Order Date)`; Rows = `SUM(Sales)`; choose Line Chart.
3. **Profit by Category:** Columns = `Category`; Rows = `SUM(Profit)`; choose Bar Chart.
4. Add `Region` and `Category` to Filters, then select **Show Filter** for each.

**Expected result:** Three related tables, working relationships, a yearly sales trend line chart, and a bar chart comparing profit by category.

---

```
## Program 2: Implement FIXED, INCLUDE, and EXCLUDE LOD Expressions in Tableau

```shell

**Objective:** Use Level of Detail (LOD) expressions to solve business analysis problems with `Sample Superstore.xlsx`.

### 1. FIXED — Customer Total Sales

Create a calculated field named `Customer Total Sales`:

{ FIXED [Customer Name] : SUM([Sales]) }

**Use:** Place `Customer Name` on Rows and `Customer Total Sales` on Columns. Sort descending and display as a bar chart.

### 2. INCLUDE — Regional Customer Average

Create a calculated field named `Regional Customer Average`:

{ INCLUDE [Customer Name] : AVG([Sales]) }

**Use:** Place `Region` on Rows and `Regional Customer Average` on Columns. Display as a bar chart.

### 3. EXCLUDE — Regional Sales without Sub-Category Detail

Create a calculated field named `Regional Sales`:

{ EXCLUDE [Sub-Category] : SUM([Sales]) }

**Use:** Add `Category`, `Sub-Category`, and `Regional Sales` to a view. Display as a highlight table or bar chart.

Create three worksheets: **FIXED**, **INCLUDE**, and **EXCLUDE**.

**Expected result:** The three worksheets demonstrate customer-level sales, average sales at regional level, and sales aggregated while excluding the Sub-Category level.

---

```
## Program 3: Execute SVM Classification from Tableau Using TabPy

```shell

**Objective:** Use a Support Vector Machine (SVM) classifier with the Iris dataset and show the predictions in Tableau.

### Install dependencies

Run in a terminal where Python and pip are available:

pip install tabpy scikit-learn pandas numpy

Start the TabPy server:

tabpy

If port `9004` is already in use on Windows, find the process:

netstat -ano | findstr :9004

Stop the process using its actual PID:

taskkill /PID <actual_pid_number> /F

Then start `tabpy` again. In Tableau, open **Help → Settings and Performance → Manage Analytics Extension Connection**, select TabPy, set server to `localhost` and port to `9004`, then test the connection.

Connect Tableau to `Iris.csv` using **Data → Connect to Data → Text File**.

### Tableau calculated field: `SVM Prediction`

Paste the following into the calculated field editor. The `SCRIPT_STR` expression is Tableau syntax, and the Python code inside it is executed by TabPy.

SCRIPT_STR(
"
from sklearn.svm import SVC
from sklearn.preprocessing import LabelEncoder
import pandas as pd

df = pd.DataFrame({
    'SepalLengthCm': _arg1,
    'SepalWidthCm': _arg2,
    'PetalLengthCm': _arg3,
    'PetalWidthCm': _arg4,
    'Species': _arg5
})

le = LabelEncoder()
y = le.fit_transform(df['Species'])
X = df[['SepalLengthCm', 'SepalWidthCm', 'PetalLengthCm', 'PetalWidthCm']]

model = SVC(kernel='rbf', C=1.0, gamma='scale')
model.fit(X, y)
preds = model.predict(X)

return le.inverse_transform(preds).tolist()
",
ATTR([Sepal Length Cm]),
ATTR([Sepal Width Cm]),
ATTR([Petal Length Cm]),
ATTR([Petal Width Cm]),
ATTR([Species])
)

### Build the scatter plot
1. Put `Id` on Detail and make it a discrete dimension.
2. Set the `SVM Prediction` table calculation to compute using `Id` (Specific Dimensions → check `Id`).
3. Set `Petal Length Cm` and `Petal Width Cm` to **Measure → Attribute**.
4. Columns = `ATTR(Petal Length Cm)`.
5. Rows = `ATTR(Petal Width Cm)`.
6. Marks → Color = `SVM Prediction`; Detail = `Id`.

**Expected result:** Iris observations appear in a scatter plot, colored by SVM prediction.

**Note:** This follows the lab manual's workflow: it fits and predicts on the same rows supplied to the calculation, so it demonstrates classification in Tableau but is not a held-out test of model accuracy.

---

```
## Program 4: Design Interactive Reports in Power BI Using Sorting, Filtering, Slicers, Drill-Down, and Ranking

```shell

**Objective:** Build an interactive sales report using the following sample data.

### Dataset

Create a table named `Sales` with these rows:

| Order ID | Region | State | City | Category | Product | Sales | Quantity | Profit |
|---|---|---|---|---|---|---:|---:|---:|
| O001 | South | Karnataka | Bengaluru | Technology | Laptop | 55000 | 2 | 8000 |
| O002 | South | Karnataka | Mysuru | Furniture | Chair | 12000 | 4 | 2500 |
| O003 | West | Maharashtra | Mumbai | Technology | Mobile | 30000 | 3 | 6000 |
| O004 | North | Delhi | Delhi | Office Supplies | Paper | 5000 | 10 | 1200 |
| O005 | West | Maharashtra | Pune | Furniture | Table | 18000 | 2 | 3500 |

### Visuals to create
1. **Sales by Region:** Clustered Column Chart; X-axis = `Region`; Y-axis = `Sales`; title = `Sales by Region`.
2. **Sales by Category:** Donut Chart; Legend = `Category`; Values = `Sales`.
3. **Product Sales:** Table visual containing `Product`, `Category`, `Sales`, and `Profit`.

### Sorting and filters
- Select the Sales by Region chart → three dots (`...`) → **Sort by → Sales → Descending**.
- Visual-level filter: select the Product Sales table, add `Category` to **Filters on this visual**, and select `Technology`.
- Page-level filter: add `Region` to **Filters on this page** and select `South`.
- Report-level filter: add the required field to **Filters on all pages**, then select the desired value.

### Slicers
- Add a Slicer visual and put `Region` in its Field.
- Optionally change the slicer style to Dropdown.
- Add a second Slicer and put `Category` in its Field.

### Drill-down
Create a Column Chart with this X-axis hierarchy in order:
1. `Region`
2. `State`
3. `City`

Enable the visual's Drill Down option. Select a region to see its states, then drill further into cities.

### DAX measure: Product Rank

Select **Modeling → New measure** and enter:

Product Rank =
RANKX(
    ALL('Sales'[Product]),
    CALCULATE(SUM('Sales'[Sales])),
    ,
    DESC,
    DENSE
)

Create a Table visual with `Product`, `Category`, `Sales`, and `Product Rank`. Sort the table by `Product Rank`.

**Expected result:** An interactive report with regional and category sales visuals, product details, sorting, filters, slicers, drill-down, and product ranking.

---

```
## Program 5: Create Measures, Calculated Columns, and a Calculated Table Using DAX in Power BI

```shell

**Objective:** Use aggregate and date functions in DAX on a table named `Sales`.

### Dataset

| OrderID | OrderDate | Product | Category | Quantity | Sales | Profit |
|---:|---|---|---|---:|---:|---:|
| 101 | 01-01-2025 | Laptop | Technology | 2 | 50000 | 8000 |
| 102 | 15-01-2025 | Chair | Furniture | 4 | 12000 | 3000 |
| 103 | 10-02-2025 | Mobile | Technology | 3 | 30000 | 5000 |
| 104 | 20-02-2025 | Table | Furniture | 2 | 18000 | 4000 |
| 105 | 05-03-2025 | Printer | Technology | 1 | 15000 | 2500 |

Load the dataset into Power BI and ensure `OrderDate` is recognized as a Date column.

### Measures

Choose **Modeling → New measure** for each formula.

**Total Sales**
Total Sales = SUM(Sales[Sales])

**Total Profit**
Total Profit = SUM(Sales[Profit])

**Average Sales**
Average Sales = AVERAGE(Sales[Sales])

**Total Quantity**
Total Quantity = SUM(Sales[Quantity])

### Calculated columns

Choose **Table view → New column** for each formula.

**Profit Status**
Profit Status = IF(Sales[Profit] >= 5000, "High", "Low")

**Order Year**
Order Year = YEAR(Sales[OrderDate])

**Order Month**
Order Month = MONTH(Sales[OrderDate])

### Calculated table

Choose **Modeling → New table** and enter:

Product Summary =
SUMMARIZE(
    Sales,
    Sales[Product],
    "Total Sales", SUM(Sales[Sales]),
    "Total Profit", SUM(Sales[Profit])
)

### Create the report
1. Add three Card visuals for `Total Sales`, `Total Profit`, and `Average Sales`.
2. Add a Table visual containing `Product`, `Sales`, `Profit`, and `Profit Status`.
3. Add a Column Chart with Axis = `Product` and Values = `Total Sales`.

**Expected result:** Measures, calculated columns, a product summary table, and report visuals that summarize sales and profit.

---

```

## Notes
- Tableau calculated fields and Power BI DAX formulas must be entered in their respective application editors; they are not shell commands.
- The shell blocks are included for terminal commands only (Program 3 dependency installation and TabPy startup).
- Use the field names shown in your imported dataset. If your CSV/Excel headers differ, update the formulas to match.
