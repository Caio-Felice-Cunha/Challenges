## Challenge link: https://www.hackerrank.com/challenges/java-stdin-and-stdout-1/problem?isFullScreen=true

# Challenge: Java Stdin and Stdout I
Most HackerRank challenges require you to read input from stdin (standard input) and write output to stdout (standard output).

One popular way to read input from stdin is by using the Scanner class and specifying the Input Stream as System.in. For example:
``` java
Scanner scanner = new Scanner(System.in);
String myString = scanner.next();
int myInt = scanner.nextInt();
scanner.close();


System.out.println("myString is: " + myString);
System.out.println("myInt is: " + myInt);
```

The code above creates a Scanner object named  and uses it to read a String and an int. It then closes the Scanner object because there is no more input to read, and prints to stdout using System.out.println(String). So, if our input is:
``` java
Hi 5
```
Our code will print:
``` java
myString is: Hi
myInt is: 5
```
Alternatively, you can use the BufferedReader class.

Task
In this challenge, you must read  integers from stdin and then print them to stdout. Each integer must be printed on a new line. To make the problem a little easier, a portion of the code is provided for you in the editor below.

Input Format

There are  lines of input, and each line contains a single integer.

Sample Input
```
42
100
125
```
Sample Output
```
42
100
125
```
# Answer:

``` java
import java.io.*;
import java.util.*;

public class Solution {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        int numb1 = scanner.nextInt();
        int numb2 = scanner.nextInt();
        int numb3 = scanner.nextInt();
        
        scanner.close();
        
        System.out.println(numb1);
        System.out.println(numb2);
        System.out.println(numb3);
    }
}
``` 

### Result:
![image](https://github.com/user-attachments/assets/b0ff5328-1696-4e1f-9da8-7849146ab1aa)


## Explanation:
Let's break down the solution to this challenge step-by-step. The challenge requires us to read three integers from standard input (stdin) and then print each integer on a new line to standard output (stdout).

### Detailed Breakdown

#### 1. Import Statements
```java
import java.io.*;
import java.util.*;
```
These lines import necessary libraries:
   - `java.io.*`: This imports all classes in the `java.io` package. In this case, it's not used directly, but it’s commonly included in Java programs that involve input/output operations.
   - `java.util.*`: This imports all classes in the `java.util` package, which includes the `Scanner` class that we use for reading input.

#### 2. Class Definition
```java
public class Solution {
```
- `public class Solution`: This defines a public class named `Solution`. The class must be named `Solution` because HackerRank typically specifies this in their environment.
- The class is declared as `public`, meaning it can be accessed from outside the package. This is required when running in HackerRank’s environment.

#### 3. Main Method
```java
public static void main(String[] args) {
```
- `public`: This method is accessible from outside the class, which is necessary because it is the entry point for execution.
- `static`: The `main` method is static, meaning it belongs to the class and not to instances of the class. This allows the Java runtime to call the method without having to instantiate the class.
- `void`: This indicates that the method does not return any value.
- `String[] args`: This parameter is an array of `String` objects that allows arguments to be passed to the program via the command line. However, it is not used in this challenge.

#### 4. Creating a Scanner Object
```java
Scanner scanner = new Scanner(System.in);
```
- `Scanner scanner`: This creates a `Scanner` object named `scanner`.
- `new Scanner(System.in)`: This initializes the `scanner` object to read input from `System.in`, which is Java’s standard input stream (stdin). The `Scanner` class provides various methods to read different types of input (e.g., `nextInt()`, `nextDouble()`, `nextLine()`, etc.).

#### 5. Reading Integers
```java
int numb1 = scanner.nextInt();
int numb2 = scanner.nextInt();
int numb3 = scanner.nextInt();
```
- Each line reads an integer from the standard input and stores it in a variable.
   - `scanner.nextInt()`: This method reads the next integer token from the input stream. If the input is not an integer, it will throw an `InputMismatchException`.
- We store each integer in its respective variable (`numb1`, `numb2`, `numb3`).

#### 6. Closing the Scanner
```java
scanner.close();
```
- `scanner.close()`: This closes the `scanner` object, freeing up the resources associated with it. It’s good practice to close `Scanner` after reading input to prevent resource leaks.

#### 7. Printing the Integers
```java
System.out.println(numb1);
System.out.println(numb2);
System.out.println(numb3);
```
- Each line uses `System.out.println()` to print the value of the respective integer variable to the standard output (stdout).
   - `System.out.println()`: This method outputs the value to the console followed by a newline. This matches the challenge’s requirement to print each integer on a new line.

### Example Walkthrough
Suppose the input is:
```
42
100
125
```
- **Step 1**: `scanner.nextInt()` reads `42` and assigns it to `numb1`.
- **Step 2**: `scanner.nextInt()` reads `100` and assigns it to `numb2`.
- **Step 3**: `scanner.nextInt()` reads `125` and assigns it to `numb3`.
- **Step 4**: The `scanner` is closed.
- **Step 5**: Each integer is printed to a new line:
  ```
  42
  100
  125
  ```

### Summary
This solution effectively reads exactly three integers from input, assigns them to variables, and prints them one per line. By leveraging the `Scanner` class, the code is straightforward and efficient for handling basic input/output tasks.

