# Hints for rust stuff
- Rust Tools
    - Cargo
        - Cargo Commands
        - Cargo.toml initial file
        - Cargo.toml metadata
        - Cargo.toml dependencies
- Variable Naming
    - Camel Case
    - Snake Case
    - Screaming Snake Case

## Cargo
===
### Cargo Commands
- cargo new
    - Create a new binary executable crate
- cargo new --lib
    - Create a new library crate 
- cargo build
    - Compiles our crate
- cargo build --release
    - Compiles our crate with optimizations
- cargo run 
    - Compiles our crate and runs the compiled executable
- cargo test
    - Run all tests in a crate
- cargo doc --open
    - Build and open our crate's documentation in a web browser
- cargo clean
    - Cleans up temporary files created during compilation
- cargo publish
    - Publishes your crate to emcrates.io
- cargo install
    -Installs a binary directly from *crates.io*

### Cargo.toml Initial File
```toml
{
    [package]
    name = "mybinary"
    version = "0.1.0"
    edition = "2021"

    [dependencies]
    # We can specify external dependencies here
}
```

### Cargo.toml Medtadata
```toml
{
    [package]
    name = "mybinary"  # The crate name
    version = "0.1.0"  # The crate's current version
    edition = "2021"   # Which "Rust Edition" the crate utilizes
    
    description = ""   # A description of our crate for `crates.io`
    keywords = ""      # Keywords for searches on `crates.io`
    documentation = "" # URL to the crate's documentation
    homepage = ""      # URL to the crate's homepage
    repository = ""    # URL of the crate's source repository
    authors = [""]     # The authors of the crate
    license = ""       # The crate's license
    rust-version = ""  # The minimum supported Rust version
}
```

### Cargo Dependencies
To declare a dependecy, we add the crate name and the desired version number under the *[dependencies]* table.
```toml
{
    [dependencies]
    serde = "1.0.2"      # Uses version "1.0.2"
    serde_derive = "1.0" # Uses version "1.0.X"
    heck = "*"           # Uses version "X.X.X"
}
```
Versioning has a multitude of ways to deal with complex dependency resolution. More information about Cargo’s semantic versioning can be found in the The Cargo Book

When adding dependencies in this manner, Cargo will automatically download the source code for these libraries from crates.io and utilize them when compiling our crate.

#### Remote Dependencies
We can also pull dependencies directly from a git repository with the git field:
```toml
{
    serde = { git = "https://github.com/serde-rs/serde" }
    
    # A specific branch
    serde = { git = "https://github.com/serde-rs/serde", branch = "next" }
    
    # A specific commit hash
    serde = { git = "https://github.com/serde-rs/serde", rev = "7e19ae8c9486a3bbbe51f1befb05edee94c454f9" }
}
```

#### Local Dependencies
Pulling dependencies from our local filesystem is possible with the path field:
```toml
{
    my_library = { path = "./../my_library" }
}
```

#### Features
Some crates have parts of their library behind a feature gate. We can enable specific features with the features field.
```toml
{
    # The `features` field always takes an array of values
    uuid = { version = "0.8", features = ["serde", "v4"] }
    
    # We can disable features that are added by default.
    wee_alloc = { version = "0.4.5", default_features = false }
}
```

## Variable Naming
===
### CamelCase = Types & Traits
```rust
{
    struct UnitStruct;
    
    struct TupleStruct(T); // ... with generic type T
    
    struct StructName {
        field: NamedTuple,
    }
    
    enum EnumName {
        VariantName,
    }
    
    type TypeAlias = u8;
    
    trait TraitName {}
}
```

### snake_case = attributes, variables, functions, and macros
```rust
 {
    // Attributes
    #![attribute_name]
    
    // Variables
    let variable_name = true;
    
    // Functions
    fn function_name() {
        function_call();
    }
    
    // Macros
    macro_name!();
    macro_name![];
    macro_name! {};
}
```

### Screaming SNAKE_CASE
```rust
{
    const EIGHTY_EIGHT: u32 = 88;
}
```rust