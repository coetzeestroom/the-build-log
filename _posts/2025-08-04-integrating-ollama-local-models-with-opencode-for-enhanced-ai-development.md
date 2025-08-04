---
layout: post
title: "Integrating Ollama Local Models with Opencode for Enhanced AI Development"
date: 2025-08-01 10:30:00 +0000
categories: [AI, LLM, Development]
author: Ray
tags: [Ollama, Opencode, Local LLM, AI, Tooling, Development Setup]
image: assets/images/Gemini_Generated_Image_1novvy1novvy1nov.png
prompt: generate me a landscape header page image for a personal blog called "the build log" which is about playing with computers, ai, music, linux and general techi stuff. The image should use an illustrative style, and use colours that are 'retro' - beige, brown, cyan. There should cute miniature jack russel wearing a pair of headphones. The jack russel is be completely white, but has brown ears. He's the focal point of the image. His name is 'Nobby', and he wearing a collar and name tag in the shape of a raspberry pi. The theme for today's blog article is about integrating opencode with Ollama. Include the words 'opencode' and Ollama and images of llamas 
---
## Integrating Ollama Local Models with Opencode for Enhanced AI Development

In the rapidly evolving landscape of AI development, the ability to leverage powerful Large Language Models (LLMs) locally offers significant advantages in terms of privacy, cost-efficiency, and offline capabilities. [Opencode](https://opencode.ai) is a versatile AI coding agent that can be extended to work with various LLM providers. This article will guide you through setting up Ollama-based locally hosted models to work seamlessly with Opencode, drawing insights from community discussions and practical examples.

### Why Local Models with Opencode?

Combining Opencode with local LLMs powered by [Ollama](https://ollama.com/) provides a robust development environment:

*   **Privacy:** Your code and data remain on your machine, crucial for sensitive projects.
*   **Cost-Effectiveness:** Eliminate API costs associated with cloud-based LLMs.
*   **Offline Development:** Work on your projects without an internet connection.
*   **Customization:** Experiment with various open-source models and fine-tune them for specific tasks.

### Setting Up Ollama

Before configuring Opencode, ensure you have Ollama installed and your desired models pulled.

1.  **Install Ollama:** Follow the instructions on the [Ollama website](https://ollama.com/download) to install it on your operating system.
2.  **Pull Models:** Use the `ollama pull` command to download models. For example, to pull a tool-capable model like `qwen3:30b` or `devstral`:
    ```bash
    ollama pull qwen3:30b
    ollama pull devstral
    ```
    Ensure the models you choose are known to support tool calling for optimal integration with Opencode.

### Configuring Opencode for Ollama

Opencode's configuration is managed through an `opencode.json` file, where you define your LLM providers and models. To integrate Ollama, you'll add a new provider entry.

According to a discussion on the Opencode GitHub repository, Issue #1068, users have successfully configured Ollama models for tool use. Here's an example configuration snippet from that issue:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      },
      "models": {
        "qwen3:30b": {
            "name": "Qwen3:30b 65k MoE - local",
            "tools": true,
            "reasoning": true,
            "options": { "num_ctx": 65536 }
        },
        "devstral": {
            "name": "Devstral 14b Dense - local",
            "tools": true,
            "reasoning": false,
            "options": { "num_ctx": 131072 }
        }
      }
    }
    // ... other providers
  }
}
```
**Reference:** [Tool use with Ollama models. · Issue #1068 · sst/opencode](https://github.com/sst/opencode/issues/1068)

**Key points in this configuration:**

*   **`provider.ollama`**: Defines Ollama as a new LLM provider.
*   **`npm`: `@ai-sdk/openai-compatible`**: Specifies the npm package that provides compatibility with OpenAI-like APIs, which Ollama often mimics.
*   **`options.baseURL`**: This is crucial. It points to your local Ollama API endpoint, typically `http://localhost:11434/v1`. Ensure your Ollama server is running on this address.
*   **`models`**: Under this, you list the specific Ollama models you want Opencode to use.
    *   **`qwen3:30b` and `devstral`**: These are examples of model names.
    *   **`name`**: A user-friendly name for the model in Opencode.
    *   **`tools: true`**: **This is vital for enabling tool calling.** It tells Opencode that this model is capable of using tools.
    *   **`reasoning: true/false`**: Indicates if the model has strong reasoning capabilities, which can influence Opencode's behavior.
    *   **`options.num_ctx`**: Sets the context window size for the model.

### Addressing Tool Calling Challenges

While setting up, you might encounter issues where Ollama models don't consistently perform tool calls. This has been a topic of discussion within the Opencode community. Issue #1034, titled "Local Ollama tool calling either not calling or failing outright," highlights this challenge.

As described in Issue #1034:
> "Hi, I'm trying a local `Ollama` model that supports tool calls (`qwen3:32b`). When I ask for example \"What networks am I connected to?\" the model just thinks about which tool to use to fulfill the request, but never does anything. Occasionally it will do something, as I've seen with prompts like \"What's in my .config files?\", and its seemingly correctly generating the tool-call `JSON` in the ballpark of `{\"Bash\": \"LS\", ...}` or something, but the command never actually executes anything and is just printed to the chat."
**Reference:** [Local Ollama tool calling either not calling or failing outright · Issue #1034 · sst/opencode](https://github.com/sst/opencode/issues/1034)

The configuration snippet from Issue #1034 is similar:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama (local)",
      "options": {
        "baseURL": "http://127.0.0.1:11434/v1"
      },
        "qwen3:32b": {
          "name": "Qwen 3 32B (local)"
      }
    }
  }
}
```

**Troubleshooting Tips for Tool Calling:**

*   **Verify Ollama Server:** Ensure your Ollama server is running and accessible at the `baseURL` specified in `opencode.json`.
*   **Model Capability:** Confirm that the specific Ollama model you're using is indeed "tool-capable." Not all models support tool calling out-of-the-box.
*   **Opencode Settings:** Double-check that tools are enabled for your Ollama model within Opencode's settings.
*   **Prompt Engineering:** Sometimes, explicit prompting can encourage tool use. Try phrasing your requests in a way that clearly indicates the need for a tool.
*   **Logs:** Check Opencode's internal logs for any errors related to tool execution or model responses.

### Best Practices and Tips

*   **Start Simple:** Begin with a basic tool-capable model and simple tasks to ensure your setup is correct.
*   **Monitor Ollama:** Keep an eye on Ollama's console output for any errors or warnings.
*   **Update Regularly:** Keep both Opencode and Ollama updated to their latest versions to benefit from bug fixes and new features.
*   **Experiment:** Try different Ollama models. Some models might be better at tool calling than others.

### Conclusion

Integrating Ollama local models with Opencode opens up a world of possibilities for private, cost-effective, and powerful AI-assisted development. While challenges with tool calling can arise, understanding the configuration and troubleshooting steps, as discussed in the Opencode community, will help you overcome them. Embrace the power of local LLMs and enhance your coding workflow with Opencode!