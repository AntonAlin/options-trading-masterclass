# Options Trading Masterclass

A single, self-contained Jupyter notebook covering options pricing theory from first
principles through to the models actually used on trading desks — with every formula
implemented in code, checked numerically against an independent method, and traced back to
the paper it comes from.

## What's inside

`options_trading_masterclass.ipynb` (47 cells, pre-executed with all plots and outputs saved):

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

Every model is either cross-checked against a second, independently-derived method (e.g.
closed-form Greeks vs. finite differences, the binomial tree vs. Black-Scholes, Heston's
Fourier price vs. its own Monte Carlo simulation, SABR calibrated back to the smile it was fit
to) or validated against a model-free identity (put-call parity, no-arbitrage conditions on the
vol surface). The notebook prints and asserts these checks inline rather than just asserting
the final numbers are right.

## Running it

```bash
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook options_trading_masterclass.ipynb
```

The notebook runs end-to-end in well under a minute on a single core — nothing in it depends
on external data or network access; every "market" used to demonstrate calibration (smiles,
surfaces) is generated synthetically from one of the models in the notebook itself, so results
are fully reproducible.

## Scope and disclaimer

This is educational material on derivatives pricing theory, not investment advice, and not
production risk infrastructure. Real trading systems need considerably more care around
calibration stability, market data quality, and numerical edge cases than a teaching notebook
can show.

## References

See the notebook's final section for full citations. Core sources include Black & Scholes
(1973), Merton (1973, 1976), Cox-Ross-Rubinstein (1979), Brennan & Schwartz (1977), Heston
(1993), Dupire (1994), Hagan et al. (2002), and Longstaff & Schwartz (2001).
