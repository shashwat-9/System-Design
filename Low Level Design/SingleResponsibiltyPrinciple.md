# Single Responsibility Principle
 - This is the 'S' in the SOLID principle.
 - Over the years, across various projects, engineers have figured out that certain coding practice makes the work easy
and efficient.
 - This principle states that if we have a single piece of code, then it should have the responsibility of doing just one
thing, but doing it really well.
 - In unix programming, there's a similar concept, where we have small composable functions which put together can do many
things, but the single function does only one thing and does that thing really well.
 - In this programming, we figure out if a certain function belongs to the given class, or a certain logic belongs to the
given function.
 - In the lesson, there is a game engine class, where only two functions, namely start and move are present.
 - The game engine doesn't contain the function isComplete or suggestMove as there it seems some sort of intelligence involved.
 - suggestMove is in a separate class called AIMove, and another class RuleEngine class looks after the state of the board.

## Unit Tests
 - The Test classes should be named as ClassNameTest.
 - Behavior-driven testing (BDT) is a testing method in which the testing scenarios are based on user behavior.
 - It basically means writing different test functions for different functions.


###### In summary, the Single Responsibility Principle states that :
 1. The code should cover all the relevant functions that it should do.
 2. The code shouldn't take unnecessary functions.

