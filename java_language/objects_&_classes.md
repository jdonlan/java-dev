#Custom Classes
Complex data sets that aren't easily managed with the default data collections should be handled by using custom classes. To determine if a custom class is an appropriate answer, consider the following:

* Does the data have multiple types?
* Is there a need to access the data multiple times?
* Is advanced functionality/filtering needed for the data?

If one or more of these questions can be answered in the affirmative, it is likely a good opportunity to utilize a custom class.

###Example
An app is created that manages an employee roster. Each employee would be associated with several data points including a name, department, employee number, years of service, etc. Instead of having these all stored in code as individual elements, we can create a single Employee class that contains all of these points of data.

```
// Employee.java
public class Employee {
	// Member variables
	private int mYearsOfService;
	private String mName;
	private String mDepartment;
	
	// Class constructor
	public Employee() {
		mYearsOfService = 0;
		mName = mDepartment = "";
	}
	
	public Employee(String _name, String _department, int _service) {
		mYearsOfService = _service;
		mName = _name;
		mDepartment = _department;
	}
	
	// Getter and setter methods.
	public int getYearsOfService() {
		return mYearsOfService;
	}
	
	public void setYearsOfService(int _service) {
		mYearsOfService = _service;
	}
	
	public String getName() {
		return mName;
	}
	
	public void setName(String _name) {
		mName = _name;
	}
	
	public String getDepartment() {
		return mDepartment;
	}
	
	public void setDepartment(String _department) {
		mDepartment = _department;
	}
}
```

Since the Employee class holds all of our employee data, we can now pass around a single Employee object instead of passing around multiple variables when dealing with employee data. This makes our code more readable and adheres to the object-oriented principles of the Java language. If a set of data is related and has common functions that it can perform, make it into a class. The data becomes the member variables and the functions become the class methods.

NOTE: *When creating a new class, you would create a new .java file. The .java file must be named exactly the same as the name of the class or the class won't compile. As per the Java naming conventions, all class names should begin with a capital letter and should be named to be descriptive of the class contents.*

