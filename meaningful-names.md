# Meaningful Names <!-- omit in toc -->

![](/Assets/Images/my-name-is-meaningful.png)

Simple rules for creating good names for variables, classes, namespaces, packages, source files, directories and executables.

- [1. Use Intention Revealing Names](#1-use-intention-revealing-names)
- [2. Make Context Explicit](#2-make-context-explicit)
- [3. Avoid Disinformative Names](#3-avoid-disinformative-names)
- [4. Avoid Noninformative Names](#4-avoid-noninformative-names)
- [5. Use Pronounceable Names](#5-use-pronounceable-names)
- [6. Use Searchable Names](#6-use-searchable-names)
- [7. Avoid Adding Type/Scope Information Into Names](#7-avoid-adding-typescope-information-into-names)
- [8. Avoid Mental Mapping](#8-avoid-mental-mapping)
- [9. Class Names](#9-class-names)
- [10. Method Names](#10-method-names)
- [11. Use Static Factory Methods](#11-use-static-factory-methods)
- [12. Choose Clarity Over Entertainment Value](#12-choose-clarity-over-entertainment-value)
- [13. Use Consistent Vocabulary](#13-use-consistent-vocabulary)
- [14. Use Solution Domain Names](#14-use-solution-domain-names)
- [15. Use Problem Domain Names](#15-use-problem-domain-names)
- [16. Add Meaningful Context](#16-add-meaningful-context)
- [17. Don't Add Unneeded Context](#17-dont-add-unneeded-context)

## 1. Use Intention Revealing Names

The name of a variable, function, or class, should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.

**Bad:**

```csharp
int d; // Elapsed time in days.
```

```csharp
var dataFromDatabase = _customerRepository.GetAll();
```

**Good:**

```csharp
int elapsedTimeInDays;
```

```csharp
var customers = _customerRepository.GetAll();
```

## 2. Make Context Explicit

Below is the code for getting flagged cells in mine sweeper game.

**Bad**

```csharp
public List<int[]> GetThem()
{
    List<int[]> list1 = new List<int[]>();

    foreach (int[] x in TheList)
    {
        if (x[0] == 4)
        {
            list1.Add(x);
        }
    }

    return list1;
}
```

Above code assumes that the reader of this code knows -

1. What kinds of things are in TheList?
2. What is the significance of the zeroth subscript of an item in TheList?
3. What is the significance of the value 4?
4. How to use the list being returned?

**Good**

```csharp
public enum CellStatus
{
	Flagged
}

const int StatusValue = 0;

public List<int[]> GetFlaggedCells()
{
   List<int[]> flaggedCells = new List<int[]>();

   foreach (int[] cell in GameBoard)
   {
       if (cell[StatusValue] == CellStatus.Flagged)
       {
           flaggedCells.Add(cell);
       }
   }

   return flaggedCells;
}
```

**Better**

Instead of using `int[]` for representing a cell, define a class `Cell` with property `IsFlagged` to encapsulate functionality of a cell.

```csharp
public List<Cell> GetFlaggedCells()
{
   List<Cell> flaggedCells = new List<Cell>();

   foreach (Cell cell in GameBoard)
   {
       if (cell.IsFlagged)
       {
           flaggedCells.Add(cell);
       }
   }

   return flaggedCells;
}
```

## 3. Avoid Disinformative Names

1. Avoid words whose entrenched meanings vary from our intended meaning. For example, hp, aix, and sco would be poor variable names because they are the names of Unix platforms or variants.
2. Beware of using names which vary in small ways. For example, `XYZControllerForEfficientHandlingOfStrings` and `XYZControllerForEfficientStorageOfStrings`.

## 4. Avoid Noninformative Names

1. Do not distinguish names by misspelling one name. For example, using `Klass` because `Class` was used for something else. Correcting spelling error in this case will lead to compile time error.
2. Do not distinguish names by adding number series.

   **Bad**

   ```csharp
   public static void CopyCharacters(char[] a1, char[] a2)
   {
   	for (int i = 0; i < a1.Length; i++)
   	{
   		a2[i] = a1[i];
   	}
   }
   ```

   **Good**

   ```csharp
   public static void CopyCharacters(char[] source, char[] destination)
   {
   	for (int i = 0; i < source.Length; i++)
   	{
   		destination[i] = source[i];
   	}
   }
   ```

3. Do not distinguish names by adding noise words such as Info, Data etc. For example, `Customer` vs `CustomerInfo`.

## 5. Use Pronounceable Names

**Bad:**

```csharp
var yyyymmddstr = DateTime.Now.ToString("yyyy/MM/dd");
```

**Good:**

```csharp
var currentDate = DateTime.Now.ToString("yyyy/MM/dd");
```

## 6. Use Searchable Names

If a variable or constant might be used in multiple places in a body of code, it is important to give it a search-friendly name.

1. Avoid using single letter names unless it's a local variable inside a short method.

   **Bad**

   ```csharp
   var e = new Employee
   {
       FirstName = "John",
       LastName = "Doe"
   };

   var output = JsonConvert.SerializeObject(e);
   ```

   **Good**

   ```csharp
   var employee = new Employee
   {
       FirstName = "John",
       LastName = "Doe"
   };

   var employeeJson = JsonConvert.SerializeObject(employee);
   ```

2. Avoid using numeric constants.

   **Bad**

   ```csharp
   public decimal CalculateGrossPay(Employee employee)
   {
      decimal grossPay;

      if (employee.HoursWorked > 40) // What is 40?
      {
          var basePay = employee.HourlyPayRate * 40; // What is 40?

          var overtimeHours = employee.HoursWorked - 40; // What is 40?

          var overtimePay = overtimeHours * employee.HourlyPayRate * 1.5m; // What is 1.5m?

          grossPay = basePay + overtimePay;
      }
      else
      {
          grossPay = employee.HoursWorked * employee.HourlyPayRate;
      }

      return grossPay;
   }
   ```

   **Good**

   ```csharp
   const decimal BaseHours = 40;
   const decimal OvertimeRate = 1.5m;

   public decimal CalculateGrossPay(Employee employee)
   {
      decimal grossPay;

      if (employee.HoursWorked > BaseHours)
      {
          var basePay = employee.HourlyPayRate * BaseHours;

          var overtimeHours = employee.HoursWorked - BaseHours;

          var overtimePay = overtimeHours * employee.HourlyPayRate * OvertimeRate;

          grossPay = basePay + overtimePay;
      }
      else
      {
          grossPay = employee.HoursWorked * employee.HourlyPayRate;
      }

      return grossPay;
   }
   ```

## 7. Avoid Adding Type/Scope Information Into Names

Avoid adding type or scope information into names. It makes names harder to read and it could mislead the reader when type or scope is changed but the author forgot to change the name accordingly.

1. Don't use Hungarian notation.

   **Bad**

   ```csharp
   string sFirstName;
   int iHoursWorked;
   DateTime dtUpdatedDate;
   ```

   **Good**

   ```csharp
   string firstName;
   int hoursWorked;
   DateTime updatedDate;
   ```

2. Don't use member prefixes.

   **Bad**

   ```csharp
    public class Employee
    {
        private string m_firstName;

        public string FirstName
        {
            get
            {
                return m_firstName;
            }

            set
            {
                m_firstName = value;
            }
        }
    }
   ```

   **Good**

   ```csharp
   public class Employee
   {
       public string FirstName { get; set; }
   }
   ```

## 8. Avoid Mental Mapping

Readers shouldn’t have to mentally translate your names into other names they already know.

**Bad**

```csharp
var result = _employeeRepository.GetAll();

foreach (var e in result)
{
    CalculateSalary(e);
	// ...
	// ...
	SendPaycheck(e); // Reader needs to mentally map e to Employee object.
	// ...
	// ...
	Log(e);
}
```

**Good**

```csharp
var employees = _employeeRepository.GetAll();

foreach (var employee in employees)
{
    CalculateSalary(employee);
	// ...
	// ...
	SendPaycheck(employee);
	// ...
	// ...
	Log(employee);
}
```

## 9. Class Names

A class name should be a noun or noun phrase like `Customer`, `WikiPage`, `Account` and `AddressParser`. A class name should not be a verb.

## 10. Method Names

A method name should be a verb or verb phrase like `PostPayment`, `DeletePage` and `Save`.

## 11. Use Static Factory Methods

When constructors are overloaded, use static factory methods with names that describe the arguments.

**Bad**

```csharp
var fiveSeconds = new TimeSpan(0, 0, 0, 5);
var fiveMinutes = new TimeSpan(0, 0, 5, 0);
var fiveHours = new TimeSpan(0, 5, 0, 0);
var fiveDays = new TimeSpan(5, 0, 0, 0);
```

**Good**

```csharp
var fiveSeconds = TimeSpan.FromSeconds(5);
var fiveMinutes = TimeSpan.FromMinutes(5);
var fiveHours = TimeSpan.FromHours(5);
var fiveDays = TimeSpan.FromDays(5);
```

## 12. Choose Clarity Over Entertainment Value

Don't use colloquialisms or slang.

**Bad**

```csharp
public class Process
{
	public void Whack()
	{
	}
}
```

**Good**

```csharp
public class Process
{
	public void Kill()
	{
	}
}
```

## 13. Use Consistent Vocabulary

1. Avoid using different terms for same concept. Pick one term for one abstract concept and stick with it. For instance, it’s confusing to have `Fetch`, `Retrieve`, and `Get` as equivalent methods of different classes.

   **Bad**

   ```csharp
   // Same concept of getting something has been expressed using different terms in different classes.

   public class CustomerRepository
   {
       public Customer Fetch()
       {
       }
   }

   public class EmployeeRepository
   {
       public Employee Retrieve()
       {
       }
   }

   public class UserRepository
   {
       public User Get()
       {
       }
   }
   ```

   **Good**

   ```csharp
   public class CustomerRepository
   {
       public Customer Get()
       {
       }
   }

   public class EmployeeRepository
   {
       public Employee Get()
       {
       }
   }

   public class UserRepository
   {
       public User Get()
       {
       }
   }
   ```

2. Avoid using same term for two different concepts.

   **Bad**

   ```csharp
   // Different concepts of concatenating, appending and adding have been expressed using single term in different classes.

   public class SomeClass
   {
       public string Add(string leftSideValue, string rightSideValue)
       {
           return leftSideValue + rightSideValue;
       }
   }

   public class SomeOtherClass
   {
       private string _existingValue = "";

       public string Add(string value)
       {
           return _existingValue + value;
       }
   }

   public class YetAnotherClass
   {
       public List<string> Values = new List<string>();

       public void Add(string value)
       {
           Values.Add(value);
       }
   }
   ```

   **Good**

   ```csharp
   public class SomeClass
   {
       public string Concat(string leftSideValue, string rightSideValue)
       {
           return leftSideValue + rightSideValue;
       }
   }

   public class SomeOtherClass
   {
       private string _existingValue = "";

       public string Append(string value)
       {
           return _existingValue + value;
       }
   }

   public class YetAnotherClass
   {
       public List<string> Values = new List<string>();

       public void Add(string value)
       {
           Values.Add(value);
       }
   }
   ```

## 14. Use Solution Domain Names

Since other programmers are going to read the code use computer science terms, like algorithm names, pattern names, and math terms etc. Every name doesn't have to come from the problem domain because we don’t want programmers to have to run back and forth to the customer asking what every name means when they already know the concept by a different name.

**Bad**

```csharp
public class Jobs // But it's essentially a job queue.
{
}
```

**Good**

```csharp
public class JobQueue
{
}
```

## 15. Use Problem Domain Names

The code that has more to do with problem domain concepts should have names drawn from the problem domain. In Healthcare domain it makes more sense to use `MedicalRecordNumber` as oppose to just `RecordId` or `Id`.

**Bad**

```csharp
public class PatientHistory
{
	public string RecordId { get; set; }
}
```

**Good**

```csharp
public class PatientHistory
{
	public string MedicalRecordNumber { get; set; }
}
```

## 16. Add Meaningful Context

Add meaningful context to names by enclosing them in well-named classes, functions, or namespaces. Prefix a name as a last resort.

**Bad**

```csharp
string firstName;
string lastName;
string line1;
string line2;
string city;
string state;
string zipCode;
```

**Not Ideal**

```csharp
string addressFirstName;
string addressLastName;
string addressLine1;
string addressLine2;
string addressCity;
string addressState;
string addressZipCode;
```

**Good**

```csharp
public class Address
{
	public string FirstName { get; set; }
	public string LastName { get; set; }
	public string Line1 { get; set; }
	public string Line2 { get; set; }
	public string City { get; set; }
	public string State { get; set; }
	public string ZipCode { get; set; }
}
```

## 17. Don't Add Unneeded Context

**Bad**

```csharp
public class Employee
{
	public int EmployeeId { get; set; }
	public string EmployeeFirstName { get; set; }
	public string EmployeeLastName { get; set; }
	public DateTime EmployeeStartDate { get; set; }
}
```

**Good**

```csharp
public class Employee
{
	public int Id { get; set; }
	public string FirstName { get; set; }
	public string LastName { get; set; }
	public DateTime StartDate { get; set; }
}
```
