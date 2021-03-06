# 2: Definitions and Proofs

Let's define some valsi, just like any good Lojban tutorial. Since I assume
that the reader has already consumed an introductory tutorial and a grammar
reference, I will attempt to focus on the formal side of our budding
correspondence between Lojban and categorical logic.

## Identity: {du}

The first selbri we'll define is `{du}`. With `{du}`, we can relate any object
to itself. How do we talk about this property formally? We'll use [natural
deduction](https://en.wikipedia.org/wiki/Natural_deduction), which is a common
style of formal proof. The idea is to start from some assumptions, and apply
some rules, coming to some conclusions.

To start, let's give an axiom for `{du}`. An axiom is an assumption that we
can always make. I'll put this axiom in a block by itself, but I'm also going
to give it a petname in parentheses so that we can refer to it later easily.

    da du da (id-refl)

"id" is short for "identity", and that's precisely what `{du}` does. "refl" is
short for "reflexive", which is one of the three properties of [equivalence
relations](https://en.wikipedia.org/wiki/Equivalence_relation). An equivalence
relation is a relational way to talk about equality, and `{du}` definitely
should be an equivalence relation, so let's add those other rules. The second
property is called "symmetry", and it means that we can swap the sumti without
changing the bridi. To show what a swap looks like, we'll introduce rules. A
rule has some assumptions on top, and then a bar, and then some conclusions on
the bottom.

    da du de
    ======== (id-sym)
    de du da

Here I've drawn a double bar to show which part of the rule is which. I will
sometimes also draw single bars. The double bar is reversible; it can be
turned upside down to gain another rule. For `{du}` this is unsurprising, but
some double bars later on may feel confusing at first. I encourage the reader
to take their time and carefully examine the diagrams to make sure that they
can read each step in a proof.

In relational logic, many rules are reversible. This is an instance of the
[microcosm principle](https://ncatlab.org/nlab/show/microcosm+principle),
where algebraic structures tend to show up within their own categorified
presentations. We will eventually show that rules can be manipulated like
sumti.

A rule can have multiple assumptions. Here, I'll use an ampersand to separate
multiple assumptions.

    da du de & de du di
    =================== (id-trans)
         da du di

And in relational logic, rules can have multiple conclusions! This happens if
we take a reversible rule like `(id-trans)` and run it backwards.

## Conversion Operators: {se}

`{se}` is one of what I will call "operators" on selbri; they are cmavo which
alter the structure around a selbri without changing the relation itself.
`{se}` also has reversible rules, as one might expect.

     da broda de
    ============== (se-intel) [CLL 5.11]
    de se broda da

"intel" is short for "introduction" and "elimination", which are traditionally
two separate rules. With relational logic, it's one reversible rule instead.
In brackets, "CLL 5.11" is a citation.

We can start to describe a conversion-normal form for selbri by imagining
applying `(se-intel)` backwards whenever possible, eliminating `{se}`.

Finally, let's finish our tour of natural deduction by building a proof. A
proof is a tree of rules, with the conclusions of one rule becoming the
assumptions of the next rule. When a proof only assumes axioms, then I will
call its conclusions theorems, and consider them as honorary axioms. Here is
our first proof; it starts from an axiom and applies a single rule, concluding
in our first theorem.

     da du da   (id-refl)
    =========== (se-intel)
    da se du da

What we have concluded is that, in any context, `{da se du da}` is a true
bridi. We could give this theorem a name and reuse it in the future. Here's
another useful theorem.

     da du de
     ========   (id-sym)
     de du da
    =========== (se-intel)
    da se du de

This theorem states the symmetry of `{du}` using `{se}`.

# Composition: {gi'e}

When a variable shows up in two different bridi, then we might be able to
combine them. The underlying relations compose cleanly, so most of the
difficulty is in Lojbanic arrangement. We need to use the concept of
contravariance, and turn around one of the bridi, in order to apply `{gi'e}`.

    da broda de & da brode di
    ========================= (gi'e-intel)
    da broda de gi'e brode di

Our ampersand happens to commute, so `{gi'e}` commutes as well. To see this in
full, let's do a deduction that can't be done with only two dimensions. The
`(sr)` rule is short for "structural rearrangement"; we are doing bookkeeping
that doesn't affect the logical correctness of assumptions and that would be
free if we could stand our proof up like a tree in three dimensions. Recall
that `{gi'e}` groups to the left.

    da broda da gi'e brode de gi'e brodi di
    ======================================= (gi'e-intel)
    da broda da gi'e brode de & da brodi di
    ======================================= (gi'e-intel)
    da broda da & da brode de & da brodi di
    ======================================= (sr)
    da broda da & da brodi di & da brode de
    ======================================= (gi'e-intel)
    da broda da gi'e brodi di & da brode de
    ======================================= (gi'e-intel)
    da broda da gi'e brodi di gi'e brode de

In the middle, those two triangles of assumptions are each two-dimensional,
but stacked on top of each other, giving a three-dimensional structure to the
proof.

To get more comfortable with proofs, let's do another one. This proof is
closer in phrasing to traditional categorical texts, where given `f : X -> Y`
and `g : Y -> Z`, one can form `f;g : X -> Z`. Note that, unlike in
traditional notation, we do not forget our intermediate connecting values
between relations.

     da broda de & de brode di
    ============================ (se-intel)
    de se broda da & de brode di
    ============================ (gi'e-intel)
    de se broda da gi'e brode di

One more rule connects `{gi'e}` back to `{du}` by letting us effectively
rewrite across an equality. This is like performing a sort of unification,
too.

    da broda de & de du di
    ====================== (du-intel)
         da broda di

We can use both of these new rules to write the more categorically-oriented
proof, which says that composition with the identity relation has no effect.

    da broda de gi'e du di
    ====================== (gi'e-intel)
    da broda de & de du di
    ====================== (du-intel)
         da broda di

Along with the other rules introduced, we now have a dagger category on
selbri, with `{du}` as our identity selbri, `{gi'e}` for composition, and
`{se}` for the dagger.

## Weakening

Sometimes we'll want to duplicate a step, or have a duplicate copy of a bridi.
In those cases, we can use "structural weakening" to add/remove a copy.

       bu'a
    =========== (sw)
    bu'a & bu'a

## Scoping

Scoping comes into play at this time. Up until this point, we have not
explained the scoping and binding of our `{da}` variables. However, scoped
negation and plural quantifiers do not work with our `(se-intel)` rule. The
fix comes from work done on bicategories of relations and [regular
logic](https://en.wikipedia.org/wiki/Regular_category#Regular_logic_and_regular_categories):
All quantifiers must either be lifted to the top level, or be single positive
existential quantifiers.

In more words, `{da}` is fine because it only binds one thing once, it asserts
that the bound thing satisfies the bridi it's bound within, and it only claims
that there is one satisfying thing at a time rather than all things at once,
and it fortunately has all of these properties by default. When we need other
quantifiers, we will use a
[prenex](https://lojban.org/publications/cll/cll_v1.1_xhtml-section-chunks/section-da-and-zohu.html).

## Interpreting natural deduction

Each logic in natural deduction can be used to give judgements, like "P is
true." Wikipedia lists "P is possibly true," "P is always true," "P is true at
a given time," and "P is constructible from the given resources," as other
examples in other logics. What does relational logic give us? Relational logic
judgements are of the form "P is true multiple times, under each of the given
contexts;" we can imagine that a bridi is not just true or false, but true for
each of the many possible values that are being related.
