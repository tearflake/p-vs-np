# P vs. NP solution proposition

**type:** *Research paper*  
**content:** *Derivation of linear time complexity solution to Boolean satisfiability problem*  
**author:**  *tearflake*  
**references:** *Wikipedia*  

## table of contents

- **[1. introduction](#1-introduction)**  
- **[2. converting to conjunctive normal form in linear time complexity](#2-converting-to-conjunctive-normal-form-in-linear-time-complexity)**  
- **[3. converting to disjunctive normal form in linear time complexity](#3-converting-to-disjunctive-normal-form-in-linear-time-complexity)**  
- **[4. conclusion](#4-conclusion)**  

## 1. introduction

The [P versus NP problem](https://en.wikipedia.org/wiki/P_versus_NP_problem) is a major problem in computer science. It asks whether every problem whose solution can be quickly verified (technically, verified in polynomial time) can also be solved quickly (again, in polynomial time). P set denotes a class of problems that can be solved in polynomial time, while NP set denotes a class of problems that can be verified in polynomial time. If P equals NP, a lot of possibilities open, regarding the reduced amount of time needed to produce solutions for a whole range of problems.

In computational complexity theory, an [NP-complete](https://en.wikipedia.org/wiki/NP-completeness) decision problem is one belonging to both the NP and the [NP-hard](https://en.wikipedia.org/wiki/NP-hardness) complexity classes. A problem p in NP is NP-complete if every other problem in NP can be transformed (or reduced) into p in polynomial time. That means that a polynomial solution to any NP-complete problem would imply that P equals NP. [Boolean satisfiability problem](https://en.wikipedia.org/wiki/Boolean_satisfiability_problem) represents such an NP-complete problem.

In this short paper, we introduce a linear complexity time solution to the Boolean satisfiability problem. First, we present a method for converting any logic formula to its [conjunctive](https://en.wikipedia.org/wiki/Conjunctive_normal_form) normal form. Next, building upon this method, we present a method for converting any logic formula to its [disjunctive](https://en.wikipedia.org/wiki/Disjunctive_normal_form) normal form from which solutions to the Boolean satisfiability problem may be derived. Lastly, we overview the described method of finding solutions while concluding that P set equals set NP set. 

## 2. converting to conjunctive normal form in linear time complexity

We say that a logic formula is in conjunctive normal form (CNF) when it takes a form:

```
(A1 \/ A2 \/ ...) /\ (B1 \/ B2 \/ ...) /\ ...
```

To convert a logic formula to its CNF in linear complexity time, we may reach for a set of novel transformation rules inspired by the ones from [sequent calculus](https://en.wikipedia.org/wiki/Sequent_calculus):

```
     Γ |- Δ, (A1 \/ A2 \/ ...)
 1. ---------------------------
        Γ |- Δ, A1, A2, ...


             Γ, (A1 \/ A2 \/ ...) |- Δ   
 2. -------------------------------------------
     A1 |- B     A2 |- B     ...     Γ, B |- Δ


     Γ, (A1 /\ A2 /\ ...) |- Δ
 3. ---------------------------
        Γ, A1, A2, ... |- Δ


             Γ |- Δ, (A1 /\ A2 /\ ...)
 4. -------------------------------------------
     Γ |- Δ, B     B |- A1     B |- A2     ...


     Γ |- Δ, (A1 -> A2)
 5. --------------------
      Γ -> Δ, ¬A1, A2


      Γ, (A1 -> A2) -> Δ
 6. ----------------------
     Γ, (¬A1 \/ A2) -> Δ


     Γ -> Δ, ¬A1
 7. -------------
     Γ, A1 -> Δ

     Γ, ¬A1 -> Δ
 8. -------------
     Γ -> Δ, A1

           A1, A2, ... |- B1, B2, ...
 9. --------------------------------------
     ¬A1 \/ ¬A2 \/ ... \/ B1 \/ B2 \/ ...
```

The novel rules that are the essence of this short paper are rule `2.` and rule `4.`. We corroborate them by rules `5.` and `6.`.

We start by introducing a sequent from the formula F we want to convert: `|- F`. After that, we apply the above nine rules in noted order of precedence. The first eight rules are the rules for normalizing sequents, while the ninth rule converts each sequent to a disjunction. Lastly, we concatenate each such disjunction with the `/\` operator, forming the CNF formula of the starting formula F.

Because all the above rules produce results of linear space complexity of expressions it starts from, we conclude that the whole procedure of **producing CNF from any formula takes a linear time complexity**.

## 3. converting to disjunctive normal form in linear time complexity

We say that a logic formula is in disjunctive normal form (DNF) when it takes a form:

```
(A1 /\ A2 /\ ...) \/ (B1 /\ B2 /\ ...) \/ ...
```

We already concluded that it takes a linear amount of time to convert any formula to CNF formula. However, it is also possible to convert any formula to its DNF in linear amounts of time using only negation and conversion to CNF. We start from boolean formula F. The procedure of converting to its DNF consists of three simple steps:

```
1. negate formula F,
2. convert the negated formula F to its CNF,
3. negate again the resulting formula and apply De Morgan's laws to get DNF of the starting formula F.
```

In step 3, from the negated CNF formula we extract its DNF. Normally, converting a random formula to its DNF takes an exponential time in the worst case. But in step 3, fortunately, we are dealing with the best case scenario. Thus, we are always encountering the following pattern:

```
 ¬( (A1 \/ A2 \/ ...) /\ ... /\ (Z1 \/ Z2 \/ ...) )
-----------------------------------------------------
  (¬A1 /\ ¬A2 /\ ...) \/ ... \/ (¬Z1 /\ ¬Z2 /\ ...)
```

The pattern seems extremely simple, and we are assuming it can always be performed at least in linear complexity time.

All that remains to exclude every contradictory disjunct from the final formula. If all disjuncts are contradictory, the formula F does not have a satisfiable solution.

The first step takes a constant amount of time, while the second and third step take a linear amount of time regarding the number of atoms, which leads us to a total of **linear time complexity for producing DNF from any logic formula**.

## 4. conclusion

Disjunctive normal form has a convenient property of easy extraction of all possible solutions to since every consisting disjunct represents one possible solution set to Boolean satisfiability problem.

An answer to the P = NP question would determine whether problems that can be verified in polynomial time can also be solved in polynomial time. Deriving DNF in linear time complexity implies a positive answer to this question which may be of crucial importance to many scientific fields. If there is no mistake in the presented method of converting any formula to its disjunctive normal form, we may have very valuable computing material at our disposal.
