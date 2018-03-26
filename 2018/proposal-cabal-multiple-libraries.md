# Proposal for the Haskell Summer of Code 2017

## Personal info

**Name**: Francesco Gazzetta

**Contact**: francygazz@gmail.com, `fgaz` on freenode and most sites. See [fgaz.me/about](https://fgaz.me/about) for details.

### Relevant experience

I started to learn Haskell about four years ago, and continued to use and study it since then, writing most of [my projects](https://fgaz.me/projects) in it and contributing to others.

I took part in HSoC 2017 with the "Last mile for cabal new-build" project, after which I continued following cabal's development and contributing a bit.

## Project info

### Project title

Support for Multiple Public Libraries in a .cabal package

### Goal of the project

Large scale haskell projects tend to have a problem with lockstep distribution of packages (especially backpack projects, being extremely granular). The unit of distribution (package) coincides with the buildable unit of code (library), and consequently each library of such an ecosystem (ex. amazonka) requires duplicate package metadata (and tests, benchmarks...).

This project aims to separate these two units by introducing multiple libraries in a single cabal package.

This proposal is based on [this issue](https://github.com/haskell/cabal/issues/4206) by ezyang.

### Plan of action

* Add a `visibility` attribute to the convenience library stanza
* Have a way to depend on one of those libraries
  * `build-tool-depends` is a great starting point, as it leverages new-build's per-component-builds
* Define a syntax for depending on a specific library in the `build-depends` field of the cabal file format
  * In particular, defining the version bounds syntax is tricky and the solution should be evaluated carefully

This proposal also affects other parts of the Haskell ecosystem, which will need to be adapted accordingly:

* Hackage needs an interface to navigate libraries
* Haddock needs to be modified too

<!-- ### Approximate timeline. -->
### Timeline highlights

**Community Bonding period**

Maybe take advantage of this period to compensate the [first week of August](#cubscouts)

-- TODO More timeline details
<!--
**May, second half**
**June, first half**
**June, second half**
**July, first half**
**July, second half**
-->

<a name="cubscouts">
**August, first week**

Unfortunately this week overlaps with my cub scouts camp, so I won't be able to do any work during this time.

---

The latest version of this proposal is available in markdown format
[here](http://github.com/fgaz/gsoc/blob/master/2018/proposal-cabal-multiple-libraries.md).

