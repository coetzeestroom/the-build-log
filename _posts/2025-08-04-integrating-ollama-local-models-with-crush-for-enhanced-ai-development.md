---
layout: post
title: "Integrating Ollama Local Models with Crush for Enhanced AI Development"
date: 2025-08-01 10:00:00 +0000
author: Ray
categories: [AI, LLM, Ollama, Crush, Local Development]
tags: [AI, LLM, Ollama, Crush, Local Models, Development, Open Source]
image: assets/images/Gemini_Generated_Image_jtawz1jtawz1jtaw.png
prompt: generate me a landscape header page image for a personal blog called "the build log" which is about playing with computers, ai, music, linux and general techi stuff. The image should use an illustrative style, and use colours that are 'retro' - beige, brown, cyan. There should cute miniature jack russel wearing a pair of headphones. The jack russel is be completely white, but has brown ears. He's the focal point of the image. His name is 'Nobby', and he wearing a collar and name tag in the shape of a raspberry pi. The theme for today's blog article is about integrating Crush with Ollama 
---

# Unleash Local LLMs: A Guide to Ollama and Crush Integration

In the rapidly evolving world of Large Language Models (LLMs), the ability to run models locally offers unparalleled privacy, control, and often, cost-effectiveness. This guide will walk you through setting up Ollama, a powerful tool for running LLMs on your machine, and integrating it with Crush, a sleek terminal-based interface for interacting with your models.

## Why Local LLMs with Ollama and Crush?

**Ollama** simplifies the process of running open-source LLMs locally. It handles model downloads, setup, and provides an API for interaction, making it incredibly easy to experiment with various models.

**Crush** by Charmbracelet offers an intuitive and feature-rich terminal interface for your LLMs. It provides session management, LSP integration for code context, and the flexibility to switch between models, making it an ideal companion for local AI development and interaction.

Combining Ollama and Crush empowers you to:
*   **Maintain Privacy:** Your data stays on your machine.
*   **Cost-Effectiveness:** Eliminate API costs associated with cloud-based LLMs.
*   **Offline Development:** Work on your projects without an internet connection.
*   **Customization:** Fine-tune models and integrate them into your local workflows.
*   **Experiment Freely:** Easily switch between different models and configurations.

## Step 1: Setting Up Ollama

First, you need to install Ollama and download your desired LLM.

1.  **Install Ollama:**
    Visit the official Ollama website ([ollama.ai](https://ollama.ai/)) and follow the installation instructions for your operating system (macOS, Linux, Windows).

2.  **Download an LLM:**
    Once Ollama is installed, you can download models using the `ollama pull` command. For example, to download the Llama 3 model:
    ```bash
    ollama pull llama3
    ```
    You can explore other available models on the Ollama website.

3.  **Verify Ollama is Running:**
    Ollama typically runs as a background service. You can verify it's running by checking its status or by trying to run a model:
    ```bash
    ollama run llama3 "Hello, how are you?"
    ```
    This will start the Llama 3 model and process your prompt.

## Step 2: Installing Crush

Next, let's install Crush. You have several options depending on your system.

**Using Homebrew (macOS/Linux):**
```bash
brew install charmbracelet/tap/crush
```

**Using NPM:**
```bash
npm install -g @charmland/crush
```

**Using Go:**
```bash
go install github.com/charmbracelet/crush@latest
```

For other installation methods (Arch Linux, Nix, Debian/Ubuntu, Fedora/RHEL, or direct binaries), refer to the [Crush GitHub README](https://github.com/charmbracelet/crush/blob/main/README.md) for detailed instructions.

## Step 3: Configuring Crush for Ollama

Crush can connect to Ollama because Ollama exposes an OpenAI-compatible API. We'll create a `crush.json` configuration file to tell Crush how to find and use your local Ollama models.

1.  **Create `crush.json`:**
    Crush looks for configuration in a few places, with project-local files taking precedence. For a global setup, create the file at `$HOME/.config/crush/crush.json`.

    ```bash
    mkdir -p $HOME/.config/crush
    touch $HOME/.config/crush/crush.json
    ```

2.  **Add Ollama as a Custom Provider:**
    Open `$HOME/.config/crush/crush.json` in your favorite text editor and add the following JSON configuration. Replace `"llama3"` with the `id` of the model you pulled with Ollama if it's different.

    ```json
    {
      "$schema": "https://charm.land/crush.json",
      "providers": {
        "ollama": {
          "type": "openai",
          "base_url": "http://localhost:11434/v1",
          "api_key": "ollama",
          "models": [
            {
              "id": "llama3",
              "name": "Llama 3 (Ollama)",
              "cost_per_1m_in": 0,
              "cost_per_1m_out": 0,
              "context_window": 8192,
              "default_max_tokens": 4096
            }
          ]
        }
      }
    }
    ```
    **Explanation of the configuration:**
    *   `"ollama"`: This is a custom name for your provider within Crush.
    *   `"type": "openai"`: Specifies that Ollama's API is compatible with OpenAI's.
    *   `"base_url": "http://localhost:11434/v1"`: This is the default local API endpoint for Ollama.
    *   `"api_key": "ollama"`: For local Ollama, the API key isn't strictly necessary for authentication, but Crush requires a value. You can use a placeholder like `"ollama"` or an empty string.
    *   `"models"`: An array defining the models available through this provider.
        *   `"id"`: The exact name of the model you pulled with `ollama pull` (e.g., `llama3`).
        *   `"name"`: A user-friendly name that will appear in Crush.
        *   `"cost_per_1m_in"` and `"cost_per_1m_out"`: Set to `0` as local models don't incur monetary costs.
        *   `"context_window"` and `"default_max_tokens"`: These values depend on the specific model you are using. Refer to the model's documentation or Ollama's model library for accurate numbers. The values provided are common for Llama 3.

## Step 4: Running and Testing Crush with Ollama

With Ollama running and Crush configured, you can now launch Crush and interact with your local LLM.

1.  **Start Crush:**
    Simply run `crush` in your terminal:
    ```bash
    crush
    ```

2.  **Select Your Model:**
    Crush should now present you with a list of available models, including "Llama 3 (Ollama)" (or whatever name you gave it). Select your Ollama model.

3.  **Interact with Your Local LLM:**
    You can now start typing your prompts and interact with your local LLM directly from your terminal!

## Troubleshooting Tips

*   **Ollama Not Running:** Ensure the Ollama service is active. Restart it if necessary.
*   **Incorrect `base_url`:** Double-check that `http://localhost:11434/v1` is the correct API endpoint for your Ollama installation.
*   **Model ID Mismatch:** Ensure the `"id"` in your `crush.json` exactly matches the name of the model you pulled with `ollama pull`.
*   **Firewall Issues:** If you're having connection problems, check your firewall settings to ensure that Crush can communicate with Ollama on port `11434`.

## Conclusion

By following these steps, you've successfully set up a powerful local LLM environment using Ollama and integrated it with Crush for a seamless terminal-based experience. This setup provides a robust foundation for private, efficient, and flexible AI interactions directly on your machine. Experiment with different models, explore Crush's features, and unlock the full potential of local LLMs!
