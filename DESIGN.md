# Crow Language Design Document

## Overview

Crow is a statically-typed, functional programming language that combines the elegant syntax of Gleam with the performance characteristics of C. The language compiles directly to C code, which is then compiled to native machine code for maximum performance.

## Design Goals

### Primary Goals
- **Gleam-like Syntax**: Clean, functional syntax with pattern matching and immutable data structures
- **C-like Performance**: Compile to C for native performance with minimal runtime overhead
- **Type Safety**: Static type system with type inference to catch errors at compile time
- **Memory Safety**: Ownership-based memory management inspired by Rust
- **Interoperability**: Seamless integration with existing C libraries

### Secondary Goals
- **Developer Experience**: Fast compilation, clear error messages, and good tooling
- **Concurrency**: Lightweight threads with message passing
- **Modularity**: Clean module system for code organization
- **Simplicity**: Keep the language simple and learnable

## Language Syntax

### Core Syntax Features

```crow
// Variable declarations
let x = 42
let message = "Hello, world!"

// Function definitions
fn add(a: Int, b: Int) -> Int {
  a + b
}

// Public functions
pub fn multiply(a: Int, b: Int) -> Int {
  a * b
}

// Type definitions
type Person {
  name: String
  age: Int
}

// Algebraic data types
type Result(ok, error) {
  Ok(ok)
  Error(error)
}

// Pattern matching
fn greet(person: Person) -> String {
  case person {
    Person(name, age) if age >= 18 -> "Hello, " <> name
    Person(name, _) -> "Hi, " <> name
  }
}

// List operations
let numbers = [1, 2, 3, 4, 5]
let doubled = numbers |> map(fn(x) { x * 2 })

// Error handling
fn divide(a: Int, b: Int) -> Result(Int, String) {
  if b == 0 {
    Error("Division by zero")
  } else {
    Ok(a / b)
  }
}
```

### Key Syntax Characteristics
- **Immutable by default**: All variables are immutable unless explicitly marked as mutable
- **Expression-based**: Everything is an expression, including control flow
- **Pattern matching**: Comprehensive pattern matching on all data types
- **Pipe operator**: `|>` for function composition
- **Type inference**: Types are inferred when possible, explicit when needed
- **Module system**: Clean import/export system

## Compiler Architecture

### High-Level Architecture

```
Source Code (.crow)
    ↓
[Lexer] → Tokens
    ↓
[Parser] → AST
    ↓
[Type Checker] → Typed AST
    ↓
[Code Generator] → C Code (.c)
    ↓
[C Compiler] → Native Binary
```

### Compilation Phases

#### 1. Lexical Analysis
- **Input**: Source code string
- **Output**: Token stream
- **Responsibilities**:
  - Tokenize keywords, identifiers, literals, operators
  - Handle comments and whitespace
  - Report lexical errors

#### 2. Parsing
- **Input**: Token stream
- **Output**: Abstract Syntax Tree (AST)
- **Responsibilities**:
  - Parse expressions, statements, and declarations
  - Build hierarchical AST representation
  - Report syntax errors

#### 3. Type Checking
- **Input**: AST
- **Output**: Typed AST with type annotations
- **Responsibilities**:
  - Type inference and checking
  - Pattern matching exhaustiveness checking
  - Module resolution and dependency analysis
  - Report type errors

#### 4. Code Generation
- **Input**: Typed AST
- **Output**: C source code
- **Responsibilities**:
  - Generate efficient C code
  - Handle memory management
  - Optimize function calls and data structures
  - Generate runtime support code

## Type System

### Core Types

#### Primitive Types
- `Int`: 64-bit signed integer
- `Float`: 64-bit floating point
- `String`: UTF-8 encoded string
- `Bool`: Boolean (`true` or `false`)
- `Unit`: Empty type (similar to `()` in Rust)

#### Composite Types
- **Tuples**: `(Int, String)` - Fixed-size heterogeneous collections
- **Lists**: `[Int]` - Dynamic homogeneous collections
- **Records**: `{name: String, age: Int}` - Named field structures
- **Variants**: `Result(Int, String)` - Sum types with data
- **Functions**: `(Int, Int) -> Int` - Function types

#### Advanced Types
- **Option**: `Option(Int)` - Nullable values
- **Result**: `Result(Int, String)` - Error handling
- **References**: `Ref(Int)` - Mutable references
- **Arrays**: `Array(Int)` - Fixed-size mutable arrays

### Type Inference

The type system uses Hindley-Milner type inference with extensions:

```crow
// Type inference examples
let x = 42                    // x: Int
let y = 3.14                  // y: Float
let z = "hello"               // z: String
let add = fn(a, b) { a + b }  // add: (Int, Int) -> Int
let pair = (x, y)             // pair: (Int, Float)
```

### Pattern Matching

Comprehensive pattern matching on all types:

