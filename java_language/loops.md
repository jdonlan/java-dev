#Loops
Loops are a cornerstone of any programming language. They allow developers to iterate through collections of data, repeat code a set amount of times, or even allow an application to wait for a condition to be met before continuing on. This section will take a look at loops in Java for use in Android development.

There are many circumstances in which a developer will need to run through a block code in sequence - usually for a number of times that is not predefined or known prior to compiling the application including:

* Working with dynamic Array data
* Loading content
* Animating objects
* and more...

Loops allow developers to define a block of code, establish a set of rules for how many iterations the code should run, and exit the code execution upon an established criteria being met. In Java, there are four primary looping methods which, for the most part, can be used interchangeably, depending on the needs of the application and developer preference.

##For Loop
The For Loop is the go-to for many developers as it is rather ubiquitous across languages. In Java, the For Loop functions very similarly to JavaScript and many other C style languages. It consists of 3 primary components:

* Declaration - Assigning values of loop variables.
* Condition - Condition on which to cease execution.
* Operation - Adjustments to loop variables each cycle.

Using a For Loop is relatively straight forward. The primary steps are as follows:

1. Define the range of the loop - start & finish.
2. Outline the condition, if any, in which the loop should cease running.
3. Define the the iteration pattern.

For example, if a developer needs to iterate through each value of a given array myArray, the range would be defined as the first index to the last. As Java Arrays are zero-based arrays, this range would be represented by in code by 0 through the length of the array minus 1. The condition in which to cease running the loop, would be when the loop reached an index that was out of bounds. As each value in the array needs to be evaluated, the iteration is a single increment.

```
for(int i=0; i<myArray.length; i++){ 
    //CODE HERE
}
```

In the above block of code, an integer i is declared to serve as the iterating value (int i=0). It is given an initial value of 0 as that is the desired starting point from above. 

The second segment of the loop (i reaches the length of the array. Assuming the array has 5 values, the indexes will range from 0 to 4. As such, the length of 5 will be the first value out of range, signaling that the loop should cease running. 

The final segment of the loop (i++) indicates that the value of i should be incremented by 1 every time the loop runs. In effect, the loop will run 5 times, with i starting at 0 and continuing to 1, then 2, then 3, and then 4, running the code in the loop each time. At that point, the loop will begin to run a 6th time, where i will increment to 5 and trigger the cease condition defined. As such, the loop will exit, and the encapsulated code will not be executed.

##For Each Loop
Another staple of programming loops is the For Each loop. The premise behind a For Each loop is similar to a standard For loop, but provides less control over the iteration of content. In return, it provides a simple approach to cycling through data collections without needing to define a cease condition or defined iterative operation. As such, the For Each loop is typically reserved for cycling through data collection content exclusively. The For Each loop simply needs to define the collection and a storage object for each distinct value in the collection. To accomplish the same end as the For Loop defined above, the For Each equivalent would appear as follows:

```
for(myValue : myArray){
    //CODE
}
```

In the above code example, myValue represents an individual object from myArray. As the loop runs, the code will pull a new value from myArray to and store it in myValue. Like the example For Loop, the For Each will execute 5 times, once for each value in the array.

##While Loop
Unlike the For or For Each loops, the While loop is utilized for running a block of code based solely on a condition. While loops are often the best choice when looking for specific values in a data collection or waiting for a specific condition to be met within an application before continuing code execution. For example, if you were building a Blackjack application, and you wanted the dealer AI to hit under 16, a simple While loop could achieve this goal:

```
while (dealerTotal < 16){
    Card newCard = deck.deal();
    dealerTotal += newCard.value;
}
```

In the above example, the application first evaluates the dealerTotal and compares it to provided value of 16. If the total is less than 16, the encapsulated code executes, getting a new card from the deck.deal() method, adds the value of that card to the dealerTotal and returns to the beginning of the loop. The loop will then re-evaluate the dealerTotal and compare it to the requisite value of 16. If it still falls short, the loop will execute again, if it is at or above 16, however, the loop will cease and the remainder of the application will continue executing.

##Do While Loop
The Do While Loop is a variant of the While loop that functions in much the same way with one primary difference - in the While loop, the condition is run prior to the execution of the encapsulated code; in a Do While loop, however, the encapsulated code is run prior to the condition. Looking back at the example above, the players in Blackjack always need to be dealt 2 cards initially. As such, a Do While loop can be used to deal the initial cards:

