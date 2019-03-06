# Coding Standards

## Formatting and Style

1.	Lines should be no longer than 120 charactes long to easy readability. Don't do this
```cs
var postedActivityEmployees = postedActivity.ActivityEmployees.Select(item => new EF.ActivityEmployee { ActivityID = activity.ID, ID = item.ID.Equals(Guid.Empty) ? Guid.NewGuid() : item.ID, Position = item.Position, Count = item.Count, AnnualSalary = item.AnnualSalary, TimeSpentPercentage = item.TimeSpentPercentage, EmployeeTypeID = item.EmployeeTypeID });
```
2.	Do not use regions since they usually mean you are trying to hide a large chunck of code which is probably points to the need for refactoring

3.	Remove Using statement that aren’t actually referenced in the file
![alt text](http://url/to/img.png)
 
3.	Do not indent object intializers and initialize each property on a new line:

Instead of this:
```cs
IEnumerable<Customer> customers = dbCustomers.Select(customer => new Customer { Name = customer.Name, Address = customer.Address, Number = customer.Number });

\\Do this:
IEnumerable<Customer> customers = dbCustomers.Select(customer => new Customer
{
    Name = customer.Name,
    Address = customer.Address,
    Number = customer.Number
}); 
```
4.	Omit extra blank lines

5.	When using LINQ, seprate lines by LINQ function for easy readibility

Instead of this:
```cs
var missingArrivalTimes = tripDays.Where(item => item.Date >= currentSurvey.DateStart && item.Date <= currentSurvey.DateEnd).Any(tripDay => tripDay.Trips.OrderBy(trip => trip.Ordinal).First().TripMode.RequiresTime && !tripDay.TimeArrived.HasValue);

\\Do this:

var missingArrivalTimes = tripDays
	.Where(item => item.Date >= currentSurvey.DateStart && item.Date <= currentSurvey.DateEnd)
	.Any(tripDay => tripDay.Trips
	.OrderBy(trip => trip.Ordinal)
	.First().TripMode.RequiresTime && !tripDay.TimeArrived.HasValue);
```

## 	Naming

1	Using meaningful names tha reveal the intention of the variable or method

2.	Class, variable, and field names should be nouns. Method names should be verbs or verb/object pairs

3.	Do not use abbreviations or acronyms as part of a name. Use names that are easy to pronounce. 

## 	Maintainability

1.	Functions should be kept small. They should do ONE thing and do it well

2. The stepdown rule: as much as possible, order your functions from top to bottom so that they read like a narrative and the code reader can easily follow the flow.

3. Functions should, as a rule, have no more than 3 parameters. If you end up with a function with more than 3 parameters, use a structure or a class for passing multiple arguments. In general, the fewer the number of parameters, the easier it is to understand a method. Additionally, unit testing a method with many parameters requires many scenarios to test
```cs
\\too many arguments
function ChangeAddress(string streetAddress, string city, string state, int zipCode)
{
}

\\Create an object/structure that represents the variables
function ChangeAddress(Address addressToChange)
{
}
```
4.	Don’t use magic numbers or strings – use constants or enumerations instead

5.	Declare and initialize variable as late as possible. Define and initialize each variable at the point where it is needed.

6.	Don’t make explicit comparisons to true or false
```cs
//wrong and bad style 
while(condition == false) 

//also wrong
while(condition != true)

//Do this
while(condition)
```

7.	Don’t change a loop variable inside a for or foreach loop. Updating the loop variable within the loop body is generally considered confusing, even more so if the loop variable is modified in more than one place
```cs
for(int index = 0; index < 10; ++index)
{
   if (some condition)
   {
     index = 11; // Use 'break' or 'continue' instead
   }
}
```
8.	Use Lambda expressions instead of delegates.
```cs
\\Don't do this
Customer c = Array.Find(customers, delegate(Customer c)
{
  return c.Name == "John";
}

\\Do this, much more readable
var customer = customers.Where(c => c.Name == "John");
```
9. Avoid ref and out parameters. They make code less understandable and might cause people to introduce bugs. Prefer returning compound object instead.

10. Be caferul with multiple return statements. One entry, one exit is a sound principle and keeps the control flow readable. However, if the method is very small then multiple return statements may actually improve readibility over some central boolean flag that is updated at various points.

11. 

