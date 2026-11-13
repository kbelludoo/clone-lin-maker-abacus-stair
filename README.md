# clone-lin-maker-abacus-stair

Experimental **LIN** clone of MakerDAO / Sky `StairstepExponentialDecrease.price` — Dutch-auction stairstep exponential decay (`price = rmul(top, rpow(cut, dur / step, RAY'))`).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [makerdao/dss](https://github.com/makerdao/dss) `fa4f6630afb0624d04a003e920b0d71a00331d98`
  - `src/abaci.sol` sha256 `a4c3dc0368d66a5bf84e586f0cf76e2e80fb8b336c00348a97cc0d4ddcb016ca` blob `6cbd666267fc9f534ae52a45926eec5aa44b3bc5`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_maker_abacus_stair.lin`](src/lin_maker_abacus_stair.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/maker_abacus_stair_c11.c`](test/oracles/maker_abacus_stair_c11.c)
- **Transpilable kernel:** [`test/oracles/maker_abacus_stair_kernel.c`](test/oracles/maker_abacus_stair_kernel.c)
- **Proofs (lin-open only):** `examples/maker_abacus_stair/` and `make maker-abacus-stair-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256 ray. Not a production abacus replacement.

Named goldens (`cut=0.99e9`, `step=90`): PAR `dur=0` → **1000000000**;
HOLD `dur=89` → **1000000000**;
STEP1 `dur=90` → **990000000**;
STEP2 `dur=180` → **980100000**;
STEP3 `dur=270` → **970299000**;
ODD `top=123456789 dur=90` → **122222221**;
`step=0` and `cut>RAY'` fail-close;
price is not wards-auth; LinearDecrease tau and continuous ExponentialDecrease `n=dur` are absent;
OVERFLOW `1e18*1e18` and `1e10*1e9` fail-close while naive i64 wraps.

## Reproduce proofs (in lin-open)

```
make maker-abacus-stair-proof
```

## Not claimed

Uint256 ray / mainnet StairstepExponentialDecrease.price / Clipper.take parity.
LIN replacing Maker, Sky, clipper, or the EVM. TVL or dollar savings. zk soundness.
Hidden `cut`/`step` storage, `wards`/`file`, `block.timestamp`, LinearDecrease, or continuous ExponentialDecrease.
