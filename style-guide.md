# Style Guide
This guide is adopted from [Roblox's Lua Style Guide](https://roblox.github.io/lua-style-guide/).

## Principles
- Code should be optimized for reading, not writing.
	- You write your code once. Multiple people may need to read your code, even you.
- Tab sizing should generally be small, generally only 2.
	- You should not use spaces for indentation, and instead use tabs.
- Write using [pascal case](https://www.theserverside.com/definition/Pascal-case).
	- All statements should be wrote using pascal case, regardless of use.

## File Structure
- Services used by the file, using `GetService`.
- Module imports, using `require`.
- File paths, usually supporting already declared services
- Global variables
- Program

## Require
- All `require` calls should be at the beginning of a file, maintaining [file structure](#file-structure).

```lua
local HttpService = game:GetService('HttpService')
local RunService = game:GetService('RunService')

local Players = game:GetService('Players')
local ReplicatedStorage = game:GetService('ReplicatedStorage')

local Shared = ReplicatedStorage.Shared
local Services = ReplicatedStorage.Services

local Module1 = require(Shared.Module1)
local Module2 = require(Services.Module2)
```

## Metatables & Object Orientated Programming (OOP)
Metatables are generally used for OOP, but can also do much more, such as guarding against typos.

### Classes
First, we create an empty table, generally called the file name. Next, we assign our metamethods, we go ahead and assign `__index` on the class - this is a trick that allows us to use the class's table as the metatable for instances as well.

```lua
local Class = {}
Class.__index = Class
```

In most cases, we should now create a default constructor of our class, generally called `New`.

Methods that don't operate on instances of our class are usually defined using a dot (`.`), while methods that do operate on instances on our class should use a colon (`:`).

```lua
function Class.New()
	local self = {}
	self.Phrases = {
		'foo',
		'bar',
		'foobar'
	}

	return setmetatable(self, Class)
end
```

We can now define methods that operate on instances of our class. You should use a colon (`:`) to define these.

```lua
function Class:Say()
	local Phrase = self.Phrases[math.random(1, #self.Phrases)]

	return Phrase
end
```

Our class is now ready to use, go ahead and test it in a server script.

```lua
local ReplicatedStorage = game:GetService('ReplicatedStorage')

local Classes = ReplicatedStorage.Classes

local Class = require(Classes.Class)

local ClassInstance = Class.New()

print(ClassInstance.Phrases)
ClassInstance:Say()
```

### Typos
Indexing into a table gives you `nil` if the key isn't present, which can cause errors that are difficult to trace!

We are able to solve this problem by using the `__index` metamethod, an example is shown below.

```lua
local MyEnum = {
    A = "A",
    B = "B",
    C = "C",
}

setmetatable(MyEnum, {
    __index = function(self, key)
        warn(string.format('%s is not a valid member of MyEnum!', key))
    end,
})
```

## Punctuation
Don't use semicolons (`;`), they should only be used to seperate multiple statements on a single line, which you shouldn't be doing anyway.
