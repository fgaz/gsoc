# Final report for GSoC 2018

**Haskell.org - Multiple Libraries project - Francesco Gazzetta**

## Work done

### Main "multiple libraries" PR

The majority of my work is available in PR [#5526](https://github.com/haskell/cabal/pull/5526)

It implements support for multiple libraries per package, as described in
[#4206](https://github.com/haskell/cabal/issues/4206).

There are some `TODO`s in it. See "Future work" for more info.

EDIT: Since I'm going to do more work on that pr and I'll likely rewrite the
git history, I created another branch and pr to record the state of the
`multiple-libraries-v4` branch at the moment GSoC2018 ended:
[#5528](https://github.com/haskell/cabal/pull/5528)

#### Trying it

You can try the multiple libraries functionality by building the example project
that can be found [here](./example-project/).

Just run `cabal new-build`.

### Other work: refactors

While working on the main functionality I noticed a few much-needed refactor
opportunities.

#### The `Dependency`-as-constraint problem

In cabal, `Dependency` is used in two seemingly related but now incompatible
ways:

* As a dependency specification ("I want this component from this package, version range xy")
* As a constraint ("Package p must be in version range xy")

During GSoC I repeatedly tried to separate the two concepts.
My first early attempt failed due to missing knowledge about the codebase.
The second attempt is a rebase of [#4265](https://github.com/haskell/cabal/pull/4265)
and can be found on my [`library-dependency` branch](https://github.com/fgaz/cabal/tree/library-dependency).
Multilibs took a different direction though (multiple components in a single Dependency),
so that patch is again unmergeable and will probably have to be redone.
Still, it can be used as a reference.

#### The pervasive `Maybe UnqualComponentName`

`Maybe UnqualComponentName` was used pervasively to mean "a library name", where
`Nothing` is the main library and `Just libname` a sublibrary.
In my main pr I added a new datatype to substitute it: `LibraryName`.

I wasn't able, though, to replace all uses of `Maybe UnqualComponentName`
without breaking functionality, so this branch is still unmerged.

The full refactor can be found on my
[`maybeunqual-to-libraryname` branch](https://github.com/fgaz/cabal/tree/maybeunqual-to-libraryname).

## Future work

The main work is done, but there's still some mostly small things to do and polish.

### Cleanups

* Clean the history of the pr
* Fix the few tests that still fail (mostly for missing imports and differing output)
* Make sure roundtripping works
* Clean up a few functions where I left TODOs. Mostly code that will be removed
  if BC handling is moved to a single place.
* I used an empty set wherever I couldn't come up with a sensible Dependency default.
  I now know better and I should fix it. (it doesn't impact functionality but it could one day)

### Missing features

* A `visibility = public | private` field to restrict access to sublibraries
* Version guard the new functionality and forbid the old one in new cabal versions
* Tests! Possibly lots of them! And docs

### Other work

* Maybe: remove all the bits of code that handle the old syntax (there's lots
  of them) and do it all in a single spot.
* Unrelated but really needed in my opinion: Merge the two refactors I mentioned above

## Challenges & learnings

...a blog post will follow :)

