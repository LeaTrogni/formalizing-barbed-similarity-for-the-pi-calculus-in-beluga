# Project files overview

```
sources.cfg                           Collects all Beluga source files and it is used to execute them  
1_definitions.bel                     Contains the encoding of syntax, semantics and behavioural equivalences of the pi-calculus  
2_lts.bel                             Contains proofs of basic properties of the late semantics LTS  
3_cong_equiv.bel                      Contains the proof of the equivalence between two different encodings of structural congruence  
4_contexts.bel                        Contains the proofs of some properties of (process) contexts  
5_barbsim.bel                         Contains the proofs of some properties of barbed similarity  
6_structcong_in_barbsim.bel           Contains the proof of the inclusion of structural congruence in barbed similarity  
7_counterexample.bel                  Contains the proof that the same pair of contexts is barbed similar and not barbed similar at the same time
```

The first six files are the same as the ones in the original project, with one critical difference: the LTS is missing two replication rules for communication. For this reason, there are some holes the proof of a theorem in the sixth file, which we recall below.  

- `cong_fstepleft_impl_fstepright and cong_fstepright_impl_fstepleft and cong_bstepleft_impl_bstepright and cong_bstepright_impl_bstepleft`  
If $P \equiv Q$, then
  - if $P \xrightarrow{\alpha} P'$, then there exists some $Q'$ such that $Q \xrightarrow{\alpha} Q'$ and $P' \equiv Q'$;
  - if $Q \xrightarrow{\alpha} Q'$, then there exists some $P'$ such that $P \xrightarrow{\alpha} P'$ and $P' \equiv Q'$.

The last file shows that, assuming that the theorem with holes is true, it is possible to prove that a pair of processes is barbed similar, but, at the same time, it is possible to prove that the same pair is not barbed similar. Therefore, supposing that these LTS rules are preserved by structural congruence leads to a contradiction. Below is an informal proof of the same result, which explains the notation used in the comments of the last file.

Let $P \coloneqq \nu z (\overline{a}y.\overline{z}y.0 \mid a(x).z(x).b(x).0)$

Then, using the restriction, parallel and communication rules, $P$ can perform three different transitions:

- $P \xrightarrow{\overline{a}y} Q_1 \coloneqq \nu z (\overline{z}y.0 \mid a(x).z(x).b(x).0)$
- $P \xrightarrow{a(x)} Q_2 \coloneqq \nu z (\overline{a}y.\overline{z}y.0 \mid z(x).b(x).0)$
- $P \xrightarrow{\tau} Q_3 \coloneqq \nu z (\overline{z}y.0 \mid z(x).b(x).0)$

Using the replication rule, $!P$ can perform three corresponding transitions:

- $!P \xrightarrow{\overline{a}y} Q_1 \mid !P$
- $!P \xrightarrow{a(x)} Q_2 \mid !P$
- $!P \xrightarrow{\tau} Q_3 \mid !P$

Then, using again the left communication rule, we can obtain the following transition

$P \mid !P \xrightarrow{\tau} Q_1 \mid (Q_2 \mid !P)$

Is it matched by an action performed by $!P$? The only candidate is 

$!P \xrightarrow{\tau} Q_3 \mid !P$, 

so let’s suppose that 

$Q_1 \mid (Q_2 \mid !P) \equiv Q_3 \mid !P$.

This implies that 

$Q_1 \mid (Q_2 \mid !P) \ \dot{\leq} \ Q_3 \mid !P$.

At the same time, we can see that

$Q_3 \mid !P \xrightarrow{\tau} R \coloneqq \nu z(0 \mid b(x).0)$

and $R \downarrow_b$, but no $\tau$ transition from $Q_1 \mid (Q_2 \mid !P)$ has a result which has the barb $b$, hence 

$Q_1 \mid (Q_2 \mid !P) \ \dot{\not\leq} \ Q_3 \mid !P$.