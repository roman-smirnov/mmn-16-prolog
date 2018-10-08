# MMN 16 - Prolog



## Question 1

### Transitions

$$
\begin{array}{l}
A \rarr EF \mid FF \\
E \rarr FC \mid BB \\
F \rarr CB \mid BB \\
C \rarr BA \\
B \rarr AA
\end{array}
$$

### Start

$D​$

### Goal

$ A^*$ (where the asterisk denotes the Kleene star)

### Costs

All transitions have an equal cost of 1

### Heuristic Function Definition

$$
h(Y_1 Y_2) = h(Y_1) + h(Y_2) 
$$

where the base evaluations are
$$
\begin{array}{l}
h(D) = 6 \\
h(E) = 5 \\
h(F) = 4 \\
h(C) = 3 \\
h(B) = 2 \\
h(A) = 0
\end{array}
$$

### Evaluation Function Definition

The evaluation function is defined as $f(n) = g(n) + h(n)$ where $g(n)$ is the cumulative path weight and $h(n)$ is the heuristic estimate. 

### Search Algorithm

We'll define our best-first search algorithm as the _A*_ algorithm, and $f(n)$ as its evaluation function. 

### Graph Definition

We'll define a directed graph such that each node stands for symbol string and each edge a transition (thick edge: AND, thin edge: OR).

### Search Tree Illustration

The algorithm will explore the node having the lowest estimate at each iteration, yielding the below search tree. 

```mermaid
graph TD
F1[F]
F2[F] 
B1[B]
B2[B]
B3[B]
B4[B]
BB1[BB]
BB2[BB]
CB1[CB]
CB2[CB]
AA1[AA]
AA2[AA]
AA3[AA]
AA4[AA]
D --> |g=1,h=8,f=9| FF
D --> |g=1,h=9,f=10| EF
	FF ==> |g=1,h=4,f=5| F1
	FF ==> |g=1,h=4,f=5| F2
		F1 --> |g=2,h=5,f=7| CB1
		F1 --> |g=2,h=4,f=6| BB1
		F2 --> |g=2,h=5,f=7| CB2
		F2 --> |g=2,h=4,f=6| BB2
			BB1 ==> |g=2,h=2,f=4| B1
			BB1 ==> |g=2,h=2,f=4| B2
			BB2 ==> |g=2,h=2,f=4| B3
			BB2 ==> |g=2,h=2,f=4| B4
				B1 --> |g=3,h=0,f=3| AA1
                B2 --> |g=3,h=0,f=3| AA2
                B3 --> |g=3,h=0,f=3| AA3
                B4 --> |g=3,h=0,f=3| AA4
```



## Question 2

### Game Description

The deck has some number of cards. Each card has an integer value from the range [1,39] (with replacement). 

The game is played by 2 players. Each player starts with no cards.

Each player draws a card in turn. The aggregate sum of a player hand can't exceed 40. Players _must_ draw cards until no more cards can be drawn without exceeding the limit on deck cumulative value. 

The game ends when _both_ players can't draw any more cards. 

### State Definition

A state is a tuple encoding the list of available cards, the sum of cards held by each player, and the turn indicator `(set_of_cards, turn_indicator, h_player_sum, c_player_sum)`. 

### Start State

The game starts with both players having no cards on hand. We'll define the starting turn to be of  `h_player`. 

### Transition

At each transitionone card is removed from the deck and its value added to the current players total such that the limit is not exceeded, otherwise no card is drawn. In addition, the turn sign bit flips. 

### Terminal State

Both players can't draw another card. 

### Goal

Cumulative hand value doesn't exceed limit and its distance from it is minimal. 

### Heuristic Function

Average value between worst and best card. The rationale for this choice is the opponent won't allow the player to make the best choice, but since the player still has _some_ choice, they won't be left with the worst card either. 

### Evaluation Function

Total value of hand + heuristic evaluation.

### Q2.1

We're given a deck of cards having the value `[2,18,20,32]`. Running the Alpha-Beta algorithm 2 turns deep will yield the following search tree (bold edges are pruned):

```mermaid
graph LR
H1 --> |c=2,g=2,h=25,f=27| C1 
	C1 ==> |c=18,g=20,h=10,f=30| H2
	C1 ==> |c=20,g=22,h=9,f=31| H3
	C1 --> |c=32,g=34,h=0,,f=34| H4
H1 --> |c=18,g=18,h=10,f=28| C2
	C2 --> |c=2,g=20,h=10,f=30| H5
	C2 --> |c=20,g=38,h=1,f=39| H6
H1 --> |c=20,g=20,h=9,f=29| C3 
	C3 --> |c=2,g=22,h=9,f=31| H7
	C3 --> |c=18,g=38,h=1,f=39| H8
H1 --> |c=32,g=32,h=1,f=33| C4
	C4 --> |c=2,g=34,h=0,f=34| H9


```



### Q2.2



```mermaid
graph LR
H1 --> |c=2,g=2| C1 
	C1 ==> |c=18,g=20| H2
		H2 ==> |c=20,g=40| C5
	C1 ==> |c=20,g=22| H3
		H3 ==> |c=18,g=40| C6
	C1 --> |c=32,g=34| H4
H1 --> |c=18,g=18| C2
	C2 ==> |c=2,g=20| H5
		H5 ==> |c=20,g=40| C7
	C2 --> |c=20,g=38| H6
H1 --> |c=20,g=20| C3 
	C3 --> |c=2,g=22| H7
		H7 --> |c=18,g=40| C8
	C3 --> |c=18,g=38| H8
H1 --> |c=32,g=32| C4
	C4 --> |c=2,g=34| H9

```



### Q2.3

Searching down to the leaves allows the player to win every time. The heuristic function yield a losing move. We hypothesize the existence of a non-hedging heuristic better suited to a zero-sum game (e.g assume taking the 2nd best choice). 





