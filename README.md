# 1_definitions.bel

All the definitions used in the other files (except for some auxiliary inductive definitions used instead of the existential quantifier in theorem statements).

List of the currently defined types and schemas:

- `LF names: type`  
(empty, all names are variables from the schema `ctx`)
- `LF proc: type`  
(processes)
- `schema ctx = names`  
(context schema for names)
- `LF eqp: proc → proc → type`  
(equality of processes)
- `LF eqn: names → names → type`
- `LF not_possible : type`  
(empty type, used for negation)
- `inductive pctxM : (g: ctx) (g': ctx) [g' ⊢ proc ] →  [g ⊢ proc ] → [g' ⊢ proc ] →  [g ⊢ proc ] → ctype`  
(`pctxM P CP Q CQ` means that there is some context such that `CP` is the image of `P` and `CQ` is the image of `Q`)
- `LF cong: proc → proc → type`  
(structural congruence, defined using compatibility laws)
- `inductive CongM: (g:ctx) [g |- proc] -> [g |- proc] → ctype`  
(structural congruence, defined using contexts)
- `LF f_act: type`, `LF b_act: type`  
(free and bound actions)
- `LF eqf: f_act → f_act → type`, `LF eqb: b_act → b_act → type`  
(equalities of actions)
- `LF fstep: proc → f_act → proc → type`  
`and bstep: proc → b_act → (names → proc) → type`  
(transition relation)
- `inductive Barb_in : (g:ctx) [g |- proc] -> [g |- names] -> ctype`  
(observable input action)
- `inductive Barb_out : (g:ctx) [g |- proc] -> [g |- names] -> ctype`  
(observable output action)
- `LF barb_in_rew: proc → names → type`  
(process of the form $\nu \vec{z} (x(y).P \mid Q)$, associated to the name $x$)
- `LF barb_in_struct : proc → names → type`  
(alternative definition of observable input action)
- `LF barb_out_rew: proc → names → type`  
(process of the form $\nu \vec{z} (\overline{x}y.P \mid Q)$, associated to the name $x$)
- `LF barb_out_struct : proc → names → type`  
(alternative definition of observable output action)
- `coinductive BarbSim : (g:ctx) [g |- proc] -> [g |- proc] -> ctype`  
(barbed similarity)
- `coinductive BarbSimUpTo : (g:ctx) [g |- proc] -> [g |- proc] -> ctype`  
(barbed similarity up to)
- `inductive BarbPre: (g':ctx) [g' |- proc] -> [g' |- proc] -> ctype`  
(barbed precongruence)
- `inductive BarbPre': (g:ctx) [g |- proc] -> [g |- proc] -> ctype`  
(characterization of barbed precongruence used in context lemma)
- `coinductive LargPreCong: (g':ctx) [g' |- proc] -> [g' |- proc] -> ctype`  
(largest precongruence included in barbed similarity)

 # 2_lts.bel

Basic properties of the operational semantics

- `res_or_open`  
 If $P \xrightarrow{\overline{x}y} P'$ and $z$ is a name, then there are two possible cases: if $z=y$, then $\nu z\, P \xrightarrow{\overline{x}(z)} P'$, otherwise $\nu z\, P \xrightarrow{\overline{x}y} \nu z\, P'$.
- `par_rep`  
 If $S \mid !P \xrightarrow{\tau} R$, then there is $Q$ such that $S \mid (P \mid P) \xrightarrow{ \tau} Q$ and $R \equiv Q \mid !P$.

# 3_cong_equiv.bel
Proof that `LF cong: proc → proc → type` and `inductive SubstM : (g:ctx) (g':ctx) [g |- proc] -> [g' |- proc] -> [g |- proc] -> [g' |- proc] -> ctype` are equivalent (using compatibility laws or contexts is the same)

- `pctxMtocong` is an additional lemma that proves that congruence defined with compatibility laws is preserved by contexts.
- `congtoCongM` and `CongMtocong` are the two implications of the main result.

# 4_contexts.bel
Properties of contexts (defined as in `pctxM`, i.e. as pairs of processes put in the same context)
From now on $((P,C_P),(Q,C_Q))$ means that there exists a context $C$ such that $C[P] = C_P$ and $C[Q] = C_Q$.

