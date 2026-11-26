# LangChain v1 Hands-On

A learning repo for exploring **LangChain v1** through Jupyter notebooks. It covers the basics of agents, model integration (Google Gemini, OpenAI, Groq), tools, and messages.

Dependencies are managed with [uv](https://docs.astral.sh/uv/), a fast Python package and project manager.

## Prerequisites

- macOS, Linux, or Windows
- Python **3.10 or newer** (3.12 recommended). LangChain v1 does not work on Python 3.9, where `from langchain.agents import create_agent` fails with an `ImportError`.
- VS Code with the **Python** and **Jupyter** extensions
- API keys for Google AI Studio, OpenAI, and Groq

## Setup

### 1. Install uv

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# or with Homebrew
brew install uv
```

Check that it works:

```bash
uv --version
```

### 2. Initialise the project

```bash
uv init
```

Creates `pyproject.toml`, `.python-version`, and starter files.

Then pin a supported Python version:

```bash
uv python pin 3.12
```

Also make sure `pyproject.toml` has `requires-python = ">=3.12"` (or `>=3.10`). If it says 3.9, uv builds the environment with that version and LangChain v1 won't install.

### 3. Create the virtual environment

```bash
uv venv --python 3.12
```

Creates a `.venv` folder in the project using Python 3.12. uv downloads this Python version automatically if it isn't installed.

### 4. Activate the virtual environment

```bash
source .venv/bin/activate
```

On Windows, use `.venv\Scripts\activate`. Your prompt should now start with `(.venv)` or the project name. Run `deactivate` to leave the environment.

Verify the Python version:

```bash
python --version
```

### 5. Create `requirements.txt`

Create a `requirements.txt` file in the project root listing the packages used by the notebooks:

```text
langchain
langchain-openai
langchain-groq
langchain-google-genai
python-dotenv
```

| Package | Purpose |
|---|---|
| `langchain` | Core LangChain v1 library (agents, tools, messages) |
| `langchain-openai` | OpenAI model integration |
| `langchain-groq` | Groq model integration |
| `langchain-google-genai` | Google Gemini (AI Studio) integration |
| `python-dotenv` | Loads API keys from the `.env` file |

### 6. Install the dependencies

```bash
uv add -r requirements.txt
```

Installs every package in `requirements.txt` into `.venv` and records them in `pyproject.toml` and `uv.lock`.

### 7. Add your API keys

Create a `.env` file in the project root:

```env
GOOGLE_API_KEY=your_google_ai_studio_key
GROQ_API_KEY=your_groq_api_key
OPENAI_API_KEY=your_openai_api_key
```

Where to get the keys:

- **Google AI Studio:** https://aistudio.google.com/apikey
- **OpenAI:** https://platform.openai.com/api-keys
- **Groq:** https://console.groq.com/keys

> **Never commit `.env` to git.** Add it to `.gitignore` before your first commit:
>
> ```bash
> echo ".env" >> .gitignore
> ```
>
> If a key was committed by mistake, remove it from history and rotate it in the provider's dashboard.

Load the keys in a notebook with:

```python
from dotenv import load_dotenv
load_dotenv()
```

### 8. Install the Jupyter kernel

```bash
uv add ipykernel
```

Lets VS Code run notebooks using the packages in `.venv`.

### 9. Attach the notebook to the venv kernel

1. Open a `.ipynb` file in VS Code.
2. Click **Select Kernel** at the top right.
3. Choose **Python Environments...** and pick the `.venv` interpreter (Python 3.12).
4. If it doesn't appear, press `Cmd+Shift+P`, run **Python: Select Interpreter**, and choose `.venv/bin/python`. Then run **Developer: Reload Window**.

Confirm the notebook uses the right interpreter by running this in a cell:

```python
import sys
print(sys.executable)   # should end in .venv/bin/python
print(sys.version)      # should show 3.12.x
```

### 10. Run the notebooks

Run the cells from top to bottom. A good order is:

| Notebook | Topic |
|---|---|
| `1-langchainintro.ipynb` | Introduction and building a first agent with `create_agent` |
| `2-modelintegration.ipynb` | Using different model providers |
| `3-tools.ipynb` | Defining and using tools |
| `4-messages.ipynb` | Working with messages |

## Useful commands

| Command | What it does |
|---|---|
| `uv add <package>` | Installs a package and adds it to `pyproject.toml` |
| `uv remove <package>` | Removes a package |
| `uv sync` | Recreates the environment from `pyproject.toml` and `uv.lock` |
| `uv run python script.py` | Runs a script inside the project environment without activating it |
| `uv pip list` | Lists installed packages |
| `python -c "import langchain; print(langchain.__version__)"` | Checks the installed LangChain version (should be `1.x`) |

## Troubleshooting

- **`zsh: permission denied: .venv/bin/activate`:** The script must be sourced, not executed. Use `source .venv/bin/activate`.
- **`ImportError: cannot import name 'create_agent'`:** The environment is using Python 3.9 or an old LangChain. Recreate the venv with Python 3.12 and run `uv add -U langchain`.
- **Notebook prints Python 3.9 instead of 3.12:** The wrong kernel is selected. Reselect the `.venv` kernel and restart it.
- **`ModuleNotFoundError`:** Install the missing package with `uv add <package>` and restart the kernel.
- **GitHub blocks the push because of secrets:** Remove `.env` from git history before pushing. Don't use the "allow secret" link.

## Getting started on a fresh clone

```bash
git clone https://github.com/sambit77/langchain-hands-on.git
cd langchain-hands-on
uv sync
source .venv/bin/activate
```

Then create your own `.env` file with the keys from step 7 and open the notebooks.