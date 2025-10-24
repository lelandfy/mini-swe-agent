# Mini-SWE-Agent Feature List

## Project Overview
**Mini-SWE-Agent** is a minimalist AI-powered software engineering agent that solves GitHub issues and programming tasks. It's designed as a 100-line alternative to the full SWE-Agent, focusing on simplicity while maintaining effectiveness (68% resolution rate on SWE-bench verified benchmark).

## Core Features

### 🤖 AI Agent Capabilities
- **Autonomous Problem Solving**: Solves GitHub issues and programming tasks using bash commands only
- **Multi-Model Support**: Works with any language model through LiteLLM integration
- **Linear History**: Simple conversation flow without complex tool interfaces
- **Cost & Step Limits**: Configurable limits for model calls and execution steps

### 🔧 Command Line Tools

#### Primary CLI Commands
- **`mini`** - Simple REPL-style interface for interactive problem solving
- **`mini -v`** - Visual pager-style interface (Textual UI)
- **`mini-extra`** / **`mini-e`** - Extended functionality commands

#### Specialized Scripts
- **GitHub Issue Solver** (`github_issue.py`) - Direct GitHub issue resolution
- **SWE-bench Batch Runner** (`swebench.py`) - Batch processing for benchmark evaluation
- **Trajectory Inspector** (`inspector.py`) - Browse and analyze agent conversation logs
- **Hello World Demo** (`hello_world.py`) - Basic functionality demonstration

### 🌐 Environment Support

#### Local Environment
- **Direct Execution**: Runs commands directly on local machine using `subprocess.run`
- **Environment Variables**: Support for custom environment configuration
- **Working Directory**: Configurable working directory for command execution

#### Containerized Environments
- **Docker Support**: Full Docker container integration for sandboxed execution
- **SWE-ReX Docker**: Specialized Docker environment for enhanced security
- **Singularity/Apptainer**: HPC-friendly container support
- **Custom Images**: Support for custom container images and configurations

### 🧠 Model Integration

#### Supported Model Providers
- **Anthropic Models**: Native support with cache control optimization
- **LiteLLM Models**: Universal model compatibility (OpenAI, Claude, etc.)
- **Test Models**: Built-in test models for development
- **Roulette Models**: Random model switching for performance optimization

#### Model Features
- **Cost Tracking**: Automatic cost calculation and monitoring
- **Call Counting**: Track number of API calls made
- **Retry Logic**: Robust error handling with exponential backoff
- **Cache Control**: Intelligent caching for Anthropic models
- **Key Rotation**: Multi-key support for parallel processing

### 🎯 Interactive Modes

#### User Interaction Modes
- **Human Mode** (`/u`): User directly issues commands
- **Confirm Mode** (`/c`): Agent requests confirmation before executing commands
- **YOLO Mode** (`/y`): Agent executes commands without confirmation

#### User Interface Options
- **Terminal Interface**: Rich console output with syntax highlighting
- **Textual UI**: Full-screen pager-style interface with navigation
- **History Support**: Command and task history with search functionality
- **Interruption Handling**: Graceful handling of keyboard interrupts

### 📊 Batch Processing & Evaluation

#### SWE-bench Integration
- **Dataset Support**: Full, Verified, Lite, Multimodal, Multilingual variants
- **Parallel Processing**: Multi-threaded execution for batch jobs
- **Progress Tracking**: Real-time progress monitoring with Live UI
- **Results Management**: Automatic trajectory saving and results aggregation

#### Evaluation Features
- **Instance Filtering**: Regex-based filtering and slicing of test instances
- **Reproducible Results**: Deterministic execution with seed control
- **Performance Metrics**: Automatic calculation of success rates
- **Error Handling**: Comprehensive error tracking and reporting

### 📁 Configuration System

#### Configuration Files
- **YAML-based Config**: Structured configuration using YAML files
- **Template System**: Jinja2 templating for dynamic configuration
- **Environment-specific**: Different configs for different use cases
- **Built-in Configs**: Pre-configured settings for common scenarios

#### Customizable Settings
- **Agent Behavior**: System prompts, response templates, execution limits
- **Environment Setup**: Working directories, environment variables, timeouts
- **Model Parameters**: Temperature, API keys, model-specific settings
- **UI Preferences**: Visual modes, styling, interaction preferences

### 🔍 Debugging & Analysis Tools

#### Trajectory Management
- **Conversation Logging**: Complete conversation history preservation
- **JSON Export**: Structured trajectory data in JSON format
- **Trajectory Browser**: Interactive browsing of saved conversations
- **Step-by-step Navigation**: Detailed inspection of agent reasoning

#### Development Support
- **Rich Logging**: Comprehensive logging with multiple levels
- **Error Tracking**: Detailed error reporting and stack traces
- **Performance Monitoring**: Cost and time tracking for optimization
- **Testing Framework**: Comprehensive test suite with pytest

### 🔐 Security & Sandboxing

#### Execution Safety
- **Container Isolation**: Full sandboxing through Docker/Singularity
- **Timeout Protection**: Command timeouts to prevent infinite loops
- **Resource Limits**: Configurable resource constraints
- **Clean Environment**: Isolated execution environments

#### Security Features
- **No Persistent State**: Each command runs in fresh subprocess
- **Environment Validation**: Safe environment variable handling
- **Access Controls**: Configurable access restrictions
- **Token Management**: Secure API key handling and rotation

### 📈 Performance & Optimization

#### Efficiency Features
- **Minimal Dependencies**: Only essential packages required
- **Fast Startup**: Quick initialization and execution
- **Memory Efficient**: Low memory footprint during execution
- **Parallel Execution**: Multi-threaded batch processing

#### Optimization Tools
- **Cache Control**: Intelligent caching for API calls
- **Model Switching**: Dynamic model selection for optimal performance
- **Resource Monitoring**: Real-time tracking of resource usage
- **Performance Profiling**: Built-in performance analysis tools

### 🎨 User Experience Features

#### Convenience Tools
- **Auto-configuration**: First-time setup assistance
- **History Management**: Persistent command and task history
- **Tab Completion**: Command completion support
- **Syntax Highlighting**: Rich text formatting and highlighting

#### Documentation & Help
- **Built-in Help**: Comprehensive help system with examples
- **FAQ Integration**: Common questions and solutions
- **Usage Examples**: Practical examples and tutorials
- **Configuration Guide**: Detailed configuration documentation

---

This comprehensive feature list covers all major aspects of the mini-SWE-agent project, from its core AI capabilities to its various interfaces, environments, and tooling. The project is designed to be a lightweight yet powerful alternative to more complex AI agent frameworks, focusing on simplicity and effectiveness in solving software engineering tasks.