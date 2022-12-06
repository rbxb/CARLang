# CARLang

A notation language for defining Cellular Automata rules. Designed with the help of OpenAI's ChatGPT. A manuscript of the original conversation can be found [here](chat_manuscript.md).

_Most of the following text was written by ChatGPT_

**CARL (Cellular Automaton Rules Language)** is a notation for defining the rules and behavior of cellular automata. It allows users to specify the possible states of cells, the shape and size of the neighborhoods used to calculate transitions, and the conditions and actions that determine how cells transition between states. The notation uses a simple, intuitive syntax that allows users to easily define complex rules for cellular automata. It is designed to be flexible and extensible, allowing users to define custom functions and conditions for use in transition rules. CARL is intended to provide a standard, consistent way of defining cellular automata, enabling users to easily create and share their own cellular automaton rules.

## Syntax

A CARL rule is defined using the following syntax:

```carl
rule <rule_name>

states <state_definition>

neighborhood function <function_name>(<arguments>) <function_body>

initial state function <function_name>(<arguments>) <function_body>

transition (<condition> -> <action>) <transition_condition>
```

The **rule** keyword is followed by the name of the rule, which must be unique within the context in which the rule is defined. The **states** keyword is followed by the definition of the possible states of the cells in the cellular automaton. The **neighborhood function** keyword is followed by the name of the neighborhood function, which is used to define the shape and size of the neighborhoods used to calculate transitions. The **initial state function** keyword is followed by the name of the initial state function, which is used to define the initial state of the cells in the cellular automaton. The **transition** keyword is followed by a condition and an action, separated by the -> operator. The condition specifies which state the cell is in, and the action specifies the state that the cell will transition to. The transition condition specifies the conditions under which the transition should occur.

## Examples

Here are some examples of CARL rules that demonstrate the syntax and usage of the notation:

#### Conway's Game of Life

```carl
rule conway

states [DEAD, ALIVE]

neighborhood function nbhd(x, y) abs(x) + abs(y) <= 1

initial state function init(x, y) x == 0 || x == grid_width - 1 || y == 0 || y == grid_height - 1 ? ALIVE : DEAD

transition (ALIVE -> DEAD) count(nbhd where ALIVE) < 2
transition (ALIVE -> ALIVE) count(nbhd where ALIVE) == 2 || count(nbhd where ALIVE) == 3
transition (ALIVE -> DEAD) count(nbhd where ALIVE) > 3
transition (DEAD -> ALIVE) count(nbhd where ALIVE) == 3
```

This conway rule defines the rules of Conway's Game of Life. The states section defines two possible states for the cells: DEAD and ALIVE. The neighborhood function defines a circular neighborhood with a radius of 1. The initial state function sets the cells along the edges of the grid to be ALIVE, and all other cells to be DEAD. The transition rules apply the following rules:

    A cell that is ALIVE with less than 2 ALIVE neighbors becomes DEAD
    A cell that is ALIVE with 2 or 3 ALIVE neighbors stays ALIVE
    A cell that is ALIVE with more than 3 ALIVE neighbors becomes DEAD
    A cell that is DEAD with exactly 3 ALIVE neighbors becomes ALIVE

Together, these rules define the behavior of Conway's Game of Life.

#### Multiple Neighborhood Cellular Automata

Here is another example of a CARL rule that defines a multiple neighborhood cellular automaton:

```carl
rule mnca

states [0, 1, 2]

neighborhood function nbhd_1(x, y) x ** x + y ** y <= 9
neighborhood function nbhd_2(x, y) x ** x + y ** y <= 25

initial state function init(x, y) abs(x) + abs(y) <= 5 ? 1 : 0

transition (1 -> 0) count(nbhd_1 where 1) > 6 || count(nbhd_2 where 1) < 10
transition (0 -> 1) count(nbhd_1 where 1) == 3
```

This mnca rule defines a multiple neighborhood cellular automaton (MNCA). The states section defines three possible states for the cells: 0, 1, and 2. The neighborhood function section defines two neighborhoods: nbhd_1 is a circular neighborhood with a radius of 3, and nbhd_2 is a circular neighborhood with a radius of 7. The initial state function sets the cells within a square radius of 5 of the origin to 1.

#### Continous States

You can specify a continous state by using floating point values. Here is an example of a continous MNCA:

```
rule continuous_mnca

states [0.0, 1.0]

neighborhood function nbhd(x, y) abs(x) <= 2 && abs(y) <= 2

initial state function init(x, y) x == 0 || x == grid_width - 1 || y == 0 || y == grid_height - 1 ? 1.0 : 0.0

transition (state -> sum(nbhd) * 0.1) sum(nbhd) < 10
transition (state -> 1.0) sum(nbhd) > 3.0 && sum(nbhd) < 5.0
```

The first transition function applies the following rule: a cell with a neighborhood sum of less than 10 becomes 0.1 times the sum of the neighborhood states (up to a maximum of 1.0). The second transition function applies the following rule: a cell with a neighborhood sum greater than 3.0 but less than 5.0 becomes 1.0. Together, these transition functions define a cellular automaton with continuous state values that transition based on the sum of the states of the cells in the neighborhood.

#### Multiple Layers

You can also define a rule where each cell has multiple values associated with it:

```carl
rule multi_dimensional

states [0.0, 1.0] x 2

neighborhood function nbhd(x, y) abs(x) + abs(y) <= 1

initial state function init(x, y) x == 0 || x == grid_width - 1 || y == 0 || y == grid_height - 1 ? [1.0, 0.0] : [0.0, 0.0]

transition (state[0] -> sum(nbhd) * 0.1) sum(nbhd) < 10
transition (state[1] -> 1.0) sum(nbhd) > 3.0 && sum(nbhd) < 5.0
```

In this multi_dimensional rule, the transition functions use indexed access to specify which state is being accessed or modified. The first transition function sets the first state of the cell to be 0.1 times the sum of the neighborhood states (up to a maximum of 1.0) if the sum of the neighborhood is less than 10. The second transition function sets the second state of the cell to be 1.0 if the sum of the neighborhood is greater than 3.0 but less than 5.0.

## Conclusion

CARL is a simple, intuitive notation for defining the rules and behavior of cellular automata. It allows users to specify the possible states of cells, the shape and size of the neighborhoods used to calculate transitions, and the conditions and actions that determine how cells transition between states. The syntax of CARL is designed to be easy to read and understand, making it simple for users to create and share their own cellular automaton rules. CARL is flexible and extensible, allowing users to define custom functions and conditions for use in transition rules. By providing a standard, consistent way of defining cellular automata, CARL enables users to easily create and explore complex and interesting patterns and behaviors in cellular automata.





