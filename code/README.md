# Project files overview

```
sources.cfg                           Collects all Beluga source files and it is used to execute them  
1_definitions.bel                     Contains the encoding of syntax, semantics and behavioural equivalences of the pi-calculus  
2_lts.bel                             Contains proofs of basic properties of the late semantics LTS  
3_cong_equiv.bel                      Contains the proof of the equivalence between two different encodings of structural congruence  
4_contexts.bel                        Contains the proofs of some properties of (process) contexts  
5_barbsim.bel                         Contains the proofs of some properties of barbed similarity  
6_structcong_in_barbsim.bel           Contains the proof of the inclusion of structural congruence in barbed similarity  
7_barbs_equiv.bel                     Contains the proof of the equivalence between two different encodings of barbs  
8_barbprecong.bel                     Contains the proofs of some properties of barbed precongruence  
9_context_lemma.bel                   Contains the proof of the context lemma  
```

Below is a more detailed list of the contents of each file. The notations for the syntax and operational semantics are from [The Pi-Calculus - A Theory of Mobile Processes](https://doi.org/10.1017/9781316134924)

## 1_definitions.bel

All the definitions used in the other files (except for some auxiliary inductive definitions used instead of the existential quantifier in theorem statements).

List of the currently defined types and schemas:

- `LF names: type`  
(empty, all names are variables from the schema `ctx`, ranged over by $x,y,z,\dots$)
- `LF proc: type`  
(processes, ranged over by $P,Q,R,\dots$)
- `schema ctx = names`  
(context schema for names)
- `LF eqp: proc → proc → type`  
(equality of processes)
- `LF eqn: names → names → type`
- `LF not_possible : type`  
(empty type, used for negation)
- `inductive PCtx : (g: ctx) (g': ctx) [g' ⊢ proc ] →  [g ⊢ proc ] → [g' ⊢ proc ] →  [g ⊢ proc ] → ctype`  
(relation used to model process contexts: $(P,P',Q,Q')$ belongs to the relation if there exists some context $C$ such that $P'=C[P]$ and $Q'=C[Q]$)
- `LF cong: proc → proc → type`  
(structural congruence, defined using compatibility laws)
- `inductive Cong: (g:ctx) [g |- proc] -> [g |- proc] → ctype`  
(structural congruence, defined using contextual closure, denoted by $\equiv$)
- `LF f_act: type`, `LF b_act: type`  
(free and bound actions)
- `LF eqf: f_act → f_act → type`, `LF eqb: b_act → b_act → type`  
(equalities of actions)
- `LF fstep: proc → f_act → proc → type`  
`and bstep: proc → b_act → (names → proc) → type`  
(transition relation that defines the late semantics LTS, transitions are denoted by $P \xrightarrow{\alpha} Q$)
- `inductive Barb_in : (g:ctx) [g |- proc] -> [g |- names] -> ctype`  
(observable input action defined via LTS, denoted by $\downarrow_x$)
- `inductive Barb_out : (g:ctx) [g |- proc] -> [g |- names] -> ctype`  
(observable output action defined via LTS, denoted by $\downarrow_{\overline{x}}$)
- `LF barb_in_rew: proc → names → type`  
(process of the form $\nu \vec{z} (x(y).P \mid Q)$, associated to the name $x$)
- `LF barb_in_struct : proc → names → type`  
(structural definition of observable input action)
- `LF barb_out_rew: proc → names → type`  
(process of the form $\nu \vec{z} (\overline{x}y.P \mid Q)$, associated to the name $x$)
- `LF barb_out_struct : proc → names → type`  
(structural definition of observable output action)
- `coinductive BarbSim : (g:ctx) [g |- proc] -> [g |- proc] -> ctype`  
(barbed similarity, denoted by $\dot{\leq}$)
- `coinductive BarbSimUpTo : (g:ctx) [g |- proc] -> [g |- proc] -> ctype`  
(barbed similarity up to)
- `inductive BarbPre: (g':ctx) [g' |- proc] -> [g' |- proc] -> ctype`  
(barbed precongruence, denoted by $\leq_B$)
- `inductive BarbPre': (g:ctx) [g |- proc] -> [g |- proc] -> ctype`  
(characterization of barbed precongruence used in context lemma, denoted by $\leq_{\mid \sigma}$)
- `coinductive LargPreCong: (g':ctx) [g' |- proc] -> [g' |- proc] -> ctype`  
(largest precongruence included in barbed similarity)

 ## 2_lts.bel

Basic properties of the operational semantics

- `res_or_open`  
 If $P \xrightarrow{\overline{x}y} P'$ and $z$ is a name, then there are two possible cases: if $z=y$, then $\nu z\, P \xrightarrow{\overline{x}(z)} P'$, otherwise $\nu z\, P \xrightarrow{\overline{x}y} \nu z\, P'$.
- `par_rep`  
 If $S \mid !P \xrightarrow{\tau} R$, then there is $Q$ such that $S \mid (P \mid P) \xrightarrow{ \tau} Q$ and $R \equiv Q \mid !P$.

## 3_cong_equiv.bel
Proof that `cong` and `Cong` are equivalent

- `PCtxtocong` is an additional lemma that proves that congruence defined with compatibility laws is context closed.
- `congtoCong` and `Congtocong` are the two inclusions that show the main result.

## 4_contexts.bel
Properties of contexts (defined as in `PCtx`).  
The notation $((P,C_P),(Q,C_Q))$ means that there exists a context $C$ such that $C[P] = C_P$ and $C[Q] = C_Q$.

- `PCtx_symm`  
If $((P,C_P),(Q,C_Q))$ then $((Q,C_Q),(P,C_P))$.
- `PCtx_trans`  
If $((P,C_P),(R,C_R))$ and $Q$ is another process, then there exists $C_Q$ such that $((P,C_P),(Q,C_Q))$ and $((Q,C_Q),(R,C_R))$.
- `PCtx_funct`  
If $((P,C_P),(P,C_P'))$ then $C_P = C_P'$.
- `PCtx_comp`  
If $((P,C_P),(Q,C_Q))$ and $((C_P,C_{C_P}),(C_Q,C_{C_Q}))$, then $((P,C_{C_P}),(Q,C_{C_Q}))$.
- `PCtx_preserves_barb_in`  
If for each name $x$ $P \downarrow_x$ implies $Q \downarrow_x$ and $((P,C_P),(Q,C_Q))$, then for each name $x$ $C_P \downarrow_x$ implies $C_Q \downarrow_x$. 
- `PCtx_preserves_barb_out`  
If for each name $x$ $P \downarrow_{\overline{x}}$ implies $Q \downarrow_{\overline{x}}$ and $((P,C_P),(Q,C_Q))$, then for each name $x$ $C_P \downarrow_{\overline{x}}$ implies $C_Q \downarrow_{\overline{x}}$.  

## 5_barbsim.bel

Properties of barbed similarity (`BarbSim`, $\dot{\leq}$).

- `barb_sim_refl`, `barb_sim_trans`  
Barbed similarity is a preorder.
- `in_barb_sim`, `out_barb_sim`, `res_barb_sim`  
Barbed similarity is compatible with input prefix, output prefix and restriction.
- `barbsim_implies_barbsimupto`, `barbsimupto_implies_barbsim`  
Barbed similarity and barbed similarity up to $\dot{\leq}$ are the same relation.

## 6_structcong_in_barbsim.bel

Inclusion of structural congruence in barbed similarity.

- `strengthen_fstep and rec strengthen_bstep`  
If $x$ does not occur free in $P$ and $P \xrightarrow{\alpha} P'$, then $x$ does not occur free in $\alpha$ and $P'$.
- `cong_fstepleft_impl_fstepright and cong_fstepright_impl_fstepleft and cong_bstepleft_impl_bstepright and cong_bstepright_impl_bstepleft`  
If $P \equiv Q$, then
  - if $P \xrightarrow{\alpha} P'$, then there exists some $Q'$ such that $Q \xrightarrow{\alpha} Q'$ and $P' \equiv Q'$;
  - if $Q \xrightarrow{\alpha} Q'$, then there exists some $P'$ such that $P \xrightarrow{\alpha} P'$ and $P' \equiv Q'$.
- `structcong_implies_barbsim_left`  
If $P \equiv Q$, then $P \ \dot{\leq} \ Q$.
- `structcong_implies_barbsim_right`  
If $Q \equiv P$, then $P \ \dot{\leq} \ Q$.

## 7_barbs_equiv.bel

Equivalence between barbs defined via LTS (`Barb_in`, `Barb_out`) and structurally (`barb_in_struct`, `barb_out_struct`).

- `res_new`  
If $x$ does not occur in $P$, then $\nu x P \equiv P$.
- `barb_in_red_to_Barb_in`  
If $P \equiv \nu \vec{z} (x(y).Q \mid R)$, where $x \notin \vec{z}$, then $P \downarrow_x$.
- `barb_in_rew_par`  
If $P \equiv \nu \vec{z} (x(y).Q \mid R)$ and $w \neq x$, then also $\nu w(P \mid P') \equiv \nu \vec{z'} (x(y').Q' \mid R')$ for some $\vec{z'},y',Q',R'$.
- `barb_in_cong`  
If $P \equiv \nu \vec{z} (x(y).Q \mid R)$ and $Q \equiv P$, then $Q \equiv \nu \vec{z} (x(y).Q \mid R)$.
- `Barb_in_to_barb_in_red`  
If $P \downarrow_x$, then $P \equiv \nu \vec{z} (x(y).Q \mid R)$, for some $\vec{z}, y, Q, R$ such that $x \notin \vec{z}$.
- `barb_out_red_to_Barb_out`  
If $P \equiv \nu \vec{z} (\overline{x}y.Q \mid R)$, where $x \notin \vec{z}$, then $P \downarrow_{\overline{x}}$.
- `barb_out_rew_par`  
If $P \equiv \nu \vec{z} (\overline{x}y.Q \mid R)$ and $w \neq x$, then also $\nu w(P \mid P') \equiv \nu \vec{z'} (\overline{x}y'.Q' \mid R')$ for some $\vec{z'},y',Q',R'$.
- `barb_out_cong`  
If $P \equiv \nu \vec{z} (\overline{x}y.Q \mid R)$ and $Q \equiv P$, then $Q \equiv \nu \vec{z} (\overline{x}y.Q \mid R)$.
- `Barb_out_to_barb_out_red`  
If $P \downarrow_{\overline{x}}$, then $P \equiv \nu \vec{z} (\overline{x}y.Q \mid R)$, for some $\vec{z}, y, Q, R$ such that $x \notin \vec{z}$.


## 8_barbprecong.bel
Properties of barbed precongruence (`BarbPre`, $\leq_B$).

- `barbpre_refl`, `barbpre_trans`  
Barbed precongruence is a preorder.
- `barbpre_implies_barbsim`  
Barbed precongruence is included in barbed similarity.
- `barbpre_precong`  
Barbed precongruence is a precongruence.
- `barbpre_implies_largprecong`, `largprecong_implies_barbpre`  
Barbed precongruence is the largest precongruence included in barbed similarity.
- `structcong_implies_largprecong`  
Structural congruence is included in the largest precongruence included in barbed similarity.

## 9_context_lemma.bel

Properties of $\leq_{\mid\sigma}$ (`BarbPre'`) and context lemma.

- `strength_subst_in`, `strength_subst_out_b`, `strength_subst_out_f`  
 If $P \sigma \xrightarrow{\alpha} P'$ and $\alpha \neq \tau$, then there exists $\beta, P''$ such that $P \xrightarrow{\beta} P''$, $\alpha = \beta \sigma$.
- `barbpre'_implies_barbsim`  
$\leq_{\mid\sigma}$ is included in barbed similarity.
- `barbpre'_in`  
$\{ (x(y).(P \sigma) \mid R, x(y).(Q \sigma) \mid R) \mid P \leq_{\mid \sigma} Q\} \cup \dot{\leq}$ is included in barbed similarity
- `barbpre'_out`  
$\{ (\overline{x}y.(P \sigma) \mid R, \overline{x}y.(Q \sigma) \mid R) \mid P \leq_{\mid \sigma} Q\} \cup \dot{\leq}$ is included in barbed similarity
- `barbpre'_rep`  
$\{ (!(P \sigma) \mid R, !(Q \sigma) \mid R) \mid P \leq_{\mid \sigma} Q\}$ is included in barbed similarity up to $\dot{\leq}$
- `barbpre'_is_precong`  
$\leq_{\mid\sigma}$ is a precongruence.
- `barbpre'_implies_largprecong`  
$\leq_{\mid\sigma}$ is included in the largest precongruence included in barbed similarity.
- `context_lemma`  
$\leq_{\mid\sigma}$ is included in barbed precongruence.