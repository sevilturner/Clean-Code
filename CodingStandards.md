# Coding Standards

## Formatting and Style

1.	Lines should be no longer than 120 charactes long to easy readability. Don't do this
```cs
var postedActivityEmployees = postedActivity.ActivityEmployees.Select(item => new EF.ActivityEmployee { ActivityID = activity.ID, ID = item.ID.Equals(Guid.Empty) ? Guid.NewGuid() : item.ID, Position = item.Position, Count = item.Count, AnnualSalary = item.AnnualSalary, TimeSpentPercentage = item.TimeSpentPercentage, EmployeeTypeID = item.EmployeeTypeID });
```
2.	Do not use Regions in class since they usually mean you are trying to hide a large chunck of code which is probably points to the need for refactoring

3.	Remove Using statement that aren’t actually referenced in the file
 
4.	Do not indent object intializers and initialize each property on a new line:

Instead of this:
```cs
IEnumerable<Customer> customers = dbCustomers.Select(customer => new Customer { Name = customer.Name, Address = customer.Address, Number = customer.Number });
```
Do this:
```cs
IEnumerable<Customer> customers = dbCustomers.Select(customer => new Customer
{
    Name = customer.Name,
    Address = customer.Address,
    Number = customer.Number
}); 
```
5.	Omit extra blank lines in a method

6.	When using LINQ, seprate lines by LINQ function for easy readibility

Instead of this:
```cs
var missingArrivalTimes = tripDays.Where(item => item.Date >= currentSurvey.DateStart && item.Date <= currentSurvey.DateEnd).Any(tripDay => tripDay.Trips.OrderBy(trip => trip.Ordinal).First().TripMode.RequiresTime && !tripDay.TimeArrived.HasValue);
```
Do this:
```cs
var missingArrivalTimes = tripDays
	.Where(item => item.Date >= currentSurvey.DateStart && item.Date <= currentSurvey.DateEnd)
	.Any(tripDay => tripDay.Trips
	.OrderBy(trip => trip.Ordinal)
	.First().TripMode.RequiresTime && !tripDay.TimeArrived.HasValue);
```

## 	Naming

1.	Using meaningful names tha reveal the intention of the variable or method

2.	Class, variable, and field names should be nouns. Method names should be verbs or verb/object pairs

3.	Do not use Abbreviations or Acronyms as part of a name. Use names that are easy to pronounce. 
```cs
// Correct
ProductCategory productCategory;
 ```
Avoid
```cs
ProductCategory prodCat;
```
4. Do prefix interfaces with the letter I
```cs
public interface IAddress
{
 
}
```

## 	Maintainability

1.	Functions should be kept small. They should do ONE thing and do it well. The more lines of code in a method the harder it is to understand. Everyone recommends 20-25 lines of code is good.

2. The stepdown rule: as much as possible, order your functions from top to bottom so that they read like a narrative and the code reader can easily follow the flow.

3. Avoid functions with too many parameters. If you end up with a function with more than 3 parameters, use a structure or a class that puts all these parameters together. This is generally a better design and valuable abstraction. Additionally, unit testing a method with many parameters requires many scenarios to test.

```cs
\\too many arguments
function ChangeAddress(string streetAddress, string city, string state, int zipCode)
{
}
```
Create an object/structure that represents the variables
```cs
function ChangeAddress(Address addressToChange)
{
}
```

4.	Don’t use magic numbers or strings – use constants or enumerations instead

5.	Declare and initialize variable as late as possible. Define and initialize each variable at the point where it is needed.

6. Keep Class Size Small. If class tries to do many things, it violates the Single Responsibility Principle. *S in SOLID:*

**"The single responsibility principle states that every object should have a single responsibility, and that responsibility should be entirely encapsulated by the class. All its services should be narrowly aligned with that responsibility.”**

7.	Don’t make explicit comparisons to true or false
```cs
//wrong and bad style 
while(condition == false) 

//also wrong
while(condition != true)
```
Do this
```cs
while(condition)
```

8.	Don’t change a loop variable inside a for or foreach loop. Updating the loop variable within the loop body is generally considered confusing, even more so if the loop variable is modified in more than one place
```cs
for(int index = 0; index < 10; ++index)
{
   if (some condition)
   {
     index = 11; // Use 'break' or 'continue' instead
   }
}
```
9.	Use Lambda expressions instead of delegates.

Don't do this
```cs
Customer c = Array.Find(customers, delegate(Customer c)
{
  return c.Name == "John";
}
```
Do this, much more readable
```cs
var customer = customers.Where(c => c.Name == "John");
```
10. Avoid ref and out parameters. They make code less understandable and might cause people to introduce bugs. Prefer returning compound object instead. *According to Robert C. Martin:*

**"A comment that has gotten old, irrelevant, and incorrect is obsolete.  Comments get old quickly.  It is best not to write a comment that will become obsolete.  If you find an obsolete comment, it is best to update it or get rid of it as quickly as possible.  Obsolete comments tend to migrate away from the code they once described.  They become floating islands of irrelevance and misdirection in the code."**

11. Be caferul with multiple return statements. One entry, one exit is a sound principle and keeps the control flow readable. However, if the method is very small then multiple return statements may actually improve readibility over some central boolean flag that is updated at various points.

12. Avoid Obsolete Comments
