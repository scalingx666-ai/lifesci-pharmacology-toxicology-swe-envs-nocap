# Issue

**Title:** Five pretraining correctness fixes

## Description

Five independent correctness fixes in the pretraining path. Each is a separate
commit with the full reasoning in its message; the short version is below.

All five stayed hidden for the same reason: every documented recipe passes the
flags that route around them. They surface as soon as someone pretrains with a
non-default configuration.

### 1. Bond vocab labels were misaligned under bond dropout

`bond_random_mask` re-parses the SMILES independently of `MolGraph`, which skips
bonds when `--bond_drop_rate > 0`. Labels were matched to bonds by position
alone, so the label vector came out too long and every label after the first
dropped bond described a different bond from the one it landed on. The bond
vocab head trained on shifted targets, with no error and a loss that still went
down.

Fixed by reusing `BatchMolGraph.rdkit_bond_idx`, the packed-row to
RDKit-bond-index map the cuik-molmaker path already maintains for exactly this
shape of problem. With no bond dropout the gather is the identity, so existing
runs are bit-identical — confirmed end to end with a seeded pretrain.

### 2. `--seed` did not seed anything

`--seed` was on the pretrain parser but `args.seed` was read nowhere, so every
pretrain was unseeded and two runs of the same command diverged. On a 9k
fixture the run-to-run validation-loss spread was ~0.1, larger than the effect
an A/B comparison typically tries to measure.

Unset (the default) leaves every RNG untouched. Set, it seeds
torch/cuda/numpy/random, installs a `worker_init_fn` — the vocab masking and
bond dropout draw from `np.random` inside DataLoader workers, which torch does
not reseed — and gives bit-identical runs.

### 3. `--hidden_size` defaulted to 3

The pretrain parser defaulted `--hidden_size` to 3, with help text claiming an
"actual dimension = hidden_size * 100" scaling that exists nowhere in the repo.
It never bit because every entry point passes the flag explicitly, and because
the default `--num_attn_head 4` fails at construction on `d_model % h == 0` —
but a head count dividing 3 would have silently built a 3-dimensional encoder.

800 matches both released checkpoints and makes the pretrain and finetune
parsers agree.

### 4. DDP aborted at small `--depth`

`MPNEncoder` runs its message-passing loop `range(ndepth - 1)` times, so a drawn
depth of 1 or less skips the loop and leaves `W_h` without gradient for that
batch. DDP is built with `find_unused_parameters=False` whenever
`embedding_output_type` is `both`, which then aborts the step with `Expected to
have finished reduction in the prior iteration`.

Both dynamic-depth samplers are bounded below by `depth - 3`, so the draw can
reach 1 only when `--depth <= 4` — and the pretrain parser defaults to 3. At
`--depth 5` and above the new predicate is False, so the usual `both` +
`--depth 6` keeps DDP's fast path unchanged.

### 5. cMIM pretrain crashed without `--tensorboard`

`KERMTCMIMTrainer` unpacks `recon_accuracy=None` unless `--tensorboard` is set,
and the accumulation below is guarded accordingly, but the train-metrics dict
called `.item()` on it unguarded. A cMIM pretrain without `--tensorboard` died
with `AttributeError: 'NoneType' object has no attribute 'item'` at the first
train-log interval. Guarded with the same test `KERMTHybridTrainer` already uses
a few hundred lines below in the same file.

### Tests

`tests/unit/test_vocab_task_inputs.py` covers the two properties that matter for
fix 1: the gather is the identity when nothing is dropped, and it stays aligned
when bonds are — including ring molecules, where the packed bond order is a
permutation of the RDKit order.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `be03ce8d4fb8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.