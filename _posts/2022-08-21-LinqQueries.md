---
title: LINQ Queries
date: 2022-08-19 11:00:00 -500  
categories: [docs]
tags: [linq, entity framework, ef] # TAG names should always be lowercase
---
# Documentation: LINQ Queries with Entity Framework
## This documentation covers general LINQ database queries. Throughout these examples we are using the Microsoft Northwind Database for demo purposes. The _context is our DbContext implementation, and is used throughout. All queries are shown asynchronously, and results are returned via a return Ok(results) statement. We will be passing in <strong>customerId</strong> and later <strong>employeeId</strong> as search parameters.

## This documentation is organized in a way that shows the T SQL version of a query, and then a few ways of replicating it with LINQ.

---

### <strong>Return all columns and rows from a table:</strong>

#### SQL Example:
```sql
SELECT * FROM dbo.Orders;
```
#### LINQ Versions:
```c#
var result = await _context.Orders.ToListAsync();

// OR

var result = await (
    from orders in _context.Orders
    select orders)
    .ToListAsync();
```
---
### <strong>Return specific columns and all rows from a table:</strong>
#### SQL Example:
```sql
SELECT OrderId, CustomerId, EmployeeId, OrderDate 
FROM dbo.Orders;
```
#### LINQ Versions:
```c#
var result = await _context.Orders
	.Select(o => new 
    {
        o.OrderId, o.CustomerId, o.EmployeeId, o.OrderDate
    })
	.ToListAsync();

// OR

var result = await (
    from orders in _context.Orders
	select new
	{
		OrderId = orders.OrderId,
		CustomerId = orders.CustomerId,
		EmployeeId = orders.EmployeeId,
		OrderDate = orders.OrderDate
	})
    .ToListAsync();
```
---
### <strong>Return all columns from a table based on a customerId:</strong>


#### SQL Example:
```sql
SELECT * FROM dbo.Orders WHERE CustomerId = @CustomerId;
```
#### LINQ Versions:
```c#
var result = await _context.Orders
	.Where(o => o.CustomerId == customerId)
	.ToListAsync();

// OR

var result = await (
	from orders in _context.Orders
	where orders.CustomerId == customerId
	select orders)
	.ToListAsync();
```
---
### <strong>Return the top three records with specific columns from a table based on a customerId. Order by OrderDate descending:</strong>

#### SQL Example:
```sql
SELECT TOP(3) OrderId, CustomerId, EmployeeId, OrderDate
FROM Orders
WHERE CustomerId = @CustomerId
ORDER BY OrderDate DESC;
```
#### LINQ Versions:
```c#
var result = await _context.Orders
	.Where(o => o.CustomerId == customerId)
	.OrderByDescending(o => o.OrderDate)
	.Select(o => new
	{
		o.OrderId, o.CustomerId, o.EmployeeId, o.OrderDate
	})
	.Take(3)
	.ToListAsync();

// OR

var result = await (
	from orders in _context.Orders
	where orders.CustomerId == customerId
	orderby orders.OrderDate descending
	select new
    {
	    OrderId = orders.OrderId,
		CustomerId = orders.CustomerId,
		EmployeeId = orders.EmployeeId,
		OrderDate = orders.OrderDate
	})
	.Take(3)
	.ToListAsync();
```
---
### <strong>Return specific columns for records matching two parameters:</strong>


#### SQL Example:
```sql
SELECT OrderId, OrderDate 
FROM Orders
WHERE CustomerId = @CustomerId 
    AND EmployeeId = @EmployeeId;

```
#### LINQ Versions:
```c#
var result = await _context.Orders
	.Where(o => o.customerId == customerId
		&& o.EmployeeId == employeeId)
	.Select(o => new
	{
		o.OrderId, o.OrderDate
	})
	.ToListAsync();

// OR

var result = await (
	from orders in _context.Orders
	where orders.CustomerId == customerId
	&& orders.EmployeeId == employeeId
	select new
	{
		orders.OrderId, orders.OrderDate
	})
	.ToListAsync();
```
---
### <strong>Return specific columns from two tables that match a parameter (inner join), and order by a column descending:</strong>

