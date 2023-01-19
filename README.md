# Memory Management and Garbage Collection

## Learning Goals

- Understand how Java manages memory
- Create multiple references to one object
- Introduce Java's garbage collector

## Memory Management

In Java, objects live in **memory**. When we start the JVM, some memory from the operating system (OS) will be
allocated to the JVM. Once the memory is allocated, we can use that memory to run our Java program. Memory is an
important concept because it helps us create new objects and remove them.

Think of a sandbox. We can build a castle with the sand inside the sandbox. When we are done, we can destroy the
castle, and it returns that sand used to the rest of the sandbox. Memory in Java works a lot like playing in a sandbox!
We have a finite amount of memory to create objects and when those objects are no longer useful to us, we can return
that memory to create new objects.

When talking about memory in programming languages, we will often talk about the **stack** and the **heap**.

### The Stack

The **stack** is where a method stores its variables that are primitive
types  (think of `int` and `double`), along with pointers for all
reference type variables (such as `String`, `Dog`, etc).


Remember that a **reference** is a strongly typed handle for an object. We can use the reference to access the object's
value in memory. All objects in Java, except for primitive types, are accessed through references.

For example, let's look at the `Person` class:

```java
public class Person {

  private String name;
  private int age;

  public static void main(String[] args) {
    int currentYear = 2023;
    String greeting = "Welcome";

    Person person1 = new Person();
    Person person2 = new Person();

    person1.name = "Bola";
    person1.age = 26;

    person2.name = "Kim";
    person2.age = 41;

  }
}
```

Each time a method is called, a new frame is added to the call stack. The
frame contains the method's local variables as well as the parameter variables.

We can see in the image below that the `main()` method's local variables (`currentYear`, `greeting`, `person1`, `person2`)
are stored on the call stack in the method frame.  

![strings inline](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/strings_inlined.png)

### The Heap

The **heap** is where the Java objects live. This is where new objects are created and stored and where they will be
cleaned up when they are no longer referenced by a variable, which is called garbage collection.

Strings are also objects, so in fact they are also stored in the heap as shown below!

![strings as objects](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/strings_objects.png)

But by default the Java Visualizer displays strings in each variable as if
they were primitive values, even though they aren't:

![strings inline](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/strings_inlined.png)





## Object References

Let's update the `main()` method to print the name and age of each person:

```java
public class Person {

  private String name;
  private int age;

  public static void main(String[] args) {
    int currentYear = 2023;
    String greeting = "Welcome";

    Person person1 = new Person();
    Person person2 = new Person();

    person1.name = "Bola";
    person1.age = 26;

    person2.name = "Kim";
    person2.age = 41;

    System.out.println(person1.name + " is " + person1.age);
    System.out.println(person2.name + " is " + person2.age);

  }
}
```

Running the program produces:

```text
Bola is 26
Kim is 41
```


Recall that a reference variable such as `person1` stores the object's address on the heap.
A reference variable is like any other variable in that its value can be reassigned.
This means we can change the pointer to have the variable point at a different object.

Consider what happens if we execute the assignment statement `person1 = person2;`:

```java
System.out.println(person1.name + " is " + person1.age);
System.out.println(person2.name + " is " + person2.age);

person1 = person2;

System.out.println("After reassignment:");
System.out.println(person1.name + " is " + person1.age);
System.out.println(person2.name + " is " + person2.age);
```


The program output produces:

```text
Bola is 26
Kim is 41
After reassignment: Kim is 41
After reassignment: Kim is 41
```

Recall that an assignment statement copies the value on the right hand side into the variable on the left hand side:

![assignment statement](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/assignment_statement.png)

The assignment statement `person1 = person2;` causes the memory address stored in `person2` to be copied into the variable `person1`.
The result is that `person1` will point at the same object as `person2`, i.e. the person named "Kim".

