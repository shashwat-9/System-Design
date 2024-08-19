# Single Responsibility Principle
 - This is the 'S' in the SOLID principle.
 - Over the years, across various projects, engineer have figured out that certain coding practices makes the work easy
and efficient.
 - This principle states that if we have a single piece of code, then it should have the responsibility of doing just one
thing, but doing it really well.
 - In unix programming, there's a similar concept, where we have small composable functions which put together can do many
things, but the single function does only one thing and do that thing really well.
 - In this programming, we figure out if a certain function belongs to the given class, or a certain logic belongs to the
given function.
 - In the lesson, there is a game engine, where only two functions, namely start and move are present.
 - The game engine don't contain the function isComplete or suggestMove as there it seems some sort of intelligence involved.
 - suggestMove is in a separate class called AIMove, and another class RuleEngine class looks after the state of the board.

## Unit Tests
 - The Test classes should be named as ClassNameTest.
 - Behaviour-driven testing (BDT) is a testing method in which the testing scenarios are based on user behaviour.
 - It basically means writing different test functions for different functions.


###### In summary, the Single Responsibility Principle states that :
 1. The code should cover all the relevant functions that it should do.
 2. The code shouldn't take un-necessary functions.

