# Drafts of my programming language

It's designed for the maximum simplicity, with very little reserved keywords.
CPU time is valued over memory usage.

It's aimed at user space software using math and concurrency: GUIs, 3D graphics, simulations, machine learning.

Heavily inspired by:
- Go
- Odin
- HolyC
- C
- Zig

Draft layout based on https://odin-lang.org/docs/overview/.

## Comments

```
# A comment
```

## String and character literals

Both string literals and character literals are enclosed in single quotes.
Special characters are escaped with a backslash `\`.

```
'This is a string'
'A'
'\n'
'C:\\Windows\\notepad.exe'
```

Raw string literals are enclosed in a single back ticks.

```
`C:\Windows\notepad.exe`
```

The length of a string can be found using the built-in `len` function:

```
len('A string')
len(other_string)
```

## Numbers

Underscores are allowed for better readability: `1_000_000_000` (one billion).
A number that contains a dot is a floating-point literal: `1.0e9` (one billion).

Binary literals are prefixed with `0b`, and hexadecimal literals with `0x`.

## Variable declarations

```
x: int
y, z: int

# Volatile
addr: [8]byte = 0x3CF0F00
reg: int = raw(addr)
```

Variables are initialized to zero by default unless specified otherwise.

```
x := 10
y, z := 20, 30
```

## Basic types

```
any

# true or false
bool

# 64-bit signed integer
int

# 32-bit floating-point
float

# 32-bit unsigned integer, used to represent Unicode code points
rune

# 8-bit unsigned integer, used to represent binary data and ASCII
byte
```

### Zero values
- `0` for numeric and rune types
- `false` for boolean types
- `nil` for pointers

## Packages

Programs consist of packages. A package is a directory of code files, all of which have the same package declaration at the top.
Execution starts in the package's `main` procedure.

### `import` statement

The following program imports the `fmt`, `os`, `net` and `time` packages from the `core` library collection.

```
package main

import 'core:fmt,os,net'
import 'core:time'

fn main() {
}
```

### Exported names

All identifiers in a package are exported.
Prefixing identifier with a single `_` (underscore) is a convetion for marking declarations private.

### Organizing packages

Packages may be thematically organized by placing them in subdirectories of another package.
For example: `core:image/png`.

## Control flow statements

### `for` statement

```
for i := 0; i < 10; ++i {
  fmt.println(i)
}

i := 0
for i < 10 {
  i += 1
}

for {
}
```

### `if` statement

```
if x >= 0 {
  fmt.println('x is positive')
}

if x := foo(); x < 0 {
  fmt.println('x is negative')
}
```

### `defer` statement

```
fn main() {
  x := 123
  defer fmt.println(x)
  x = 234
}
```

## Bitwise operations

Bitwise operations can be applied to `byte` and `[n]byte` types.

```
fn main() {
  b := [2]byte{0xFF, 0xA8}

  b <<= 2
  b >>= 2
  b = b | (b << 1)

  b[0] = !b[0]
  b[1] |= 7

  sb: byte = 0b00101011
  sb &= 63 
}
```

## Functions

```
fn multiply(a, b int) -> int {
  return a * b
}

asm fn multiply(a, b int) -> int {
  mul  a0,a0,a1
  ret
}
```