Let's step through with the Java Visualizer to see what happens.
We'll use the browser-based visualizer at 
[this link](https://pythontutor.com/visualize.html#code=public%20class%20Person%20%7B%0A%0A%20%20%20%20private%20String%20name%3B%0A%20%20%20%20private%20int%20age%3B%0A%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20int%20currentYear%20%3D%202023%3B%0A%20%20%20%20%20%20%20%20String%20greeting%20%3D%20%22Welcome%22%3B%0A%0A%20%20%20%20%20%20%20%20Person%20person1%20%3D%20new%20Person%28%29%3B%0A%20%20%20%20%20%20%20%20Person%20person2%20%3D%20new%20Person%28%29%3B%0A%0A%20%20%20%20%20%20%20%20person1.name%20%3D%20%22Bola%22%3B%0A%20%20%20%20%20%20%20%20person1.age%20%3D%2026%3B%0A%0A%20%20%20%20%20%20%20%20person2.name%20%3D%20%22Kim%22%3B%0A%20%20%20%20%20%20%20%20person2.age%20%3D%2041%3B%0A%0A%20%20%20%20%20%20%20%20System.out.println%28person1.name%20%2B%20%22%20is%20%22%20%2B%20person1.age%29%3B%0A%20%20%20%20%20%20%20%20System.out.println%28person2.name%20%2B%20%22%20is%20%22%20%2B%20person2.age%29%3B%0A%0A%20%20%20%20%20%20%20%20person1%20%3D%20person2%3B%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20System.out.println%28%22After%20reassignment%3A%22%29%3B%0A%20%20%20%20%20%20%20%20System.out.println%28person1.name%20%2B%20%22%20is%20%22%20%2B%20person1.age%29%3B%0A%20%20%20%20%20%20%20%20System.out.println%28person2.name%20%2B%20%22%20is%20%22%20%2B%20person2.age%29%3B%0A%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=17&heapPrimitives=nevernest&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false).
You'll have to wait a few seconds while
it loads, but the visualization should start at line 19, the first print statement after both objects
have been created and assigned values for name and age.

![python tutor starting point](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/pythontutor_startpoint.png)

Press the `Next >` button to execute each line:

| Code                                                   | Java Visualizer                                                                                                                                                                                                                                                                                                                                                                                              |
|--------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `System.out.println(person1.name+" is "+person1.age);` | ![person step1](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/person_step1.png)                                                                                                                                                                                                                                                                                                |
| `System.out.println(person2.name+" is "+person2.age);` | ![person step2](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/person_step2.png)                                                                                                                                                                                                                                                                                                |
| `person1 = person2;`                                   | ![person step3](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/person_step3.png)<br><br>The value (a memory address) stored in `person2`<br> is copied into `person1`. This results in both variables<br> referencing the same `Person` instance named "Kim".<br> The `Person` instance named "Bola" is automatically<br>deleted after it stops being referenced by a variable. |
| `System.out.println("After reassignment:");`           | ![person step4](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/person_step4.png)                                                                                                                                                                                                                                                                                                |
| `System.out.println(person1.name+" is "+person1.age);` | ![person step5](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/person_step5.png)                                                                                                                                                                                                                                                                                                |
| `System.out.println(person2.name+" is "+person2.age);` | ![person step6](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/person_step6.png)                                                                                                                                                                                                                                                                                                |


After executing the statement `person1 = person2`, both variables point at the same `Person` object,
so the last two print statements print the same values.

```text
Bola is 26
Kim is 41
After reassignment: Kim is 41
After reassignment: Kim is 41
```

While it may seem odd to have two variables pointing at the same object, this actually is a very
common occurrence in a Java program.  For example, calling a method that takes an object as a parameter
may result in multiple variables pointing at the same object.  




## Garbage Collection

Now that we can see how objects are created within memory in Java, we can look at how that memory is cleaned up!

Java has something called **garbage collection** that will remove objects that are no longer needed. When an object is
no longer useful, the garbage collector will free those objects from memory to make space for new ones that may be
created. This process happens automatically in the background, so us, programmers, do not have to worry too much about
deleting unused objects within their code. Pretty cool, huh?

Let's see why we end up with only one object after executing the statement `person1 = person2;`.

| Code                                                                                                                                                                                                                                                            | Java Visualizer                                                                                                        |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| The program initially created two `Person` class instances.                                                                                                                                                                                                     | ![person before](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/before_reassignment.png)  |
| `person1 = person2;`<br><br>The memory address stored in `person2` is copied into `person1`.<br>This results in variables `person1` and `person2` both referencing<br> the same `Person` named "Kim" and no variable referencing<br> the `Person` named "Bola". | ![person during](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/during_reassignment.png)  |
| The garbage collector deletes the `Person` object named "Bola",<br>since there is no longer a variable referencing the object.                                                                                                                                  | ![person after](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/after_reassignment.png)    |

The `Person` instance named "Bola" became **eligible for garbage collection** when the variable `person1` stopped referencing it.
An object is not useful if it can't be reached through a variable, so the garbage collector frees the object from memory so another
object can use that space if needed.


## Comprehension Check

### Challenge #1

Assume the following `PizzaTopping` class:

```java
public class PizzaTopping {
    private String name;
    private int likes;

    public static void main(String[] args) {
        PizzaTopping topping1 = new PizzaTopping();
        PizzaTopping topping2 = new PizzaTopping();
        topping1.name = "Mushroom";
        topping1.likes = 10;
        topping2.name = "Sausage";
        topping2.likes = 15;

        System.out.println(topping1.name + " has " + topping1.likes + " likes");
        System.out.println(topping2.name + " has " + topping2.likes + " likes");
    }
}
```

<details>
    <summary>How many instances of <code>PizzaTopping</code> are created?</summary>

  <p>Answer: <code>2</code></p> 
  <p>The <code>new</code> operator is used two times: <code>new PizzaTopping()</code></p>

</details>

<br>

<details>
    <summary>What is printed when the program executes?</summary>

  <p>Answer: </p>
  <p>Mushroom has 10 likes<br>Sausage has 15 likes</p>

</details>

<br>

###  Challenge #2

Let's update the  `PizzaTopping` class to add a new assignment statement before the print statements:

`PizzaTopping favoriteTopping = topping2;`

The updated class is shown below:


```java
public class PizzaTopping {
    private String name;
    private int likes;

    public static void main(String[] args) {
        PizzaTopping topping1 = new PizzaTopping();
        PizzaTopping topping2 = new PizzaTopping();
        topping1.name = "Mushroom";
        topping1.likes = 10;
        topping2.name = "Sausage";
        topping2.likes = 15;

        PizzaTopping favoriteTopping = topping2;
        
        System.out.println(topping1.name + " has " + topping1.likes + " likes");
        System.out.println(topping2.name + " has " + topping2.likes + " likes");

    }
}
```

<details>
    <summary>How many instances of <code>PizzaTopping</code> are created? Carefully consider how objects are created before looking at the answer.</summary>

  <p>Answer: <code>2</code></p> 
  <p>The <code>new</code> operator is still only used two times: <code>new PizzaTopping()</code></p>
  <p>The assignment statement <code>PizzaTopping favoriteTopping = topping2;</code> does not create a new object
     since it <b>does not</b> call the <code>new</code> operator. </p>
  <p>The memory address stored in <code>topping2</code> is copied into the variable <code>favoriteTopping</code>, resulting in both
     variables pointing at the same object.</p>
  <img src="https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/challenge_2.png">

</details>

<br>


###  Challenge #3

Let's further update the  `PizzaTopping` class to increment the number of likes for the favorite topping,
and then print the name of the favorite topping:

```java
PizzaTopping favoriteTopping = topping2;
favoriteTopping.likes++;
System.out.println("My favorite topping is " + favoriteTopping.name);
        
```

The updated class is shown below:


```java
public class PizzaTopping {
    private String name;
    private int likes;

    public static void main(String[] args) {
        PizzaTopping topping1 = new PizzaTopping();
        PizzaTopping topping2 = new PizzaTopping();
        topping1.name = "Mushroom";
        topping1.likes = 10;
        topping2.name = "Sausage";
        topping2.likes = 15;

        PizzaTopping favoriteTopping = topping2;
        favoriteTopping.likes++;
        System.out.println("My favorite topping is " + favoriteTopping.name);

        System.out.println(topping1.name + " has " + topping1.likes + " likes");
        System.out.println(topping2.name + " has " + topping2.likes + " likes");

    }
}

```

<details>
    <summary>What is printed when the program executes?  Carefully consider the object references before looking at the answer.</summary>

  <p>Answer: </p>
  <p>My favorite topping is Sausage<br>Mushroom has 10 likes<br>Sausage has 16 likes</p>
  <table>
  <tr>
  <td>Before executing <code>favoriteTopping.likes++;</code></td>
  <td><img src="https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/challenge_2.png"></td>
  </tr>
  <tr>
  <td>After executing <code>favoriteTopping.likes++;</code></td>
  <td><img src="https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/challenge_3.png"></td>
  </tr>
  </table>
  <p>The variables <code>favoriteTopping</code> and <code>topping2</code> both reference the same <code>PizzaTopping</code> object named "Sausage".  We can access the
     object using either variable.</p>

</details>

<br>

Try stepping through the code using the Java Visualizer to confirm your understanding of memory management.

## Conclusion

A method's local variables and parameters are stored in a frame on the call stack.  However, each object is allocated
space in the heap to store its instance variables.  It is possible to assign multiple variables to reference the same object
in the heap.  The garbage collector will remove an object from the heap once there are no variables referencing it.

## Resources

- [Learning Java, 5th Edition](https://learning.oreilly.com/library/view/learning-java-5th/9781492056263/ch01.html#learnjava5-CHP-1-SECT-4.3)
- [Head First Java, 3rd Edition](https://learning.oreilly.com/library/view/head-first-java/9781492091646/ch09.html#the_stack_and_the_heap_where_things_live)
