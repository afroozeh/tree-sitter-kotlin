# Kotlin Grammar for Tree-sitter

Gitar’s Kotlin grammar for Tree-sitter began as a fork of [tree-sitter-kotlin](https://github.com/fwcd/tree-sitter-kotlin). Our goal is to provide a Kotlin grammar tailored for **program analysis**, not just syntax highlighting. This requires correctness and adherence to the Kotlin compiler. Over time, our grammar has diverged so much from the fork that it effectively stands as a new grammar.  

- The original repo is based on the [Kotlin specification grammar](https://kotlinlang.org/docs/reference/grammar.html), which only loosely reflects the compiler’s actual grammar. Our grammar more closely matches the compiler.  
- We handle newline characters directly in the grammar rules to take advantage of full parser context. See our [blog post](https://gitar.ai/blog/parsing-kotlin) for details on newline handling in Kotlin.  
- Some lexer hacks are still needed for newline handling — for example, distinguishing newlines after binary operators (to avoid conflicts with operator precedence) and ignoring newlines inside brackets.  
- For program analysis, whitespace and comments often need to be properly attached to nodes. To support this, we’ve split some rules (such as `class_declaration`) to better preserve whitespace.  

## Setup

To run the grammar, install the [tree-sitter-cli](https://crates.io/crates/tree-sitter-cli). From the project root:  

- `tree-sitter generate` → Generates the `parser.c` file. (Note: the file is currently quite large — a known Tree-sitter issue. We’re working on ways to reduce the size.)  
- `tree-sitter test` → Runs all tests in the `test/corpus` directory.  
- `tree-sitter build --wasm` → Builds the WebAssembly (wasm) file, which you can use to run the local playground with `tree-sitter play`.  
