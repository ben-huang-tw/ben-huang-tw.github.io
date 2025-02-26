# LINQ to SQL Examples With Method


本範例中的 SQL 大部分來自於 [ZenTut - SQL tutorial](http://www.zentut.com/sql-tutorial/)

## SELECT Column Examples

```cs
//SELECT CompanyName,City FROM Customers;
var query = from c in db.Customers
            select new
            {
                c.CompanyName,
                c.City
            };
```

```cs
//SELECT LastName + ', ' + FirstName AS fullname FROM employees
var query = db.Employees
    .Select(x => new { FullName = String.Concat(x.LastName, ", ", x.FirstName) });
```

## SELECT * Example

```cs
//SELECT * FROM Customers;
var query = db.Customers;
```

## SELECT DISTINCT Example

```cs
//SELECT DISTINCT City FROM Customers;
var query = db.Customers
    .Select(x => new { x.City })
    .Distinct();
```

## WHERE Clause Examples

### 等於
```cs
//SELECT * FROM Customers WHERE Country='Mexico';
var query = db.Customers
    .Where(x => x.Country == "Mexico");
```

### 不等於

```cs
//SELECT LastName,FirstName,Title,Country FROM Employees WHERE Country <> 'USA'
var query = db.Employees
    .Where(e => e.Country != "USA")
    .Select(e => new { e.LastName, e.FirstName, e.Title, e.Country });
```

### 小於

```cs
//SELECT LastName,FirstName,Title,Country,HireDate FROM Employees WHERE HireDate < '1993-01-01'
var query = db.Employees
    .Where(e => e.HireDate < new DateTime(1993, 1, 1))
    .Select(e => new { e.LastName, e.FirstName, e.Title, e.Country, e.HireDate });
```

### 大於或等於

```cs
//SELECT LastName,FirstName,Title,Country,HireDate FROM Employees WHERE HireDate >= '1993-01-01'
var query = db.Employees
    .Where(e => e.HireDate >= new DateTime(1993, 1, 1))
    .Select(e => new { e.LastName, e.FirstName, e.Title, e.Country, e.HireDate });
```

### AND

```cs
//SELECT * FROM Customers WHERE Country='Germany' AND City='Berlin';
var query = db.Customers
    .Where(c => c.Country == "Germany" && c.City == "Berlin");
```

### OR

```cs
//SELECT * FROM Customers WHERE City='Berlin' OR Country='Germany';
var query = db.Customers
    .Where(c => c.City == "Berlin" || c.Country == "Germany");
```

```cs
//SELECT * FROM Customers WHERE City='Berlin' OR City='Munchen';
var query = db.Customers
    .Where(c => c.City == "Berlin" || c.City == "Munchen");
```

### 結合 AND 和 OR

```cs
//SELECT * FROM Customers WHERE Country='Germany' AND (City='Berlin' OR Country='Germany');
var query = db.Customers
    .Where(c => c.Country == "Germany" &&
        (c.City == "Berlin" || c.Country == "Germany"));
```

### NOT

```cs
//SELECT firstname, lastname, city
//FROM Employees
//WHERE NOT (city = 'London' OR city = 'Seattle')
var query = db.Employees
    .Where(e => !(e.City == "London" || e.City == "Seattle"))
    .Select(e => new { e.FirstName, e.LastName, e.City });
```

### IS NULL

```cs
//SELECT CompanyName, Fax FROM Suppliers WHERE Fax IS NULL;
var query = db.Suppliers
    .Where(s => s.Fax == null)
    .Select(s => new { s.CompanyName, s.Fax });
```

### IS NOT NULL

```cs
//SELECT CompanyName, Fax FROM Suppliers WHERE Fax IS NOT NULL;
var query = db.Suppliers
    .Where(s => s.Fax != null)
    .Select(s => new { s.CompanyName, s.Fax });
```

### EXISTS

```cs
//SELECT CustomerID,CompanyName
//FROM Customers
//WHERE EXISTS (SELECT OrderID
//        FROM Orders
//        WHERE Orders.CustomerID = Customers.CustomerID
//        );
var query = db.Customers
    .Where(c => db.Orders
        .Where(o => o.CustomerID == c.CustomerID)
        .Any())
    .Select(c => new { c.CustomerID, c.CompanyName });
```

### NOT EXISTS

```cs
//SELECT CustomerID,CompanyName
//FROM Customers
//WHERE NOT EXISTS (SELECT OrderID
//        FROM Orders
//        WHERE Orders.CustomerID = Customers.CustomerID
//        );
var query = db.Customers
    .Where(c => !db.Orders
        .Where(o => o.CustomerID == c.CustomerID)
        .Any())
    .Select(c => new { c.CustomerID, c.CompanyName });
```

## ORDER BY Examples

### ORDER BY ASC

```cs
//SELECT * FROM Customers ORDER BY Country;
var query = db.Customers.OrderBy(c => c.Country);
```

### ORDER BY DESC

```cs
//SELECT * FROM Customers ORDER BY Country DESC;
var query = db.Customers
    .OrderByDescending(c => c.Country);
```

### ORDER BY Several Columns

```cs
//SELECT * FROM Customers ORDER BY Country,CompanyName;
var query = db.Customers
    .OrderBy(c => c.Country)
    .ThenBy(c => c.CompanyName);
```

## SELECT TOP Example

```cs
//SELECT TOP 2 * FROM Customers;
var query = db.Customers.Take(2);
```

## SQL LIKE Operator Examples

找出城市名稱以 `s` 開頭的城市：

```cs
//SELECT * FROM Customers WHERE City LIKE 's%';
var query = db.Customers
    .Where(c => c.City.StartsWith("s"));
```

找出城市名稱以 `s` 結尾的城市：

```cs
//SELECT * FROM Customers WHERE City LIKE '%s';
var query = db.Customers
    .Where(c => c.City.EndsWith("s"));
```

找出城市名稱包含 `s` 的城市：

```cs
//SELECT * FROM Customers WHERE City LIKE '%s%';
var query = db.Customers
    .Where(c => c.City.Contains("s"));
```

## IN Operator Example

```cs
//SELECT * FROM Customers WHERE City IN ('Paris','London','Seattle');
var query = db.Customers
    .Where(c => (new string[] { "Paris", "London", "Seattle" })
        .Contains(c.City));
```

## INNER JOIN Examples

```cs
//SELECT Orders.OrderID, Customers.CompanyName, Orders.OrderDate
//FROM Orders
//INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
var query = db.Orders
    .Join(db.Customers, o => o.CustomerID, c => c.CustomerID,
        (o, c) => new { o.OrderID, c.CompanyName, o.OrderDate });
```

### querying data from two tables

```cs
//SELECT productID, productName, categoryName
//FROM products
//INNER JOIN categories ON categories.categoryID = products.categoryID;
var query = db.Products
    .Join(db.Categories, p => p.CategoryID, c => c.CategoryID,
        (p, c) => new { p.ProductID, p.ProductName, c.CategoryName });
```

### querying data from three tables

```cs
//SELECT productID, productName, categoryName, companyName AS supplier
//FROM products
//INNER JOIN categories ON categories.categoryID = products.categoryID
//INNER JOIN suppliers ON suppliers.supplierID = products.supplierID
var query = db.Products
    .Join(db.Categories, p => p.CategoryID, c => c.CategoryID,
        (p, c) => new { p, c })
    .Join(db.Suppliers, t => t.p.SupplierID, s => s.SupplierID,
        (t,s) => new { t.p.ProductID, t.p.ProductName, t.c.CategoryName, s.CompanyName });
```

### self join with inner join

```cs
//SELECT e.firstname + ' ' + e.lastname AS Employee,
//       m.firstname + ' ' + m.lastname AS Manager
//FROM employees AS e
//INNER JOIN employees AS m ON m.employeeid = e.reportsto
var query = db.Employees
    .Join(db.Employees, e => e.ReportsTo, m => m.EmployeeID,
        (e, m) => new
        {
            Employee = e.FirstName + " " + e.LastName,
            Manager = m.FirstName + " " + m.LastName
        });
```

## LEFT JOIN Examples

### left outer join

```cs
//SELECT c.customerid, c.companyName, orderid
//FROM customers AS c
//LEFT JOIN orders AS o ON o.customerid = c.customerid
//ORDER BY orderid
var query = db.Customers
    .GroupJoin(db.Orders, c => c.CustomerID, o => o.CustomerID,
        (c, o) => new { c, Orders = o })
    .SelectMany(x => x.Orders.DefaultIfEmpty(),
        (x, o) => new
        {
            x.c.CustomerID,
            x.c.CompanyName,
            OrderID = (int?)o.OrderID
        })
    .OrderBy(x => x.OrderID);
```

註：`GroupJoin` 會產生一對零到多的巢狀清單(左邊的每一個可能對應到右邊的零個以上的項目，右邊的資料會變成內層的清單)。`SelectMany`可將內層的資料展開。請記得要使用 `DefaultIfEmpty()` 否則會變成 `INNER JOIN`。

### self join with left join

```cs
//SELECT e.FirstName + ' ' + e.LastName AS Employee,
//       m.FirstName + ' ' + m.LastName AS Manager
//FROM Employees AS e
//LEFT JOIN Employees AS m ON m.EmployeeID = e.ReportsTo
//ORDER BY Manager

var query = db.Employees
    .GroupJoin(db.Employees, e => e.ReportsTo, m => m.EmployeeID,
        (e, m) => new { e, m })
    .SelectMany(x => x.m.DefaultIfEmpty(),
        (x, m) => new
            {
                Employee = x.e.FirstName + " " + x.e.LastName,
                Manager = m != null ? m.FirstName + " " + m.LastName : null
            })
    .OrderBy(x => x.Manager);
```

## UNION Example

```cs
//SELECT City FROM Customers
//UNION
//SELECT City FROM Suppliers
//ORDER BY City;
var query = db.Customers.Select(x => new { x.City })
    .Union(db.Suppliers.Select(x => new { x.City }))
    .OrderBy(x => x.City);
```

## UNION ALL Example

```cs
//SELECT City FROM Customers
//UNION ALL
//SELECT City FROM Suppliers
//ORDER BY City;
var query = db.Customers.Select(c => new { c.City })
    .Concat(db.Suppliers.Select(s => new { s.City }))
    .OrderBy(x => x.City);
```

## GROUP BY Examples

### GROUP BY with SUM function

```cs
//SELECT CategoryID,SUM(UnitsInStock)
//FROM Products
//GROUP BY CategoryID;
var query = db.Products
    .GroupBy(x => new { x.CategoryID })
    .Select(x => new
        {
            x.Key.CategoryID,
            SumOfUnitsInStock = x.Sum(y => y.UnitsInStock)
        });
```

註：`GroupBy` 會產生兩層的巢狀清單。內層每個項目是可列舉的清單(`IGrouping<TKey, TElement>`)，它還具有 `Key` 屬性，以表示群組的鍵值。

### GROUP BY with COUNT function

```cs
//SELECT CategoryID, COUNT(ProductID) AS NumberOfProduct
//FROM Products
//GROUP BY CategoryID;
var query = db.Products
    .GroupBy(p => new {p.CategoryID})
    .Select(g => new {
        g.Key.CategoryID,
        NumberOfProduct = g.Count()
    });
```

### GROUP BY with AVG function

```cs
//SELECT CategoryID, AVG(UnitsInStock) AS AvgUnitsInStock
//FROM Products
//GROUP BY CategoryID;
var query = db.Products
    .GroupBy(p => new { p.CategoryID })
    .Select(g => new
    {
        g.Key.CategoryID,
        AvgUnitsInStock = g.Average(x => x.UnitsInStock)
    });
```

### GROUP BY with MIN and MAX functions

```cs
//SELECT CategoryID
//    ,MIN(UnitsInStock) AS MinUnitsInStock
//    ,MAX(UnitsInStock) AS MaxUnitsInStock
//FROM Products
//GROUP BY CategoryID;
var query = db.Products
    .GroupBy(x => new { x.CategoryID })
    .Select(g => new {
        g.Key.CategoryID,
        MinUnitsInStock = g.Min(p => p.UnitsInStock),
        MaxUnitsInStock = g.Max(p => p.UnitsInStock)
    });
```

### GROUP BY with ORDER BY

```cs
//SELECT CategoryID,COUNT(ProductID) AS NumberOfProduct
//FROM Products
//GROUP BY CategoryID
//ORDER BY COUNT(ProductID) DESC;
var query = db.Products
    .GroupBy(p => new { p.CategoryID })
    .Select(g => new
    {
        g.Key.CategoryID,
        NumberOfProduct = g.Count()
    })
    .OrderByDescending(x => x.NumberOfProduct);
```

### LEFT JOIN and GROUP BY

```cs
//SELECT Shippers.CompanyName,COUNT(Orders.OrderID) AS NumberOfOrders
//FROM Orders
//LEFT JOIN Shippers
//ON Orders.ShipVia=Shippers.ShipperID
//GROUP BY CompanyName;
var query = db.Orders
    .GroupJoin(db.Shippers, o => o.ShipVia, s => s.ShipperID,
    (o, s) => new { o, s })
    .SelectMany(x => x.s.DefaultIfEmpty(),
    (x, s) => new { s.CompanyName, x.o.OrderID })
    .GroupBy(x => new { x.CompanyName })
    .Select(x => new
    {
        x.Key.CompanyName,
        NumberOfOrders = x.Count()
    });
```

註：`GroupJoin` 會產生一個與 `GroupBy` 類似的巢狀清單，而外層清單中的每個Outter項會對應到零個以上的Inner(為List)，如果 join key 對不到 inner 中項目時，Inner List 不會是 `null`，但不包含任何項目(即 Count 等於 0，也就是 Empty)，所以上面範例中的 `DefaultIfEmpty()` 會將 Empty 轉成 null。而且必須使用 `DefaultIfEmpty()` 才會產生 `LEFT JOIN` 的 SQL。

### HAVING with SUM function

```cs
//SELECT OrderID ,SUM(UnitPrice * Quantity) AS Total
//FROM [Order Details]
//GROUP BY OrderID
//HAVING SUM(UnitPrice * Quantity) > 12000
var query = db.Order_Details
    .GroupBy(od => new { od.OrderID })
    .Select(g => new {
        g.Key.OrderID,
        Total = g.Sum(x => x.UnitPrice * x.Quantity)
    })
    .Where(x => x.Total > 12000);
```

### HAVING with COUNT function

```cs
//SELECT OrderID, COUNT(ProductID) AS Products
//FROM [Order Details]
//GROUP BY OrderID
//HAVING COUNT(ProductID) > 5;
var query = db.Order_Details
    .GroupBy(od => od.OrderID)
    .Select(x => new { OrderID = x.Key, Products = x.Count() })
    .Where(x => x.Products > 5);
```

### HAVING clause with MAX functions

```cs
//SELECT categoryID, productID, productName, MAX(unitprice) AS MaxUnitPrice
//FROM products A
//WHERE unitprice = (
//        SELECT MAX(unitprice)
//        FROM products B
//        WHERE B.categoryId = A.categoryID
//        )
//GROUP BY categoryID, productID, productName
//HAVING MAX(unitprice) > 100
var query = db.Products
    .Where(a => a.UnitPrice == (
        db.Products.Where(b => b.CategoryID == a.CategoryID)
        .Select(x => x.UnitPrice)).Max())
    .GroupBy(x => new { x.CategoryID, x.ProductID, x.ProductName })
    .Select(g => new {
        g.Key.CategoryID,
        g.Key.ProductID,
        g.Key.ProductName,
        MaxUnitPrice = g.Max(x => x.UnitPrice)
    })
    .Where(x => x.MaxUnitPrice > 100);
```

## SQL subquery examples

```cs
//SELECT customerid, companyname, city
//FROM customers
//WHERE customerid <> 'BSBEV'
//    AND city = (
//        SELECT city
//        FROM customers
//        WHERE customerid = 'BSBEV'
//        )
var query = db.Customers
    .Where(c => c.CustomerID != "BSBEV" &&
        c.City == (db.Customers.Where(x => x.CustomerID == "BSBEV")
        .Select(x => x.City).FirstOrDefault())
    )
    .Select(c => new {
        c.CustomerID,
        c.CompanyName,
        c.City
    });
```

註：上述範例所產生的 SQL 不太相同，但結果是一樣的。

### subquery with IN operator

```cs
//SELECT orderid, customerid, shipname
//FROM orders
//WHERE customerid IN (
//        SELECT customerid
//        FROM customers
//        WHERE country = 'USA'
//        )
var query = db.Orders
    .Where(o => db.Customers.Where(x => x.Country == "USA")
        .Select(x => x.CustomerID).Contains(o.CustomerID))
    .Select(o => new {
        o.OrderID,
        o.CustomerID,
        o.ShipName
    });
```

註：上述範例所產生的 SQL 不太相同，但結果是一樣的。


