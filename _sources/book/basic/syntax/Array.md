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

## Array Operations
