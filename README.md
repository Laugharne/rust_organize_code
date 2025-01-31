# Modules in Rust: Organizing Your Code


> *Source:* [Modules in Rust: Organizing Your Code (Tutorial 24/100) | by Giorgio Martinez - Freedium](https://freedium.cfd/https://towardsdev.com/modules-in-rust-organizing-your-code-tutorial-24-100-f2f1c619d319)
> A good reminder about Rust, modules and code organization !



Rust is a systems programming language known for its performance, safety, and concurrency. One of the key features that make Rust stand out is its module system, which allows developers to organize their code in a structured and maintainable way. This article will delve into the concept of modules in Rust, explaining how to define, use, and organize them effectively. We'll also provide practical examples to illustrate these concepts.

## Introduction to Modules

In Rust, a module is a way to group related items together. These items can include functions, structs, enums, constants, and even other modules. Modules help in organizing code by providing a namespace, which prevents name collisions and makes the codebase easier to navigate.

## Defining a Module

To define a module, you use the `mod` keyword followed by the module name. Here's a simple example:

```rust
    mod math {
        pub fn add(a: i32, b: i32) -i32 {
            a + b
        }
        pub fn subtract(a: i32, b: i32) -i32 {
            a - b
        }
    }
```

In this example, we define a module named `math` that contains two public functions: `add` and `subtract`.

## Using a Module

To use a module, you need to bring it into scope. This can be done using the `use` keyword:

```rust
    use math::add;
    fn main() {
        let result = add(5, 3);
        println!("The result is: {}", result);
    }
```

Here, we bring the `add` function from the `math` module into scope and use it in the `main` function.

## Module Visibility

Rust has strict rules about visibility, which are enforced using the `pub` keyword. By default, all items in a module are private to that module. To make an item public, you need to prefix it with `pub`.

## Public vs. Private Items

```rust
    mod math {
        pub fn add(a: i32, b: i32) -i32 {
            a + b
        }

    fn subtract(a: i32, b: i32) -i32 {
            a - b
        }
    }
    fn main() {
        let result = math::add(5, 3);
        println!("The result is: {}", result);
        // This will cause a compile-time error
        // let result = math::subtract(5, 3);
    }
```

In this example, the `add` function is public and can be accessed from outside the `math` module, while the `subtract` function is private and cannot be accessed.

## Re-exporting Items

Sometimes, you may want to re-export items from a module to make them more accessible. This can be done using the `pub use` keyword:

```rust
    mod math {
        pub fn add(a: i32, b: i32) -i32 {
            a + b
        }

    pub fn subtract(a: i32, b: i32) -i32 {
            a - b
        }
    }
    pub use math::add;
    fn main() {
        let result = add(5, 3);
        println!("The result is: {}", result);
    }
```

Here, we re-export the `add` function from the `math` module, making it accessible directly from the root module.

## Organizing Modules

As your codebase grows, you may want to organize your modules into a hierarchical structure. Rust allows you to nest modules within other modules.

## Nested Modules

```rust
    mod math {
        pub mod arithmetic {
            pub fn add(a: i32, b: i32) -i32 {
                a + b
            }
        pub fn subtract(a: i32, b: i32) -i32 {
                    a - b
                }
            }
        pub mod geometry {
            pub fn area_of_circle(radius: f64) -f64 {
                std::f64::consts::PI * radius * radius
            }
        }
    }
    fn main() {
        let result = math::arithmetic::add(5, 3);
        println!("The result is: {}", result);
        let area = math::geometry::area_of_circle(5.0);
        println!("The area of the circle is: {}", area);
    }
```

In this example, we have a `math` module that contains two nested modules: `arithmetic` and `geometry`. Each nested module contains its own functions.

## Module Files

For larger projects, it's common to split modules into separate files. Rust supports this by allowing you to declare a module and then define its contents in a separate file.

### Directory Structure

```
    src/
    ├── main.rs
    ├── math.rs
    ├── math/
    │   ├── arithmetic.rs
    │   └── geometry.rs
```

### main.rs

```rust
    mod math;
    fn main() {
        let result = math::arithmetic::add(5, 3);
        println!("The result is: {}", result);
        let area = math::geometry::area_of_circle(5.0);
        println!("The area of the circle is: {}", area);
    }
```

### math.rs

```rust
    pub mod arithmetic;
    pub mod geometry;
```

### math/arithmetic.rs

```rust
    pub fn add(a: i32, b: i32) -i32 {
        a + b
    }
    pub fn subtract(a: i32, b: i32) -i32 {
        a - b
    }
```

### math/geometry.rs

```rust
    pub fn area_of_circle(radius: f64) -f64 {
        std::f64::consts::PI * radius * radius
    }
```

In this setup, the `math` module is declared in `math.rs`, and its nested modules `arithmetic` and `geometry` are defined in their respective files within the `math` directory.

## Advanced Module Features

Rust's module system offers several advanced features that can help you organize your code even more effectively.

## Super and Self

The `super` keyword refers to the parent module, while `self` refers to the current module. These keywords can be useful for accessing items in nested modules.

```rust
    mod math {
        pub mod arithmetic {
            pub fn add(a: i32, b: i32) -i32 {
                a + b
            }
        pub fn subtract(a: i32, b: i32) -i32 {
                a - b
            }
        }
        pub mod geometry {
            pub fn area_of_circle(radius: f64) -f64 {
                std::f64::consts::PI * radius * radius
            }
            pub fn area_of_square(side: f64) -f64 {
                side * side
            }
        }
    }
    fn main() {
        let result = math::arithmetic::add(5, 3);
        println!("The result is: {}", result);
        let area = math::geometry::area_of_circle(5.0);
        println!("The area of the circle is: {}", area);
    }
```

## Path Aliases

Path aliases allow you to create shorter, more convenient paths for frequently used modules or items.

```rust
    mod math {
        pub mod arithmetic {
            pub fn add(a: i32, b: i32) -i32 {
                a + b
            }
        pub fn subtract(a: i32, b: i32) -i32 {
                a - b
            }
        }
        pub mod geometry {
            pub fn area_of_circle(radius: f64) -f64 {
                std::f64::consts::PI * radius * radius
            }
            pub fn area_of_square(side: f64) -f64 {
                side * side
            }
        }
    }
    use math::arithmetic as arith;
    use math::geometry as geo;
    fn main() {
        let result = arith::add(5, 3);
        println!("The result is: {}", result);
        let area = geo::area_of_circle(5.0);
        println!("The area of the circle is: {}", area);
    }
```

In this example, we create path aliases for the `arithmetic` and `geometry` modules, making the code more concise.

## Crate-Level Modules

In Rust, a crate is the smallest unit of compilation and distribution. Crates can contain multiple modules, and you can define crate-level modules using the `mod` keyword at the top level of your crate.

```rust
    // src/lib.rs
    pub mod math {
        pub mod arithmetic {
            pub fn add(a: i32, b: i32) -i32 {
                a + b
            }
            pub fn subtract(a: i32, b: i32) -i32 {
                a - b
              }
            }
            pub mod geometry {
                pub fn area_of_circle(radius: f64) -f64 {
                    std::f64::consts::PI * radius * radius
                }
                pub fn area_of_square(side: f64) -f64 {
                    side * side
                }
            }
    }
```

In this example, we define a crate-level module `math` in the `lib.rs` file, which is the root module of the crate.

## Best Practices for Organizing Modules

Organizing modules effectively is crucial for maintaining a clean and understandable codebase. Here are some best practices:

1.  Keep Modules Small and Focused: Each module should have a single responsibility. This makes the code easier to understand and maintain.
2.  Use Descriptive Names: Module names should be descriptive and reflect their purpose. This helps other developers understand the structure of the codebase.
3.  Avoid Deep Nesting: While nested modules are useful, avoid deeply nesting modules as it can make the codebase harder to navigate.
4.  Use Path Aliases: Path aliases can make your code more readable and concise, especially when dealing with deeply nested modules.
5.  Document Your Modules: Use Rust's documentation comments (`///`) to document your modules and their contents. This helps other developers understand the purpose and usage of each module.

## Conclusion

Modules are a powerful feature in Rust that allow you to organize your code in a structured and maintainable way. By defining, using, and organizing modules effectively, you can create clean, understandable, and scalable codebases. Whether you're working on a small project or a large codebase, understanding and utilizing Rust's module system can greatly enhance your productivity and the quality of your code.

## References

1.  [The Rust Programming Language](https://doc.rust-lang.org/book/)
2.  [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
3.  [Rust Standard Library](https://doc.rust-lang.org/std/)

