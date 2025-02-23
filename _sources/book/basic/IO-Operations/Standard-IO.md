# Standard I/O
## Data Input
The procedure `read` passes values from the keyboard to your variables.  
If the type is not defined, the default is floating point.  
```idl
read, a

read, a, prompt='Input value a:'
```

## Data output
The procedure `print` outputs data to the command line.
```idl
print, 'Hello RS'

print, 12B

print, 12, 12.3

print, [1:5]
```

