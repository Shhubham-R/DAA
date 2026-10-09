# ADS23702 – Advanced Data Visualization Using Power BI Laboratory

## LAB PROGRAM 1

```shell
Connect Multiple Relational Datasets In Tableau And Build Relationships To Create Interactive Visualizations.

Steps:

1. Open Tableau Desktop.
2. Select Microsoft Excel under Connect.
3. Browse and open the Sample Superstore.xlsx dataset. Click on 'Sheet 1'.
4. Drag the Orders table onto the canvas.
5. Drag the Returns table next to Orders.
6. Tableau automatically suggests a relationship.
7. Verify the relationship:
   Orders → Order ID
   Returns → Order ID
8. Drag the People table.
9. Create the relationship:
   Orders → Region
   People → Region
10. Click the relationship line.
11. Ensure the following relationships exist:
    Click the relationship line.
    Ensure the following relationships exist:
12. Open Worksheet
13. Click Sheet 1.
14. Tableau loads all fields from the related tables.
15. Drag Region to Columns.
16. Drag Sales to Rows.
17. Choose Bar Chart.
18. Create a new worksheet.
19. Drag Order Date to Columns.
20. Change it to Year.
21. Drag Sales to Rows.
22. Select Line Chart.
23. Create another worksheet.
24. Drag Category to Columns.
25. Drag Profit to Rows.
26. Choose Bar Chart.
27. Drag Region to the Filters shelf.
28. Right-click Region.
29. Select Show Filter.
30. Repeat for Category.

Expected Result:

- Three related tables are displayed in the logical data model.
- Relationships are correctly established with no errors.
- Dimensions and Measures from all tables appear in the Data pane.
- A line chart displays yearly sales trends.
- A bar chart compares profit across product categories.
```

## LAB PROGRAM 2

```shell
Implement FIXED, INCLUDE, and EXCLUDE LOD expressions to solve business analytical problems using Tableau.

Steps:

1. Launch Tableau Desktop.
2. Connect to Sample Superstore.xlsx.
3. Select the Orders table.
4. Click Sheet 1.
5. Business Problem: Implement FIXED LOD Expression: Find the total sales for each customer, irrespective of the product category selected in the view.
6. Right-click inside the Data Pane.
7. Select Create Calculated Field.
   Name it: Customer Total Sales
   { FIXED [Customer Name] : SUM([Sales]) }
8. Drag Customer Name to Rows.
9. Drag Customer Total Sales to Columns.
10. Sort in descending order.
    Display as a Bar Chart.
11. Implement INCLUDE LOD Expression: Business Problem: Calculate average sales per customer within each region.
12. Create a calculated field:
13. Regional Customer Average:
    { INCLUDE [Customer Name] : AVG([Sales]) }
14. Drag Region to Rows.
15. Drag Regional Customer Average to Columns.
16. Display as Bar Chart.
17. Implement EXCLUDE LOD Expression: Business Problem
18. Calculate regional sales while excluding the Sub-Category level.
19. Create a calculated field.
20. Regional Sales:
    { EXCLUDE [Sub-Category] : SUM([Sales]) }
21. Drag Category.
22. Drag Sub-Category.
23. Drag Regional Sales.
24. Display as Highlight Table or Bar Chart.
25. Create three worksheets:
    Sheet 1 → FIXED
    Sheet 2 → INCLUDE
    Sheet 3 → EXCLUDE

Expected Result:
```

## LAB PROGRAM 3

```shell
Execute SMV classification algorithms from Tableau using TabPy.

Steps:

1. pip install tabpy scikit-learn pandas numpy
2. tabpy
3. (If you get OSError: [WinError 10048] Only one usage of each socket address..., a previous TabPy process is still running on that port.

   Find it: netstat -ano | findstr :9004

   Note the PID in the last column, then kill it: taskkill /PID <actual_pid_number> /F

   Run tabpy again.)
4. Help → Settings and Performance → Manage Analytics Extension Connection
5. Select TabPy
6. Server: localhost, Port: 9004
7. Test Connection → OK
8. Data → Connect to Data → Text File → select Iris.csv.
9. Create a new calculated field named SVM Prediction:
10. SCRIPT_STR("
11. from sklearn.svm import SVC
12. from sklearn.preprocessing import LabelEncoder
13. import pandas as pd

14. df = pd.DataFrame({
15. 'SepalLengthCm': _arg1, 'SepalWidthCm': _arg2,
16. 'PetalLengthCm': _arg3, 'PetalWidthCm': _arg4,
17. 'Species': _arg5
18. })

19. le = LabelEncoder()
20. y = le.fit_transform(df['Species'])
21. X = df[['SepalLengthCm','SepalWidthCm','PetalLengthCm','PetalWidthCm']]

22. model = SVC(kernel='rbf', C=1.0, gamma='scale')
23. model.fit(X, y)
24. preds = model.predict(X)
25. return le.inverse_transform(preds).tolist()
26. ",
27. ATTR([Sepal Length Cm]), ATTR([Sepal Width Cm]),
28. ATTR([Petal Length Cm]), ATTR([Petal Width Cm]), ATTR([Species]))
29. A. Put Id on Detail
30. B. Convert Id to a discrete and dimension
31. C. Set the table calculation to compute across all rows
32. Right-click the SVM Prediction pill (wherever you placed it, e.g. on Color)
33. Edit Table Calculation...
34. Compute Using → Specific Dimensions
35. Check Id
36. Click OK
37. D. Set the axis fields to ATTR, not SUM
38. Right-click each axis pill (Petal Length Cm, Petal Width Cm) → Measure → Attribute, so Tableau keeps one value per row instead of aggregating.
39. Build the Scatter Plot
40. Columns: ATTR(Petal Length Cm)
41. Rows: ATTR(Petal Width Cm)
42. Marks card
43. Color → SVM Prediction: Compute Using Id
44. Detail → Id (discrete)

Result: Different flower species are displayed in different colors based on the SVM prediction.
```

