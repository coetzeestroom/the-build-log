---
layout: post
title: "Installing OpenCode (by SST): Your Open-Source Terminal AI Coding Agent"
author: Ray
categories: [AI, Tools, Development]
tags: [AI, terminal, coding, agent, installation, OpenCode, SST]
image: assets/images/Gemini_Generated_Image_21zagu21zagu21za.png
---

Are you looking for a powerful, open-source AI coding agent that integrates seamlessly with your terminal workflow? **OpenCode**, developed by SST, is designed to be just that. It's a versatile tool that brings intelligent coding assistance directly to your command line, supporting various AI providers and focusing on a rich terminal user interface.

This guide will walk you through the straightforward installation process for OpenCode, getting you ready to enhance your development experience.

## Installation Methods

OpenCode offers several convenient ways to install it on your system. Choose the method that best fits your environment and preferences.

### 1. The "YOLO" Install Script (Quickest Way)

For a fast and easy installation of the latest version, you can use the provided `curl` script. This method is often the quickest way to get started.

```bash
curl -fsSL https://opencode.ai/install | bash
```

> **Tip:** If you have versions older than 0.1.x installed, it's recommended to remove them before proceeding with this installation.

### 2. Using Package Managers

OpenCode is also available through popular package managers, offering a more integrated installation experience for many users.

#### npm (Node.js Package Manager) / Bun / pnpm / Yarn

If you have Node.js and npm (or Bun, pnpm, Yarn) installed, you can install OpenCode globally:

```bash
npm i -g opencode-ai@latest        # or bun/pnpm/yarn
```

#### Homebrew (macOS)

macOS users can leverage Homebrew for a simple installation:

```bash
brew install sst/tap/opencode
```

#### AUR (Arch Linux)

Arch Linux users can install OpenCode from the Arch User Repository (AUR) using their preferred AUR helper (e.g., `paru`):

```bash
paru -S opencode-bin
```

### Custom Installation Directory

The install script provides flexibility for where OpenCode is installed. It respects the following environment variables for the installation path, in order of priority:

1.  `$OPENCODE_INSTALL_DIR`: Specify a custom directory.
2.  `$XDG_BIN_DIR`: Adhere to the XDG Base Directory Specification.
3.  `$HOME/bin`: A standard user binary directory.
4.  `$HOME/.opencode/bin`: The default fallback location.

Here are examples of how to use these variables:

```bash
OPENCODE_INSTALL_DIR=/usr/local/bin curl -fsSL https://opencode.ai/install | bash
XDG_BIN_DIR=$HOME/.local/bin curl -fsSL https://opencode.ai/install | bash
```

## Documentation and Configuration

For more in-depth information on how to configure OpenCode, including setting up API keys for various AI providers (like Anthropic, OpenAI, Google), custom commands, and advanced features, please refer to the official documentation:

[**OpenCode Documentation**](https://opencode.ai/docs)

## Basic Usage

Once installed, you can launch OpenCode by simply typing `opencode` in your terminal:

```bash
opencode
```

This will open the interactive terminal user interface, ready for your coding queries.

## Conclusion

OpenCode by SST offers a robust and flexible AI coding agent experience directly in your terminal. With its straightforward installation and comprehensive features, it's a valuable tool for any developer looking to integrate AI assistance into their daily workflow. Get started today and explore the power of terminal-native AI!
