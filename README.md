# Formalizing Barbed Similarity for the $\pi$-Calculus in Beluga

## Overview

This repository contains a formalization of the context lemma for barbed similarity in the $\pi$-calculus in [Beluga](https://complogic.cs.mcgill.ca/beluga/index.html), as requested by the third challenge of the [Concurrent Calculi Formalisation Benchmark](https://concurrentbenchmark.github.io/).

A more detailed description of the code structure can be found [here](https://github.com/LeaTrogni/formalizing-barbed-similarity-for-the-pi-calculus-in-beluga/blob/main/code/README.md).

We also present an argument against a different LTS definition, which can be found [here](https://github.com/LeaTrogni/formalizing-barbed-similarity-for-the-pi-calculus-in-beluga/blob/main/code/replication_rule_change/README.md).


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

For the LTS change, it is sufficient to run 

```Shell
beluga code/replication_rule_change/sources.cfg 
```

to perform the type reconstruction of the formalization. Since there are some holes, expected result is

```Shell
## Type Reconstruction begin: code/replication_rule_change/1_definitions.bel ##
## Type Reconstruction done:  code/replication_rule_change/1_definitions.bel ##
## Type Reconstruction begin: code/replication_rule_change/2_lts.bel ##
## Type Reconstruction done:  code/replication_rule_change/2_lts.bel ##
## Type Reconstruction begin: code/replication_rule_change/3_cong_equiv.bel ##
## Type Reconstruction done:  code/replication_rule_change/3_cong_equiv.bel ##
## Type Reconstruction begin: code/replication_rule_change/4_contexts.bel ##
## Type Reconstruction done:  code/replication_rule_change/4_contexts.bel ##
## Type Reconstruction begin: code/replication_rule_change/5_barbsim.bel ##
## Type Reconstruction done:  code/replication_rule_change/5_barbsim.bel ##
## Type Reconstruction begin: code/replication_rule_change/6_structcong_in_barbsim.bel ##
## Type Reconstruction done:  code/replication_rule_change/6_structcong_in_barbsim.bel ##
## Holes: code/replication_rule_change/6_structcong_in_barbsim.bel  ##
File "code/replication_rule_change/6_structcong_in_barbsim.bel", line 528,
column 69: Hole number 0, <anonymous>
  Meta-context:
    g : ctx
    P3 : (g |- proc)
    F1 : (g |- fstep P3 (f_out X Y) P')
    B1' : (g |- bstep P3 (b_in X) (\x42. P'1))
  Computation context:
    c : [g |- cong (p_rep P3) (P3 p_par (p_rep P3))]
    f :
      [g |-
         fstep
           (P3 p_par (p_rep P3))
           f_tau (P' p_par ((P'1[.., Y]) p_par (p_rep P3)))]
  Goal:
  [g |-
     ex_fstepcong
       (P3 p_par (p_rep P3))
       (p_rep P3) f_tau (P' p_par ((P'1[.., Y]) p_par (p_rep P3)))]
File "code/replication_rule_change/6_structcong_in_barbsim.bel", line 529,
column 69: Hole number 1, <anonymous>
  Meta-context:
    g : ctx
    P3 : (g |- proc)
    B1 : (g |- bstep P3 (b_in X) (\x42. P'))
    F1' : (g |- fstep P3 (f_out X Y) P'1)
  Computation context:
    c : [g |- cong (p_rep P3) (P3 p_par (p_rep P3))]
    f :
      [g |-
         fstep
           (P3 p_par (p_rep P3))
           f_tau ((P'[.., Y]) p_par (P'1 p_par (p_rep P3)))]
  Goal:
  [g |-
     ex_fstepcong
       (P3 p_par (p_rep P3))
       (p_rep P3) f_tau ((P'[.., Y]) p_par (P'1 p_par (p_rep P3)))]
File "code/replication_rule_change/6_structcong_in_barbsim.bel", line 530,
column 71: Hole number 2, <anonymous>
  Meta-context:
    g : ctx
    P3 : (g |- proc)
    B1 : (g |- bstep P3 (b_out X) (\x42. P'))
    B2' : (g |- bstep P3 (b_in X) (\x42. P'1))
  Computation context:
    c : [g |- cong (p_rep P3) (P3 p_par (p_rep P3))]
    f :
      [g |-
         fstep
           (P3 p_par (p_rep P3))
           f_tau (p_res (\z. (P' p_par (P'1 p_par (p_rep (P3[..]))))))]
  Goal:
  [g |-
     ex_fstepcong
       (P3 p_par (p_rep P3))
       (p_rep P3) f_tau (p_res (\z. (P' p_par (P'1 p_par (p_rep (P3[..]))))))]
File "code/replication_rule_change/6_structcong_in_barbsim.bel", line 531,
column 71: Hole number 3, <anonymous>
  Meta-context:
    g : ctx
    P3 : (g |- proc)
    B1 : (g |- bstep P3 (b_in X) (\x42. P'))
    B2' : (g |- bstep P3 (b_out X) (\x42. P'1))
  Computation context:
    c : [g |- cong (p_rep P3) (P3 p_par (p_rep P3))]
    f :
      [g |-
         fstep
           (P3 p_par (p_rep P3))
           f_tau (p_res (\z. (P' p_par (P'1 p_par (p_rep (P3[..]))))))]
  Goal:
  [g |-
     ex_fstepcong
       (P3 p_par (p_rep P3))
       (p_rep P3) f_tau (p_res (\z. (P' p_par (P'1 p_par (p_rep (P3[..]))))))]
## Type Reconstruction begin: code/replication_rule_change/7_counterexample.bel ##
## Type Reconstruction done:  code/replication_rule_change/7_counterexample.bel ##
## Holes: code/replication_rule_change/7_counterexample.bel  ##
File "code/replication_rule_change/6_structcong_in_barbsim.bel", line 528,
column 69: Hole number 0, <anonymous>
  Meta-context:
    g : ctx
    P3 : (g |- proc)
    F1 : (g |- fstep P3 (f_out X Y) P')
    B1' : (g |- bstep P3 (b_in X) (\x42. P'1))
  Computation context:
    c : [g |- cong (p_rep P3) (P3 p_par (p_rep P3))]
    f :
      [g |-
         fstep
           (P3 p_par (p_rep P3))
           f_tau (P' p_par ((P'1[.., Y]) p_par (p_rep P3)))]
  Goal:
  [g |-
     ex_fstepcong
       (P3 p_par (p_rep P3))
       (p_rep P3) f_tau (P' p_par ((P'1[.., Y]) p_par (p_rep P3)))]
File "code/replication_rule_change/6_structcong_in_barbsim.bel", line 529,
column 69: Hole number 1, <anonymous>
  Meta-context:
    g : ctx
    P3 : (g |- proc)
    B1 : (g |- bstep P3 (b_in X) (\x42. P'))
    F1' : (g |- fstep P3 (f_out X Y) P'1)
  Computation context:
    c : [g |- cong (p_rep P3) (P3 p_par (p_rep P3))]
    f :
      [g |-
         fstep
           (P3 p_par (p_rep P3))
           f_tau ((P'[.., Y]) p_par (P'1 p_par (p_rep P3)))]
  Goal:
  [g |-
     ex_fstepcong
       (P3 p_par (p_rep P3))
       (p_rep P3) f_tau ((P'[.., Y]) p_par (P'1 p_par (p_rep P3)))]
File "code/replication_rule_change/6_structcong_in_barbsim.bel", line 530,
column 71: Hole number 2, <anonymous>
  Meta-context:
    g : ctx
    P3 : (g |- proc)
    B1 : (g |- bstep P3 (b_out X) (\x42. P'))
    B2' : (g |- bstep P3 (b_in X) (\x42. P'1))
  Computation context:
    c : [g |- cong (p_rep P3) (P3 p_par (p_rep P3))]
    f :
      [g |-
         fstep
           (P3 p_par (p_rep P3))
           f_tau (p_res (\z. (P' p_par (P'1 p_par (p_rep (P3[..]))))))]
  Goal:
  [g |-
     ex_fstepcong
       (P3 p_par (p_rep P3))
       (p_rep P3) f_tau (p_res (\z. (P' p_par (P'1 p_par (p_rep (P3[..]))))))]
File "code/replication_rule_change/6_structcong_in_barbsim.bel", line 531,
column 71: Hole number 3, <anonymous>
  Meta-context:
    g : ctx
    P3 : (g |- proc)
    B1 : (g |- bstep P3 (b_in X) (\x42. P'))
    B2' : (g |- bstep P3 (b_out X) (\x42. P'1))
  Computation context:
    c : [g |- cong (p_rep P3) (P3 p_par (p_rep P3))]
    f :
      [g |-
         fstep
           (P3 p_par (p_rep P3))
           f_tau (p_res (\z. (P' p_par (P'1 p_par (p_rep (P3[..]))))))]
  Goal:
  [g |-
     ex_fstepcong
       (P3 p_par (p_rep P3))
       (p_rep P3) f_tau (p_res (\z. (P' p_par (P'1 p_par (p_rep (P3[..]))))))]
```