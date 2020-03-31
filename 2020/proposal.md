# Proposal for the Google Summer of Code 2020

<small>
**Note**: this proposal contains links. If you are not able to view them, there is a markdown version at
https://github.com/fgaz/gsoc/blob/master/2020/proposal.md
</small>

## Personal info

**Name**: Francesco Gazzetta

**Contact**: fgaz@fgaz.me, `fgaz` on Freenode and most sites. See [fgaz.me/about](https://fgaz.me/about) for details.

### Relevant experience

I started to learn Haskell about six years ago, and continued to use and study it since then, writing most of [my projects](https://fgaz.me/projects) in it and contributing to others.

I took part in HSoC 2017 with the "Last mile for cabal new-build" project, and GSoC 2018 with the "Support for Multiple Public Libraries in a Cabal package" project.

As part of my bachelor thesis, I developed [a GHC plugin](https://git.sr.ht/~fgaz/singleton-optimizer) for the optimization of singletons.

I'm currently taking relevant university courses in Haskell, such as Compiler Construction.

## Project info

### Project title

`SPECIALIZABLE` GHC pragma... and more!

### Potential mentors

* Carter Schonwald
* Andreas Klebinger

### Goals of the project

#### `SPECIALIZABLE` pragma

##### Motivation

One of the many optimizations that GHC can perform is specialization.
GHC can create multiple versions of generic code specialized to particular types, avoiding to pass typeclass dictionaries around and opening possibilities for further optimizations.

To tell GHC to specialize a function, one can use the `SPECIALIZE` pragma.
To be able to specialize, GHC needs the unfolding of the function to be available, either from the module itself or from interface files.

To ensure that the unfolding is available to library users, authors tend to use the `INLINEABLE` pragma, which does have the effect of exposing the unfolding, but has an only tangentially related meaning.

Moreover, one may not actually want to inline the function, but specifying both `INLINEABLE` and `NOINLINE` is not possible.
This can lead to unwanted increases of code size.

Another disadvantage is that the optimization of the unfolding happens at the use site, which could cause redundant work to happen.
There is also no control over whether the specialization of the use sites will actually fire or not.
While in practice it does quite reliably happen, the original meaning is lost.

##### Proposed idea

The idea is also described in [issue #12463](https://gitlab.haskell.org/ghc/ghc/issues/12463), from which this proposal originates.

A way to solve the problem would be to have a new `SPECIALIZABLE` pragma.
The pragma would expose the inlining for the purpose of specialization, without affecting inline heuristics, and without conflicting with `INLINE` and `NOINLINE`.
It would also have the same effect of an explicit `SPECIALIZE` pragma for each concrete use of the function.

Optional controls could be:

* What part of the type can be specialized
* Enforcement of specialization (or specialization failure warnings)
* Optimization

##### Extended proposal: recursive/transitive `SPECIALIZABLE`

A further addition could be a recursive/transitive `SPECIALIZABLE` (also described in the ticket), which would have the additional benefit of lifting the requirement for a library user of annotating intermediate polymorphic functions with `SPECIALIZABLE` to ensure that the monomorphic use sites are specialized.

#### Other goals

The main goal probably won't take the whole GSoC period to complete, especially if only the normal version of `SPECIALIZABLE` is implemented.

As such, the project will expand to other tasks still in the context of GHC.
These new tasks may arise during the main one, or could be picked from these other ideas:

* Adding the possibility of handling signalling NaNs and other numeric errors explicitly
* Improvements to the rules engine, such as use constraints
* If deemed short enough and if not picked up by others, parts of the other GHC [ideas](https://summer.haskell.org/ideas.html)

### Plan of action


<!-- ### Approximate timeline. -->
### Timeline highlights


**Community Bonding period**

During the community bonding period, or in general before the beginning of GSoC, I'd like to work on a smaller patch to get myself acquainted with the process and the codebase.

**First Half**

After the first week, during which I'll have little time due to exams, I'll work on the main task, starting from a minimal `SPECIALIZABLE` implementation and eventually expanding it in scope as my understanding of the problem develops.

Once a prototype is complete, I'll also start working on validating the impact of the new pragma, by producing (and finding in real libraries) code examples that can benefit from it.

I'll also evaluate whether there are obstacles for the extended version of `SPECIALIZABLE`.

Before the second half of GSoC starts, at least the basic `SPECIALIZABLE` should be complete and validated.

**Second half**

The second half will be dedicated to finishing the recursive variant of `SPECIALIZABLE`, if started and still not complete, and to working on the other tasks for any remaining amount of time.

---

The latest version of this proposal is also available in markdown format
[here](https://github.com/fgaz/gsoc/blob/master/2020/proposal.md).

