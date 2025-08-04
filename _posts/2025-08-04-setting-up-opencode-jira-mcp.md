---
layout: post
title: "Setting Up Opencode to Use the Jira MCP Server"
date: 2025-08-04 20:30:00 +0200
categories: [Opencode, Jira, Confluence, Automation, MCP]
tags: [Opencode, Jira, Confluence, MCP, Integration, Automation, DevOps]
image: assets/images/Gemini_Generated_Image_21zagu21zagu21za.png

---

### 🛠️ Setting Up Opencode to Use the Jira MCP Server

**Overview**  
This guide explains how to configure Opencode to communicate with your Atlassian Jira MCP (Multi-Project Connectivity) server, enabling seamless automation of tasks like issue management, content creation, and data synchronization.

---

#### 🧱 Step 1: Configure the MCP Server  
1. **Enable the MCP Server**  
   Ensure your Jira MCP server is running and accessible. Verify the server URL and authentication credentials (e.g., API token or username/password).  

   Example configuration snippet (adjust as needed):  
   ```json
   {
     "server_url": "https://your-jira-mcp-server.com",
     "enabled": true,
     "proxy_url": "sian.com/v1/sse"
   }
   ```

2. **Set Up Proxy Settings**  
   If your network requires a proxy, configure it in Opencode's settings. This ensures secure communication between Opencode and the MCP server.

---

#### 🚀 Step 2: Start the Opencode Proxy  
1. **Launch the Proxy Service**  
   Run the Opencode proxy in the background to establish a persistent connection to the MCP server. This allows real-time data synchronization and task automation.  

   ```bash
   opencode-proxy start
   ```

2. **Verify Connectivity**  
   Check logs to confirm successful connection to the MCP server. Look for confirmation messages like `Connected to Jira MCP server at [URL]`.

---

#### 📌 Step 3: Use Opencode to Interact with Atlassian  
Once configured, Opencode can automate the following tasks:  

1. **Search Jira Issues**  
   Use Opencode to query Jira issues via JQL (e.g., `project = "Support" AND status = "In Progress"`).  

2. **Create New Issues or Pages**  
   Automate issue creation in Jira or Confluence page generation with templated content.  

3. **Update Existing Content**  
   Modify Jira issues, Confluence pages, or other Atlassian artifacts using Opencode’s API integration.  

---

#### 📝 Example Workflow  
**Scenario**: Automate issue creation in Jira when a Confluence page is updated.  
1. Configure Opencode to monitor Confluence page changes.  
2. Use a webhook or scheduled task to trigger a Jira issue creation via the MCP server.  
3. Sync the Jira issue ID back to the Confluence page for reference.  

---

#### 🛠️ Troubleshooting Tips  
- **Connection Errors**: Verify the MCP server URL and proxy settings.  
- **Permissions**: Ensure Opencode has the necessary permissions to access Jira/Confluence resources.  
- **Logs**: Check Opencode’s debug logs for detailed error messages.  

---

#### 📈 Benefits of Integration  
- **Efficiency**: Reduce manual tasks with automated workflows.  
- **Consistency**: Maintain synchronized data across Jira and Confluence.  
- **Scalability**: Handle complex operations with minimal user intervention.  

---

By following this guide, you’ll unlock powerful automation capabilities for your Atlassian workflows using Opencode. Let me know if you need further customization! 🚀
