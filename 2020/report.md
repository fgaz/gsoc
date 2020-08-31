# Final report - GSoC 2020 - `SPECIALIZABLE` pragma

## What was done

* [A prototype](https://gitlab.haskell.org/ghc/ghc/-/merge_requests/3630) was
  implemented. It demonstrates the behaviour of the pragma, but its design has
  a few flaws that would prevent integration with GHC.
* [A GHC proposal](https://github.com/ghc-proposals/ghc-proposals/pull/357)
  was written to propose the change to the wider community.
* [A final version of the implementation](https://gitlab.haskell.org/fgaz/ghc/-/tree/specializable/basic1try4)
  was written. This version has a cleaner and more maintainable design, and
  will supercede the prototype.

## What still needs to be done

* The new works on most cases, but still has a few imperfections due to this issue:
  * The pragma isn't propagated to references to the specialized function.
    The issues caused by this are marked as FIXME in the code.

## In the future

* The possible extensions mentioned in the GHC proposal can be considered

