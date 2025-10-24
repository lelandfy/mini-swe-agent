# Architecture and Components

This document provides an overview of the architecture and components of the mini-swe-agent project.

## Architecture

The project follows a modular architecture with a clear separation of concerns. The main components are:

-   **Agent**: The core logic of the agent, responsible for interacting with the model and the environment.
-   **Model**: The language model used by the agent to generate actions.
-   **Environment**: The execution environment where the agent's actions are performed.
-   **Run**: Entry points for running the agent in different modes.
-   **Config**: Configuration files for the different components.

This modular design allows for easy extension and customization of the agent's capabilities.

## Components

### Agent (`src/minisweagent/agents`)

The agent is the brain of the system. It orchestrates the interaction between the model and the environment to solve a given task.

-   `default.py`: The base agent that implements the main control flow. It takes a task, queries the model for an action, executes the action in the environment, and repeats the process until the task is completed.
-   `interactive.py`: Extends the `DefaultAgent` with interactive features. It allows the user to confirm or deny the actions proposed by the model.
-   `interactive_textual.py`: Provides a rich Textual-based user interface for the interactive agent, offering a more advanced user experience.

### Model (`src/minisweagent/models`)

The model component is responsible for interfacing with different language models.

-   `litellm_model.py`: A generic interface for a wide range of language models, powered by the `litellm` library.
-   `anthropic.py`: A specialized interface for Anthropic's models, which have their own API.
-   `test_models.py`: A set of deterministic models used for testing purposes.

### Environment (`src/minisweagent/environments`)

The environment is the sandbox where the agent executes commands.

-   `local.py`: A simple environment that executes commands directly on the local machine using `subprocess.run`.
-   `docker.py`: An environment that executes commands inside a Docker container, providing a sandboxed and reproducible environment.
-   `singularity.py`: An environment that uses Singularity containers, which are often used in high-performance computing environments.

### Run (`src/minisweagent/run`)

The run scripts are the entry points for different tasks and modes of operation.

-   `mini.py`: The main entry point for the interactive agent. It can be run in a simple REPL-style mode or a more advanced visual mode.
-   `github_issue.py`: An entry point specifically designed for solving GitHub issues. It uses a sandboxed environment to safely execute code.
-   `hello_world.py`: A simple example that demonstrates how to use the `DefaultAgent`.

### Config (`src/minisweagent/config`)

The config component provides a flexible way to configure the agent, model, and environment.

-   The configuration is defined in YAML files, which are easy to read and modify.
-   `mini.yaml`: The default configuration for the interactive agent.
-   `default.yaml`: The default configuration for the default agent.
-   `github_issue.yaml`: The configuration for the `github_issue.py` entry point.

## Control Flow

The agent's control flow is a loop that consists of the following steps:

1.  **Query**: The agent sends the current task and the history of previous interactions to the model.
2.  **Action**: The model returns a command to be executed.
3.  **Execution**: The agent executes the command in the specified environment.
4.  **Observation**: The agent captures the output of the command and adds it to the history.
5.  **Termination**: The loop continues until the agent determines that the task is complete, or a predefined limit (e.g., number of steps, cost) is reached.
