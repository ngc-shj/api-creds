# api_creds - Secure API Credentials Manager

A security-enhanced command-line utility for securely storing and using API credentials for various AI services like OpenAI, Anthropic, Hugging Face, and others.

## Features

- **Strong Encryption** - Uses OpenSSL AES-256-CBC to securely encrypt API keys at rest
- **Environment Variable Management** - Easily set credentials as environment variables
- **Secure Input Method** - Add API keys with hidden input (like password entry)
- **XDG Compliance** - Follows XDG Base Directory Specification for file storage
- **Flexible Usage** - Run as a command or source as shell functions
- **Tab Completion** - Supports Bash and Zsh tab completion
- **Key Rotation** - Regularly rotate your encryption key for enhanced security

## Installation

```bash
# Clone the repository
git clone https://github.com/ngc-shj/api_creds.git

# Make the script executable
chmod +x api_creds/api_creds

# Create a symlink in your PATH (optional)
ln -s "$(pwd)/api_creds/api_creds" ~/.local/bin/api_creds

# Set up shell completion (optional)
api_creds completion
```

## Usage

### Adding API Keys

```bash
# Secure interactive mode (recommended)
api_creds add OPENAI_API_KEY
# You'll be prompted to enter the key securely (input will be hidden)

# Direct mode (not recommended, leaves keys in shell history)
api_creds add OPENAI_API_KEY sk-abcdef123456
```

### Using API Keys

```bash
# Method 1: Eval (recommended for single use)
eval $(api_creds use OPENAI_API_KEY)

# Method 2: Source the script first, then use functions
source api_creds use OPENAI_API_KEY

# Method 3: Run a command with credentials
api_creds run OPENAI_API_KEY python my_script.py

# Use multiple credentials
api_creds run OPENAI_API_KEY ANTHROPIC_API_KEY python my_script.py
```

### Other Commands

```bash
# List all stored credentials (only shows names, not values)
api_creds list

# Remove a credential
api_creds remove OPENAI_API_KEY

# Export all credentials as environment variables
api_creds export

# Rotate encryption key (recommended security practice)
api_creds rotate-key

# Show debug information
api_creds debug

# Reset encryption (backs up existing credentials)
api_creds reset
```

## Shell Completion

For tab completion, add the following to your shell configuration file:

### Bash
```bash
# Add to ~/.bashrc
if command -v api_creds >/dev/null 2>&1; then
  source <(api_creds completion bash)
fi
```

### Zsh
```zsh
# Add to ~/.zshrc
if command -v api_creds >/dev/null 2>&1; then
  source <(api_creds completion zsh)
fi
```

## Security Information

- API keys are encrypted with AES-256-CBC using OpenSSL
- A master key is securely stored in your data directory
- Key files and credential files are protected with restrictive permissions (600)
- Temporary files are securely deleted using `shred`
- Input is masked when entering API keys interactively

## File Locations

Following XDG Base Directory Specification:

- Configuration: `$XDG_CONFIG_HOME/api_creds/` (typically `~/.config/api_creds/`)
- Data files: `$XDG_DATA_HOME/api_creds/` (typically `~/.local/share/api_creds/`)
- State files: `$XDG_STATE_HOME/api_creds/` (typically `~/.local/state/api_creds/`)

## Requirements

- Bash 3.2+
- OpenSSL
- coreutils (for the `shred` command)

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
