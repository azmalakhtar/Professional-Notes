# I. Welcome
## 1. Introduction
## 2. A Map of the Territory
### 2.1 The Parts of a Language
#### 2.1.1 Scanning
Converts the source code (a stream of characters) into a list of tokens. Also known as lexing or lexical analysis.

#### 2.1.2 Parsing
The list of tokens is given **grammar**. It is turned into a syntax tree.
A parser takes the flat sequence of tokens and builds a tree structure that mirrors the nested nature of the grammar

#### 2.1.3 Static Analysis
Determining what is the scope and values of identifiers. If the language is statically typed, then we can also type check.

Upto this point, it is the **front-end** of the implementation.

#### 2.1.4 Intermediate Representations
In the middle, the code may be stored in some intermediate representation (IR) that isn’t tightly tied to either the source or destination forms (hence “intermediate”). Instead, the IR acts as an interface between these two languages. This makes supporting different languages and architecture much less tedious.

#### 2.1.5 Optimization
This was **middle-end**.

#### 2.1.6 Code Generation
We can either generate instructions for a real CPU or a virtual one (virtual machine aka bytecode).
#### 2.1.7 Virtual Machines
#### 2.1.8 Runtime


### 2.2 Shortcuts and Alternate Routes
#### 2.2.1 Single-pass compilers
Some simple compilers interleave parsing, analysis, and code generation so that they produce output code directly in the parser, without ever allocating any syntax trees or other IRs. That means as soon as you see some expression, you need to know enough to correctly compile it.
C & Pascal were designed around this limitation.

#### 2.2.2 Tree-walk interpreters
Some programming languages begin executing code right after parsing it to an AST (with maybe a bit of static analysis applied). To run the program, the interpreter traverses the syntax tree one branch and leaf at a time, evaluating each node as it goes.

#### 2.2.3 Transpiler
You write a front end for your language. Then, in the back end, instead of doing all the work to lower the semantics to some primitive target language, you produce a string of valid source code for some other language that’s about as high level as yours.

#### 2.2.4 Just-in-time compilation
On the end user’s machine, when the program is loaded—either from source in the case of JS, or platform-independent bytecode for the JVM and CLR—you compile it to native code for the architecture their computer supports.

### 2.3 Compilers and Interpreters
- Compiling is an implementation technique that involves translating a source language to some other—usually lower-level—form.
- When we say a language implementation “is a compiler”, we mean it translates source code to some other form but doesn’t execute it. The user has to take the resulting output and run it themselves.
- Conversely, when we say an implementation “is an interpreter”, we mean it takes in source code and executes it immediately. It runs programs “from source”.

> A language implementation can be a compiler, an interpreter, or a mix of both.

# II. A TREE-WALK INTERPRETER
# III. A BYTECODE VIRTUAL MACHINE

---
### Resources
- [Crafting Interpreters](http://craftinginterpreters.com/)