# Formalizing Barbed Similarity for the $\pi$-Calculus in Beluga

## Overview

This repository contains a formalization of the context lemma for barbed similarity in the $\pi$-calculus in [Beluga](https://complogic.cs.mcgill.ca/beluga/index.html), as requested by the third challenge of the [Concurrent Calculi Formalisation Benchmark](https://concurrentbenchmark.github.io/).

A more detailed description of the code structure can be found [here](https://github.com/LeaTrogni/formalizing-barbed-similarity-for-the-pi-calculus-in-beluga/blob/main/code/README.md).



## Usage instructions

Once Beluga is installed and the correct opam switch is enabled (instructions can be found [here](https://github.com/Beluga-lang/Beluga/blob/master/INSTALL)), it suffices to run

```Shell
beluga code/sources.cfg 
```

to perform the type reconstruction of the formalization. Expected result is

```Shell
## Type Reconstruction begin: ./code/1_definitions.bel ##
## Type Reconstruction done:  ./code/1_definitions.bel ##
## Type Reconstruction begin: ./code/2_lts.bel ##
## Type Reconstruction done:  ./code/2_lts.bel ##
## Type Reconstruction begin: ./code/3_cong_equiv.bel ##
## Type Reconstruction done:  ./code/3_cong_equiv.bel ##
## Type Reconstruction begin: ./code/4_contexts.bel ##
## Type Reconstruction done:  ./code/4_contexts.bel ##
## Type Reconstruction begin: ./code/5_barbsim.bel ##
## Type Reconstruction done:  ./code/5_barbsim.bel ##
## Type Reconstruction begin: ./code/6_structcong_in_barbsim.bel ##
## Type Reconstruction done:  ./code/6_structcong_in_barbsim.bel ##
## Type Reconstruction begin: ./code/7_barbs_equiv.bel ##
## Type Reconstruction done:  ./code/7_barbs_equiv.bel ##
## Type Reconstruction begin: ./code/8_barbprecong.bel ##
## Type Reconstruction done:  ./code/8_barbprecong.bel ##
## Type Reconstruction begin: ./code/9_context_lemma.bel ##
## Type Reconstruction done:  ./code/9_context_lemma.bel ##
```