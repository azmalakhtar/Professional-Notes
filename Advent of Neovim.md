## 1. Why neovim?
You can use `:messages` to see all the messages.
To configure neovim without exiting from `init.lua`, type `:source %`
To run specified line of lua, visually select the lines and then type `:` then `lua`

## 2. Lua
Lua is elegant 
- Lua uses "Mechanisms over policies"

### Comments
```lua
-- this is my first single line comment
--[[ this is my
first multiline
comment
--]]
```

### Variables
```lua
local age = 20

local string = "hello, world"
local sing = 'hell-lo'
local crazy = [[ this is
a multiline
string]]

local nothing = nil

print(string)
print(sing)
print(crazy)
print(age)
print(nothing)

```

### Functions
```lua
local function greet(name) 
	print("hello", name)
end

local specialGreeting = function(name)
	print("hey big boy", name)
end

local adder = function(value)
	return function(another) 
		return another + value
	end
end

greet("azmal")
specialGreeting("azmal")
local addSeven = adder(7)
print(addSeven(12))
print(addSeven(3))
```

### Tables
```lua
-- as a list
local list = { "first", 2, false, function() print("fourth") end }
print(list[1]) -- one-indexed
print(list[4])
print(list[5]) -- nil

-- as a map
local fn = function() end

local t = {
	age = 21,
	["enrolment number"] = "GK9022",
	[fn] = true
}

print(t.age)
print(t["enrolment number"])
print(t[fn])

```

### Control flow 
```lua
-- for
local list = {2, 3, 5, 7, 11, 13, 17}
local map = { tj_devries = 9.5, theprimeagen = 0 }

for i = 1, 10 do 
	print(i)
end

for i = 1, #list do 
	print(i, list[i])
end

for i, val in ipairs(list) do
	print(i, val)
end

for i, val in pairs(map) do
	print(i, val)
end
```

```lua
-- if
local function action(isAdult) 
	if isAdult then
		print("you can drive")
	else
		print("you can't drive")
	end
end

-- only nil and false are "falsy" 
action()
action(nil)
action(false)

-- everything else is "truthy"
action(true)
action(0)
action({})

```

### Modules
Different files are modules and you can return things from files so that other files can import it.

### Calling
You can pass literal strings and literal tables without using parentheses.
```lua
local setup = function(opts) 
	if opts.default == nil then
		opts.default = 17
	end
	print(opts.default, opts.other)
end

setup { default = 12, other = "gruvbox" }
setup { other = "onedark" }

```

## 4. Package Manager 
You can see lazy installed packages in `~/.local/share/nvim/`


## 6. Treesitter
It serves two main functions:
1. It makes it easy to write parsers.
2. Its parsers are incremental.

## 7. LSP
LSP is a way to communicate between editors and an external program about the state of the editor and the state of the code.
You need to tell the lsp where are the external program are, when to load them and any config.