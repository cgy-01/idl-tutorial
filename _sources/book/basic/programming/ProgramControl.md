# Program Conntrol
If you have a foundation of in other language, such as C, Python and java, this part is very simple, with only a little difference in format.  
## Select Structure
### if 
```idl
if Expression then Statement1 [else Statement2]
```
```idl
if Expression then begin

    Statements1

endif [else begin

    Statements2

    endelse]
```
```idl
if Expression1 then begin

    Statements1

endif else if Expression2 then begin

    Statements2

endif else if Expression3 then begin

    Statements3

    ...
    ...

endif else begin

    Statementsn

endif
```
### case
```idl
case expression of

    expression: statement(s)
    
    ...
    
    expression: statement(s)

[ else: statement(s) ]

endcase
```
### switch
```idl
switch expression of

   expression: statement

   ...

   expression: statement

else: statement

endswitch
```
## Loop Structure
### for
```idl
for i = Start_n, End_n, [Step=1] do
```
```idl
for i = Start_n, End_n, [Step=1] do begin

    Statements

endfor
```
### while
```idl
while Expression do Statement
```
```idl
while Expression do begin
    
    Statements

endwhile
```

##  Continue & Break
Use `continue` to go into the next loop, skipping the following statements of this loop, and use `break` to jump out of the loop immediately.
