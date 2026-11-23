# AI Hedge Fund

This is a proof of concept for an AI-powered hedge fund.  The goal of this project is to explore the use of AI to make trading decisions.  This project is for **educational** purposes only and is not intended for real trading or investment.

This system employs several agents working together:

1. Aswath Damodaran Agent - The Dean of Valuation, focuses on story, numbers, and disciplined valuation
2. Ben Graham Agent - The godfather of value investing, only buys hidden gems with a margin of safety
3. Bill Ackman Agent - An activist investor, takes bold positions and pushes for change
4. Cathie Wood Agent - The queen of growth investing, believes in the power of innovation and disruption
5. Charlie Munger Agent - Warren Buffett's partner, only buys wonderful businesses at fair prices
6. Michael Burry Agent - The Big Short contrarian who hunts for deep value
7. Peter Lynch Agent - Practical investor who seeks "ten-baggers" in everyday businesses
8. Phil Fisher Agent - Meticulous growth investor who uses deep "scuttlebutt" research 
9. Rakesh Jhunjhunwala Agent - The Big Bull of India
10. Stanley Druckenmiller Agent - Macro legend who hunts for asymmetric opportunities with growth potential
11. Warren Buffett Agent - The oracle of Omaha, seeks wonderful companies at a fair price
12. Valuation Agent - Calculates the intrinsic value of a stock and generates trading signals
13. Sentiment Agent - Analyzes market sentiment and generates trading signals
14. Fundamentals Agent - Analyzes fundamental data and generates trading signals
15. Technicals Agent - Analyzes technical indicators and generates trading signals
16. Risk Manager - Calculates risk metrics and sets position limits
17. Portfolio Manager - Makes final trading decisions and generates orders
    
<img width="1042" alt="Screenshot 2025-03-22 at 6 19 07 PM" src="https://github.com/user-attachments/assets/cbae3dcf-b571-490d-b0ad-3f0f035ac0d4" />


**Note**: the system simulates trading decisions, it does not actually trade.

