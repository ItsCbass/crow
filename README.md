# Crow 🌙

*An amalgomation of Gleam and C*

## What's the Big Idea?

Imagine you're writing code that feels as clean and expressive as Gleam (gorgeous, beautiful, perfect), but it runs as fast as C. No virtual machines, no garbage collection overhead, just pure speed.

Crow idealy should compile directly to C code, which then gets compiled to native machine code. Something like zig.

## Why Crow?

I love Gleam. The syntax is clean, pattern matching is a dream, and the type system just makes sense. Take that beautiful syntax and couple it with raw performance that only comes from being close to the metal. That's where Crow comes in.

Think of it as Gleam's performance-focused cousin who decided to skip the BEAM and go straight to the hardware. We keep all the good stuff, the functional programming, the immutability, the pattern matching - but we make it ⚡ fast.

## The Architecture (The Fun Part)

### The Compilation Pipeline

Here's how your Crow code becomes lightning-fast machine code:

```
Your beautiful Crow code
    ↓
[Lexer] "Hey, what are these words?"
    ↓
[Parser] "Oh, I see what you're trying to do!"
    ↓
[Type Checker] "Wait, that doesn't make sense..."
    ↓
[C Code Generator] "Let me translate this to C"
    ↓
[GCC/Clang] "Now THIS I can work with!"
    ↓
Native binary
```

### The Type System

Crow's type system is where the magic happens.

```crow
// Type inference that just works
let x = 42                    // x: Int (obviously)
let y = 3.14                  // y: Float (makes sense)
let add = fn(a, b) { a + b }  // add: (Int, Int) -> Int (clever!)

// Pattern matching!
case person {
  Person(name, age) if age >= 18 -> "Hello, " <> name
  Person(name, _) -> "Hi, " <> name
}
```

### Memory Management

Here's where we get a bit Rust-y. Crow idealy uses an ownership system that's inspired by Rust. No garbage collector, no memory leaks, just clean, predictable memory management.

The idea is simple - every value has exactly one owner. When that owner goes out of scope, the value gets cleaned up. Keeping it clean and beautifuly fast.

## The Roadmap

Right now, Crow is in its early stages. Here's what I'm working on:

### Phase 1: The Basics (Current)
- [ ] Lexer
- [ ] Parser
- [ ] Type checker
- [ ] C code generator
- [ ] Basic standard library

### Phase 2: The Good Stuff
- [ ] Pattern matching
- [ ] Algebraic data types
- [ ] Error handling with Result types
- [ ] Module system
- [ ] Concurrency primitives

### Phase 3: The Polish
- [ ] Compiler optimizations
- [ ] Language server
- [ ] Package manager
- [ ] Debugger
- [ ] Profiler

## The Vision

My dream is to create a language that feels as natural as Gleam but performs like C. A language where you can write functional code without sacrificing performance, where pattern matching is first-class, and where the type system helps you instead of getting in your way.

Crow is about bringing together the best ideas from the functional programming world with the raw power of systems programming. It's about making fast code that's also beautiful.

## License

MIT License - because good ideas should be shared.