# astra-registry-canary-2

**A test fixture. Nothing here is a plugin for users, and nothing here is
signed with a key any shipped Astra build trusts.** This repository is a
static source for one rehearsal, and nothing else. It has no workflows, no
secrets and no deploy keys, and it runs no bot.

## What it is for

The [Astra plugin registry](https://github.com/mihailinl/astra-registry)'s R2
exit (contract ROLL-60) needs two things to accept a key rotation signed with
the registry's throwaway `tools/testkeys` keys: a staging plugins service, and
a debug 0.2.x daemon. The rotation is a series of `signed` commits, which the
service reads from a branch named `signed`.

The series was first served from
[`mihailinl/astra-registry-canary`](https://github.com/mihailinl/astra-registry-canary),
cut at T0 = 2026-09-22. Its withdrawal lists expire seven days after T0, on
2026-09-29. The walk slipped towards that date. A `signed` that has served a
step can never be rewound (SERVE-18), and the service compiles the branch name
`signed` in. So the same series, cut again at **T0 = 2026-09-26**, needs a
`signed` of its own. This is that repository.

| Here | What it is |
|---|---|
| branch `signed` | The rotation line of the registry's `tools/testkeys/fixtures/rehearsal-r2b/`, one commit per step, each the exact commit the registry's signer made when the series was generated |
| branch `signed-compromise` | D10's compromise line, if it is walked: rotation steps 0-2, then `compromise/00-drop-2026a` |
| Pages | Serves branch `signed` (legacy build, root), so `https://mihailinl.github.io/astra-registry-canary-2/registry/v1/*.json` is always a step the branch carries |
| `main`'s merge of `refs/rehearsal-source/*` | The fixture generator's throwaway registry history for this cut, merged with `-s ours`. It holds the commits every step's `Source-Commit` names, so the service's TRUST-3 holds. `main`'s tree is unchanged by it |

## The rules

- **Everything on `signed` and `signed-compromise` is TEST-ONLY.** It is
  signed by keys whose private halves are public in the registry. No shipped
  Astra build trusts them.
- **Only `astra-registry`'s `tools/testkeys/rehearsal-push.mjs --step N`
  pushes there.** It pushes one fast-forward commit per step and never forces.
  It refuses this repository the first cut's commits, and refuses the first
  canary this cut's. The branches are never rewound: a service that has
  accepted a step refuses a head that does not descend from it (SERVE-18).
- **The lists in this cut expire on 2026-10-03**, from 00:00Z for step 0 to
  11:00Z for step 5. After that the branches are a record, and nothing here is
  served again.
- The runbook is astra-plugins-ops `runbooks/roll-60-rehearsal.md`.

## Licence

GPL-3.0-or-later (see `LICENSE`), like the rest of the registry. Copyright
Minice.
