# Options Trading Masterclass

Self-contained Jupyter notebooks on derivatives pricing and trading, from first principles
through to the models and mechanics actually used on trading desks — every formula implemented
in code, checked numerically against an independent method, and traced back to the paper it
comes from.

## Notebooks

### `options_trading_masterclass.ipynb`

Options pricing theory end to end: the Black-Scholes-Merton model, the Greeks (first order and
higher order), implied volatility and the smile, binomial trees, Monte Carlo methods
(variance reduction, Longstaff-Schwartz for American options), finite-difference PDE methods,
stochastic volatility (Heston), local volatility (Dupire), the SABR model, Merton
jump-diffusion, volatility surface construction and no-arbitrage checks, trading structures and
portfolio Greeks, and a delta-hedging P&L simulation. 47 cells, pre-executed with all plots and
outputs saved.

1. Option fundamentals — payoffs, moneyness, put-call parity
2. The Black-Scholes-Merton model (derivation, closed form, checked against brute-force Monte Carlo)
3. The Greeks — Delta, Gamma, Vega, Theta, Rho, plus higher-order Vanna, Volga, Speed, Zomma (all checked against finite differences)
4. Implied volatility — Newton-Raphson and Brent inversion, the volatility smile
5. Binomial trees (Cox-Ross-Rubinstein) — convergence to Black-Scholes, early-exercise premium
6. Monte Carlo methods — antithetic and control-variate variance reduction; Longstaff-Schwartz least-squares Monte Carlo for American options
7. Finite-difference PDE methods — Crank-Nicolson with the Brennan-Schwartz early-exercise algorithm
8. Stochastic volatility — the Heston model, semi-analytic pricing via the "Little Trap" characteristic function, and Monte Carlo cross-check
9. Local volatility — Dupire's formula, recovered numerically from a Heston call-price surface
10. The SABR model — Hagan et al.'s asymptotic formula, calibrated by least squares to a target smile
11. Jump-diffusion — the Merton (1976) model, and why it produces skew even with constant diffusion vol
12. Building and checking a volatility surface — calendar-spread and butterfly no-arbitrage conditions
13. Trading structures and portfolio Greeks — straddles, spreads, iron condors, net-Greek aggregation
14. A delta-hedging simulation showing the theta-vs-gamma P&L tradeoff as a function of realized vs. implied vol
15. A model comparison table — what to use when, and why
16. Full references

### `vix_futures_market_making.ipynb`

Written from the seat of a market maker on the Cboe VIX futures desk in Chicago. VIX futures
have no cost-of-carry model — VIX isn't a tradable spot asset — so the notebook prices them the
only rigorous way: model the dynamics of the variance state VIX is a transform of, and take a
risk-neutral expectation. Builds on the same CIR/Heston variance machinery as the options
notebook, then goes further into market-making mechanics and systematic trading.

1. The VIX index — model-free variance replication from an option chain, validated against a known analytic Heston variance-swap rate
2. VIX as a closed-form transform of the variance state
3. Pricing VIX futures — Jensen's convexity gap, the Gruenbichler-Longstaff / CIR (noncentral chi-squared) approach, validated by exact CIR Monte Carlo (Broadie-Kaya style)
4. The term structure — contango vs. backwardation, roll yield
5. VIX futures "Greeks" — variance-delta and the vega bucket ladder across the strip
6. Market making the book — Avellaneda-Stoikov inventory-aware quoting simulated on a realistic CIR-driven futures path, compared to naive symmetric quoting
7. Trading the curve — systematic roll-yield carry, a backwardation risk filter stress-tested against a Volmageddon-style tail jump, calendar-spread mean reversion
8. Risk management — hedging a multi-month strip with a single-instrument variance-delta hedge
9. Case study — Volmageddon, February 5, 2018
10. Model comparison and full references

### `cross_venue_information_share.ipynb`

Original research rather than training material: an econometric instrument for asking **where
informed traders actually transact — spot, or the leveraged derivative?** (Black 1975; Easley,
O'Hara & Srinivas 1998). The design exploits a data advantage that is rare outside crypto: the
Binance public archive carries an exact aggressor flag on *both* the spot and the perpetual-future
tape for the same symbol on the same clock, so trade direction is never inferred with Lee-Ready.

What the notebook actually establishes is a **methodological negative result**:

1. Standard price-discovery machinery (VECM, Gonzalo-Granger component shares, Hasbrouck
   information shares with Cholesky-ordering bounds), validated against a planted truth.
2. The natural flow-based extension — decomposing the efficient-price innovation across the two
   venues' signed order flow via an order-invariant LMG/Shapley decomposition.
3. **A demonstration that this fails.** Under a null of *equally informed* venues, ordinary
   microstructure asymmetry (transitory noise-trade impact, quote-adjustment speed) biases the
   statistic by up to **0.37** — the same magnitude as any effect it could plausibly detect. On
   real data it would return a large, significant, entirely spurious number with nothing in the
   output to signal a problem.
4. **The fix**: Hasbrouck (1991) long-run cumulative impulse response, which isolates *permanent*
   flow impact by construction, since transitory impact reverses inside the response horizon.
5. **Verification**: unbiased under every asymmetry tested (0.498–0.502 against a truth of 0.500),
   near-exact recovery of the true share (max error 0.006), plus a power analysis, a
   funding-settlement natural experiment with a pre-specified placebo, and a pre-registered
   analysis plan.

The notebook makes **no empirical claim about real markets** — the environment it was written in
blocked the data source, so the real-data driver is written and gated behind a flag. It is a
validated instrument, not a finding. A staleness diagnostic that degrades *both* estimators is
reported rather than buried, as the thing most likely to sink the study.

The real-data section expects the loaders from the companion
[`microstructure-lab`](https://github.com/AntonAlin/microstructure-lab) repo.

Every model is either cross-checked against a second, independently-derived method (e.g.
closed-form Greeks vs. finite differences, a tree vs. Black-Scholes, Heston's Fourier price vs.
its own Monte Carlo simulation, exact CIR simulation vs. the semi-analytic VIX futures price) or
validated against a model-free identity (put-call parity, no-arbitrage conditions on the vol
surface, Jensen's inequality). All three notebooks print and assert these checks inline rather
than just asserting the final numbers are right.

## Running it

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook options_trading_masterclass.ipynb   # or vix_futures_market_making.ipynb
                                                     # or cross_venue_information_share.ipynb
```

All three notebooks run end-to-end in a couple of minutes on a single core, and none of them
requires external data or network access to reproduce every result shown. Every "market" used to demonstrate calibration
or backtesting (smiles, surfaces, VX term structures, trading episodes) is generated
synthetically from a model built earlier in the same notebook, so results are fully
reproducible and every backtest is a Monte Carlo stress test against the model rather than a
historical replay.

## Scope and disclaimer

The first two notebooks are educational material on derivatives pricing and market-making
theory; the third is exploratory research methodology. None of it is investment
advice, and not production risk or trading infrastructure. Real trading systems need
considerably more care around calibration stability, market data quality, and numerical edge
cases than a teaching notebook can show.

## References

See each notebook's final section for full citations. Core sources include Black & Scholes
(1973), Merton (1973, 1976), Cox-Ross-Rubinstein (1979), Brennan & Schwartz (1977), Heston
(1993), Dupire (1994), Hagan et al. (2002), Longstaff & Schwartz (2001), Cox-Ingersoll-Ross
(1985), Gruenbichler & Longstaff (1996), Broadie & Kaya (2006), Avellaneda & Stoikov (2008),
Hasbrouck (1991, 1995), Gonzalo & Granger (1995), and Easley, O'Hara & Srinivas (1998).