[![Twitter Follow](https://img.shields.io/twitter/follow/virattt?style=social)](https://twitter.com/virattt)

## Disclaimer

This project is for **educational and research purposes only**.

- Not intended for real trading or investment
- No investment advice or guarantees provided
- Creator assumes no liability for financial losses
- Consult a financial advisor for investment decisions
- Past performance does not indicate future results

By using this software, you agree to use it solely for learning purposes.

## Table of Contents
- [Setup](#setup)
  - [Using Poetry](#using-poetry)
  - [Using Docker](#using-docker)
- [Offline verification](#offline-verification)
- [Usage](#usage)
  - [Running the Hedge Fund](#running-the-hedge-fund)
  - [Running the Backtester](#running-the-backtester)
- [Contributing](#contributing)
- [Feature Requests](#feature-requests)
- [License](#license)

## Setup

The CLI and backtester are for educational simulation. If you are only reviewing this checkout, skip environment setup and use [Offline verification](#offline-verification).

### Using Poetry

Use Python 3.11 as the reference version. [`pyproject.toml`](pyproject.toml) declares `^3.11` (at least 3.11, below 4.0), and [`poetry.lock`](poetry.lock) records the dependency resolution. With Poetry installed, `poetry install` runs from the repository root. It is not required for source review.

### Using Docker

The [Dockerfile](docker/Dockerfile) also uses Python 3.11. The [Compose file](docker/docker-compose.yml) and its [shell](docker/run.sh) and [Windows](docker/run.bat) wrappers live under `docker/`, not the repository root. The standard Docker examples below assume an already-built `ai-hedge-fund` image and a configured simulation environment; the normal wrapper launch does not build the image. Image builds, model access, and market-data access are separate from offline review.

## Offline verification

Start with the [Swarm-ready handoff](SWARM_READY.md). From the repository root, confirm the eight setup and entrypoint paths are tracked:

```bash
git --no-lazy-fetch ls-files --error-unmatch -- pyproject.toml poetry.lock docker/Dockerfile docker/docker-compose.yml docker/run.sh docker/run.bat src/main.py src/backtester.py
```

Compare the Python requirement with the Docker base image and confirm the example entrypoints are [`src/main.py`](src/main.py) and [`src/backtester.py`](src/backtester.py). This check does not install dependencies, import the package, start containers, run simulations, or contact model or market-data providers.

## Usage

### Running the Hedge Fund

#### With Poetry
```bash
poetry run python src/main.py --ticker AAPL,MSFT,NVDA
```

#### With Docker
**Note**: All Docker commands must be run from the `docker/` directory.

```bash
# Navigate to the docker directory first
cd docker

# On Linux/Mac:
./run.sh --ticker AAPL,MSFT,NVDA main

# On Windows:
run.bat --ticker AAPL,MSFT,NVDA main
```

**Example Output:**
<img width="992" alt="Screenshot 2025-01-06 at 5 50 17 PM" src="https://github.com/user-attachments/assets/e8ca04bf-9989-4a7d-a8b4-34e04666663b" />

You can also specify a `--ollama` flag to run the AI hedge fund using local LLMs.

```bash
# With Poetry:
poetry run python src/main.py --ticker AAPL,MSFT,NVDA --ollama

# With Docker (from docker/ directory):
# On Linux/Mac:
./run.sh --ticker AAPL,MSFT,NVDA --ollama main

# On Windows:
run.bat --ticker AAPL,MSFT,NVDA --ollama main
```

You can also specify a `--show-reasoning` flag to print the reasoning of each agent to the console.

```bash
# With Poetry:
poetry run python src/main.py --ticker AAPL,MSFT,NVDA --show-reasoning

# With Docker (from docker/ directory):
# On Linux/Mac:
./run.sh --ticker AAPL,MSFT,NVDA --show-reasoning main

# On Windows:
run.bat --ticker AAPL,MSFT,NVDA --show-reasoning main
```

You can optionally specify the start and end dates to make decisions for a specific time period.

```bash
# With Poetry:
poetry run python src/main.py --ticker AAPL,MSFT,NVDA --start-date 2024-01-01 --end-date 2024-03-01 

# With Docker (from docker/ directory):
# On Linux/Mac:
./run.sh --ticker AAPL,MSFT,NVDA --start-date 2024-01-01 --end-date 2024-03-01 main

# On Windows:
run.bat --ticker AAPL,MSFT,NVDA --start-date 2024-01-01 --end-date 2024-03-01 main
```

### Running the Backtester

#### With Poetry
```bash
poetry run python src/backtester.py --ticker AAPL,MSFT,NVDA
```

#### With Docker
**Note**: All Docker commands must be run from the `docker/` directory.

```bash
# Navigate to the docker directory first
cd docker

# On Linux/Mac:
./run.sh --ticker AAPL,MSFT,NVDA backtest

# On Windows:
run.bat --ticker AAPL,MSFT,NVDA backtest
```

**Example Output:**
<img width="941" alt="Screenshot 2025-01-06 at 5 47 52 PM" src="https://github.com/user-attachments/assets/00e794ea-8628-44e6-9a84-8f8a31ad3b47" />


You can optionally specify the start and end dates to backtest over a specific time period.

```bash
# With Poetry:
poetry run python src/backtester.py --ticker AAPL,MSFT,NVDA --start-date 2024-01-01 --end-date 2024-03-01

# With Docker (from docker/ directory):
# On Linux/Mac:
./run.sh --ticker AAPL,MSFT,NVDA --start-date 2024-01-01 --end-date 2024-03-01 backtest

# On Windows:
run.bat --ticker AAPL,MSFT,NVDA --start-date 2024-01-01 --end-date 2024-03-01 backtest
```

You can also specify a `--ollama` flag to run the backtester using local LLMs.
```bash
# With Poetry:
poetry run python src/backtester.py --ticker AAPL,MSFT,NVDA --ollama

# With Docker (from docker/ directory):
# On Linux/Mac:
./run.sh --ticker AAPL,MSFT,NVDA --ollama backtest

# On Windows:
run.bat --ticker AAPL,MSFT,NVDA --ollama backtest
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

**Important**: Please keep your pull requests small and focused.  This will make it easier to review and merge.

## Feature Requests

If you have a feature request, please open an [issue](https://github.com/virattt/ai-hedge-fund/issues) and make sure it is tagged with `enhancement`.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
