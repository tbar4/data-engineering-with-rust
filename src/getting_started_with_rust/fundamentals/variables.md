# Variables
<!-- toc -->
# Variables and Mutability
By default, variables are immutable in Rust. This means that once a value is bound to a variable name, you are no longer able to make changes to it. For example, the following will give you an error:

```rust
fn main() {
	// Let's assign the value of 5 to `x`
	let x = 5;
	println!("The value of x is: {x}");
	
	// Let's try reassigning `x` to a value of 6
	x = 6;
	println!("The value of x is: {x}");
}
```

These copiler errors can seem burdensome and frustrating at first, but you will soon grow to love the rich and detailed feedback of the Rust compilier. Rust's compiler even tells you how to correct the error in most instances. By reading the output of the compiler, we can see that we need to add a `mut` identifier to our variable of `x`. `mut` will turn our variable into a mutable instance. That means we can make changes to it, but this can also be dangerous. Which is why it is has to be specified, so you are aware that you are doing something dangerous (this is a recurring theme in Rust). Let now change our `x` variable to be *mutable* :

```rust
fn main() {
	// Let's assign the value of 5 to `x`
	let mut x = 5;
	println!("The value of x is: {x}");
	
	// Let's try reassigning `x` to a value of 6
	x = 6;
	println!("The value of x is: {x}");
}
```
## Constants
Like immutable variables, constants are values that are bound to a name and are not allowed to change, but there are a few differences between constants and variables. You are not allowed to use `mut` with constants. You also use `const` keyword when declaring a *constant* and you need to annotate the type (i.e. if my constanct is an integer I need to put `: u8` after the name of the constant). Constants can be delared in any scope, they also need to be set and cannot be the result of a runtime expression. Rust's standard naming convention for constants is all uppercase with an underscore between words. 

```rust
fn main() {
	let FOUR_HOURS_IN_SECONDS = 4 * 60 * 60;
	
	println!("{FOUR_HOURS_IN_SECONDS}");
}
```

## Shadowing
You can declare a new variable with the same name as a previous variable, you would then say the first variable is *shadowed* by the second. the second variable overshadows the first, taking any uses of the variable name to itself until either it itself is shadowed or the scope ends. Here is an example:

```rust
fn main() {
	// Assign 5 to `x`
	let x = 5;
	
	// Reassign the variable of `x`
	let x = x + 1;
	
	{
		// Assign `x` a new value to this inner scope
		let x = x * 2;
		
		println!("The value of x in the inner scope is: {x}");
	}
	
	println!("The value of x in the outer scope is: {x}");
}
```

You might be curious why this ran but the previous code gave an error when we tried to reassign `x`. Look close at the usage of `let` in both code blocks. Shadowing is different from marking a variable as mut because we’ll get a compile-time error if we accidentally try to reassign to this variable without using the let keyword. By using let, we can perform a few transformations on a value but have the variable be immutable after those transformations have been completed. The other difference between mut and shadowing is that because we’re effectively creating a new variable when we use the let keyword again, we can change the type of the value but reuse the same name.

Let's test this out in practice. We are going to initialize `spaces` as a *string* type then reassign it as a *integer* type:
```rust
fn main() {
	let spaces = "     ";
	let spaces = spaces.len();
}
```

We don't get an error because we reinitialzied with `let`. Now, let's try it without `let` and read the compiler error:

```rust
fn main() {
	let mut spaces = "     ";
	spaces = spaces.len();
}
```

Even though spaces is mutable, we tried to assign it a different type, which throws a compiler errror.