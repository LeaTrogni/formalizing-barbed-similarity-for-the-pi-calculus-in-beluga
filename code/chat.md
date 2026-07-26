Yes—I see a plausible strategy, but I would not change the global `ctx` schema or induct over substitution support. The cleaner route is to expose the entire domain of the substitution as inductive data and process every domain variable, including fixpoints.

## The more fundamental obstruction

There are actually two problems, not just support.

`BarbPre'` quantifies over an arbitrary morphism `$S:$[h |- g]` and an arbitrary `R:[h |- proc]` ([definition](/home/alberto/REPS/gits/formalizing-barbed-similarity-for-the-pi-calculus-in-beluga/code/1_definitions.bel:224)). But `BarbPre` obtains contextual closure through `PCtx` ([definition](/home/alberto/REPS/gits/formalizing-barbed-similarity-for-the-pi-calculus-in-beluga/code/1_definitions.bel:217)), and `PCtx` can only remove a suffix of the hole’s LF context through binders. Its base case requires identical contexts, and it has no constructor for transporting a process from `g` to an unrelated or extended context `h`.

Consequently, one cannot directly build

```text
PCtx [g |- P] [h |- Cσ,R[P]] ...
```

for arbitrary `g`, `h`, and `$S:$[h |- g]`. Even after solving support induction, this world-changing mismatch remains.

I would therefore introduce a Kripke/world-extension version of `BarbPre` before attempting the reverse implication.

## Use a proof-carrying substitution

The upstream substitution-variable tests contain almost exactly the needed datatype: an inductive relation indexed by a built-in substitution, with one constructor per domain entry. See [`match-subst.bel`](https://github.com/Beluga-lang/Beluga/blob/820615cc4758086eb7641f62340a3ab93a689303/t/code/success/substvars/match-subst.bel).

For this development:

```beluga
inductive NSub : {g:ctx} {h:ctx} {$S:$[h |- g]} ctype =
  | NSNil :
      NSub [] [h] $[h |- ^]

  | NSCons :
      {Y:[h |- names]}
      NSub [g] [h] $[h |- $S] ->
      NSub [g, x:names] [h] $[h |- $S, Y]
;
```

This provides the right induction principle: `NSCons` exposes the image `Y` of the final domain variable.

I prototyped this under Beluga 1.1.3. A function constructing `NSub S` for arbitrary `S` type-reconstructs, but adding its `/ total g ... /` declaration produces a coverage failure involving an abstract weakened substitution. Thus it reproduces the implementation limitation described in the [informal proof](</home/alberto/Documents/mainsol.pdf>).

For a fully checked development, I would initially define a new characterization that takes `NSub S` as an explicit premise. Equivalence with the raw `$S` formulation can remain a separate metatheoretic/coverage issue.

## Duplicate domain and codomain instead of finding support

Construct a joint context consisting of:

```text
passive codomain h ; active copy of domain g
```

using an explicit context relation:

```beluga
inductive Ext :
  {h:ctx} {g:ctx} {k:ctx} {$W:$[k |- g]} ctype =

  | ExtNil :
      Ext [h] [] [h] $[h |- ^]

  | ExtCons :
      Ext [h] [g] [k] $[k |- $W] ->
      Ext [h] [g, x:names] [k, z:names]
          $[k, z:names |- $W[..], z]
;
```

Here `$W` injects the original domain into the fresh active suffix of `k`. This is the same general technique as:

- the lockstep context/substitution relation in [`equal/alg-equal-ctxrel.bel`](https://github.com/Beluga-lang/Beluga/blob/820615cc4758086eb7641f62340a3ab93a689303/examples/equal/alg-equal-ctxrel.bel);
- `SubCtx`, `Map`, and context reification in [`cpp13/cc.bel`](https://github.com/Beluga-lang/Beluga/blob/820615cc4758086eb7641f62340a3ab93a689303/examples/cpp13/cc.bel).

Now every source variable is an active copy, while every image `Y` belongs to the passive prefix. Even when the mathematical substitution fixes `x`, the formal server communicates from the active copy of `x` to its passive copy. Therefore:

- no equality test on names is needed;
- no support or fixpoint complement is needed;
- repeated images and non-injective substitutions are harmless;
- induction is simply over `NSub`, hence over the full domain.

## Strengthen precongruence with world extension

I suggest a relation along these lines:

```text
BarbPreK [g |- P] [g |- Q]
```

whose observation says:

```text
for every passive base h,
joint context k,
injection W witnessed by Ext h g k W,
and PCtx on P[W] and Q[W],
the resulting processes are barbed similar.
```

Schematically:

```beluga
Ext [h] [g] [k] $[k |- $W] ->
PCtx [k |- P[$W]] [l |- CP]
     [k |- Q[$W]] [l |- CQ] ->
BarbSim [l |- CP] [l |- CQ]
```

The current `BarbPre` is recovered using `h=[]` and the identity copy. The reverse direction requires this stronger Kripke form because the substitution context needs the codomain names and the active domain copies simultaneously.

## Use a locked substitution harness

For `m:NSub S`, build a context equivalent to

```text
νa.νb. ((Outm(a) | Inm(a,b,P)) | b(w).R)
```

where:

- `Outm(a)` sequentially outputs every image recorded by `m`;
- `Inm(a,b,P)` receives the corresponding active variables;
- after all variables have been received, it becomes `b̄a.P`;
- `R` remains guarded as `b(w).R`.

After `|g|+1` communications, the unique result is structurally congruent to:

```text
P[$S] | R
```

The restrictions make `a` and `b` fresh by HOAS, so the explicit freshness assumptions in the informal argument disappear.

I would avoid introducing natural-number-indexed multisteps initially. Prove a combined forcing lemma by induction on `NSub`:

1. Take the unique head τ-transition of the left harness.
2. Apply `.BarbSim_tau`.
3. Invert the matching transition of the right harness; guardedness forces it to be the corresponding communication.
4. Recurse on the tail of `NSub`.
5. In `NSNil`, perform the final communication on `b`.
6. Remove `0` and the unused restrictions using structural congruence inclusion.

This combines Lemmas 4.5.1–4.5.3 and avoids arithmetic on the number of transitions.

## About tagging `ctx`

A tag by itself will not constrain a built-in substitution: `$S` can still map a tagged assumption arbitrarily unless an explicit relation indexed by `$S` proves how each block is mapped. Moreover, changing the global `ctx = names` would affect every binder-sensitive theorem and all occurrences of `[g,x:names]`.

If tags are used, I would use them only in a separate enriched schema, with the meanings:

- passive codomain name;
- active domain copy.

I would not use “in support” versus “fixed”: determining that classification is precisely the operation Beluga cannot expose. The `Ext` relation above carries the same distinction without changing the existing schema.

My recommended first milestone is therefore:

1. Add `NSub`, `Ext`, `BarbPreK`, and a restricted `BarbPreMap`.
2. Prove the reverse implication `BarbPreK -> BarbPreMap`.
3. Only afterward address equivalence with the existing raw `$S` and non-Kripke definitions.

That isolates the mathematical proof from the two Beluga limitations: coverage for abstract substitutions and transport between contextual worlds.
