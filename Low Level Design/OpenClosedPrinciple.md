### Open Closed Principle
Software Entities should be Open for extension and closed for modification.

Adding a wrapper over a class is not a good option to extend, because as the number of wrappers keeps on getting increasing, 
the inner core will be hard to change, as multiple functions will be calling the inner class methods.

Rather than hardcoding the logic in a function, we can create rules(or configs) and the function runs for each rule(or config).
This way the extensibility factor is with the configurations. The configurations may include Function or BiFunction object.

An example from the lessons: The various functions(object Functions and BiFunctions) are passed as a rule in the rule 
map and passed for checking in the rule engine. This way we can extend the functionality, without actually changing
the code logic.

### DRY - Don't Repeat Yourself
Every piece of knowledge must have a single unambiguous representation within a system.

In java, functions are there that can be treated like objects and methods on them like apply() could be used,
there are two ways to return the variable dynamically, Function<T, R>, BiFunction<T1, T2, F>

A good way to reduce the size of code is to extract out the common code and create a separate common function.

Many nested for loops is a bad pattern and is a code smell.

##### The Rubber Duck Debugging theory
This theory states that when a programmer needs to debug their code, they should explain the program line by line
to a rubber duck. Often, the solution will present itself by this act. :)

