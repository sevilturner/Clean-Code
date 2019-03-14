# Clean Coding Standards  
 Most software defects are introduced when changing existing code. Clean code minimises the risk of introducing defects by making the code as easy to understand as possible. 

## Formatting and Style

1.	Lines should be no longer than 120 charactes long for easy readability.    

Don't do this:
```cs
var postedActivityEmployees = postedActivity.ActivityEmployees.Select(item => new EF.ActivityEmployee { ActivityID = activity.ID, ID = item.ID.Equals(Guid.Empty) ? Guid.NewGuid() : item.ID, Position = item.Position, Count = item.Count, AnnualSalary = item.AnnualSalary, TimeSpentPercentage = item.TimeSpentPercentage, EmployeeTypeID = item.EmployeeTypeID });
```
2.	Avoid using Regions in class. 
Regions usually mean you are trying to hide a large chunck of code which probably points to the need for refactoring. 

3.	Remove Using statement that aren’t actually referenced in the file and organize it by right clicking on namespace -> Remove and Sort Using. 
 
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
5.	Omit extra blank lines in a method. 

6.	When using LINQ, seprate lines by LINQ function for easy readibility. 

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

1.	Use meaningful names that reveal the intention of the variable or method. 

2.	Class, variable, and field names should be nouns. Method names should be verbs or verb/object pairs. 
```cs
public class Employee
{
}
public class BusinessLocation
{
}
public class DocumentCollection
{
}
```

3.	DO NOT use Abbreviations or Acronyms as part of the name. Use names that are easy to pronounce.  

Correct:
```cs
ProductCategory productCategory;
 ```
Avoid:
```cs
ProductCategory prodCat;
```
4. DO prefix interfaces with the letter ```cs I```:
```cs
public interface IAddress
{
 
}
```

## 	Maintainability

1. Single Responsibility Principle (SRP).   
A class should have one, and only one, reason to Keep Class Size Small. A class should have one, and only one, reason to change. If class tries to do many things, it violates the Single Responsibility Principle. *S in SOLID:*

**"The single responsibility principle states that every object should have a single responsibility, and that responsibility should be entirely encapsulated by the class. All its services should be narrowly aligned with that responsibility.”**

2. Open Closed Principle (OCP).     
You should be able to extend a classes behaviour without modifying it.

3.	METHODS SHOULD BE KEPT SMALL.    
Methods should do ONE thing and do it well. The more lines of code in a method the harder it is to understand. Everyone recommends 20-25 lines of code is good. If you need comments to understand the purpose of the method, it is a good place to start refactoring that method. 

4. The stepdown rule: as much as possible.  
Order your method from top to bottom so that they read like a narrative and the code reader can easily follow the flow.

5. Avoid method with too many parameters.    
If you end up with a function with more than 3 parameters, use a structure or a class that puts all these parameters together. This is generally a better design and valuable abstraction. Additionally, unit testing a method with many parameters requires many scenarios to test.

Too many arguments
```cs 
function ChangeAddress(string streetAddress, string city, string state, int zipCode)
{
}
```
Create an object/structure that represents the variables:
```cs
function ChangeAddress(Address addressToChange)
{
}
```

6.	Don’t use magic numbers or strings.   
Use constants or enumerations instead.

7.	Declare and initialize variable as late as possible.   
Define and initialize each variable at the point where it is needed.

8.	Don’t make explicit comparisons to true or false

Wrong and bad style:
```cs 
while(condition == false) 
```
Also wrong:
```cs
while(condition != true)
```
Do this:
```cs
while(condition)
```

9.	Don’t change a loop variable inside a ```cs for``` or ```cs foreach``` loop.    
Updating the loop variable within the loop body is generally considered confusing, even more so if the loop variable is modified in more than one place
```cs
for (int index = 0; index < 10; ++index)
{
   if (some condition)
   {
     index = 11; // Use 'break' or 'continue' instead
   }
}
```

10.	Use Lambda expressions instead of delegates.  

Don't do this:
```cs
Customer c = Array.Find(customers, delegate(Customer c)
{
  return c.Name == "John";
}
```
Do this. This is much more readable:
```cs
var customer = customers.Where(c => c.Name == "John");
```

11. Be caferul with multiple return statements.  
One entry, one exit is a sound principle and keeps the control flow readable. However, if the method is very small then multiple return statements may actually improve readibility over some central boolean flag that is updated at various points.

Avoid this:
```cs
if(product.Price>15)
	{
	   return false;
	}
else if(product.IsDeleted)
	{
	   return false;
	}
else if(!product.IsFeatured)
	{
	   return false;
	}
else if()
	{
	   //.....
	}
return true;
```
DO this:
```cs
var isValid = true;

if(product.Price>15)
	{
	   isValid= false;
	}
else if(product.IsDeleted)
	{
	   isValid= false;
	}
else if(!product.IsFeatured)
	{
	   isValid= false;
	}
return isValid;
```
12. Avoid Obsolete Comments.  
 *According to Robert C. Martin:*

**"A comment that has gotten old, irrelevant, and incorrect is obsolete.  Comments get old quickly.  It is best not to write a comment that will become obsolete.  If you find an obsolete comment, it is best to update it or get rid of it as quickly as possible.  Obsolete comments tend to migrate away from the code they once described.  They become floating islands of irrelevance and misdirection in the code."**

13. If a variable is declared but is not used anywhere, delete it.

14. Needless Repetition.  

Code contains lots of code duplication: exact code duplications or design duplicates (doing the same thing in a different way). Making a change to a duplicated piece of code is more expensive and more error-prone because the change has to be made in several places with the risk that one place is not changed accordingly.

15. Needless Complexity.   

The design contains elements that are currently not useful. The added complexity makes the code harder to comprehend. Therefore, extending and changing the code results in higher effort than necessary.

16. Keep it Simple, Stupid (KISS).  
Reduce complexity as much as possible.

17. Boy Scout Rule.    
Leave the campground cleaner than you found it. If the code is complex, refactor it by simplifying the code.
