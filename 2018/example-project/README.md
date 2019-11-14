# Example project for the "multiple libraries in a single package" Cabal feature

Try it by running `cabal v2-build all` in this directory
(you have to use GHC>=8.8).
You'll see that sublibraries are correctly build and used by other packages:

```
$ cabal v2-build all
Resolving dependencies...
Build profile: -w ghc-8.8.1 -O1
In order, the following will be built (use -v for more details):
 - b-0.1.0.0 (lib) (configuration changed)
 - b-0.1.0.0 (lib:anothersublib) (configuration changed)
 - b-0.1.0.0 (lib:sublib) (configuration changed)
 - a-0.1.0.0 (exe:e) (first run)
 - a-0.1.0.0 (lib) (first run)
Configuring library 'anothersublib' for b-0.1.0.0..
Configuring library for b-0.1.0.0..
Preprocessing library for b-0.1.0.0..
Preprocessing library 'anothersublib' for b-0.1.0.0..
Building library 'anothersublib' for b-0.1.0.0..
Building library for b-0.1.0.0..
Configuring library 'sublib' for b-0.1.0.0..
Preprocessing library 'sublib' for b-0.1.0.0..
Building library 'sublib' for b-0.1.0.0..
Configuring executable 'e' for a-0.1.0.0..
Configuring library for a-0.1.0.0..
Preprocessing executable 'e' for a-0.1.0.0..
Building executable 'e' for a-0.1.0.0..
Preprocessing library for a-0.1.0.0..
Building library for a-0.1.0.0..
[1 of 1] Compiling Main             ( A.hs, /home/fgaz/gsoc/2018/example-project/dist-newstyle/build/x86_64-linux/ghc-8.8.1/a-0.1.0.0/x/e/build/e/e-tmp/Main.o )
[1 of 1] Compiling A1               ( A1.hs, /home/fgaz/gsoc/2018/example-project/dist-newstyle/build/x86_64-linux/ghc-8.8.1/a-0.1.0.0/build/A1.o )
Linking /home/fgaz/gsoc/2018/example-project/dist-newstyle/build/x86_64-linux/ghc-8.8.1/a-0.1.0.0/x/e/build/e/e ...
```

Now try to change the visibility of a sublibrary to private:
edit `b/b.cabal` and replace `public` with `private` on one of the sublibraries.
Then delete `dist-newstyle` (see note at the end) and try to rebuild:

```
$ rm -r dist-newstyle
$ cabal v2-build all
Resolving dependencies...
Build profile: -w ghc-8.8.1 -O1
In order, the following will be built (use -v for more details):
 - b-0.1.0.0 (lib) (first run)
 - b-0.1.0.0 (lib:anothersublib) (first run)
 - b-0.1.0.0 (lib:sublib) (first run)
 - a-0.1.0.0 (exe:e) (first run)
 - a-0.1.0.0 (lib) (first run)
Configuring library for b-0.1.0.0..
Configuring library 'anothersublib' for b-0.1.0.0..
Preprocessing library for b-0.1.0.0..
Building library for b-0.1.0.0..
Preprocessing library 'anothersublib' for b-0.1.0.0..
Building library 'anothersublib' for b-0.1.0.0..
[1 of 1] Compiling BTop             ( BTop.hs, /home/fgaz/gsoc/2018/example-project/dist-newstyle/build/x86_64-linux/ghc-8.8.1/b-0.1.0.0/build/BTop.o )
Configuring library 'sublib' for b-0.1.0.0..
Preprocessing library 'sublib' for b-0.1.0.0..
Building library 'sublib' for b-0.1.0.0..
[1 of 1] Compiling BSub             ( BSub.hs, /home/fgaz/gsoc/2018/example-project/dist-newstyle/build/x86_64-linux/ghc-8.8.1/b-0.1.0.0/l/sublib/build/sublib/BSub.o )
Configuring executable 'e' for a-0.1.0.0..
Configuring library for a-0.1.0.0..
cabal: Encountered missing or private dependencies:
b : {anothersublib, sublib} ==0.1.0.0

cabal: Encountered missing or private dependencies:
b : {anothersublib, sublib} ==0.1.0.0

cabal: Failed to build a-0.1.0.0. The failure occurred during the configure
step.
Failed to build exe:e from a-0.1.0.0. The failure occurred during the
configure step.
```

**Note**: there seems to be a problem with reconfiguration avoidance. `cabal-install` does not reconfigure `a`, so the visibility change does not take effect until a reconfigure. I'm currently investigating this. Fortunately this does not affect released packages, since they are immutable.

