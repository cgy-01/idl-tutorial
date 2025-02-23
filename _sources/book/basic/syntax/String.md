# String
## Create String
Use `''` or `""` to tell IDL it's a string.
```idl
a = "Hello RS"
a = 'Hello RS'
a = "say 'Hello' RS"
a = 'say "Hello" to RS'
```
## Link Strings
### +
### strjoin
### array.join

## String Functions
### strlen
### strlowcase. strupcase
### strcompress, strtrim
### strcmp
### strpos
### strmid
### strsplit
### insert, remove, replace
## Type Conversion
To convert other types to string, use `string()` `var.toString()`.  
To convert string to other types, use `fix()` `long()` `float()`.  

## String Reading
`reads` reads data from a string variable according to the specified format.
```idl
str = "1 2 3 4"
reads, str, a1, a2, a3, a4
print, a1, a2, a3, a4
```
```{warning}
By default, `reads` outputs data as a float number. To output in other type, you need to create the variable in advance."
```
```idl
data "49.17127 123.06716 7/2/2013 16:44:56"
Lat = 0.0 & Lon = 0.0 & Date = '' & Time = ''
reads, data Lat, Lon, Date, Time, format='(f9.5, f10.5, a9, a8)'
help, Lat, Lon, Date, Time
```