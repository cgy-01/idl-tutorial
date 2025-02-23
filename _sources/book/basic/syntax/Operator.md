# Operator
## Mathematical Operators

|priority|Operators|
|-|-|
|1|`^` `++` `-`|
|2|`*` `/` `mod`|
|3|`+` `-` `<` `>`|
`<` and `>` find min and max of numbers.
```idl
3<1<7<2
3>1>7>2
1<3<2<5<2
3>(-1)
```
||Other Usual Operators||
|:-:|:-:|:-:|
|abs|sqrt|exp|
|alog|alog10|factorial|
|sin|cos|tan|
|asin|acos|atan|
## Relational Operators
`EQ` `NE` `GT` `LT` `GE` `LE`  
equality, not equal, greater than, less than, greater than or equal, less than or equal. 
## Logical Operators
`&&` `||` `~`  
AND, OR, NOT
## Bitwise Operators
`and` `or` `xor` `not`  
bitwise AND, bitwise OR, bitwise exclusive OR, bitwise NOT
## Conditional Expressions
`expr1?Expr2:expr3` if `expr1` is True, return `expr2`, otherwise return `expr3`.  
```idl
a = 3
b = 2
c = a gt b? a: b
print, c
```
## Operator Precedence
The table is priority of usual operators.
|priority|Operators|
|-|-|
|1|`()` `[]`|
|2|`*` (pointer) `^` `++` `-`|
|3|`*` `/` `MOD` `#` `##`|
|4|`+` `-` `<` `>` `NOT` `~`|
|5|`EQ` `NE` `LT` `GT` `LE` `GE`|
|6|`AND` `OR`|
|7|`&&` `\|\|` `~`|
|8|`?`|
|9|`=`|