```crow
fn process(value: Option(Int)) -> String {
  case value {
    Some(x) -> "Got: " <> int_to_string(x)
    None -> "Nothing"
  }
}
```

## Memory Management

### Ownership System

Crow uses an ownership-based memory management system inspired by Rust:

- **Ownership**: Each value has exactly one owner
- **Borrowing**: References can borrow values without taking ownership
- **Lifetimes**: Compile-time tracking of value lifetimes
- **Move semantics**: Values are moved rather than copied by default

### Memory Layout

- **Stack allocation**: Local variables and function parameters
- **Heap allocation**: Dynamic data structures (lists, strings, etc.)
- **Reference counting**: For shared ownership scenarios
- **Manual allocation**: For performance-critical code

### Garbage Collection

- **No GC by default**: Ownership system prevents most memory leaks
- **Optional RC**: Reference counting for shared data
- **Arena allocation**: For temporary data structures

## Concurrency Model

### Lightweight Threads

- **Green threads**: Mapped to OS threads by the runtime
- **Message passing**: Communication between threads via channels
- **No shared mutable state**: Prevents data races
- **Fault isolation**: Thread failures don't affect others

### Concurrency Primitives

```crow
// Spawn a new thread
let handle = spawn(fn() {
  // Thread body
})

// Message passing
let (sender, receiver) = channel()
sender.send(42)
let value = receiver.recv()

// Synchronization
let mutex = Mutex(shared_data)
let lock = mutex.lock()
```

## Standard Library

### Core Modules

- **io**: File and console I/O operations
- **collections**: Lists, maps, sets, and other data structures
- **concurrency**: Threading and synchronization primitives
- **math**: Mathematical functions and constants
- **string**: String manipulation and formatting
- **time**: Date and time operations
- **random**: Random number generation

### C Interoperability

- **FFI**: Foreign Function Interface for C library integration
- **Bindings**: Automatic generation of C bindings
- **Memory sharing**: Safe sharing of data with C code

## Implementation Strategy

### Phase 1: Core Language (MVP)
- [x] Basic syntax and lexer
- [x] Parser for expressions and statements
- [ ] Type checker with basic types
- [ ] C code generator for simple programs
- [ ] Basic standard library

### Phase 2: Advanced Features
- [ ] Pattern matching
- [ ] Algebraic data types
- [ ] Module system
- [ ] Error handling with Result types
- [ ] Memory management system

### Phase 3: Performance & Tooling
- [ ] Compiler optimizations
- [ ] Concurrency primitives
- [ ] Package manager
- [ ] Language server
- [ ] Debugger

### Phase 4: Ecosystem
- [ ] Standard library expansion
- [ ] C library bindings
- [ ] Documentation and tutorials
- [ ] Community tools and integrations

## File Structure

```
crow/
├── src/
│   ├── main.rs              # CLI entry point
│   ├── lexer.rs             # Lexical analysis
│   ├── parser.rs            # Syntax analysis
│   ├── ast.rs               # Abstract syntax tree
│   ├── type_checker.rs      # Type checking
│   ├── codegen.rs           # C code generation
│   ├── runtime/             # Runtime support
│   └── stdlib/              # Standard library
├── examples/                # Example programs
├── tests/                   # Test suite
├── docs/                    # Documentation
├── Cargo.toml              # Rust dependencies
└── DESIGN.md               # This document
```

## Performance Targets

### Compilation Speed
- **Goal**: Compile 10,000 lines in < 1 second
- **Strategy**: Incremental compilation, parallel processing

### Runtime Performance
- **Goal**: Within 10% of equivalent C code
- **Strategy**: Direct C compilation, minimal runtime overhead

### Memory Usage
- **Goal**: Minimal heap allocation, stack-based where possible
- **Strategy**: Ownership system, arena allocation

## Error Handling

### Compile-time Errors
- **Syntax errors**: Clear error messages with source location
- **Type errors**: Detailed type mismatch information
- **Pattern matching**: Exhaustiveness checking

### Runtime Errors
- **Panic**: For unrecoverable errors (like Rust)
- **Result types**: For recoverable errors
- **Stack traces**: Detailed error information

## Future Considerations

### Language Extensions
- **Generics**: Parametric polymorphism
- **Traits**: Interface-like behavior
- **Macros**: Compile-time code generation
- **Async/await**: Asynchronous programming

### Tooling
- **IDE support**: Full language server implementation
- **Profiler**: Performance analysis tools
- **Formatter**: Code formatting tool
- **Linter**: Static analysis and style checking

### Platform Support
- **Cross-compilation**: Support for multiple target architectures
- **Embedded**: Support for embedded systems
- **WebAssembly**: Compilation to WASM for web deployment

---

This design document serves as the foundation for the Crow language implementation. It will be updated as the language evolves and new requirements emerge.
