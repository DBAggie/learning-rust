# Contents
- Scope
- Ownership

#Scope and Ownership

## *The concept of scope is very important in the Rust language because of its implications on memory management. The main goal of Rust is to avoid memory-based errors, so strict scoping rules allow the compiler to know when memory can safely be accessed. *

## Blocks
In Rust, code can be separated into blocks. A block of code is a collection of statements and an optional expression contained within `{}`:

```rust
{
    // Statement block
    {
        let number_1 = 11;
        let number_2 = 31;
        let sum = number_1 + number_2;
        println!("{sum}");
    }
    
    // Expression block
    {
        let number_1 = 11;
        let number_2 = 31;
        number_1 + number_2
    }
}
```

Blocks can be treated as the single statement or expression they evaluate to. This means we can assign variables to a block of code:

```rust
{
    let sum = {
        let number_1 = 11;
        let number_2 = 31;
        number_1 + number_2
    };
    
    println!("{sum}");
}
```

If we look closely, we can see that functions are actually just callable, named blocks.

```rust
{
    fn sum() -> u32 {
        let number_1 = 11;
        let number_2 = 31;
        number_1 + number_2
    }
}
```

## Scope
Scope is the concept of whether or not a particular item exists in memory and is accessible at a certain location in our codebase. In Rust, the scope of any particular item is limited to the block it is contained in.

When a block is closed, all of its values are released from memory and are then considered out-of-scope. If an item is accessible, it is considered in-scope.

The following example showcases how scoping works by shadowing a variable. Note how our last println!() call will print “10” because our second declaration of number is dropped from memory at the end of its containing block.

```rust
{
    let number = 10;
    
    {
        println!("{number}"); // Prints "10"
    
        let number = 22;
        println!("{number}"); // Prints "22"
    } // Our second declaration of `number` is dropped from memory here.
    // It is now considered out-of-scope.
    
    println!("{number}"); // Prints "10"
}
```

### Visibility
We can make an item accessible outside of its normal scope by denoting it as public with the pub keyword.

All items in Rust are private by default. Private items can only be accessed within their declared module and any children modules.

```rust
{
    mod numbers {
        pub const ZERO: i32 = 0;
    }
    
    mod another_scope {
        use super::numbers::ZERO;
    
        fn print_zero() {
            println!("{ZERO}");
        }
    }
}
```

Fields of complex datatypes have their own visibility qualifiers.

```rust
{
    pub struct Number {
        pub value: i32,
    }
    
    let mut number = Number { value: 0 };
    
    // We can only access value directly because it is public
    number.value += 1;
    println!("{}", number.value);
}
```

When our crate is a library, all items denoted as public will be accessible to anyone who imports our library. More information about pub and use can be found in the modules article.

## Ownership
Scoping rules in Rust are very strict, but with good reason. Managing lifetimes and mutability in a memory-safe way is much easier when we disallow accessing items from parent blocks.
Rules

The rules that the Rust compiler follows to validate that we do not attempt unsafe behavior are as follows:

- Each value in Rust has a variable that’s called its owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

These rules of ownership have differing implications depending on whether our data is stored on the stack or the heap.
Stack vs Heap

If we assign a variable to an existing variable with a stack-based type such as i32, it will make a computationally inexpensive copy of that value.

```rust
{
    let stack_1 = 32;
    let stack_2 = stack_1; // The value of `stack_1` is copied into `stack_2`
    
    // We now have two values we can work with
    println!("{stack_1}");
    println!("{stack_2}");
}
```

When working with datatypes that utilize the heap, such as `String`, we cannot copy values from one variable to another since heap-based types do not implement the `Copy` trait.

Instead of copying, Rust will instead move the value out of original variable and into the new one.

```rust
{
    let heap_1 = String::from("Only you can!");
    let heap_2 = heap_1; // The value of `heap_1` is moved into `heap_2`
    
    // We cannot print `heap_1` because it's now owned by `heap_2`
    println!("{heap_2}");
}
```

We can choose to `clone()` our data, which is equivalent to copying on the heap. But unlike implicit copying, cloning data must always be explicitly stated.

We can clone any type that implements the `Clone` trait.

```rust
{
    let heap_1 = String::from("Prevent corruption!");
    let heap_2 = heap_1.clone(); // We have now cloned the data from `heap_1` into `heap_2`
    
    // We now have two values we can work with
    println!("{heap_1}");
    println!("{heap_2}");
}
```

Be aware that cloning is only necessary when we need another copy of the data. When we are not in need of a separate copy, we can instead reference the data.

## Functions
Ownership rules with functions work much the same way.

```rust
{
    // When we have a function that return a value, the ownership of that value is passed to the caller.
    fn abc() -> String {
        "abc".to_string()
    }
    
    let letters = abc(); // The value created in `abc()` is now owned by `letters`
    
    // When we have a function that passes a value through it, it can be thought of as temporarily taking ownership of that value until the function call has completed.
    fn print_through(s: String) -> String {
        s
    }
    
    let finished = print_through(letters); // letters has been moved into finished
}
```

The rules of ownership above are taken directly from The Rust Book. A more in-depth explanation as to how these rules operate directly in memory can be found there.