Excercise 4 - AI - Reasoning under uncertainty
--------------------------

###Semantics of probability
  - Expected values E[X], average value, infinity times
  - X Random variable (Typical large letter), range or scope
  - Probability distributions tells us what values random variable will take
      P(X) >= 0
      \\[\sum_{x \in Range(X)} P(X=x)=1\\]
  - Joint distribution $P(X_1,X_2,..., X_n)$
  - Marginal distribution
    If you have a joint $P(X,Y)$ then the marginal of X is $P(X)= \sum_y P(X,Y)$
  - Conditional distribution $P(X|Y) = \frac{P(X,Y)}{P(Y)}$
  - Bayes rule $P(X|Y) = \frac{P(Y|X) \cdot P(X)}{P(Y)}$
    - set Y to evidence
    - set X to hypothesis
    - This can be used in AI, ex spam mail and the word "Viagra"
    - one could marginalize ot h to get probability of e based on the numerator
    - $P(h|e) \propto P(e|h) \cdot P(h)$ (is normalized if... something equals one)


### [Bayesian belief network](http://en.wikipedia.org/wiki/Bayesian_network)
  - Nodes and edges
    - Nodes are random variable (can describe some events)
    - Edges are dependencies
  - Gives Sprinkler, rain, grass wet example from wikipedia
  - $P(S,R,G) = P(G|S,R) \cdot P(S) \cdot P(R)$ 
  - $= P(G|S,R \cdot P(S|R) \cdot P(R)$, common rule, something dependencies
    - Chain Rule of probability, start with P(children | parent) times P(parent)
  - 

###Probabilistic dependency
  - $X \bot Y | Z if P(X|Y,Z) = P(X|Y)$ aka conditional independence

###Active trail in a Bayesian belief
  - What direction can the flow of influence go?
    - Both ways if trail is active, therefore knowing something about a children can give information about the parent

```
        o->x->o       Active
                      Not active if x observed
        o<-x->o       Active
                      Not active if x observed

    o       o
      \    /
       v  v            V-structure, not active
         x
                        If one observe value of Grass being wet for example
                        one could make assumption about the parents.
```

### 6.2 in the book

```  
       (Power in building)               (Projector plugged in)
           |            \              /
           v             v            v
    (Light switch)          (Power in wire)     (Projector switch)
           |                       |            /
           V                       v           v
    (Room light on)             (Power in projector)           (Lamp Works)
           |                                      \            /
           v                                       v          v
    (Sam reading a book)          (Mirror working)   (Projector Lamp)
                                                  \           |
                                                   v          v
                            (Ray is awake)     (Screen is lit up)
                                          \           |
                                           v          v
                                          (Ray says "Screen is dark")  
```

 1. Can knowledge of the value of *Proj. plugged in* affect your belief in *Sam reading a book*
    - Our thought:They are independent of each other, 
    - Projector plugged in are independent of power in building, and since *Sams reading a book* not have projector plugged in as a parent, they are independent
 2. Can knowledge of *Screen lit up* affected your belief in "Sam reading a book"
    - Our thoughts We know that there is power in the building
    - Yes, it can affect. Since their are a trail between them. They are sharing a parent *power in building*
 3. Can knowledge of *Proj plugged in* affect your belief in *Sam reading a book* **given** that you have observed a value for *Screen lit up*
  - Screen lit up is a descendent to power in building is observed. And we know *projector plugged in*, a parent to *screen lit up*. We now have a V-structure and therefore can affect *Sams reading a book*
 4. Which variables could have their probabilities charged if just *Lamp works was observed*
  - Flow of influence downwards. Projector, screen lit up, Ray says screen is dark. We have a V-structure but we don't know anything about lamp works children. V-structure, but can not make assumptions of parents.
 5. Which variables could have their probabilities charged if just *Power in projector* was observed.
  - All descendants, children and parents, also parents of parents and their children's. therefore we don't know anything about *Light of, Lamp works, Ray is awake, Mirror working*
 

### Some exercise

| Flu  | Sneeze | Snore |   P   | 
| ---- | ------ | ----- | ----- |
|  T   |    T   |   T   | 0.064 |
|  T   |    T   |   F   | 0.096 |
|  T   |    F   |   T   | 0.016 |
|  T   |    F   |   F   | 0.024 |
|  F   |    T   |   T   | 0.096 |
|  F   |    T   |   F   | 0.144 |
|  F   |    F   |   T   | 0.224 |
|  F   |    F   |   F   | 0.336 |

1. P(Flu|Sneeze)
  - Difficult: -
  - Answer: 0.064 + 0.096
2. P(Flu|!Sneeze)
  - Difficult: - 
  - Answer: 0.016 + 0.024
3. P(Flu)
  - Difficult: 0.064 + 0.096 + 0.024 + 0.016
  - Answer: -
4. P(Sneeze | Flu)
  - Difficult: 3
  - Answer: 
5. P(!Flu | Sneeze)
  - Difficult: -
  - Answer: 
6. P(Flu | Sneeze)
  - Difficult: 2
  - Answer: 
7. P(Sneeze | Flu,Snore)  (Probability of sneezing given Flu and Snore)
  - Difficult: 1
  - Answer: 
    - We want sneeze to be true. 
    - Add up probability of right hand side.
    - 0.064 + 0.016 , P(Flu, Snore)
    - 0.064         , P(Flu, Sneeze, Snore)
    - $\frac{0.064}{0.064 + 0.016}$
    - Marginalizing gives us 0.8
8. P(Flu| Sneeze,Snore)
  - Difficult: 4
  - Answer: 

### Exam question
  Abdul claims he is psychic, he can predict coin tosses
   $A =$ Abdul being psychic
   $P(A) = 0.1 = P_0$
   $B_k$ Abdul predicts the $k$th coin toss correctly
   $H_k$ coin comes up heads for the $k$th toss
   Assumption: coin is fair
**a**
```
      H_1    A     H_2
         \  /  \  /   
          v v   v v    
          B_1   B_2     
   
```
**b** Marginal probability of $P(B_1)$
  - $P(B_k | A) = 1 $ Being a psychic is he can always predict the coin
  - $P(B_k | \neg A) = 0.5$
  - $P(B_k) = \sum_A{P(B_k,A)}$
  - $P(B_k|) \cdot P(A) + P(B|\neg A) \cdot P(\neg A)$
  - $P(B_k|) \cdot P(A) + P(B|\neg A) \cdot P(\neg A)$
$P(B|A) = \frac{P(B,A)}{P(A)}$
$P(B,A) = P(B|A) \cdot P(A)$

