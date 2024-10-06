# Data Types
<!-- toc -->
Data Types tell Rust which can of data is being specified so it knows how to work with that data. Rust has two major subsets of data types: *scalar* and *compound*.

Rust is a *statically typed* language, which means it needs to know the *types* of all variables at compile time. 

## Scalar Types
A scalar type represents a single value. Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters. 

### Integer
An integer is a number without a fractional component. E.g. 1, 2, 3, 4, 5... You can use the prefix `u` for unsigned integer types (i.e. non-negative numbers don't need a *sign*  `-`). Or you can use `i` for integers that could potentially be negative. 

| Length | Signed | Unsigned|
|-----|-----|-----|
|8-bit|i8|u8|
|16-bit|i16|u16|
|32-bit|i32|u32|
|64-bit|i64|u64|
|128-bit|i128|u128|
|arch|isize|usize|

Signed and unsigned refer to whether it’s possible for the number to be negative—in other words, whether the number needs to have a sign with it (signed) or whether it will only ever be positive and can therefore be represented without a sign (unsigned). Each signed variant can store numbers from –(2n – 1) to 2n – 1 – 1 inclusive, where n is the number of bits that variant uses. So an i8 can store numbers from –(27) to 27 – 1, which equals –128 to 127. Unsigned variants can store numbers from 0 to 2n – 1, so a u8 can store numbers from 0 to 28 – 1, which equals 0 to 255.

|Number literals | Example |
|-----|-----|
|Decimal | 98_222 |
|Hex | 0xff |
|Octal | 0o77 | 
|Binary | 0b1111_0000 |
|Byte (u8 only) | b'A' |

So how do you know which type of integer to use? If you’re unsure, Rust’s defaults are generally good places to start: integer types default to i32.

### Floating-Point Types

Rust also has two primitive types for floating-point numbers, which are numbers with decimal points. Rust’s floating-point types are f32 and f64, which are 32 bits and 64 bits in size, respectively. The default type is f64 because on modern CPUs, it’s roughly the same speed as f32 but is capable of more precision. All floating-point types are signed.

```rust
fn main() {
	let x: f32 = 4.234;
}
```

### The Boolean Type

As in most other programming languages, a Boolean type in Rust has two possible values: true and false. Booleans are one byte in size.

```rust
fn main() {
	let t = true;
	
	let f: bool = false;
}
```

### The Character Type
Rust’s char type is the language’s most primitive alphabetic type.

```rust
fn main() {
	let a = 'b';
	let z: char = 'Z';
}
```

Note that we specify char literals with single quotes, as opposed to string literals, which use double quotes.

## Compund Types

Compound types can group multiple values into one type. Rust has two primitive compound types: *tuples* and *arrays*.

### The Tuple Type

A tuple is a general way of grouping together a number of values with a variety of types into one compound type. Tuples have a fixed length: once declared, they cannot grow or shrink in size. We create a tuple by writing a comma-separated list of values inside parentheses.

```rust
fn main() {
	let tup: (i32, f64, u8) = (1000, 3.14159, 7);
	
	let (a, b, c) = tup;
	
	println!("The value of b is: {b}");
}
```

It then uses a pattern with let to take tup and turn it into three separate variables, `a`, `b`, and `c`. This is called destructuring because it breaks the single tuple into three parts. 

We can also access a tuple element directly by using a period (.) followed by the index of the value we want to access.

```rust
fn main() {
	let tup: (i32, f64, u8) = (1000, 3.14159, 7);
	
	let one_thousand = tup.0;
	let text_pi = tup.1;
	let lucky_number_seven = tup.2;
	
	println!("{text_pi}");
}
```

**The tuple without any values has a special name, *unit*.**

### The Array Type

Another way to have a collection of multiple values is with an *array*. Unlike a tuple, every element of an array must have the same type. Unlike arrays in some other languages, arrays in Rust have a fixed length. 

```rust
fn main() {
	let a  = [1, 2, 3, 4, 5];
	println!("{:?}", a);
}
```

Arrays are useful when you want your data allocated on the *stack* rather than the *heap* or when you want to ensure you always have a fixed number of elements. An array isn’t as flexible as the vector type, though. A vector is a similar collection type provided by the standard library that is allowed to grow or shrink in size. If you’re unsure whether to use an array or a vector, chances are you should use a vector. However, arrays are more useful when you know the number of elements will not need to change. 

You write an array’s type using square brackets with the type of each element, a semicolon, and then the number of elements in the array.

```rust
fn main() {
	let a: [i32; 5] = [1, 2, 3, 4, 5];
	println!("{:?}", a)
}
```

You can also initialize an array to contain the same value for each element by specifying the initial value, followed by a semicolon, and then the length of the array in square brackets.

```rust
fn main() {
	let a = [5; 10];
	
	println!("{:?}", a);
}
```

An array is a single chunk of memory of a known, fixed size that can be allocated on the stack. You can access elements of an array using indexing,

```rust
fn main() {
	let a = [1, 2, 3, 4, 5];
	
	let first = a[0];
	let second = a[1];
	
	println!("{first} then {second}");
}
```

Let’s see what happens if you try to access an element of an array that is past the end of the array.

```rust
fn main() {
	let a = [1, 2, 3, 4, 5];
	
	let err = a[8];
}
```

The program resulted in a runtime error at the point of using an invalid value in the indexing operation.