## LAB PROGRAM 4

```shell
Design interactive reports using sorting, filtering, slicers, drill-down, and multi-level ranking using PowerBI:

Dataset:

Order ID Region State City Category Product Sales Quantity Profit
O001 South Karnataka Bengaluru Technology Laptop 55000 2 8000
O002 South Karnataka Mysuru Furniture Chair 12000 4 2500
O003 West Maharashtra Mumbai Technology Mobile 30000 3 6000
O004 North Delhi Delhi Office Supplies Paper 5000 10 1200
O005 West Maharashtra Pune Furniture Table 18000 2 3500

Steps:

1. Load the Dataset: Interactive Sales dataset
2. Create Visual 1 – Sales by Region
3. Select Clustered Column Chart.
4. Drag Region → X-axis.
5. Drag Sales → Y-axis.
6. Rename the title to Sales by Region.
7. Visual 2 – Sales by Category
8. Select Donut Chart.
9. Drag Category → Legend.
10. Drag Sales → Values.
11. Visual 3 – Product Sales
12. Select Table.
13. Add: Product, Category, Sales, Profit
14. This table will later be used for sorting and ranking.
15. Apply Sorting
16. Select the Sales by Region chart.
17. Click the three dots (…) on the visual.
18. Select Sort by → Sales.
19. Select Descending.
20. Apply Filters
21. A. Visual-level Filter
22. Select the Product Sales table.
23. Locate the Filters pane.
24. Drag Category into Filters on this visual.
25. Select Technology.
26. B. Page-level Filter
27. Drag Region into Filters on this page.
28. Select South.
29. C. Report-level Filter
30. Drag the required field into Filters on all pages.
31. Select the required value.
32. Add a Slicer
33. Click a blank area of the report canvas.
34. Select Slicer from the Visualizations pane.
35. Drag Region into the slicer's Field.
36. Change the slicer style to Dropdown if required.
37. Create a Category Slicer
38. Add another slicer.
39. Drag Category into it.
40. Select Technology, Furniture, etc.
41. Create Drill-Down
42. Create a Column Chart
43. Drag Region to the X-axis.
44. Drag State below Region in the X-axis hierarchy.
45. Drag City below State.
46. Enable the Drill Down button on the visual.
47. Now select a region such as South. Power BI will display the states belonging to South.
48. Create a Multi-Level Ranking
49. Modeling → New Measure
50. Product Rank =
51. RANKX(
52. ALL('Sales'[Product]),
53. CALCULATE(SUM('Sales'[Sales])),
54. ,
55. DESC,
56. DENSE
57. )
58. Create the Ranking Table
59. Insert a Table visual.
60. Add Product, Category, Sales, Product Rank
61. Sort the table by Product Rank.

Result:
```

## LAB PROGRAM 5

```shell
Create measures, calculated columns, and calculated tables using aggregate and date functions in DAX using PowerBI

Dataset: Sales

OrderID OrderDate Product Category Quantity Sales Profit
101 01-01-2025 Laptop Technology 2 50000 8000
102 15-01-2025 Chair Furniture 4 12000 3000
103 10-02-2025 Mobile Technology 3 30000 5000
104 20-02-2025 Table Furniture 2 18000 4000
105 05-03-2025 Printer Technology 1 15000 2500

Steps:

1. Load the Dataset
2. Create Measures
3. Select Modeling → New Measure.
4. Total Sales: Total Sales = SUM(Sales[Sales])
5. Total Profit: Total Profit = SUM(Sales[Profit])
6. Average Sales: Average Sales = AVERAGE(Sales[Sales])
7. Total Quantity: Total Quantity = SUM(Sales[Quantity])
8. Create a Calculated Column
9. Select Table view → New Column.
10. Profit Status: IF(Sales[Profit] >= 5000, "High", "Low")
11. Order Year: Order Year = YEAR(Sales[OrderDate])
12. Order Month: Order Month = MONTH(Sales[OrderDate])
13. Create a Calculated Table
14. Select Modeling → New Table.
15. Product Summary =
16. SUMMARIZE(
17. Sales,
18. Sales[Product],
19. "Total Sales", SUM(Sales[Sales]),
20. "Total Profit", SUM(Sales[Profit])
21. )
22. Create a Simple Report
23. Create three Card visuals: Total Sales, Total Profit, Average Sales
24. Create one Table visual containing: Product | Sales | Profit | Profit Status
25. Create one Column Chart: Axis: Product, Values: Total Sales

Result:
```
