# Style Guide
This guide is adopted from [Roblox's Lua Style Guide](https://roblox.github.io/lua-style-guide/).

## Principles
- Code should be optimized for reading, not writing.
	- You write your code once. Multiple people may need to read your code, even you.

## File Structure
- Services used by the file, using `GetService`.
- Module imports, using `require`.
- Global variables
- Program

## Require
- All `require` calls should be at the beginning of a file, maintaining [file structure]() accordingly.
