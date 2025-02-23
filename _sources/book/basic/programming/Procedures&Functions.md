# Procedures & Functions
## Procedures
A procedure is a collection of idl instructions. According to IDL official documentation:

> A sequence of one or more IDL statements can be given a name, compiled, and saved for future use with the procedure definition statement.

Here are some examples.

```idl
Pro test1
    deg = 180
    radian = deg * !dtor
    print, radian
end
```
Use the command line to calling the procedure.
```idl
IDL> test1
```
You can also add some parameters to the procedure.
```idl
pro test2, deg
    radian = deg * !dtor
    print, radian
end
```
Parameter values ​​need to be passed in when calling from the command line.
```idl
IDL> test2, 180
```

## Functions
The biggest difference between procedures and functions is that functions must return values, while procedures need not. The return value of a function can be assigned to a variable.
```idl
function test3, deg
    radian = deg * !dtor
    return, radian
end
```
Use `()` to pass in parameters.
```idl
IDL> result = test3(180)
IDL> help, result
```

## Calling
IDL procedures and functions can call each other, using parameters and keywords to pass data.
```idl
pro test4
    x = 2 & y = 3 & z = 4
    volume = cal_v(x, y, z)
    print, 'Volume:', volume
end

function cal_v, x, y, z
    return, x * y * z
end
```