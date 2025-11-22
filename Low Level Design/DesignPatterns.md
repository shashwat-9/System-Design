# Design Patterns

### Prototype Design Pattern

Prototype Design Pattern is a creational design pattern used when the type of object to create is determined by 
prototypical instance, which is cloned to produce new objects.

It is basically, when a class has a method to create an instance of the same type, with the required state
changes in it.

### Iterator Design Pattern

An Iterator in Java is an object used to traverse and access elements sequentially in a collection.

Two primary functions are :
1. next() -> Iterator at next element
2. hasNext() -> Is there a next element

The Iterator Design Pattern decouples the logic of iteration from the structure of the collection, making the code more 
modular and reusable.

### Builder Design Pattern
The use case typically arises when the number of fields are large, and which one is going to be set during construction
is unsure. This will lead to a huge number of parametrized constructor, with different parameters.

This design patten lets one set the field on the go, and then build at the end to create an object.

We create a class called `ClassNameBuilder` wherein all the setters are placed along with the build method. The build 
method calls the constructor with the set parameters and thus returns an object of the actual class. Each set method
returns the object of the same `ClassNameBuilder` type.

Each setter returns the object of the same type, thus calling another setter on the returned object.
Then at the end, the build method is called, which creates an object with the set fields.

Adding a new field in the Class will not lead to a increase in the number of constructors, but