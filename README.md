# P vs. NP solution proposal

**type of document:** *research paper*  
**content:** *quadratic time complexity solution to Boolean satisfiability problem and P vs. NP problem consequences*  
**author:** *tearflake*  
**references:** *Wikipedia*  
**intended audience:** *beginners in sequent calculus and propositional logic*  

## table of contents

- **[1. introduction](#1-introduction)**  
- **[2. converting to conjunctive normal form in quadratic time complexity](#2-converting-to-conjunctive-normal-form-in-quadratic-time-complexity)**  
- **[3. converting to disjunctive normal form in quadratic time complexity](#3-converting-to-disjunctive-normal-form-in-quadratic-time-complexity)**  
- **[4. extracting set of satisfiable solutions](#4-extracting-set-of-satisfiable-solutions)**
- **[5. conclusion](#4-conclusion)**  

## 1. introduction

The [P versus NP problem](https://en.wikipedia.org/wiki/P_versus_NP_problem) is a major problem in computer science. It asks whether every problem whose solution can be quickly verified (technically, verified in polynomial time) can also be solved quickly (again, in polynomial time). P set denotes a class of problems that can be solved in polynomial time, while NP set denotes a class of problems that can be verified in polynomial time. If P equals NP, a lot of possibilities open, regarding the reduced amount of time needed to produce solutions for a whole variety of problems.

In computational complexity theory, an [NP-complete](https://en.wikipedia.org/wiki/NP-completeness) decision problem is one belonging to both the NP and the [NP-hard](https://en.wikipedia.org/wiki/NP-hardness) complexity classes. A problem p in NP is NP-complete if every other problem in NP can be transformed (or reduced) into p in polynomial time. That means that a polynomial solution to any NP-complete problem would imply that P equals NP. [Boolean satisfiability problem](https://en.wikipedia.org/wiki/Boolean_satisfiability_problem) represents such an NP-complete problem.

In this short paper, we introduce a quadratic complexity time solution to the Boolean satisfiability problem. First, we present a method for converting any logic formula to its [conjunctive](https://en.wikipedia.org/wiki/Conjunctive_normal_form) normal form. Next, building upon this method, we present a method for converting any logic formula to its [disjunctive](https://en.wikipedia.org/wiki/Disjunctive_normal_form) normal form. Further, we show how to extract all the solutions to the Boolean satisfiability problem from disjunctive normal form. Lastly, we overview the described method of finding solutions while concluding that P set equals set NP set. 

## 2. converting to conjunctive normal form in quadratic time complexity

We say that a logic formula is in conjunctive normal form (CNF) when it takes a form:

```
(A1 \/ A2 \/ ...) /\ (B1 \/ B2 \/ ...) /\ ...
```

To convert any logic formula to its CNF in quadratic complexity time, we may reach for a set of novel transformation rules inspired by the ones from [sequent calculus](https://en.wikipedia.org/wiki/Sequent_calculus):

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
      Γ |- Δ, ¬A1, A2


      Γ, (A1 -> A2) |- Δ
 6. ----------------------
     Γ, (¬A1 \/ A2) |- Δ


     Γ |- Δ, ¬A1
 7. -------------
     Γ, A1 |- Δ


     Γ, ¬A1 |- Δ
 8. -------------
     Γ |- Δ, A1


           A1, A2, ... |- B1, B2, ...
 9. --------------------------------------
     ¬A1 \/ ¬A2 \/ ... \/ B1 \/ B2 \/ ...
```

The rules which differ from traditional sequent calculus are rules `2.`, `4.`, `5.` and `6.`. Notice that, unlike in traditional sequence calculus normalization strategy, `Γ` and `Δ` in each rule is reproduced only once. This is how we avoid exponential space complexity in applying rules. Additionally, rule `9.` is used for translating normalized sequents to disjunctions.

We start by introducing a sequent from the formula F we want to convert: `|- F`. After that, we apply the above nine rules in noted order of precedence. The first eight rules are the rules for normalizing sequents, while the ninth rule converts each sequent to a disjunction. Lastly, we concatenate such disjunctions with the `/\` operator, forming the CNF formula of the starting formula F.

Considering the total number of syntactic nodes on which any above rule can be applied, the whole procedure of **producing CNF from any formula takes a quadratic time complexity** regarding number of atoms in starting formula.

## 3. converting to disjunctive normal form in quadratic time complexity

We say that a logic formula is in disjunctive normal form (DNF) when it takes a form:

```
(A1 /\ A2 /\ ...) \/ (B1 /\ B2 /\ ...) \/ ...
```

We already concluded that it takes a quadratic time complexity to convert any formula to CNF formula. However, it is also possible to convert any formula to its DNF in quadratic time complexity using only negation and conversion to CNF. We start from boolean formula F. The procedure of converting to its DNF consists of three simple steps:

```
1. negate formula F,
2. convert the negated formula F to its CNF,
3. negate again the resulting formula and apply De Morgan's laws to get DNF of the starting formula F.
```

In step 3, from the negated CNF formula we extract its DNF. Normally, converting a random formula to its DNF takes an exponential time in the worst case. But in step 3, fortunately, we are dealing with the best case scenario. Thus, we are always encountering the following pattern:

```
  ¬((A1 \/ A2 \/ ...) /\ ... /\ (Z1 \/ Z2 \/ ...))
-----------------------------------------------------
  (¬A1 /\ ¬A2 /\ ...) \/ ... \/ (¬Z1 /\ ¬Z2 /\ ...)
```

The is a simple pattern, and we are assuming it can always be performed at most in linear complexity time.

All that remains is to eliminate double negations and to exclude every contradictory disjunct from the final formula. If all disjuncts are contradictory, the formula F does not have a satisfiable solution.

The first step takes a constant amount of time, the second step takes a quadratic amount of time, while the third step takes a linear amount of time. This leads us to a total of **quadratic time complexity for producing DNF from any logic formula** regarding number of atoms in starting formula.

## 4. extracting set of satisfiable solutions

DNF has a convenient property of easy extraction of all possible solutions to Boolean satisfiability problem. Namely, each disjunct of DNF represents one possible solution set of the starting formula. For example, if we get a DNF like:

```
(¬A /\ B /\ C) \/ (¬A /\ ¬B) \/ (A /\ ¬B /\ ¬C)
```

this means that we have three possible solution sets: `{A -> F, B -> T, C -> T}` or `{A -> F, B -> F}` or `{A -> T, B -> F, C -> F}`.

## 5. conclusion

An answer to the P = NP question would determine whether problems that can be verified in polynomial time can also be solved in polynomial time. Deriving DNF in quadratic time complexity implies a positive answer to this question which may be of crucial importance to many scientific fields. If there are no mistakes in the presented method of converting any formula to its disjunctive normal form, we may have very valuable computing material at our disposal.
