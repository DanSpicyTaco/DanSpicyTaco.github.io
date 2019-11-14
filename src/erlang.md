# Erlang
## Introduction

**Erlang**: A functional language with built in concurrency support
- Good for building large, distributed applications
- Not good for low-level system software

Advantages:
- Fast context switching
- Asynchronous message passing
- Built in concurrency

Disadvntages:
- Not as fast as lower level languages like C

### Erlang Shell

- Compile a file - c(file).
- Exit the shell - halt().

## Basics: Sequential Programming
### Data Types

- Everything is either a number, an atom or a variable
- Numbers: ints and floats are considered the same
- Different bases can be represented as B#val for base B
- Chars are numbers but can represent them as $char
- Atoms: a combination of #define's and strings - their value is their name
- Can represent numbers and atoms as _bit strings_ - `<<number>>`
- Variable: start with a capital letter & can only be bound once

### Operators

- +, -, *, /
- \>, >=, <, **=<**
- ==, =:=, =/=

### Data Structures

- Tuples {} - store fixed number of items
- Lists [] - store variable number of items

### Pattern Matching

- _Head/Tail Matching_: In [A|B], the first item is bound to A, the rest is bound to B (can be an empty string)  
> `[A,B|C] = [1,2,3,4,5,6,7]`  
> Binds A = 1, B = 2, C = [3,4,5,6,7]  

- _Generator_: `Variable <- List` - iterate through each element in the list as X
- _Filter_: conditional statement
- _List comprehensions_: `[Variable ||  Generator, Filter]` - create new list based on generators and filters
> `Foods = [{hotdog, dislike}, {beef, dislike}, {chicken, like}, {spaghetti, like}, {curry, like}].`  
> `Likes = [X || {X, like} <- Foods].`  
> Likes contains [chickem, spaghetti, curry]  

### Functions

- _Clause_: function statement - is scanned until match is found
> `factorial(0) -> 1;` 
> `factorial(N) -> N * factorial(N-1).` 
> Here, factorial(0) and factorial(N) are function clauses 

- _Anonymous Functions_: functions are defined like variables, so they can be used like variables
> `F = fun(X) -> X*2 end.`

## Concurrent programming

- Create new process that completes one function 
> `Pid = spawn(Mod, Func, Args)`

## Resources

Resource | Rating (/5)
--- | ---
[Official Docs](http://erlang.org/documentation/doc-6.4/doc/index.html) | 3
[Learn you some Erlang](https://learnyousomeerlang.com/content) | 5
[An Erlang Course](http://erlang.org/course/course.html) | 4
