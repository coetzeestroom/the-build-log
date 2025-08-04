---
layout: post
title: "Installing Crush: Your Terminal's New AI Coding Agent"
author: Ray
categories: [AI, Tools, Development]
tags: [AI, terminal, coding, agent, installation, Crush]
image: assets/images/Gemini_Generated_Image_cggm6rcggm6rcggm.png
---

Are you looking for a powerful AI coding assistant that lives right in your terminal? Look no further than **Crush**, the successor to the OpenCode project. Crush is a Go-based CLI application that brings intelligent coding assistance, debugging, and more, directly to your command line.

This guide will walk you through the various ways to install Crush on your system, so you can start leveraging its capabilities today.

## Installation Methods

Crush offers several convenient ways to get it up and running on your machine. Choose the method that best suits your environment.

### 1. Using the Install Script (Recommended for most users)

The quickest way to install the latest version of Crush is by using the official install script. This method is generally recommended for most users as it handles dependencies and proper placement.

```bash
curl -fsSL https://raw.githubusercontent.com/charmbracelet/crush/refs/heads/main/install | bash
```

If you need to install a specific version, you can specify it with the `VERSION` environment variable:

```bash
curl -fsSL https://raw.githubusercontent.com/charmbracelet/crush/refs/heads/main/install | VERSION=0.1.0 bash
```

### 2. Using Homebrew (macOS and Linux)

For macOS and Linux users who prefer a package manager, Homebrew provides a straightforward installation experience:

```bash
brew install charmbracelet/tap/crush
```

### 3. Using AUR (Arch Linux)

Arch Linux users can install Crush via the Arch User Repository (AUR) using their preferred AUR helper, such as `yay` or `paru`:

```bash
# Using yay
yay -S crush-bin

# Using paru
paru -S crush-bin
```

### 4. Using Go (For Go developers)

If you have Go installed and prefer to manage your Go binaries directly, you can install Crush using the `go install` command:

```bash
go install github.com/charmbracelet/crush@latest
```

## Configuration (Optional but Recommended)

After installation, you might want to configure Crush to connect with your preferred AI models (e.g., OpenAI, Anthropic Claude, Google Gemini). Crush looks for configuration in `~/.crush.json`, `$XDG_CONFIG_HOME/crush/.crush.json`, or `./.crush.json`.

You'll typically need to set up environment variables for your API keys, such as `ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, or `GEMINI_API_KEY`, depending on the models you plan to use. Refer to the [Crush GitHub repository](https://github.com/charmbracelet/crush) for detailed configuration options.

## Basic Usage

Once installed, you can start Crush by simply typing `crush` in your terminal:

```bash
crush
```

This will launch the interactive Terminal User Interface (TUI).

For quick, non-interactive prompts, you can pass your query directly as a command-line argument:

```bash
crush -p "Explain the use of context in Go"
```

You can also specify the output format (e.g., JSON) and suppress the spinner for scripting:

```bash
crush -p "Explain the use of context in Go" -f json -q
```

## Conclusion

Crush is a powerful and evolving AI coding agent that can significantly enhance your development workflow directly from the terminal. With multiple installation options and flexible configuration, getting started is easy. Give it a try and experience the future of terminal-based AI assistance!
