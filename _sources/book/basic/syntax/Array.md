# Array
## Create Array
Arrays can be created immediately or by functions.  
Create with `[]`
```idl
arr = [1, 2, 3, 4]
print, arr

; [start:end:step]
arr1 = [1:8]
arr2 = [1:8:2]
arr3 = [2:-6]

; Multidimensional Array
arr = [[1,2,3],[4,5,6]]

; Array Concatenation
a = [1,2,3]
b = [4,5,6]
arr1 = [a,b]
arr2 = [[a],[b]]
```
Create with functions
```idl
arr1 = intarr(6)
arr2 = indgen(6)
arr3 = indgen(2, 2)
arr4 = replicate(3.2, 2, 3)
```
Use `make_array()` to create arrays more flexibly, click [here](https://www.nv5geospatialsoftware.com/docs/MAKE_ARRAY.html) to read more.
```idl
arr1 = make_array(3, 2, /byte)
arr2 = make_array(3, 2, /byte, /index)
arr3 = make_array(3, 2, value=12L)

sz = [2, 4, 5, 1, 20]
arr4 = make_array(size=sz)
```
## Array Index
IDL array index starts from 0, and supports negetive index.
```idl
arr = indgen(6)
arr[2]
arr[2:4]
arr[*]
arr[3:*]

index = [1,3,5]
arr[index]

i = 2
arr[i:i+2]

arr[-1]
arr[-4:-1]
```

## Array Functions
### n_elements
Function `n_elements()` is used to count the number of all elements, returning 0 if the array isnot defined.
```idl
arr = findgen(3, 4)
print, n_elements(arr)

; same as array.length
print, arr.length
```
### size
`size` return a long type array,including dimensions, size, elemant type.
```idl
arr = fltarr(10, 20)
print, size(arr)
```
The first output number is dimension, the next few numbers is size of each dimension, the last two are element_type and n_elements.  
You can also use `array.ndim`, `array.dim`, `array.typrCode`, `typeName` to get the same attributes.

### max, min
`max` and `min` return the max value and the min value.
```idl
arr = [1, 3, 4.2, 6, -2.3, 3.2]
print, max(arr), min(arr)
```

### mean, median, variance, stddev
### total
### skewness, kurtosis, moment
### where
### reform, transpose, sort, reverse, shift
### uniq

## Array Operations

The operating principle of IDL array is the calculation between corresponding elements of the array.
```idl
a = [1, 1, 2, 2]
b = [2, 3, 4, 5]
print, a+b
print, a*b
```
Array and variable operations still return an array.
```idl
a = 2
b = [5, 6, 7, 8]
print, a*b, a+b
```

Assigning a variable to an array results in all elements of the subarray being assigned the variable value.
```{warning}
Assignment operation will not change the type of the array.
```
```idl
arr = [1, 1, 2, 2]
arr[0:3] = 3.1
print, arr
```
If you want to culculate arrays like matrixs, use `#` and `##`.  
Use the `#` operation to multiply the **columns** of the first array by the **rows** of the second array.  
Use the `##` operation to multiply the **rows** of the first array by the **columns** of the second array.  
```{warning}
The two arrays must be matched; otherwise, an error will occur."
```