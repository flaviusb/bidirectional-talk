# Beyond Hindley Milner with Bidirectional Type Theories

## Motivation

- Why look at type theories

## What is Hindley Milner

- Type system behind ML family
- All synthesis
- Annotations act like tests or comments
- Algorithm W

## Specific features

- 'Principal types'
- GADTs
- Parametric polymorphism
- Pattern matching

## Example

```
data List x = Cons x (List x) | Nil
map (Cons x y) f = Cons (f x) (map y f)
map Nil f = Nil
```

## Equivalently

```
data List x = Cons x (List x) | Nil
map list f =
    match list of
      case (Cons x y) -> Cons (f x) (map y f)
      case Nil        -> Nil
```


## Synthesised

```
map: ∀x.∀y. List x → (x → y) → List y
```

## Elegant

- Very few rules required
- Very few syntactic forms required
- Only one context required
- Linear time

## Issues

- Quantifiers in Type Schemes, not in Types
- Prenex polymorphism
- Monomorphization
- Let polymorphism

## More Issues

- Modules
  - Have to add on modules in add hoc ways
  - Then get types as values in even more ad hoc ways
- Relatedly, treatment of Existentials
- Functors should just be functions

## More Issues

- Higher kinded types
- Higher sorted kinds
- Universes

## More More issues

- Dependant Types
- Linear Types, Graded Modal Types...
- Not extendable without exploding

## Bidirectional Type Theory - Broadly

- 'Both directions'
- Two (or more) kinds of judgements: checking and synthesis
- Introductions vs Eliminations
- Have to introduce more rules
- Also have to introduce some kind of output context or stack

## Local reasoning - implicitly

- Local Type Inference, Pierce and Turner 2000
- Use a constraint solver locally
- Introduction forms for literal need no additional annotation

## Local reasoning - explicitly

- Extra layer of annotation of Elimination and Introduction forms
- Introducing 'goals'

## 'Locality'

- Smallness
- Detemining mode is Syntactic
- Change of direction
- Non-immediate introduction forms


## A way around 'principal types'

- Intersection types
- From nondeterministic ∧Elimx

"The whole point of an intersection type discipline is to allow ascribing many types to the same term." - Bidirectional Typing, Dunfield19

## Direction

- 'Mode' from logic programming
- Syntax directed
- Boxy Types
- Interleaving
- Eager vs Lazy vs Greedy (with subtyping)
- Subsumption rule - direction change for subtyping

## Complexity

- Intersection and union types
- Requiring Introduction forms to be annotated
- No types can come 'out of nowhere'

## Extensions from core

- Are more tractable
- Rules naturally more modular
- Introduce more modes
- Limit interactions by confining direction, locality

# Functional modules

```
map: ∀container: (type→type). ∀x. ∀y. (container x) → (x → y) → (container y).
```

# Functional modules

```
map: ∀container: (type→type) and container is mappable. ∀x. ∀y. (container x) → (x → y) → (container y).
```

# Functional modules

```
map: ∀container:(type→type). ∃mapFn∋mappable container. mapFn.
map {mapFn} = mapFn

data List x = Cons x (List x) | Nil
mapList (Cons x y) f = Cons (f x) (map y f)
mapList Nil f = Nil

mappable: ∀container:(type→type).∀x.∀y.∃map∋((container x) → (x → y) → (container y)). container → map
mappable {map} container = map

implicit mappableList = mappable {mapList} List
```


## Extension

- For example, adding linear function space
- Add a mode: Application
- Add Introduction rules for ⊸
- Stack effect
