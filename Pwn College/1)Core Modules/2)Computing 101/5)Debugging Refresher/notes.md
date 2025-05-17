`si` is shorthand for `step instruction` . if a program is at such a
state where a function is being called, it would have two 
options . One is to remain in the current frame (which can be 
done using `ni` which is short for `next instruction` ) or it can go 
into that function frame using `si`.

`finish` will complete that function till the end.
`info break` will give you info about the breakpoints.Then you can
use `del break (num)` to delete a certain break point.

![[Pasted image 20250219125225.png]]
gdb scripts can be made like this with the same gdb commands.

![[Pasted image 20250224173623.png]]
This is the solution for level-6