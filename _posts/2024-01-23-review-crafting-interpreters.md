---
layout: post
title: "Review: Crafting Interpreters"
categories: tech book review
---

## Introductory Thoughts

I finally finished reading through [Crafting Interpreters](http://www.craftinginterpreters.com/).
I'd first been made aware of the book some years ago when only a few chapters were [available online](http://www.craftinginterpreters.com/contents.html).
Every few years or so I'd check in on the book to see how he was progressing.
Eventually I'd noticed he'd finished.
Along the way I started implementing [some language](https://git.sr.ht/~oconnor0/old-zinc-d) using the book as a base.

Earlier last year I was tasked with implementing the execution portion of a reporting engine.
And so quickly I pulled for the practical compilation advice in Crafting Interpreters.
Within a few weeks, I decided that the book was valuable enough to purchase.
Then since I'd never read the second half of the book before - always always working in higher-level, garbage-collected languages, I decided to read through it.

And what a joy it is to read.
The illustrations almost always help clarify the intent.
(There were only a couple illustrations that did not seem to clarify what was happening; though perhaps that is simply because I still do not understand what is happening.)
It's probably the best resource I know on "how do I write a parser without using a generator".
Both of the parsers implemented in the book - the one in Java and the one in C - are helpful explanations of how to write a parser using different implementation schemes.

## Parsing, Aside

As an aside on parsing, I've found that [Owl](https://github.com/ianh/owl) and the online [Try Owl](https://ianh.github.io/owl/try/#example) tool have been incredibly useful for sketching out grammars for a language.
They implement a subset of CFGs known as "pushdown automata".
These are languages where each rule, or production, can only refer to rules defined later in the grammar.
This can be handled via "guarded recursion": if the reference to an earlier rule is surrounded by tokens that are only used as begin and end keywords, say `{` and `}`.
Or via "expression recursion" which is handled via operator precedence rules in a single production.
I have found that this restriction is often sufficient for my simple language purposes.
And I prefer, at least in theory, simpler grammars.
At least simpler than C++.

## And Back to the Main Point

I have not yet worked through the book to implement Lox, or some other language.
I've been wanting to learn Zig or Lobster and relearn OCaml.
This seems like a good task to do that.
But then I am not terribly interested in dynamically-typed object-oriented languages.
If I think about the language I might be interested in:

1. Structurally typed. Maybe with opt-in nominal type in some places.
2. Prefer immutable.
3. Pattern matching, destructuring, first-class paths.
4. Functions + data.
5. Curried functions. Or is it function overloading based on arity?
6. Algebraic data types.
7. A strong module system, maybe first-class.
8. Value types, local allocations, reasonable destructors, or linear/affine types.
9. Lexical scoping.

But then objects in Lox are kind of like first-class, dynamic, mutable modules.
And dynamic dispatch is useful.
So I wonder if I should implement a language that syntactically matches my interests and see if I can write code in it in the way I want without requiring static types to make it work.

In other words, can I implement the dynamic version of a statically typed language?
Instead of compile-time linear type checking, perform those checks at runtime and abort when invariants are broken.
Or the subset of the above that can be reasonably dynamically typed?

## Conclusion

Crafting Interpreters is an excellent, pragmatic introduction to compilers and interpreters and VM and is well worth reading for anyone developing software.
I don't know what else to say.

## Fantasy Console, Aside

Part of me wants to make a "fantasy console":

1. Small, maybe fixed size, canvas with blitting, pixel pushing, etc.
2. Scripted in some programmaing language I write. Perhaps the one above.
3. And then make a game.

I'd prefer not to support floating-point, but that's just me being difficult.