- `pctxM_symm`  
If $((P,C_P),(Q,C_Q))$ then $((Q,C_Q),(P,C_P))$.
- `pctxM_trans`  
If $((P,C_P),(R,C_R))$ and $Q$ is another process, then there exists $C_R$ such that $((P,C_P),(Q,C_Q))$ and $((Q,C_Q),(R,C_R))$ (the hole in the context $C$ can be filled with $R$).
- `pctxM_funct`  
If $((P,C_P),(P,C_P'))$ then $C_P = C_P'$ (filling the hole in a context with a process can only have one result).
- `pctxM_comp`  
If $((P,C_P),(Q,C_Q))$ and $((C_P,C_{C_P}),(C_Q,C_{C_Q}))$, then $((P,C_{C_P}),(Q,C_{C_Q}))$ (contexts can be composed).
- `pctxM_preserves_barb_in`  
If for each name $x$ $P \downarrow_x$ implies $Q \downarrow_x$ and $((P,C_P),(Q,C_Q))$, then for each name $x$ $C_P \downarrow_x$ implies $C_Q \downarrow_x$ 
- `pctxM_preserves_barb_out`  
If for each name $x$ $P \downarrow_{\overline{x}}$ implies $Q \downarrow_{\overline{x}}$ and $((P,C_P),(Q,C_Q))$, then for each name $x$ $C_P \downarrow_{\overline{x}}$ implies $C_Q \downarrow_{\overline{x}}$  

# 5_barbsim.bel

Properties of barbed similarity (`BarbSim`).

- `barb_sim_refl`, `barb_sim_trans`  
Barbed similarity is a preorder.
- `in_barb_sim`, `out_barb_sim`, `res_barb_sim`
Barbed similarity is compatible with input prefix, output prefix and restriction.
- `barbsim_implies_barbsimupto`, `barbsimupto_implies_barbsim`  
Barbed similarity and barbed similarity up to barbed similarity are the same relation (corollary: every simulation up to barbed similarity is included in barbed similarity).

# 6_structcong_in_barbsim.bel

Inclusion of structural congruence in barbed similarity

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

# 7_barbs_equiv.bel

Equivalence between barbs defined via lts and structurally

- `res_new`  
If $x$ does occur in $P$, then $\nu x P \equiv P$.
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


# 8_barbprecong.bel
Properties of barbed precongruence (`BarbPre`).

- `barbpre_refl`, `barbpre_trans`  
Barbed precongruence is a preorder.
- `barbpre_implies_barbsim`  
Barbed precongruence is included in barbed similarity.
- `barbpre_precong`  
Barbed precongruence is a precongruence.
- `barbpre_implies_largprecong`, `largprecong_implies_barbpre`  
Barbed precongruence is the largest precongruence included in barbed similarity.
- `structcong_implies_largprecong`  
Structural congruence is included in barbed precongruence. 

# 9_context_lemma.bel

Properties of `BarbPre'` to prove context lemma.

- `strength_subst_in`, `strength_subst_out_b`, `strength_subst_out_f`  
 If $P \sigma \xrightarrow{\alpha} P'$ and $\alpha \neq \tau$, then there exists $\beta, P''$ such that $P \xrightarrow{\beta} P''$, $\alpha = \beta \sigma$ (strengthening lemma).
- `barbpre'_implies_barbsim`  
BarbPre' is included in barbed similarity.
- `barbpre'_in`  
$\{ (x(y).(P \sigma) \mid R, x(y).(Q \sigma) \mid R) \mid P \leq_{\mid \sigma} Q\} \cup \dot{\leq}$ is included in barbed similarity
- `barbpre'_out`  
$\{ (\overline{x}y.(P \sigma) \mid R, \overline{x}y.(Q \sigma) \mid R) \mid P \leq_{\mid \sigma} Q\} \cup \dot{\leq}$ is included in barbed similarity
- `barbpre'_rep`  
$\{ (!(P \sigma) \mid R, !(Q \sigma) \mid R) \mid P \leq_{\mid \sigma} Q\} \cup \dot{\leq}$ is included in barbed similarity up to
- `barbpre'_is_precong`  
BarbPre' is a precongruence.
- `barbpre'_implies_largprecong`  
BarbPre' is included in the largest precongruence included in barbed similarity.
- `context_lemma`  
BarbPre' implies BarbPre.