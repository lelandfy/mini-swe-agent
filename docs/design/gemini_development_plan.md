# Building a Software Engineering Agent with Gemini: A Phased Plan

This document outlines a phased approach to building a software engineering agent, similar to `mini-swe-agent`, from scratch. It assumes you are directing an AI assistant like Google Gemini and have access to two key documents: `ARCHITECTURE.md` and `FEATURES.md`.

## The Core Idea

The process is broken down into distinct phases, moving from high-level setup to detailed implementation and finalization. Each phase includes a set of prompts you would give to the AI. The AI is expected to use its tools (e.g., `write_file`, `run_shell_command`, `replace`) to execute the instructions.

---

## Phase 0: Project Initialization & Setup

This phase creates the foundational structure of the project.

**Prompt 1: Create Project Directory and Git Repository**
```
"Let's start a new project called `mini-swe-agent-v2`. Create a directory for it, and inside, initialize a git repository."
```

**Prompt 2: Create Core Files**
```
"Inside `mini-swe-agent-v2`, create the following empty files and directories:
- `pyproject.toml`
- `README.md`
- `.gitignore`
- `src/`
- `tests/`"
```

**Prompt 3: Populate `.gitignore`**
```
"Populate the `.gitignore` file with standard Python exclusions, including `__pycache__/`, `*.pyc`, `.env`, and the virtual environment directory `venv/`."
```

**Prompt 4: Set up Virtual Environment**
```
"Create a Python virtual environment named `venv` inside the project directory."
```

---

## Phase 1: Scaffolding from Architecture

This phase uses `ARCHITECTURE.md` to build the skeleton of the application.

**Prompt 5: Read Architecture**
```
"I have a document named `ARCHITECTURE.md` that describes the high-level design of our project. Please read it to understand the directory structure, core components, and their responsibilities."
```
*(You would provide the content of `ARCHITECTURE.md` here)*

**Prompt 6: Create Directory Structure**
```
"Based on the 'Directory Structure' section of `ARCHITECTURE.md`, create the necessary subdirectories within the `src/` folder. For example, `src/minisweagent/agents`, `src/minisweagent/environments`, etc. Create `__init__.py` in each new module directory."
```

**Prompt 7: Create Skeleton Code**
```
"Now, read the 'Core Components' section of `ARCHITECTURE.md` again. For each component (e.g., `Agent`, `Environment`, `Model`), create the corresponding Python file and define the classes and method signatures described. The methods should be empty for now; just use `pass` in the body and include the docstrings from the architecture document."
```

---

## Phase 2: Implementing Core Features

This phase uses `FEATURES.md` to implement the logic inside the skeleton. This should be done iteratively, feature by feature.

**Prompt 8: Read Features**
```
"I have a `FEATURES.md` document that lists the required functionalities. Please read it to understand what we need to build."
```
*(You would provide the content of `FEATURES.md` here)*

**Prompt 9: Implement a Single Feature (Example)**
```
"Let's implement the first feature: 'Execute a shell command in a local environment'.

1.  Target the `LocalEnvironment` class in `src/minisweagent/environments/local.py`.
2.  Implement the `run_shell` method as described in the feature document.
3.  It should use Python's `subprocess` module to execute the command.
4.  It must capture and return stdout, stderr, and the exit code.
5.  Ensure you handle potential errors, like if a command doesn't exist."
```

**Prompt 10: Set up Testing Framework**
```
"We need to test our features. Set up `pytest` for our project. Add `pytest` to our development dependencies in `pyproject.toml`."
```

**Prompt 11: Write Tests for the Feature**
```
"Now, create a test file `tests/environments/test_local.py`. In this file, write unit tests for the `run_shell` method we just implemented. Include tests for:
1.  A simple command that should succeed (e.g., `echo 'hello'`).
2.  A command that should fail (e.g., `exit 1`).
3.  A command that produces both stdout and stderr.
4.  Verify that the returned stdout, stderr, and exit code are correct in all cases."
```

**(Repeat Prompts 9 and 11 for each feature in `FEATURES.md`)**

---

## Phase 3: Wiring Components Together

After implementing the core modules in isolation, this phase connects them.

**Prompt 12: Implement the Main Agent Logic**
```
"Now let's wire the components together in the main agent class (`DefaultAgent` in `src/minisweagent/agents/default.py`).

1.  Read the `ARCHITECTURE.md` section on 'Control Flow'.
2.  Implement the main `run` method of the agent.
3.  The agent should initialize an environment and a model based on its configuration.
4.  It should then enter a loop: get an observation from the environment, pass it to the model to get a predicted action, and execute that action in the environment.
5.  Log the trajectory (observation, action, reward) at each step."
```

**Prompt 13: Implement the CLI Entrypoint**
```
"Create a command-line interface using a library like `typer` or `argparse`.
1.  Create `src/minisweagent/__main__.py`.
2.  This CLI should allow a user to start the agent.
3.  It should take arguments to specify the configuration file, the environment type, and the model to use.
4.  This script will instantiate and run the `DefaultAgent`."
```

---

## Phase 4: Documentation and Finalization

**Prompt 14: Generate README**
```
"Based on the `FEATURES.md` and our project structure, generate a comprehensive `README.md`. It should include:
- A project description.
- Installation instructions (how to install dependencies from `pyproject.toml`).
- A quickstart guide on how to run the agent from the command line."
```

**Prompt 15: Code Quality and Linting**
```
"Set up `ruff` for linting and formatting. Add a `ruff.toml` or configure it in `pyproject.toml`. Run the linter and fix any reported issues across the entire codebase."
```

---

## Phase 5: CI/CD Automation

**Prompt 16: Create a CI Workflow**
```
"Create a GitHub Actions workflow file at `.github/workflows/ci.yaml`. This workflow should trigger on every push to the `main` branch. It must perform the following jobs:
1.  **Lint**: Check out the code, install dependencies, and run `ruff check .`.
2.  **Test**: Check out the code, install dependencies (including `pytest`), and run `pytest`."
```
