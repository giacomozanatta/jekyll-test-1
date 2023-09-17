---
title: 4. Testing Java Application
category: 2. Gradle
order: 2
---
<h2>Contents</h2>
* toc
{:toc}

## Testing
Testing is a program analysis technique that aims to check the correctness of a piece of code just by observing its executions.
Suppose that we have this Java application:  

{% highlight java %}
package org.example;

public class Main {

    public static int add(int a, int b) {
        return Math.addExact(a, b);
    }

    public static void main(String[] args) {
        int a = Integer.parseInt(args[0]);
        int b = Integer.parseInt(args[1]);

        int result = Main.add(a, b);

        System.out.println("The result is "+result);
    }
}
{% endhighlight %}
We want to check if the function adds behaves correctly. The function should return the sum of two integers: if we have, for example, 3 and 4, Main.add should return 7.   
We can check manually by running the main method. As you can see, the main function takes in input the two numbers to add as arguments. With the "run" task of gradle, one can pass arguments using the flag --args, as the next example shows: 
{% highlight bash %}
~/IdeaProjects/Gradle-GettingStarted ./gradlew run --args="3 4"

> Task :run
The result is 7

BUILD SUCCESSFUL in 799ms
2 actionable tasks: 1 executed, 1 up-to-date
~/IdeaProjects/Gradle-GettingStarted 
{% endhighlight %}
The function seems to behave correctly. But what we can say for all the other integers? We didn't know. Let's try some other cases:
{% highlight bash %}
~/IdeaProjects/Gradle-GettingStarted ./gradlew run --args="-3 -10"

> Task :run
The result is -13

BUILD SUCCESSFUL in 810ms
2 actionable tasks: 1 executed, 1 up-to-date
~/IdeaProjects/Gradle-GettingStarted ./gradlew run --args="56 -1" 

> Task :run
The result is 55

BUILD SUCCESSFUL in 728ms
2 actionable tasks: 1 executed, 1 up-to-date
~/IdeaProjects/Gradle-GettingStarted ./gradlew run --args="452 123"

> Task :run
The result is 575

BUILD SUCCESSFUL in 770ms
2 actionable tasks: 1 executed, 1 up-to-date
~/IdeaProjects/Gradle-GettingStarted ./gradlew run --args="-452 123"

> Task :run
The result is -329

BUILD SUCCESSFUL in 717ms
2 actionable tasks: 1 executed, 1 up-to-date
~/IdeaProjects/Gradle-GettingStarted 
{% endhighlight %}
What we are doing here is called manual testing: it requires that a person (tester) put itself in the shoes of an end-user to test the application's features to ensure correct behavior. However, testing an application this way could be time-consuming and costly, and often could lead to human error. Automated Testing comes with our help: it is a testing method that permits the definition of tests in a programmable way. The idea is to write some software to check automatically the execution of our program. The intuition is that, instead of manually try some combinations of numbers and check that the sum provided by our program is correct, we can define a set of cases that we want to test and then run automatically software that controls that these cases produce the correct results. This lesson aims to show how we can use a Java Framework (<a href="https://junit.org/junit5/">JUnit</a>) that permits to write and run tests. The benefits of using an automated test arises with more complex examples: suppose that you wrote a function that do something and you are manually testing it. You want to test 100 distinct input values to be sure that your function behave correctly. So you start testing, and at the 99th test you find that something is wrong. In that case, you need to fix the code, and you need to redo all the tests! This require a lot of time, and having an automated testing mechanism permits to test all the inputs at once.

**NOTE**: *this doesn't mean that manual testing is useless, indeed it is required in the early phase of the software development, before automating, to check automation feasibility. In addition, when you download and try a beta version of an application, you are actually performing some sort of manual testing.*
### Unit Testing and Integration Testing
Before going into details, it is necessary to say something about different testing processes. Here we will focus on Unit Testing, that is, testing the correctness of software component (for example, a function or a class) in isolation. This is the first phase of testing and it is considered a white-box testing: these tests are performed by a developer that knows the internal design of the software.  
If we want to test how two or more software components interact each other we need to talk about integration testing. Integration tests are perform after unit tests, and doesn't require a knowledge of the internal design of the software (black-box testing). The idea here is to test the correctness of the interface between software units.
## JUnit
<a href="https://junit.org/junit5/">JUnit</a> is a unit testing framework for the Java programming language.