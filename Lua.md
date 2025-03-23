# 1. Getting Started
## Chunks
We call each piece of code that Lua executes, such as a file or a single line in interactive mode, a chunk. A chunk is simply a sequence of commands (or statements).

To run lua in interactive mode, just type `lua` in your terminal. To exit the interactive mode, call `os.exit()` or type end-of-file control character (ctrl-D in posix, ctrl-Z in windows).

You can use `-i` option to instruct lua to start an interactive session after running a given chunk.
```bash
lua -i main.lua
```

Another way to run a chunk is with function `dofile(<file-name>)`, which immediately executes a file. Thus in interactive mode you can call `dofile()` to run another chunk.

## Some Lexical Conventions
Identifiers (or names) in Lua can be any string of letters, digits, and underscores, not beginning with a digit. Lua is case-sensitive.

> [!note]
> You should avoid identifiers starting with an underscore followed by one or more upper-case letters (e.g., _VERSION); they are reserved for special uses in Lua

> [!note]
> These are the reserved keywords.
> and end if or until break false in repeat while do for local return else function nil then elseif goto not true 

Multiline comments start with `--[[` and ends with `]]`. You can use the pattern given below to easily toggle multiline comments.
```lua
---[[
print("something")
print("another thing")
--]]
```

Lua needs no separator between consecutive statements, but we can use a semicolon if we wish. Line breaks play no role in Lua's syntax.

## Global Variables
Global variables do not need declarations, we simply use them. The non-initialized value is `nil`.

Lua does not differentiate a non-initialized variable from one that we assigned `nil`. After the assignment, Lua can eventually reclaim the memory used by the variable.

## Types and Values
Lua is a dynamically-typed language. There are no type definitions in the language; each value carries its own type.

There are eight basic types in Lua: nil, Boolean, number, string, userdata, function, thread, and table. You can get the type name of any given values by passing it to the `type()` function. The userdata type allows arbitrary C data to be stored in Lua variables.

Variables have no predefined types; any variable can contain values of any type. This means, we can use a single variable for multiple types.

### Nil
We can assign nil to a global variable to delete it.

### Booleans
Conditional tests (e.g., conditions in control structures) consider both the Boolean `false` and `nil` as false and anything else as true.
Lua has logical operators - `and`, `or`, `not`. Both `and` and `or` use short-circuit evaluation. `and` has higher precedence than `or`.

```lua
x = x or v -- it sets x to a default value v when x is not set (provided that x is not set to false)
((a and b) or c) -- It is equivalent to the C expression a ? b : c, provided that b is not false. To find the maximum - (x > y) and x or y
```

## The Stand-Alone Interpretor
When the interpreter loads a file, it ignores its first line if this line starts with a hash (`#`). This feature allows the use of Lua as a script interpreter in POSIX systems.
```lua
#!/usr/bin/lua 

print("running as a script")
```

The usage of lua is 
```bash
lua [options] [script [args]]
```

A script can retrieve its arguments through the predefined global variable `arg` . The interpreter creates the table `arg` with all the command-line arguments. For example, the following command,
```bash
lua -e "sin=math.sin" script a b
```
will result in the following table.
```lua
arg[-3] = "lua"
arg[-2] = "-e"
arg[-1] = "sin=math.sin"
arg[0] = "script"
arg[1] = "a"
arg[2] = "b"
```

#notunderstood
Before running its arguments, the interpreter looks for an environment variable named LUA_INIT_5_3 or else, if there is no such variable, LUA_INIT. If there is one of these variables and its content is @file- name, then the interpreter runs the given file. If LUA_INIT_5_3 (or LUA_INIT) is defined but it does not start with an at-sign, then the interpreter assumes that it contains Lua code and runs it. LUA_INIT gives us great power when configuring the stand-alone interpreter, because we have the full power of Lua in the configuration. We can preload packages, change the path, define our own functions, rename or delete functions, and so on.

# 2. Interlude - Eight Queen Problem
```lua
N = 8
function isPlaceOk(a, n, c)
	for i = 1, n - 1 do
		if (a[i] == c) or (a[i] - i == c - n) or (a[i] + i == c + n) then
			return false
		end
	end
	return true
end

function printSolution(a)
	for i = 1, N do
		for j = 1, N do
			io.write(a[i] == j and "X" or "-", " ")
		end
		io.write("\n")
	end
	io.write("\n")
end

function addQueen(a, n)
	if n > N then
		printSolution(a)
	else
		for c = 1, N do
			if isPlaceOk(a, n, c) then
				a[n] = c
				addQueen(a, n + 1)
			end
		end
	end
end

addQueen({}, 1)
```
