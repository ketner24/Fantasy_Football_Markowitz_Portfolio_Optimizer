# Fantasy Football — Frontier

Config-driven scoring in `ff_ml/`, plus a deployable Markowitz optimizer in `web/`.

## JavaScript app (draft + weekly lineups)

```bash
cd web
npm install
npm run dev
```

See [`web/README.md`](web/README.md) for Vercel and Docker deploy. Open a Sleeper league or
the built-in demo.

Expectations come from three inputs, not one season of box scores: the platform projection,
a recency-weighted mean over several seasons, and a defense-vs-position adjustment learned
from the real schedule. The weekly recommendation then maximises the probability of beating
your actual opponent; the risk slider is a manual override that runs both ways (negative =
deliberately chase ceiling).

## Python foundation

`ff_ml/` holds the YAML scoring engine, nflverse historical labels, leakage-safe
feature engineering and walk-forward splits, plus Sleeper/ESPN adapters.

**It contains no trained model yet** -- `features.py` builds the design matrix and
`walk_forward_splits` gives you honest evaluation folds, but nothing fits an
estimator, and nothing here feeds the web app. The web app has its own scoring
implementation in TypeScript. Closing that gap (fit a projection model, backtest
it against Sleeper projections, serve it to the app) is the single largest
remaining upside.
