# Sequential subway logic controller

In this lab we extend the previously designed Subway logic controller to include provisions for Direction (D) control for a
given configuration.
Lab Description (specs):
This is the continuation of Lab 3. In Lab 3, the direction signal D is given as a primary input signal. Now, let’s design a finite
state machine to generate signal D. Let’s assume that the traffic pattern for the track is such when two consecutive left-
to-right trains passed by, the next train must go from right-to-left. After that, there will be two more left-to-right trains,
etc. To detect the train direction through the station, we set up two light beams just above the rail track and place two
photocells, P1 and P2, some distance apart. Assume the train can fit inside of the two beams. When the beam shines on
a photocell, it produces a 0, and when the beam is blocked, it produces a 1. The train may stop at the middle of the
photocells. If this happens, no change is made to signal D. If the train stopping at the middle and then back to the direction
it came from, then this train does not count as a passing train. Design a logic circuit to generate D.

Approach:
We first construct the state transition diagram for the given specs and use state priority as discussed
in class to perform state encoding
We then write the state transition table using the state transition diagram and decided state encoding.
After deciding upon the type of flip-flop we intend to use, we make the truth table and then generate the
equations for combinational logic blocks using Quine-Mclusky method and use these to prepare the cadence
schematic.
For the Verilog model, we use the transition diagram and construct the model using if-else and case constructs.

Inputs:
P1, P2: Photocell inputs (clubbed into 2 bit bus P [P1, P2] for simulations)
Reset: global reset signal (reset on negative edge)
Outputs:
D: Direction signal for the subway controller
State (Q): State outputs (S0 to S6: 3 bit state outputs)

Description of States:
S0: Reset state (next state S1 if P = 10, S5 if P=01)
S1: 1 Train from left crossed sensor P1 (next state S2 if P=01, S6 if P=10 (train goes back to left, thus not counted)).
S2: Train coming from left crosses sensor P2 (1 train passed from left to right, next state S3 if P =10, else self-transition).
S3: 2 nd train coming from left crosses P1 (next state = S4 if P = 01, S2 if P =10 (train goes back to left, not counted)).
S4: 2 nd train from left crosses P2 (2 nd train successfully goes from left to right, Direction (D) updated from 1 to 0, next
state = S5 if P=01, else self-transition)
S5: 1 st train from right crosses sensor P2 first (next state= S6 if P =10, S4 if P =01(train from right back to right, not
counted))
S6: 1 st train from right crosses sensor P1 (successfully passed from right to left, Direction D updated to 1, next state = 1
if P =10, else self-transition with output D =1).
