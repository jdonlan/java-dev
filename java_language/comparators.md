#Comparators
Alongside custom classes come custom comparators.  A comparator is simply an operation that compares two or more values, often with the purpose of sorting. 

Using the Employee class example, we may want to compare two employees by their Employee ID.  As Employee is a custom class, Java has no default way to compare two instances of an employee object in such a way.  To accomplish this, we would need to write a custom comparator as shown below:


```
// Assume Employee class with integer ID.
// Declare new Employee Comparator
Comparator<Employee> comp = new Comparator<Employee>() {

	@Override
	public int compare(Employee lhs, Employee rhs) {
		if(lhs.ID < rhs.ID) { // lhs is less than rhs
			return -1;
		} else if(lhs.ID > rhs.ID) { // rhs is less than lhs
			return 1;
		} else { // lhs and rhs are equals
			return 0;
		}
	}
};
```

Comparing two employees by their ID becomes an easy feat with the comparator in place.  As you can see, the comparators return a fixed integer of -1, 1, or 0 which is expected by the Java run-time to define order.

```
// New list of employees.
ArrayList<Employee> employees = 
	new ArrayList<Employee>();

// Adding employees.
employees.add(new Employee(5));
employees.add(new Employee(3));
employees.add(new Employee(10));

// Performing sort with our comparator
Collections.sort(employees, comp);

// employees = { Employee(3), Employee(5), Employee(10) }
```

Java will utilize the List classes included sort functionality using the comparator we created above.  Despite having created the List in the order 5,3,10, the list will now be arranged in the order 3,5,10.