#### SQL Example:
```sql
SELECT o.OrderId,
       o.CustomerId,
       o.EmployeeId,
       o.OrderDate,
       c.CompanyName,
       c.Address,
       c.Phone
FROM Orders o
JOIN Customers c
ON o.CustomerId = c.CustomerId
WHERE o.CustomerId = @CustomerId
ORDER BY o.OrderDate DESC;

```
#### LINQ Versions:
```c#
var results = 
	await _context.Orders
	.Where(o => o.CustomerId == customerId)
	.Join(
		_context.Customers,
		order => customer.CustomerId,
		(order, customer) => new
		{
			order.OrderId,
            order.CustomerId,
            order.OrderDate,
            customer.CompanyName,
            customer.Address,
            customer.Phone
		})
	.OrderByDescending(o => o.OrderDate)
	.ToListAsync();

// OR

var results = await (
    from order in _context.Orders
	join customer in _context.Customers
    on order.CustomerId equals customer.CustomerId
	where order.CustomerId == customerId
	orderby order.OrderDate descending
	select new CustomerAndOrderInformation()
	{
		OrderId = orders.OrderId,
		CustomerId = orders.CustomerId,
		OrderDate = orders.OrderDate,
		CompanyName = customer.CompanyName,
		Address = customer.Address,
		Phone = customer.Phone
	}).ToListAsync();
```
#### \*\*Notice the second example returns a specific class we might create to capture this data. Otherwise it will be an anonymous object.\*\*
---
### <strong>Return specific columns from three tables that match a parameter (inner join), and order by OrderDate descending:</strong>

#### SQL Example:
```sql
SELECT o.OrderId,
	   o.CustomerId,
	   o.OrderDate,
	   c.CompanyName,
	   c.Address,
	   c.Phone,
	   od.ProductID,
	   (od.UnitPrice * od.Quantity) as OrderTotal
FROM dbo.Orders o
JOIN dbo.Customers c
ON o.CustomerId = c.CustomerId
JOIN dbo.[Order Details] od
ON o.OrderID = od.OrderID
WHERE o.CustomerId = @CustomerId
ORDER BY o.OrderDate DESC;
```
#### LINQ Versions:
```c#
var results = await (
    from order in _context.Orders
    join customer in _context.Customers
    on order.CustomerId equals customer.CustomerId
    join orderDetail in _context.OrderDetails
    on order.OrderId equals orderDetail.OrderId
    where order.CustomerId == customerId
    orderby order.OrderDate descending
    select new
    {
        OrderId = order.OrderId,
        CustomerId = order.CustomerId,
        OrderDate = order.OrderDate,
        CompanyName = customer.CompanyName,
        Address = customer.Address,
        Phone = customer.Phone,
        ProductId = orderDetail.ProductId,
        OrderTotal = (orderDetail.UnitPrice * 
	        orderDetail.Quantity)
    }).ToListAsync();

// OR

var results = await _context.Customers
        .Where(c => c.CustomerId == customerId)
        .Join(
            _context.Orders,
            c => c.CustomerId,
            o => o.CustomerId,
            (c, o) => new { c, o })
        .Join(
            _context.OrderDetails,
            co => co.o.OrderId,
            od => od.OrderId,
            (co, od) => new { co, od })
        .Select(result => new
        {
            result.co.o.OrderId,
            result.co.c.CustomerId,
            result.co.o.OrderDate,
            result.co.c.CompanyName,
            result.co.c.Address,
            result.co.c.Phone,
            result.od.ProductId,
            orderTotal = (result.od.Quantity * 
			result.od.UnitPrice)
        })
        .OrderByDescending(o => o.OrderDate)
        .ToListAsync();

// OR

var results = await _context.Customers
        .Where(c => c.CustomerId == customerId)
        .Join(
            _context.Orders,
            c => c.CustomerId,
            o => o.CustomerId,
            (c, o) => new { c, o })
        .Join(
            _context.OrderDetails,
            co => co.o.OrderId,
            od => od.OrderId,
            (co, od) => new { co, od })
        .Select(result => new CustomerOrderOrderDetails()
        {
            OrderId = result.co.o.OrderId,
            CustomerId = result.co.c.CustomerId,
            OrderDate = result.co.o.OrderDate,
            CompanyName = result.co.c.CompanyName,
            Address = result.co.c.Address,
            Phone = result.co.c.Phone,
            ProductId = result.od.ProductId,
            OrderTotal = (result.od.Quantity * 
			result.od.UnitPrice)
        })
        .OrderByDescending(o => o.OrderDate)
        .ToListAsync();
```
#### \*\*Again here we show the option of returning an anonymous object or a pre-determined class object.\*\*
---

<div class="text-center">
    <h3>--THIS DOCUMENTATION IS NOT COMPLETE--</h3>
</div>
---

