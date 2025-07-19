## 1. Function Basics: Declaration and Parameters
A function is a reusable block of code that performs a task. The syntax is clean and straightforward.

Go Syntax:

Go

// func <name>(<parameters>) <return_type>
func calculateBill(price int, quantity int) int {
    totalPrice := price * quantity
    return totalPrice
}
Shorthand for Same-Type Parameters:
If consecutive parameters share the same type, you can state the type only once at the end. This is a small but neat syntactic sugar.

Go

// Instead of (price int, quantity int)
func calculateBill(price, quantity int) int { ... }
↔️ Java Comparison
Java's method syntax is similar but more verbose.

Java Syntax:

Java

// <access_modifier> <return_type> <name>(<parameters>)
public int calculateBill(int price, int quantity) {
    int totalPrice = price * quantity;
    return totalPrice;
}
💡 Key Differences
Keywords & Order: Go uses the func keyword and places the return type after the parameter list. Java uses access modifiers (public, private) and places the return type before the method name.

Parameter Syntax: In Go, the variable name comes before the type (price int), which is the reverse of Java (int price).

Visibility: Go determines visibility by the case of the function name (more on this later). Java uses explicit keywords (public, private).

## 2. Advanced Return Values
This is where Go truly shines and differs significantly from Java.

Multiple Return Values
A Go function can return more than one value. This is the idiomatic way to handle operations that might fail (returning a result and an error) or that naturally produce multiple outputs.

Go Code:

Go

// This function returns both area and perimeter
func rectProps(length, width float64) (float64, float64) {
    area := length * width
    perimeter := (length + width) * 2
    return area, perimeter
}

func main() {
    // Assign both returned values to variables
    area, perimeter := rectProps(10.8, 5.6)
    fmt.Printf("Area: %f, Perimeter: %f\n", area, perimeter)
}
Named Return Values
You can name your return values in the function signature. They act like variables declared at the top of the function. A return statement without any arguments (a "naked return") will automatically return the current values of these named variables.

Go Code:

Go

// 'area' and 'perimeter' are named return values
func rectPropsNamed(length, width float64) (area, perimeter float64) {
    area = length * width
    perimeter = (length + width) * 2
    return // Naked return automatically sends back 'area' and 'perimeter'
}
Note: While clever, naked returns can sometimes harm readability in longer functions. Use them with care.

The Blank Identifier (_)
If a function returns multiple values but you only care about some of them, you can use the blank identifier _ to discard the ones you don't need. This is required because Go's compiler will error if you declare a variable and don't use it.

Go Code:

Go

// I only need the area, so I discard the perimeter.
area, _ := rectProps(10.8, 5.6)
fmt.Printf("Area: %f\n", area)
↔️ Java Comparison
Java methods can only return one value. To achieve a similar result, you must use workarounds:

Return an object (a custom class) that holds multiple values.

Return an array or a List.

Return a Map.

Java Workaround Example:

Java

// You would need a custom class to hold the results
class RectangleProperties {
    double area;
    double perimeter;
    // constructor, getters...
}

public RectangleProperties rectProps(double length, double width) {
    double area = length * width;
    double perimeter = (length + width) * 2;
    return new RectangleProperties(area, perimeter);
}
💡 Key Differences
Go's native support for multiple returns is a massive ergonomic win. It makes code cleaner and avoids the boilerplate of creating wrapper classes just to return multiple values. The common result, err := someFunction() pattern is a cornerstone of Go error handling.

## 3. Functions as First-Class Citizens
This is a powerful concept where Go and modern Java have strong parallels. In Go, functions are treated like any other variable. You can:

Assign them to variables.

Pass them as arguments to other functions.

Return them from other functions.

Assigning Functions to Variables
Go

func add(a, b int) int {
    return a + b
}

func main() {
    // 'operation' is now a variable that holds the 'add' function
    operation := add
    result := operation(3, 5) // Call the function through the variable
    fmt.Println(result) // Prints: 8
}
Passing Functions as Arguments
Go

// This function takes two integers and another function as arguments
func applyOperation(x, y int, operation func(int, int) int) int {
    return operation(x, y)
}

func main() {
    result := applyOperation(5, 2, add)
    fmt.Println(result) // Prints: 7
}
Returning Functions (Closures)
A function can return another function. The returned function "closes over" its environment, meaning it remembers the variables from the scope where it was created. This is known as a closure.

Go

// createMultiplier returns a new function
func createMultiplier(factor int) func(int) int {
    // This inner anonymous function captures the 'factor' variable
    return func(x int) int {
        return factor * x
    }
}

func main() {
    // multiplyBy2 is a function that remembers 'factor' is 2
    multiplyBy2 := createMultiplier(2)
    fmt.Println(multiplyBy2(6)) // Prints: 12

    // multiplyBy10 is a function that remembers 'factor' is 10
    multiplyBy10 := createMultiplier(10)
    fmt.Println(multiplyBy10(6)) // Prints: 60
}
↔️ Java Comparison
Since Java 8, Java also treats functions as first-class citizens through lambda expressions and functional interfaces.

Assigning to Variables: A Go func variable is analogous to a Java Function, Supplier, or Consumer interface.

Java

// Java equivalent
BiFunction<Integer, Integer, Integer> operation = (a, b) -> a + b;
int result = operation.apply(3, 5); // result is 8
Passing as Arguments: This is identical to passing a lambda to a Java method.

Java

// Java equivalent
public int applyOperation(int x, int y, BiFunction<Integer, Integer, Integer> op) {
    return op.apply(x, y);
}
Closures: Java lambdas can also be closures, capturing variables from their enclosing scope.

Java

// Java equivalent
public static Function<Integer, Integer> createMultiplier(int factor) {
    return (x) -> x * factor;
}
💡 Key Differences
The concept is the same, but the syntax is different. Go has a native, built-in func type. Java achieves this via library interfaces (java.util.function) and the special syntax of lambdas (->). The Go approach feels more integrated into the language itself, while the Java approach was added to an existing object-oriented paradigm.

## 4. Special Function Concepts
Anonymous Functions
You can declare a function without giving it a name. This is especially useful for short-lived operations or for creating closures.

Go

// Declaring and assigning an anonymous function to a variable
greet := func() {
    fmt.Println("Hello, World!")
}
greet() // Call it

// Declaring and immediately executing an anonymous function
// (Similar to an IIFE in JavaScript)
func() {
    fmt.Println("Immediately Invoked!")
}()
Public vs. Private Visibility
This is a simple but critical convention in Go. There are no public or private keywords.

Public (Exported): If a function name starts with a capital letter, it is public. It can be called by code in other packages.

Go

// This function can be imported by other packages
func CalculateArea(radius float64) float64 { ... }
Private (Unexported): If a function name starts with a lowercase letter, it is private. It can only be called by other functions within the same package.

Go

// This function is internal to the current package
func calculateArea(radius float64) float64 { ... }
