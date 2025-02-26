# LINQ to SQL Examples

本範例中的 SQL 大部分來自於 [ZenTut - SQL tutorial](http://www.zentut.com/sql-tutorial/)

## SELECT Column Examples

```cs
//SELECT CompanyName,City FROM Customers;
var query = db.Customers
    .Select(x => new { x.CompanyName, x.City });
```

```cs
//SELECT LastName + ', ' + FirstName AS fullname FROM employees
var query = from e in db.Employees
            select new { fullname = String.Concat(e.LastName, ", ", e.FirstName) };
```

## SELECT * Example

```cs
//SELECT * FROM Customers;
var query = from c in db.Customers
            select c;
```

## SELECT DISTINCT Example

```cs
//SELECT DISTINCT City FROM Customers;
var query = (from c in db.Customers
             select new { c.City }).Distinct();
```

## WHERE Clause Examples

### 等於
```cs
//SELECT * FROM Customers WHERE Country='Mexico';
var query = from c in db.Customers
            where c.Country == "Mexico"
            select c;
```

### 不等於

```cs
//SELECT LastName,FirstName,Title,Country FROM Employees WHERE Country <> 'USA'
var query = from e in db.Employees
            where e.Country != "USA"
            select new { e.LastName, e.FirstName, e.Title, e.Country };
```

### 小於

```cs
//SELECT LastName,FirstName,Title,Country,HireDate FROM Employees WHERE HireDate < '1993-01-01'
var query = from e in db.Employees
            where e.HireDate < new DateTime(1993, 1, 1)
            select new { e.LastName, e.FirstName, e.Title, e.Country, e.HireDate };
```

### 大於或等於

```cs
//SELECT LastName,FirstName,Title,Country,HireDate FROM Employees WHERE HireDate >= '1993-01-01'
var query = from e in db.Employees
            where e.HireDate >= new DateTime(1993, 1, 1)
            select new { e.LastName, e.FirstName, e.Title, e.Country, e.HireDate };
```

### AND

```cs
//SELECT * FROM Customers WHERE Country='Germany' AND City='Berlin';
var query = from c in db.Customers
            where c.Country == "Germany" && c.City == "Berlin"
            select c;
```

### OR

```cs
//SELECT * FROM Customers WHERE City='Berlin' OR Country='Germany';
var query = from c in db.Customers
            where c.City == "Berlin" || c.Country == "Germany"
            select c;
```


```cs
//SELECT * FROM Customers WHERE City='Berlin' OR City='Munchen';
var query = from c in db.Customers
            where c.City == "Berlin" || c.City == "Munchen"
            select c;
```

### 結合 AND 和 OR

```cs
//SELECT * FROM Customers WHERE Country='Germany' AND (City='Berlin' OR City='Munchen');
var query = from c in db.Customers
            where c.Country == "Germany" && (c.City == "Berlin" || c.City == "Munchen")
            select c;
```

### NOT

```cs
//SELECT firstname, lastname, city FROM Employees WHERE
//NOT (city = 'London' OR city = 'Seattle')
var query = from e in db.Employees
            where !(e.City == "London" || e.City == "Seattle")
            select new { e.FirstName, e.LastName, e.City };
```

### IS NULL

```cs
//SELECT CompanyName, Fax FROM Suppliers WHERE Fax IS NULL;
var query = from s in db.Suppliers
            where s.Fax == null
            select new { s.CompanyName, s.Fax };
```

### IS NOT NULL

```cs
//SELECT CompanyName, Fax FROM Suppliers WHERE Fax IS NOT NULL;
var query = from s in db.Suppliers
            where s.Fax != null
            select new { s.CompanyName, s.Fax };
```

### EXISTS

```cs
//SELECT CustomerID,CompanyName
//FROM Customers
//WHERE EXISTS (SELECT OrderID
//        FROM Orders
//        WHERE Orders.CustomerID = Customers.CustomerID
//        );
var query = from c in db.Customers
            where (from o in db.Orders
                   where o.CustomerID == c.CustomerID
                   select o.OrderID).Any()
            select new { c.CustomerID, c.CompanyName };
```

### NOT EXISTS

```cs
//SELECT CustomerID,CompanyName
//FROM Customers
//WHERE NOT EXISTS (SELECT OrderID
//        FROM Orders
//        WHERE Orders.CustomerID = Customers.CustomerID
//        );
var query = from c in db.Customers
            where !(from o in db.Orders
                   where o.CustomerID == c.CustomerID
                   select o.OrderID).Any()
            select new { c.CustomerID, c.CompanyName };
```

## ORDER BY Examples

### ORDER BY ASC

```cs
//SELECT * FROM Customers ORDER BY Country;
var query = from c in db.Customers
            orderby c.Country
            select c;
```

### ORDER BY DESC

```cs
//SELECT * FROM Customers ORDER BY Country DESC;
var query = from c in db.Customers
            orderby c.Country descending
            select c;
```

### ORDER BY Several Columns

```cs
//SELECT * FROM Customers ORDER BY Country,CompanyName;
var query = from c in db.Customers
            orderby c.Country, c.CompanyName
            select c;
```

## SELECT TOP Example

```cs
//SELECT TOP 2 * FROM Customers;
var query = (from c in db.Customers
             select c).Take(2);
```

## SQL LIKE Operator Examples

找出城市名稱以 `s` 開頭的城市：

```cs
//SELECT * FROM Customers WHERE City LIKE 's%';
var query = from c in db.Customers
            where c.City.StartsWith("s")
            select c;
```

找出城市名稱以 `s` 結尾的城市：

```cs
//SELECT * FROM Customers WHERE City LIKE '%s';
var query = from c in db.Customers
            where c.City.EndsWith("s")
            select c;
```

找出城市名稱包含 `s` 的城市：

```cs
//SELECT * FROM Customers WHERE City LIKE '%s%';
var query = from c in db.Customers
            where c.City.Contains("s")
            select c;
```

## IN Operator Example

```cs
//SELECT * FROM Customers WHERE City IN ('Paris','London','Seattle');
var query = from c in db.Customers
            where (new string[] { "Paris", "London", "Seattle" }).Contains(c.City)
            select c;
```

## INNER JOIN Examples

```cs
//SELECT Orders.OrderID, Customers.CompanyName, Orders.OrderDate
//FROM Orders
//INNER JOIN Customers
//ON Orders.CustomerID=Customers.CustomerID;
var query = from o in db.Orders
            join c in db.Customers on o.CustomerID equals c.CustomerID
            select new { o.OrderID, c.CompanyName, o.OrderDate};
```

### querying data from two tables

```cs
//SELECT
//    productID, productName, categoryName
//FROM
//    products
//INNER JOIN
//    categories ON categories.categoryID = products.categoryID;
var query = from p in db.Products
            join c in db.Categories on p.CategoryID equals c.CategoryID
            select new { p.ProductID, p.ProductName, c.CategoryName };
```

### querying data from three tables

```cs
//SELECT
//    productID,
//    productName,
//    categoryName,
//    companyName AS supplier
//FROM
//    products
//INNER JOIN
//    categories ON categories.categoryID = products.categoryID
//INNER JOIN
//    suppliers ON suppliers.supplierID = products.supplierID
var query = from p in db.Products
            join c in db.Categories on p.CategoryID equals c.CategoryID
            join s in db.Suppliers on p.SupplierID equals s.SupplierID
            select new { p.ProductID, p.ProductName, c.CategoryName, s.CompanyName };
```

### self join with inner join

```cs
//SELECT
//    e.firstname + ' ' + e.lastname AS Employee,
//    m.firstname + ' ' + m.lastname AS Manager
//FROM
//    employees AS e
//INNER JOIN
//    employees AS m ON m.employeeid = e.reportsto
var query = from e in db.Employees
            join m in db.Employees
                on e.ReportsTo equals m.EmployeeID
            select new
            {
                Employee = e.FirstName + " " + e.LastName,
                Manager = m.FirstName + " " + m.LastName
            };
```

## LEFT JOIN Examples

### left outer join

```cs
//SELECT c.customerid,
//       c.companyName,
//       orderid
//FROM customers c
//LEFT JOIN orders o ON o.customerid = c.customerid
//ORDER BY orderid
var query = from c in db.Customers
            join o in db.Orders on c.CustomerID equals o.CustomerID into Groups
            from g in Groups.DefaultIfEmpty()
            orderby g.OrderID
            select new
            {
                c.CustomerID, c.CompanyName,
                OrderID = g == null ? null : (int?)g.OrderID
            };
```

### self join with left join

```cs
//SELECT
//    e.FirstName + ' ' + e.LastName AS Employee,
//    m.FirstName + ' ' + m.LastName AS Manager
//FROM
//    Employees AS e
//LEFT JOIN
//    Employees AS m ON m.EmployeeID = e.ReportsTo
//ORDER BY Manager
var query = from e in db.Employees
            join m in db.Employees
                on e.ReportsTo equals m.EmployeeID
            into ManagerGroup
            from j in ManagerGroup.DefaultIfEmpty()
            select new
            {
                Employee = e.FirstName + " " + e.LastName,
                Manager = j.FirstName + " " + j.LastName
            }
                into t
                orderby t.Manager
                select t;
```



## UNION Example

```cs
//SELECT City FROM Customers
//UNION
//SELECT City FROM Suppliers
//ORDER BY City;
var query = from u in
                ((from c in db.Customers select c.City)
                    .Union(from s in db.Suppliers select s.City))
            orderby u
            select new { City = u };
```

## UNION ALL Example

```cs
//SELECT City FROM Customers
//UNION ALL
//SELECT City FROM Suppliers
//ORDER BY City;
var query = from u in
                ((from c in db.Customers select c.City)
                    .Concat(from s in db.Suppliers select s.City))
            orderby u
            select new { City = u };
```


## GROUP BY Examples

### GROUP BY with SUM function

```cs
//SELECT CategoryID,SUM(UnitsInStock)
//FROM Products
//GROUP BY CategoryID;
var query = from p in db.Products
            group p by p.CategoryID into categoryGroup
            select new
            {
                CategoryID = categoryGroup.Key,
                SumOfUnitsInStock = categoryGroup.Sum(p => p.UnitsInStock)
            };
```

註：`group...by...into categoryGroup` 會產生兩層的巢狀清單。`categoryGroup` 表示內層的清單，它還具有 `Key` 屬性，以表示群組的鍵值。

若要了解上述的 LINQ 關鍵字查詢的意義，可以用 LINQ 查詢方法來理解：

```cs
var query = db.Products
   .GroupBy (p => p.CategoryID)
   .Select (
      g =>
         new
         {
            CategoryID = g.Key,
            SumOfUnitsInStock = g.Sum (p => (Int32?)(p.UnitsInStock))
         }
   )
```

### GROUP BY with COUNT function

```cs
//SELECT CategoryID, COUNT(ProductID) AS NumberOfProduct
//FROM Products
//GROUP BY CategoryID;
var query = from p in db.Products
            group p by p.CategoryID into categoryGroup
            select new
            {
                CategoryID = categoryGroup.Key,
                NumberOfProduct = categoryGroup.Count()
            };
```

### GROUP BY with AVG function

```cs
//SELECT CategoryID, AVG(UnitsInStock) AS AvgUnitsInStock
//FROM Products
//GROUP BY CategoryID;
var query = from p in db.Products
            group p by p.CategoryID into categoryGroup
            select new
            {
                CategoryID = categoryGroup.Key,
                AvgUnitsInStock = categoryGroup.Average(p => p.UnitsInStock)
            };
```

### GROUP BY with MIN and MAX functions

```cs
//SELECT CategoryID
//    ,MIN(UnitsInStock) AS MinUnitsInStock
//    ,MAX(UnitsInStock) AS MaxUnitsInStock
//FROM Products
//GROUP BY CategoryID;
var query = from p in db.Products
            group p by p.CategoryID into categoryGroup
            select new
            {
                CategoryID = categoryGroup.Key,
                MinUnitsInStock = categoryGroup.Min(p => p.UnitsInStock),
                MaxUnitsInStock = categoryGroup.Max(p => p.UnitsInStock)
            };
```

### GROUP BY with ORDER BY

```cs
//SELECT CategoryID,COUNT(ProductID) AS NumberOfProduct
//FROM Products
//GROUP BY CategoryID
//ORDER BY COUNT(ProductID) DESC;
var query = from c in
                (from p in db.Products
                 group p by p.CategoryID into categoryGroup
                 select new
                 {
                     CategoryID = categoryGroup.Key,
                     NumberOfProduct = categoryGroup.Count()
                 })
            orderby c.NumberOfProduct descending
            select c;
```

### LEFT JOIN and GROUP BY

```cs
//SELECT Shippers.CompanyName,COUNT(Orders.OrderID) AS NumberOfOrders
//FROM Orders
//LEFT JOIN Shippers
//ON Orders.ShipVia=Shippers.ShipperID
//GROUP BY CompanyName;
var query = from o in db.Orders
            join s in db.Shippers on o.ShipVia equals s.ShipperID into Shippers
            from os in Shippers.DefaultIfEmpty()
            select new { os.CompanyName, o.OrderID } into j
            group j by j.CompanyName into g
            select new { CompanyName = g.Key, NumberOfOrders = g.Count() };
```

註：`join ... into Shippers` 會產生一個與 Group 類似的巢狀清單，而 `Shippers` 則是外層清單中的每個項目，如果 join key 對不到 inner 中項目時，`Shippers` 不會是 `null`，但不包含任何項目(即 Count 等於 0，也就是 Empty)，所以上面範例中的 `DefaultIfEmpty()` 會將 Empty 轉成 null。另外，必須使用 `DefaultIfEmpty()` 才會產生 `LEFT JOIN` 的 SQL。

### HAVING with SUM function

```cs
//SELECT OrderID ,SUM(UnitPrice * Quantity) AS Total
//FROM [Order Details]
//GROUP BY OrderID
//HAVING SUM(UnitPrice * Quantity) > 12000
var query = from od in db.Order_Details
            group od by od.OrderID into g
            let Total = g.Sum(p => p.UnitPrice * p.Quantity)
            where Total > 12000
            select new { OrderID = g.Key, Total };
```

### HAVING with COUNT function

```cs
//SELECT OrderID, COUNT(ProductID) AS Products
//FROM [Order Details]
//GROUP BY OrderID
//HAVING COUNT(ProductID) > 5;
var query = from od in db.Order_Details
            group od by od.OrderID into g
            let Products = g.Count()
            where Products > 5
            select new { OrderID = g.Key, Products };
```

### HAVING clause with MAX functions

```cs
//SELECT
//    categoryID, productID, productName, MAX(unitprice) AS MaxUnitPrice
//FROM
//    products A
//WHERE
//    unitprice = (
//    SELECT
//            MAX(unitprice)
//        FROM
//            products B
//        WHERE
//            B.categoryId = A.categoryID)
//GROUP BY categoryID,productID,productName
//HAVING MAX(unitprice) > 100
var query = from a in db.Products
            where a.UnitPrice == (
                from b in db.Products
                where b.CategoryID == a.CategoryID
                select b.UnitPrice
            ).Max()
            group a by new { a.CategoryID, a.ProductID, a.ProductName } into g
            where g.Max(x => x.UnitPrice) > 100
            select new
            {
                g.Key.CategoryID,
                g.Key.ProductID,
                g.Key.ProductName,
                MaxUnitPrice = g.Max(x => x.UnitPrice)
            };
```

## SQL subquery examples

```cs
//SELECT
//    customerid, companyname, city
//FROM
//    customers
//WHERE
//    customerid <> 'BSBEV'
//        AND city = (SELECT
//            city
//        FROM
//            customers
//        WHERE
//            customerid = 'BSBEV')
var query = from c in db.Customers
            where c.CustomerID != "BSBEV" &&
                c.City == (from x in db.Customers
                           where x.CustomerID == "BSBEV"
                           select x.City).FirstOrDefault()
            select new
            {
                c.CustomerID,
                c.CompanyName,
                c.City
            };
```

### subquery with IN operator

```cs
//SELECT
//    orderid, customerid, shipname
//FROM
//    orders
//WHERE
//    customerid IN (
//        SELECT
//            customerid
//        FROM
//            customers
//        WHERE
//            country = 'USA')
var query = from o in db.Orders
            where (from c in db.Customers
                   where c.Country == "USA"
                   select c.CustomerID)
                .Contains(o.CustomerID)
            select new
            {
                o.OrderID,
                o.CustomerID,
                o.ShipName
            };
```

註：上述範例所產生的 SQL 不太相同，但結果是一樣的。


