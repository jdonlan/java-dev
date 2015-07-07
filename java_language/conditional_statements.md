#Conditional Statements
It's almost impossible to write code that can be executed, line for line, in the exact order it's written and still create something useful. Often times, you'll need to make your code branch based on certain conditions. In the case of a weather app, for example, you wouldn't always want to show a picture of the sun to describe the weather. If it was cloudy outside, you might want to show some clouds. If it's raining, you might want to show a cloud with rain coming out of it. To allow your code to react to these varying conditions and branch accordingly, we use conditional statements (also known as control flow).

##If Statements
The most common form of conditional statements are known as "if" statements. If statements are used to determine which branch your code should take based on boolean conditions. If statements can be used to check a single condition using only the "if" part of an if statement, but it's also possible to have secondary and fallback conditions using "else if" and "else" statements as seen below.

```
// Get a random integer between 0 and 10
int i = new Random().nextInt(10);
if(i < 5) {
	// Execute this code if "i" is less than five.
} else if(i > 5) {
	// Execute this code if "i" is greater than five.
} else {
	// Execute this code if "i" isn't less than or greater than five.
	// In other words, execute this code if "i" equals five.
}
```

As you can see in the above code, the "if" and "else if" statements require some form of boolean value or condition to be specified inside the parentheses. However, the "else" part of the statement requires no condition at all since it'll execute so long as the above "if/else if" conditions aren't being met. 

It's important to note that this type of conditional must always start with an "if" condition followed by any number of optional "else if" conditions, and an optional "else" statement. It's also important to note that as soon as one of the conditions are met, none of the remaining "else if" or "else" statements will be checked. For more about the boolean conditions used for if statements, read the section of the course textbook listed in the resources section of this lesson.