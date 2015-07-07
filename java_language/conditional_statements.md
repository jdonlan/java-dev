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

It's important to note that this type of conditional must always start with an "if" condition followed by any number of optional "else if" conditions, and an optional "else" statement. It's also important to note that as soon as one of the conditions are met, none of the remaining "else if" or "else" statements will be checked.

##Switch Statements
If statements are very useful for evaluating boolean, true/false, conditions. However, what if you want to branch your code based on the value of a variable? To do this, we can use a switch statement. Switch statements allow you to branch code in any number of ways based on the value of a variable. However, due to the method used to check variable equality, switch statements only work properly with byte, short, char, int, and String types. If you need to branch based on value for other types, you'll have to create a long series of if/else statements.

To declare a switch statement, you start with the switch keyword followed by the variable you wish to branch on in parentheses, followed by a block of code. Inside the block, you would declare any number of values you want to check against by declaring them with the case keyword, the value you're expecting, and a colon. Switch statements will check each of these case statements in the order they're declared looking for one that matches the value of the variable that was declared at the top of the switch. If no values match, then nothing happens. Like with if statements though, you can declare a default case if none of the other values are a match. You can do this using the default keyword as shown below.

```
// Get a random integer between 0 and 10.
int value = new Random().nextInt(10);
switch(value) {
	case 0: // value = 0
	case 1: // value = 1
	case 2: // value = 2
	case 3: // value = 3
	case 4: // value = 4
		break;
	case 5: // value = 5
		break;
	case 6: // value = 6
	case 7: // value = 7
	case 8: // value = 8
	case 9: // value = 9
	case 10: // value = 10
		break;
	default: // Not needed but shown for demonstration
		break;
}
```

You may notice that after some cases, there is a break statement. In an if statement, if a condition is met then only the code for that condition gets executed. However, with a switch statement, as soon as the correct case is found then your code starts executing until it finds a break statement. So if we look at the above switch statement, if "value" is equal to 0, 1, 2, 3, or 4, then code execution will continue until the break at case 4 is hit.

With our case statements grouped up like this, our switch statement example works exactly the same way as our if statement example above. However, the if statement example is much shorter and much more efficient than our switch since it involves fewer checks. When writing conditional code, be sure that you're using the right conditional for the job. If boolean conditions can be checked, use an if statement. If you need to check individual values, use a switch.

####References
[http://docs.oracle.com/javase/tutorial/java/nutsandbolts/if.html](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/if.html)
[http://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html)
