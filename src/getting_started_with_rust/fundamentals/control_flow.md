# Control Flow
<!-- toc -->
The ability to run some code depending on whether a condition is true and to run some code repeatedly while a condition is true are basic building blocks in most programming languages. The most common constructs that let you control the flow of execution of Rust code are if expressions and loops.

## `if` Expressions

An if expression allows you to branch your code depending on conditions. All if expressions start with the keyword if, followed by a condition.

```rust
fn main() {
    let x = 1;

    if x > 5 {
        println!("x is greater than five and has a value of: {x}");
    } else {
        println!("x is less than five and has a value of: {x}");
    }
}
```

Unlike languages such as Ruby and JavaScript, Rust will not automatically try to convert non-Boolean types to a Boolean.

### Handling Multiple Conditions with `else if`

You can use multiple conditions by combining if and else in an else if expression. 

```rust
fn main() {
    let x = 10;

    if x % 4 == 0 {
        println!("{x} is divisible by 4");
    } else if x % 3 == 0{
        println!("{x} is divisible by 3");
    } else {
        println!("{x} is not divisible by 4 or 3");
    }
}
```

Using too many else if expressions can clutter your code, so if you have more than one, you might want to refactor your code. 

### Using an `if let`
Because if is an expression, we can use it on the right side of a let statement to assign the outcome to a variable.

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of the number is: {number}");
}
```
Remember that blocks of code evaluate to the last expression in them, and numbers by themselves are also expressions. In this case, the value of the whole if expression depends on which block of code executes. This means the values that have the potential to be **results from each arm of the if must be the same type**.

## Repition with Loops
It’s often useful to execute a block of code more than once. For this task, Rust provides several loops, which will run through the code inside the loop body to the end and then start immediately back at the beginning.

> The following code will just give a `playground timeout` error, if you run this on your own machine, the loop will continue forever. Thanks for not breaking my server!
```rust
fn main() {
    loop {
        println!("Too infinity, and beyond!");
    }
}
```

Rust also provides a way to break out of a loop using code. You can place the break keyword within the loop to tell the program when to stop executing the loop. We can use continue, which in a loop tells the program to skip over any remaining code in this iteration of the loop and go to the next iteration.

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}");
}
```

Before the loop, we declare a variable named counter and initialize it to 0. Then we declare a variable named result to hold the value returned from the loop. On every iteration of the loop, we add 1 to the counter variable, and then check whether the counter is equal to 10. When it is, we use the break keyword with the value counter * 2. After the loop, we use a semicolon to end the statement that assigns the value to result.

### Loop labes to Disambiguate Between Multiple Loops

If you have loops within loops, break and continue apply to the innermost loop at that point. You can optionally specify a loop label on a loop that you can then use with break or continue to specify that those keywords apply to the labeled loop instead of the innermost loop. Loop labels must begin with a single quote.

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```

### Conditional Loops with `while`

A program will often need to evaluate a condition within a loop. While the condition is true, the loop runs. When the condition ceases to be true, the program calls break, stopping the loop. It’s possible to implement behavior like this using a combination of loop, if, else, and break;

```rust
fn main() {
    let mut x = 3; 

    while x != 0 {
        println!("{x}");

        x -= 1;
    }

    println!("LIFTOFF!");
}
```

This construct eliminates a lot of nesting that would be necessary if you used loop, if, else, and break, and it’s clearer. While a condition evaluates to true, the code runs; otherwise, it exits the loop.

### Looping Through a collection with `for`
You can choose to use the while construct to loop over the elements of a collection, such as an array.

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
    let mut index = 0;

    while index < 5 {
        println!("The value is: {}", a[index]);

        index += 1;
    }
}
```

However, this approach is error prone; we could cause the program to panic if the index value or test condition is incorrect. For example, if you changed the definition of the a array to have four elements but forgot to update the condition to while index < 4.

As a more concise alternative, you can use a for loop and execute some code for each item in a collection.

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for i in a {
        println!("The value is: {i}");
    }
}
```

The safety and conciseness of for loops make them the most commonly used loop construct in Rust.