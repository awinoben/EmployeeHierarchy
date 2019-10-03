# Employees Hierarchy

## Task Description
Develop a .NET library assembly (DLL) for a system that handles the employee hierarchy, implemented as class Employees. This class has a constructor that takes a CSV string that contains all the employees from the company, the manager they report to, and the employee’s salary. Develop the library assembly that contains the code for the questions below. Make sure to provide unit tests for your code and an efficient solution.

a. Create a constructor that takes a CSV1 input string containing a list of employee info and validates the string. The first CSV column contains the id of the employee, the second one contains the id of the manager, and the third one contains the employee’s salary. The CEO of the company is the only employee that doesn't have a manager; in his case, the manager field will be empty. The list is not guaranteed to be sorted and can come in any random order. See the example below.

The constructor should validate that:

    1. The salaries in the CSV are valid integer numbers.
    2. One employee does not report to more than one manager.
    3. There is only one CEO, i.e. only one employee with no manager.
    4. There is no circular reference, i.e. a first employee reporting to a second employee that is also under the first employee.
    5. There is no manager that is not an employee, i.e. all managers are also listed in the employee     column.


b. Add an instance method that returns the salary budget from the specified manager. The salary budget from a manager is defined as the sum of the salaries of all the employees reporting (directly or indirectly) to a specified manager, plus the salary of the manager. See the example below.
Input type: String
Return type: long



## Solution

In this solution, the employee hierarchy is modelled as a directed acyclic graph **DAG**.

The graph data structure seemed to me like a natural fit for the problem domain in addition 
to providing powerful algorithms for traversal. 

Employees are modelled as **Vertices** and the relationship between them as **Edges**.

To ensure that an employee does not report to more than one manager, I limited the number
of incoming edges to any vertext to a `maximum of 1` and a `minimum of 0` in the case of the
**CEO** who has no supervisor.

Each employee is added to a **dictionary** data structure which ensures that there are no
duplicate values.

I validate the `salary` of each employee to ensure it is a valid integer.

I construct the graph from the dictionary thus guaranteeing that each manager is first listed as an employee.

The acyclic nature of the **DAG** ensures that there is no double linking between employees.
To ensure this, I do a Depth First Traversal **DFS** of the graph begining at the source
**Vertex** of an **Edge**, if the manager exists among the child nodes, then adding the edge 
would cause a cycle, hence I don't add it.

To get the `Salary Budget` for any manager, I do a **DFS** begining at the manager **Vertext** and 
`sum` all the salaries in the generated `path`.


### Input
The list of employees is supplied as a text file. 

The sample employee hierarchies are found in the `UnitTests/` folder.

### Output
The program makes output in the following format. This sample output is taken from running it on
the `test.txt` input file with `Employee1` as the manager of interest.

It outputs `3300` the total salary budget of the **CEO** who is `Employee1` in this case.