# bzfile
File IO Library for Battlezone 98 Redux

Quick and dirty tutorial:

```lua
-- Make sure to use require fix or manually set your package.cpath in order for the game
-- to load dlls
local bzfile = require("bzfile")

local filePath = bzfile.GetWorkingDirectory() .. "\\addon\\myfile.txt"

local file = bzfile.Open(filePath, "w", "app")

file:Writeln("Hello world!")

-- Optionally if you want the text to appear right away...
-- file:Flush()

-- Once you are done you can optionally close the file manually:
file:Close()

local readFile = bzfile.Open(filePath, "r")

-- Read line by line
local contents = file:Readln()
while contents ~= nil do
    print(contents)
    contents = file:Readln()
end

-- Or dump the whole contents of the file into a lua stirng:
local bigString = file:Dump()
```

Specification:

```lua
bzfile.Open(filePath: string, openMode: string?, writeParameter: string?) -> handle
```
Opens a handle to a file, creating a new one if write mode is specified and the file does not exist.

Valid options for open mode are:
- "r": read mode
- "w": write mode

Valid options for write parameters are:
- "app": append to file
- "trunc": truncate file (clears the file before writing)

Default options if only path is specified is "r" (read). In "w" (write) mode the default option is "app" (append).

### File Methods:

```lua
file:Write(content: string) -> &handle (reference to the handle to allow method chaining)
```
Unformatted write with no newline.

```lua
file:Writeln(content: string) -> &handle
```
Write with newline.

```lua
file:Read(count: int?) -> content: string
```
Unformatted read of count characters, defaults to 1 if not specified.

```lua
file:Readln() -> content: string
```
Reads a line.

```lua
file:Dump() -> content: string
```
Dumps the entire contents of the file into one string

```lua
file:Flush() -> &handle
```
Flushes the input/output buffer, makes text appear immediately in the file, may affect performance if called frequently.

```lua
file:Close() -> nil
```
Closes the handle to the file, file object becomes nil.

### Filesystem Functions

```lua
bzfile.GetWorkingDirectory() -> path: string
```
Gets the root directory of the game (..\common\Battlezone98Redux\).

```lua
bzfile.GetWorkshopDirectory() -> path:string
```
Gets the workshop directory (..\content\301650).

```lua
bzfile.MakeDirectory(path: string) -> nil
```
Makes a new directory at the given path.
