# Contents
- Statements
- Expressions
- Patters

## Statements

A Statement is a segment of code that does not return any value. Statements are easy to identify as they end with a semicolon (;) to denote that nothing is returned.

```rust
{
    // This is a statement, but we cannot access its value
    "purple";
}
```

When we use let to hold a piece of data, this is called a “let statement” as it holds the data but does not return the value itself:

```rust
{
    // We can access the data created by this statement with the variable answer
    let answer = "purple";
}
```

A function or macro that does not return any data is also a statement:
```rust
{
    fn say_answer() {
        let answer = "purple";
        println!("{answer}")
}
}
```

The previous example does not require a ; on the last line of the function body because println! is also a statement.


## Expressions
An Expression is a segment of code that returns a value. Since Rust is an ‘expression-oriented’ language, all code blocks will implicitly return their value unless we utilize a semicolon to terminate the expression:

```rust
{
    // This is an expression, it will return the &str "green"
    "green"
}
```


A function or macro that returns a value is also an expression:
```rust
{
    fn give_answer() -> String {
        let answer = "green".to_string();
        answer
    }
    
    println!("{}", give_answer());
}
```

## Patterns
Rust’s syntax also allows Patterns, which are special syntax rules that can be used in certain situations. Patterns help to make the language more readable and allow us to do things that are otherwise not easily accomplished.

One example of a pattern is the ability to declare multiple variables within the same let statement:

```rust
{
    let (x, y) = (5, 10);
}
```

There are many other patterns available that have their own special syntactical rules.

A comprehensive list of patterns can be found in the [Rust Reference](https://doc.rust-lang.org/reference/patterns.html#identifier-patterns). However, we will touch on all of these syntaxes in context throughout this course.

```rust
{

}
```