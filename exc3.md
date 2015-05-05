Exercise 3 - Proposition and planning
--------------------------------------


## problem 5 from exam 2014-05-27

One special thing, one should prove it using proof procedure. Easy to miss

There are 2 proof procedures.
Top-down and Bottom-up

#### Difference
Bottom-up using what we know, ex c and f in this case
We have a set of things we know, and uses logic to add more to this set. Until we can't find anything.

#### We try to solve this by Bottom-up
```
      f c             facts
      c f d           d <- c and f
      c f d b         b <- d and c
      c f d b a       a <- b and c and f
```

**Note:** easy for us, but hard to implement since there are no standard library for this. One has to implement sets.

#### Top-down using what we want.
This is an example when one makes wrong decision of atoms to unfold
```
     1.  ask a                    a <- b and c and f
     2.  ask b and c and f        f <-      (fact)
     3.  ask b and c              c <-      (fact)
     4.  ask b                    b <- c and e
     5.  ask c and e              e <- d and g
     6.  ask c and d and g        g <- e and c
     7.  ask c and d and e and c  c <-
     8.  ask c and d and e        e <- d and g    (one can see that this row is an extension of row 5 )
     9.  ask c and d and d and g  inf loop
```
#### Another approach:
```
     1.  ask a                    a <- b and c and f
     2.  ask b and c and f        f <-      (fact)
     3.  ask b and c              c <-      (fact)
     4.  ask b                    b <- d and c
     5.  ask d and c              c 
     6.  ask d                    d <- c and f
     7.  ask c and f              f
     8.  ask c                    c
     9.  ask []
```
done, since we found an empty list.

## Problem 5 from exam 2014-08-19.
**question:** give a non trivial model

- A trivial interpretation or model is where all atoms are true or all atoms are false.
- A model is a non conflicting assignment 
- A interpretation is an assignment of values to the knowledge base

in this question we only work with implications and conjunctions
Therefore If all atoms are true, then all propositions are true. 
          if all atoms are false, then all propositions are false.

First find minimum model

  *The bottom-up procedure will always give a minimum model.*
```
  m,p              facts
  m,p,f            f <- m
  m,p,f,c          c <- f
  m,p,f,c,g        g <- f and p
  m,p,f,c,g,a      a <- g
  m,p,f,c,g,a,d    d <- p and a
```

  We know that this must be true, which atoms do we have left?
```
  b, k, j
```
  we found out that if we add b,k to the minimal model we have a non-trivial model
  if we add j aswell, we have a trivial model which is not correct.


##Poole and Mackworth, Exercises 5.4
  - a.
    - Give all logical consequences = give the minimum model.
    - This is found by bottom-up procedure.
  - b.
    - if f is not a logical consequence of KB. Then it should not be in the minimal model
    - We could just give the minimal model as an answer

    - We could also take another approach (in a top down fashion)
    - ex f needs to be false, then at least one atom needs to be false.
  - c.
     ---

## Russell & Norvig, chapter 9, exercises 6 - Monkeys and bananas

Monkey and box is at high low
if monkey climbs the box he get high high
bananas are at high high

Objects
- Monkey
- Box
- Banana

Actions available are:
- go        X Y
- push      O X Y
- Climb Up  
- Climb Down
- Grasp
- Ungrasp

Locations 
- A
- B
- C

### A:
*Write down the initial state description*

How should a state look like?


#### state description
```
monkey  :: (Position, height)
box     :: Position
bananas :: (Position, height, Boolean)      One could have a conjunction since a banana is either held or it has a height and a position
```

#### Initial state 
```
monkey = (A, Low)
box    = C
banana = (B, High, False)
```
### B:
*Write down the action effects for the 6 actions*
```
  Grasp
        precond : Monkey.pos    = Bananas.pos and 
                  Monkey.height = Bananas.height and
                  !bananas.held
  
        effect  : bananas.held  = true 

  Climb up
        precond : Monkey.pos = box.pos and
                  Monkey.height = low

        effect  : Monkey.height = high

  Go X
        precond : Monkey.height = low
                  Monkey.pos != X
  
        effects : monkey.pos = X
```

#### STRIPS assumption
What we don't state in the effects should be the same as in the precond
Needs to be implemented, and could make the rules a lot more compact.

## 5. Coffee Robot
 Regression planer = Backwards planner
 
 Simpler case, how does a forward planner solve this problem
```
      [off, !rhc,swc]                     In office, don't have coffee, sam wants coffee
        /           \
     mc             mcc                   Only actions
      /               \
[lab, !rhc, swc]   [cs, !rhc, swc]
                    /       |      \
                    mc     mcc      puc
                                      \
                                    [cs, rhc, swc]
                                    /           \
                                  mc            mcc
                                  /
                              [off, rhc, swc]
                            /     |       \
                          mc     mcc      dc
                                            \
                                          [off, !rhc, !swc]     
                      (goal state, sam has got coffee and don't want it anymore)

```

**This is the first planner in the project**

The regression is a backwards planer, that is it start with the goal. But the goal is not a complete set. It is a subset of a state. The regression planner is still a search planner and therefore A* can be used. (one could say that this planer is like the top-down procedure). How many atoms are different from the start state, this is checked in each iteration.

Start with the goal:
```
                
                        [!swc]       State are given points based on this state.
                          |
                          dc
                          |
                      [off, rhc]   Before we delivered coffee, we must have been in this state. 
                    /  |    \
                 mc   mcc   pu
                 /     |       \
        [cs, rhc]   [lab, rhc] [off, cs, !rhc]   (off and cs are conflicting, so we could not have come from this state)
       /   |   \
    mc    mcc  puc
                  \
                  [cs, !rhc]
                  /       \
                mc        mcc
                /           \
          [lab, !rhc]      [off, !rhc]   <-  This is the goal state. 
```


