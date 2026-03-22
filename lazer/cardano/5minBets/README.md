# 5minBets

`5minBets` is a Cardano-native price prediction market concept written in [Aiken](https://aiken-lang.org). Users bet `UP` or `DOWN` on the `XAU/USD` (gold) price over fixed 5-minute rounds, using [Pyth Network](https://pyth.network) price data to determine the outcome.

## Status

This repository is currently an early prototype. The product idea and intended round mechanics are described below, but the on-chain implementation in this repo is still minimal and does not yet include the full betting protocol.

## Concept

Each market round follows a simple lifecycle:

1. A relayer opens a round and records the current `XAU/USD` price from Pyth.
2. Users place bets on whether the price will go `UP` or `DOWN` during the next 5 minutes.
3. After the 5-minute window closes, the relayer submits the closing price from Pyth.
4. The contract determines the winning side and enables winners to claim their proportional share of the pool.
5. A 2% protocol fee is deducted before payouts.

## Design assumptions

The idea is straightforward, but a production-ready version should make these rules explicit:

- Oracle prices must be verifiable on-chain or otherwise constrained by trusted update logic.
- The relayer must not be able to selectively skip, delay, or manipulate round settlement.
- The market should clearly define what happens in edge cases such as equal open/close prices, no bets on one side, or failed settlement.
- Asset support should be precise. If multiple stablecoins are accepted, the contract must define how they are normalized or separated.

## Current repository state

At the moment, this repo contains a basic Aiken validator example rather than the finished betting application:

```text
.github/
  workflows/
    continuous-integration.yml
validators/
  hello_world.ak
lib/                    # empty — planned home for types.ak and utils.ak
env/                    # empty
aiken.toml
aiken.lock              # pins aiken-lang/stdlib v3.0.0 for reproducible builds
plutus.json
.gitignore
README.md
```

That means this README should be read as a project direction and protocol outline, not as a description of a completed implementation.

## Planned architecture

The intended project structure for the full version would look something like this:

```text
validators/
  round_validator.ak   # Core betting logic: bet, settle, claim
lib/
  types.ak             # RoundDatum, RoundStatus, action types
  utils.ak             # Price helpers, pool math, winner checks
```

## Building

Build the current Aiken project with:

```sh
aiken build
```

This produces compiled Plutus artifacts such as `plutus.json`.

## Testing

Run the test suite with:

```sh
aiken check
```

Run only tests matching a string:

```sh
aiken check -m hello
```

## Documentation

Generate HTML documentation with:

```sh
aiken docs
```

## Continuous integration

A GitHub Actions workflow at `.github/workflows/continuous-integration.yml` runs on every push and pull request. It checks formatting (`aiken fmt --check`), runs tests (`aiken check -D`), and builds the project (`aiken build`).

## Resources

- [Aiken user manual](https://aiken-lang.org)
- [Pyth Network on Cardano](https://docs.pyth.network/price-feeds/use-real-time-data/cardano)
- [Cardano developer docs](https://developers.cardano.org)