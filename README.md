# Probability Solver Discord Bot

![Python](https://img.shields.io/badge/Python-3.8-3776AB?logo=python&logoColor=white)
![discord.py](https://img.shields.io/badge/discord.py-1.7.3-5865F2?logo=discord&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-stats-8CAAE6?logo=scipy&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-1.22-013243?logo=numpy&logoColor=white)

A Discord chatbot that solves common probability problems directly inside a chat channel. Type a command, pass in your parameters, and the bot replies with the relevant probabilities, intervals, and summary statistics — no switching between single-purpose calculator websites.

## Why this exists

Most online probability calculators handle one distribution at a time, so working through a problem set means hopping between several pages. This bot brings the **binomial**, **multinomial**, and **normal** distributions together behind one command prefix, and computes the full set of related quantities (PMF, CDF, tail probabilities, confidence intervals, mean/variance) in a single response. All numerical work is delegated to `scipy.stats`, so the results match standard statistical libraries.

## Features

The bot uses the `!` prefix. Run `!help` at any time to list every command with its description.

### Binomial — $X \sim \text{Binomial}(n, p)$

| Command | Syntax | Returns |
| --- | --- | --- |
| `!binom_prob` | `!binom_prob <n> <p> <x>` | PMF, CDF, and the tail probabilities $P(X < x)$, $P(X > x)$, $P(X \ge x)$ |
| `!binom_bounds` | `!binom_bounds <n> <p> <x1> <x2>` | Probability that $X$ falls between two bounds (both exclusive and inclusive) |
| `!binom_ci` | `!binom_ci <n> <p> <ci>` | Confidence interval at the requested level (e.g. `0.95`) |
| `!binom_extra` | `!binom_extra <n> <p>` | Mean, median, and variance |

**Example:** `!binom_prob 10 0.5 4`

### Multinomial — $(X_1, \dots, X_k) \sim \text{Multinomial}(n, \mathbf{p})$

| Command | Syntax | Returns |
| --- | --- | --- |
| `!multi_prob` | `!multi_prob <x1,x2,...> <p1,p2,...>` | Joint PMF for the given outcome counts; $n$ is inferred as the sum of the counts |
| `!multi_extra` | `!multi_extra` | Formulas for the mean and variance of each component |

**Example:** `!multi_prob 2,3,5 0.2,0.3,0.5` (counts and probabilities are comma-separated, no spaces)

### Normal — $X \sim \mathcal{N}(\mu, \sigma^2)$

| Command | Syntax | Returns |
| --- | --- | --- |
| `!norm_prob` | `!norm_prob <x> <mean> <sd>` | PDF, CDF (left tail), right tail, and the Z-score of $x$ |
| `!norm_extra` | `!norm_extra` | Key properties of the normal distribution |

**Example:** `!norm_prob 1.5 0 1`

### Utility

| Command | Syntax | Description |
| --- | --- | --- |
| `!calc` | `!calc <x> <op> <y>` | Basic calculator supporting `+`, `-`, `*`, `/` |
| `!hello` | `!hello` | Greets the user |
| `!random_music` | `!random_music` | Returns a random track from a small playlist (expect the occasional surprise) |

## The math behind it

Each command maps onto a standard closed-form expression, evaluated through `scipy.stats`.

**Binomial PMF:**

$$P(X = x) = \binom{n}{x} p^{x}(1 - p)^{n - x}, \qquad \mathbb{E}[X] = np, \qquad \operatorname{Var}(X) = np(1 - p)$$

**Multinomial PMF:**

$$P(X_1 = x_1, \dots, X_k = x_k) = \frac{n!}{x_1!\,x_2! \cdots x_k!}\, p_1^{x_1} p_2^{x_2} \cdots p_k^{x_k}, \qquad \sum_{i=1}^{k} x_i = n$$

**Normal PDF and Z-score:**

$$f(x) = \frac{1}{\sigma\sqrt{2\pi}}\, e^{-\frac{(x - \mu)^2}{2\sigma^2}}, \qquad z = \frac{x - \mu}{\sigma}$$

## Tech stack

- **Language:** Python 3.8
- **Bot framework:** [discord.py](https://discordpy.readthedocs.io/) (commands extension)
- **Numerics:** [SciPy](https://scipy.org/) (`scipy.stats`) and [NumPy](https://numpy.org/)
- **Dependency management:** [Poetry](https://python-poetry.org/)

## Project structure

```
.
├── main.py          # Bot entry point: command definitions and Discord event loop
├── binomial.py      # Binomial distribution logic (prob, bounds, CI, summary stats)
├── multinomial.py   # Multinomial distribution logic (joint prob, summary stats)
├── normal.py        # Normal distribution logic (PDF, CDF, tails, Z-score)
└── pyproject.toml   # Project metadata and dependencies (Poetry)
```

## Setup

### Prerequisites

- Python 3.8
- A Discord account and a registered bot application

### 1. Create a Discord bot

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications) and create a new application.
2. Under **Bot**, add a bot and copy its token.
3. Under **OAuth2 → URL Generator**, select the `bot` scope and the permissions to read and send messages, then use the generated URL to invite the bot to your server.

### 2. Install dependencies

Using Poetry:

```bash
poetry install
```

Or with `pip`:

```bash
pip install "discord.py==1.7.3" scipy numpy
```

### 3. Provide the bot token

The bot reads its token from an environment variable named `password`:

```bash
export password="YOUR_DISCORD_BOT_TOKEN"
```

### 4. Run the bot

```bash
python main.py
```

Once it is online in your server, send `!help` in any channel the bot can see to get started.

## Roadmap

Ideas for future iterations:

- Additional distributions (Poisson, geometric, exponential)
- Richer output using Discord embeds instead of plain messages
- Stronger input validation and clearer error messages
- Migration to Discord slash commands for autocomplete and inline help

## Author

Nguyen The Quang — [github.com/nguyenthequang](https://github.com/nguyenthequang)
