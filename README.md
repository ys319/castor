# Castor - A Gemini Environment Switcher

Castor is a simple, POSIX-compliant shell script for quickly switching between different configuration contexts for the `gemini` command. It works by managing a symbolic link (`GEMINI.md`) in your working directory to predefined configuration files, which we call "gems".

This tool is designed to be lightweight, fast, and easily integrated with shell plugin managers like [Sheldon](https://github.com/rossmacarthur/sheldon).

## Features

- **Context Switching:** Easily switch between different `gemini` configurations (gems).
- **Customizable Command:** Use the `CASTOR_GEMINI_COMMAND` variable to run any command you need.
- **POSIX-compliant:** The core script is written in `sh` for maximum portability.
- **Tab Completion:** Comes with command-line completion for Zsh.
- **Plugin Manager Friendly:** Designed to work seamlessly with Sheldon.

## Prerequisites

- A Unix-like operating system (Linux, macOS).
- A shell (e.g., `sh`, `bash`, `zsh`).
- The `gemini` command (or your custom command) must be installed and available in your `$PATH`.

## Installation

### Installation with Sheldon (Recommended)

This is the easiest and recommended way to install Castor.

1. **Add to Sheldon Config:** Add the following to your `plugins.toml` file (usually at `~/.config/sheldon/plugins.toml`). Sheldon will automatically handle cloning the repository and setting up the path and completion.

    ```toml
    # ~/.config/sheldon/plugins.toml

    [plugins.castor]
    github = "ys319/castor"
    apply = ["source"]
    ```

2. **Set Environment Variables:** Proceed to the **"Configuration"** section below.

3. **Apply Changes:** After saving your configuration, restart your shell or run `sheldon` to apply the changes.

### Manual Installation

Use this method if you are not using Sheldon.

1. **Clone the Repository:** Clone this repository to a permanent location on your local machine.

    ```sh
    git clone https://github.com/ys319/castor.git /path/to/your/castor
    ```

2. **Update Your Shell Profile:** You need to add the `bin` directory to your `$PATH` and set up tab completion. Add the following to your `~/.zshrc` or `~/.bashrc`:

    **For Zsh:**

    ```sh
    # ~/.zshrc
    # For castor
    source "/path/to/your/castor/castor.plugin.zsh"
    ```

    Remember to replace `/path/to/your/castor` with the actual path.

3. **Set Environment Variables:** Proceed to the next section.

## Configuration (Required)

Castor is configured via environment variables. You must set these in your shell's startup file (e.g., `~/.zshrc`, `~/.bashrc`).

### 1. `CASTOR_GEMS_DIR` (Required)

This variable tells Castor where your gem files are stored.

- First, create a directory for your gems:

    ```sh
    mkdir -p ~/my-gems
    touch ~/my-gems/default.md
    touch ~/my-gems/coding.md
    ```

- Then, add the export line to your shell profile:

    ```sh
    export CASTOR_GEMS_DIR="$HOME/my-gems"
    ```

### 2. `CASTOR_GEMINI_COMMAND` (Optional)

This variable specifies the command that Castor will execute. If not set, it defaults to `gemini -s`.

- You can override it to run a different command or add more arguments.

    ```sh
    # Example: Run gemini in a different mode
    export CASTOR_GEMINI_COMMAND="gemini --some-other-flag"

    # Example: Run a completely different tool
    export CASTOR_GEMINI_COMMAND="my-custom-tool --config GEMINI.md"
    ```

### 3. Reload Your Shell

To apply your changes, either start a new terminal session or run one of the following commands to replace your current shell with a new one:

- **For Zsh:**

    ```sh
    exec zsh -l
    ```

- **For Bash:**

    ```sh
    exec bash -l
    ```

## Usage

To use Castor, simply run the `castor` command with the name of the gem you want to use.

```sh
castor [gem_name]
```

- If `[gem_name]` is provided, Castor will look for a file named `${gem_name}.md` in your `$CASTOR_GEMS_DIR`.
- If `[gem_name]` is omitted, it will default to `default.md`.

Castor will then create a symbolic link named `GEMINI.md` in your current directory, pointing to the selected gem file, and then execute the configured command.

**Example:**

```sh
# This will link ./GEMINI.md -> ~/my-gems/coding.md and run `gemini -s` (or your custom command)
castor coding
```
