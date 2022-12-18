---
layout: default
title: Common Concepts
parent: Rust
grand_parent: Notes
---

## Common Concepts
Contains some of the basic general concepts of programming languages and how they apply to Rust.

### Variables
#### Mutability
Variables are immutable by default. This means once a value is bound to the variable name, the value can't be changed. 
To make variables mutable the mut keyword has to be added after the let keyword, when instancing a variable. 

```rust
let mut x : i8 = 5;
```

#### Constants
Variables that are immutable and can't be mutable, use the const keyword instead of let and the type of the value needs to be annotated. 

Can be declared in any scope even the global one and can be set only to a constant expression, that evaluates at compile time not runt time. 

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

#### Shadowing
Allows using the same variable name to use that variable instead of the first one, effectively "shadowing" it.
Additionally allows changing the type which is not possible when using a mutable variable. 

```rust
let spaces = "   "; 
let spaces = spaces.len(); 
```

### Data Types
Rust ist statitically typed, meaning all types of variable must be known at compile time. They can be inferred by the compiler most of the time, but it sometimes needs a type annotation. 

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

#### Scalar Types
Represents a single value: integers, floating-point numbers, Booleans, and characters. 

##### Integer Types
Number without fractional component. 
Signed / unsigned refers to whether the number can represent negative values. 

**n**: Represents the amount of bits that variant uses

**Signed size**: -(2^n−1) to (2^n−1) - 1

**Unsigned size**: 0 to (2^n) - 1

**LENGTH** | **SIGNED** | **UNSIGNED** |
 ----- | ------ | -------- |
8-bit  | `i8`     | `u8`   | 
16-bit | `i16`    | `u16`  |
32-bit | `i32`    | `u32`  |
64-bit | `i64`    | `u64`  |
128-bit| `i128`   | `u128` |
arch[^1]| `isize` | `usize`|

[^1]: Used architecture of pc 32-bit or 64-bit 

**NUMBER LITERALS** | **EXAMPLE** |
 -----          | ------  |
Decimal | `98_222` |
Hex | `0xff` |
Octal | `0o77` | 
Binary | `0b1111_0000` |
Byte (u8 only) | `b'A'` |
 

Integer overflows are detect at runtime by rust in debug mode and will result in a panic **integer overflow**. 

#### Floating Types
Numbers with decimal points.
All floating-point types are signed and a floating-point number is 64-bit per default.


**LENGTH** | **SIGNED** |
 ----- | ------ |
32-bit | f32 |
64-bit | f64 |

#### Boolean Type
Two possible values true and false, 1-bit in size. Defined with bool. 

#### Character Type
Defined with `char`, 32-bit in size. char literals are specified `'c'`, opposed to string literals, which use `"string"`.
Represents a Unicode scalar value, range from `U+0000` to `U+D7FF` and `U+E000` to `U+10FFFF` inclusive. 

### Compound Types
Can group multiple values into one type. 

#### Tuple Type
General way of grouping together number of values with variety of types, have a fixed length (once declared they cannot shrink or grow in size). 

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1); 
```

Single element can be accessed directly with their given index. 

```rust
let five_hundred = tup.0;
let six_point_four = tup.1;
let one = tup.2;
```

Tuple without any value is called unit, this values and type are both written as **()**. Expressions implicitly return the unit value if they don't return any other value.

##### Pattern Matching
The individual values from the tuple can be destructured with pattern matching.

```rust
let (x, y, z) = tup;
```

#### Array Type
Every element has to have the same type and the array has a fixed size.
To type annotate we first enter the type in square brackets followed by a semicolon and the size.

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

To initialize an array with the same value, we can first enter the value and then after a semicolon the amount of items that should be created with that value in square brackets. 

```rust
let a = [3; 5];
```

##### Accessing
Element can be accessed using indexing. 

```rust
let first = a[0];
let second = a[1];
```

Accessing an index past the actual size is checked at runtime by rust and will result in a panic if detected with index out of bounds. 

### Functions
Uses `fn` keyword to declare a new function and uses **snake case** as conventional style. 

```rust
fn another_function() {
    println!("Another function.");
}
```

Curly brackets tell the compiler, where function body begins and ends. 

#### Parameters
Functions can have parameters which are special variables that are part of the signature. It can then be provided with arguments for those variables.

```rust
fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```

Type of each parameter needs to be declared in a function signature. 

#### Statements / Expressions
**Statements** are instructions that perform some action and do not return a value.

```rust
let y = 6;
```

**Expression** evaluate to a resulting value. 

```rust
let x = 3;
x + 1
```

New scope block, calling a macro, calling a function are all expressions.
If you add a semicolon to the end of an expression it turns into a statement and will not return a value.

```rust
let y = {
    let x = 3;
    x + 1
};
```

#### Functions with Return Values
Return values must have their type declared after an arrow ->.

```rust
fn five() -> i32 {
    5 
}
```

5 in this context is the same as return 5;

### Comments 
Additional text to describe code.

**Single Line**:
```rust
// Example
```

**Multi Line**:
```rust
/*
 * Example
 */
```

### Control Flow
Ability to run some code (repeatedly) depending on if a condition is true or not.

#### If Expressions
Allows to branch code depending on conditions.

```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```

#### Handling Multiple Conditions with else if
Multiple conditions can be used by combining `if` and `else` in `else if` expression. 

If more than one `else if` is used, a `match` expression might be useful. 

#### Using if in let Statement
If is an expression and can therefore be assigned to a variable.

```rust
let number = if condition { 5 } else { 6 };

let number = if condition { 5 } else { "six" };
```
**Evaluates to error**

#### Repetition with Loops
Executes a block more than once, `loop`, `while`, `for`.

##### Loop
Executes a block of code over and over again forever or until explicitly stopped.

##### Returning values
Loops are expression and can therefore return expression, simply add a semicolon after the loop brackets.

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

##### Loop Labels
Loops withing loops, mean `break` and `continue` apply only to the innermost loop.

Optionally a loop label can be specified, that can be used with break or continue to specify which loop they apply to. Start with single quote. 

```rust
'counting_up: loop {
    loop {
        break 'counting_up;
    }
}
```

##### Conditional Loops
Evaluate a condition withing a loop, `while` the condition is `true` the loop runs.
When condition ceases to be `true`, the program calls `break`.

Built-in language construct `while` loop:

```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");
        number -= 1;
    }
    println!("LIFTOFF!!!");
}
```

##### Looping Through Collections
Loop through a collection and accessing all of its elements.
Built-in language construct `for` loop.

```rust
let a = [10, 20, 30, 40, 50];

for element in a {
    println!("the value is: {element}");
}

for number in (1..4).rev() {
    println!("{number}!");

}
println!("LIFTOFF!!!");
``` 

###### Getting both Index and Element
Using `arr.iter().enumerate()`, we can get a tuple with both the index and the reference to the element.

```rust
for (i, e) in a.iter().enumerate() {
    println!("Index: {i}, Value: {e}");
}
```
