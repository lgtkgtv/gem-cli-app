# Gemini cli 

```
## Node.js  LTS install 

curl -fsSL https://deb.nodesource.com/setup_22.x | sudo bash -
sudo apt-get install -y nodejs

## ONE TIME:  
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'

npm install -g npm@11.5.2
nodejs -v
npm -v

#   Change the default directory where npm installs global packages. 
#   This moves the installation location from a system-owned directory to a user-owned one, eliminating the need for sudo
export PATH=~/.npm-global/bin:$PATH

## Instructions to build gemini-cli from source code
cd $HOME
git clone https://github.com/google-gemini/gemini-cli.git
cd  gemini-cli

npm install
npm run build
npm run start

node ~/gemini-cli/bundle/gemini.js    # Launches gemini cli 
    OR
alias gem="node ~/gemini-cli/bundle/gemini.js"
# gemini auth login

```

---

# GEMINI_API_KEY 

    https://aistudio.google.com/apikey
    https://ai.google.dev/gemini-api/docs/quickstart?lang=python

```
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent" \
  -H 'Content-Type: application/json' \
  -H "X-goog-api-key: $GEMINI_API_KEY" \
  -X POST \
  -d '{
    "contents": [
      {
        "parts": [
          {
            "text": "Explain how AI works in a few words"
          }
        ]
      }
    ]
  }'
```


# **Gemini CLI**

The **Gemini CLI**, an open-source AI agent developed by Google, is designed to bring the power of the Gemini models to the developer's terminal. 
Its architecture emphasizes transparency, modularity, and extensibility, allowing developers to inspect, modify, and extend its functionality.

## Ref
* from Medium - [Unpacking the Gemini CLI: A High-Level Architectural Overview](https://medium.com/@jalateras/unpacking-the-gemini-cli-a-high-level-architectural-overview-99212f6780e7)
* from MPG ONE- [How to Use Gemini CLI: Complete Guide for Developers and Beginners](https://mpgone.com/how-to-use-gemini-cli-complete-guide-for-developers-and-beginners/)
* from Keyword - [Gemini CLI: your open-source AI agent](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/)
* from Dev Community - [Gemini CLI Full Tutorial](https://dev.to/proflead/gemini-cli-full-tutorial-2ab5)


## Core architecture

#### **Monorepo Structure**: 
The Gemini CLI is built as a monorepo, housing multiple related packages within a single repository.   
This facilitates coordinated development, shared type safety, consistent tool integration, and a simplified testing strategy, according to Medium.

#### **Client-Side Application (packages/cli)**: 
This is responsible for the user interface, handling input, displaying output, and managing conversations within the terminal.   
It uses **Ink**, a React renderer for command-line applications, for a dynamic and interactive experience.

#### **Local Server (packages/core)**: 
This acts as the intermediary between the CLI and the Gemini API.   
It orchestrates AI model interaction, manages tool execution, handles configuration, and provides services to the CLI.

#### **Reason and Act (ReAct) Loop**: 
The AI agent within the CLI utilizes the `ReAct loop`, a framework that enables it to reason and plan before executing actions, according to [MPG ONE](https://mpgone.com/how-to-use-gemini-cli-complete-guide-for-developers-and-beginners/).   
This involves analyzing the problem, selecting the appropriate tools, executing them, observing the results, and adjusting if needed.

#### **Tool System**: 
A key aspect of Gemini CLI's architecture is its comprehensive tool system.  
These tools, located in **`packages/core/src/tools/`**, give the AI direct access to the local environment to perform tasks like file operations, system command execution, and network requests. 

## Extensibility and integration

#### **Model Context Protocol (MCP)**: 
Gemini CLI supports the open-standard Model Context Protocol, enabling it to integrate with external services and tools. 
This allows developers to add new capabilities by configuring MCP servers that the CLI can communicate with, according to [DEV Community](https://dev.to/proflead/gemini-cli-full-tutorial-2ab5).

#### **Bundled Extensions**: 
Gemini CLI comes with pre-built extensions that combine `MCP servers with configuration files and custom Gemini.md files` for project-specific customization.

#### **Custom Prompts (GEMINI.md)**: 
Developers can define custom prompts and instructions within **`GEMINI.md`** files to tailor Gemini CLI's behavior and responses for specific projects and workflows. 

#### **Large Context Window**: 
Leveraging the Gemini 2.5 Pro model's 1 million token context window, Gemini CLI can process large codebases and complex contexts, according to The Keyword.

#### **Security: Local Operation**: 
The Gemini CLI runs locally on your machine, ensuring code, proprietary data, and sensitive information are not sent to external servers.

#### **Security Layers**: 
Google designed Gemini CLI with enterprise-grade security in mind, incorporating features like process isolation, fine-grained permission controls, network restrictions, and file system boundaries, according to MPG ONE.

#### **Intelligent Code Assistance**: 
Gemini CLI provides powerful AI capabilities, such as code understanding, file manipulation, command execution, and dynamic troubleshooting, according to The Keyword.


## **Built-in Tools**: 

The CLI comes with a suite of essential tools that enable it to interact with your local system and the web. These include:

**File System Tools**: `read_file`, `write_file`, `ls`, and `grep` for manipulating files and directories.  
**Execution Tools**: `run_shell_command` for executing system commands, automating build processes, and running tests.  
**Web Tools**: `web_fetch` and `web_search` for gathering external information and grounding prompts with real-time data.  


## **The Reason and Act (ReAct) Loop**
The Gemini CLI operates on a Reason and Act (ReAct) loop. 
This is the core logic that powers its intelligent behavior. 

Here's a simplified breakdown:

Reason:  
You give the CLI a prompt, such as "Fix the bug in this file."

Act:   
The CLI sends your request to the Gemini model.  
The model reasons about the task and decides it needs to use a tool.   
For example, it may call the read_file tool to inspect the contents of the file.

Observe:   
The CLI executes the tool and receives the results (in this case, the file's content).   
It then sends this output back to the model.  

Repeat:   
The model reasons again, this time with the new information.  
It may decide to use another tool, like grep to search for a specific error message, or write_file to propose a fix.   
This loop continues until the task is complete.  


## **Model Context Protocol (MCP) Servers**

This is the key to extending the Gemini CLI's functionality with custom, project-specific tools.  

* There are a few `community-contributed MCP servers` for popular services like `GitHub`, `Firebase`, and various databases, but none are officially bundled with the CLI itself. 
* The architecture is designed for you to build and integrate your own MCP servers to:
    - **Connect to Internal APIs**: Create an MCP server to access your company's proprietary services or databases.  
    - **Integrate with Project Management Tools**: Develop a server that allows the agent to create Jira tickets or update tasks in a project tracker.  
    - **Automate Custom Workflows**: Build an MCP server that encapsulates a complex, multi-step process specific to your development team, and expose it as a single tool for the Gemini agent to use.  

You configure these external MCP servers by editing the `settings.json` file in your project's `.gemini` directory.  
This allows you to define custom tools and services that are only available to the agent when it's operating within that specific project.  





