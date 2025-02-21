# Variables
## Basic Operations
IDL variables can be defined through assignment directly, and you can change the data type and dimension at any time.
```idl
a = 1.1
a = 4B
```
To check the attribute, use either `help` or `print`.
```idl
a = 1.1
help, a
arr = [1, 2, 3, 4]
help, arr
```
```idl
a = 1.1
print, a
```

## Scientific Notation
```idl
a = 6.63e-34
b = 2.998e8
```

## Variable Conversion Function
Variable conversion function is used to convert a variable from one data type to another.
```idl
a = 3B
a1 = fix(a)
b = 1.23
b1 = double(b)
```
Here are usual IDL variables data type and conversion functions.
|Data Type|Bytes|Create|Conversion Function|
|-|-|-|-|
|byte|1|0B|Byte()|
|int|2|0|Fix()|
|long|4|0L|Long()|
|long64|8|0LL|Long64()|
|uint|2|0U|Uint()|
|ulong|4|0UL|Ulong()|
|ulong64|8|0ULL|Ulong64()|
|float|4|0.0|Float()|
|double|8|0.0D|Double()|
|complex|8|Complex(0.0,0.0)|Complex()|
|dcomplex|16|Dcomplex(0.0,0.0)|Dcomplex()|
|string|0~32767|''or""|String()|
|pointer|4|Pter_New()|None|
|object|4|Obj_New()|None|

:::warning
If you try to convert a variable from a higher precision type to a lower precision type, truncation may occur if the variable exceeds the value range of the output variable.
:::

If you want to round a decimal, you can use three different functions: `round`, `floor`, and `ceil`.
```idl
arr = [1.1, 2.6, -1.1, -2.6]
print, round(arr)
print, floor(arr)
print, ceil(arr)
```
## Attributes and Methods of Variables

## Invalid Values and Infinity Values
