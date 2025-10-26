# Claude Code Installation and Configuration Tutorial

This tutorial will guide you through installing Claude Code CLI and setting up comprehensive configuration files for different development scenarios.

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Installation](#installation)
3. [Basic Configuration](#basic-configuration)
4. [Advanced Configuration](#advanced-configuration)
5. [MCP Server Setup](#mcp-server-setup)
6. [Specialized Settings](#specialized-settings)
7. [Usage Examples](#usage-examples)
8. [Troubleshooting](#troubleshooting)

## Prerequisites

- Node.js and npm installed
- Terminal/command line access
- Basic familiarity with JSON configuration files

## Installation

### Step 1: Install Claude Code Globally

```bash
# Install Claude Code globally using npm
npm install -g @anthropic-ai/claude-code
```

**Note:** If you encounter permission errors, you may need to configure npm to install global packages in your user directory:

```bash
# Create npm global directory
mkdir -p ~/.npm-global

# Configure npm to use user directory
npm config set prefix '~/.npm-global'

# Add to PATH (add this to your ~/.zshrc or ~/.bashrc)
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

### Step 2: Verify Installation

```bash
# Check if Claude Code is installed
claude --version

# View help information
claude --help
```

**Expected Output:**
```
2.0.27 (Claude Code)
```

## Basic Configuration

### Step 3: Create Global Configuration File

Create or edit the global configuration file at `~/.claude.json`:

```json
{
  "installMethod": "npm",
  "autoUpdates": true,
  "firstStartTime": "2025-10-26T02:34:28.583Z",
  "userID": "your-user-id-here",
  "sonnet45MigrationComplete": true,
  
  "verbose": false,
  "permissionMode": "default",
  "defaultModel": "claude-3-5-sonnet-20241022",
  "fallbackModel": "claude-3-5-haiku-20241022",
  "systemPrompt": "You are a helpful coding assistant with expertise in multiple programming languages and frameworks.",
  
  "allowedTools": [
    "Bash",
    "Edit", 
    "Read",
    "WebSearch",
    "FileSystem",
    "Git"
  ],
  "disallowedTools": [],
  
  "mcpConfig": [
    {
      "name": "filesystem",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/ubuntu"],
      "env": {}
    }
  ],
  
  "agents": {
    "security": {
      "description": "Security-focused code reviewer",
      "prompt": "Focus on security vulnerabilities, best practices, and potential security issues in code."
    },
    "performance": {
      "description": "Performance optimization expert",
      "prompt": "Analyze code for performance bottlenecks and suggest optimizations."
    },
    "documentation": {
      "description": "Documentation specialist",
      "prompt": "Help create clear, comprehensive documentation and code comments."
    }
  },
  
  "plugins": [],
  
  "debug": false,
  "includePartialMessages": false,
  "outputFormat": "text"
}
```

### Step 4: Create Project-Specific Configuration

Create a `.claude.json` file in your project directory:

```json
{
  "systemPrompt": "You are working on a development project. Focus on clean code, best practices, and maintainable solutions.",
  "allowedTools": [
    "Bash",
    "Edit", 
    "Read",
    "WebSearch",
    "FileSystem",
    "Git"
  ],
  "disallowedTools": [],
  
  "mcpConfig": [
    {
      "name": "project-filesystem",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/ubuntu"],
      "env": {}
    }
  ],
  
  "agents": {
    "code-reviewer": {
      "description": "Project-specific code reviewer",
      "prompt": "Review code for this project's specific requirements, coding standards, and architecture patterns."
    },
    "debugger": {
      "description": "Debugging specialist",
      "prompt": "Help identify and fix bugs, analyze error messages, and improve error handling."
    },
    "optimizer": {
      "description": "Code optimizer",
      "prompt": "Optimize code for performance, readability, and maintainability."
    }
  },
  
  "verbose": false,
  "permissionMode": "acceptEdits",
  "debug": false
}
```

## Advanced Configuration

### Step 5: Set Up MCP Servers

Create a `.mcp.json` file for MCP (Model Context Protocol) server configuration:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/ubuntu"],
      "env": {
        "NODE_ENV": "development"
      }
    },
    "git": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-git", "/home/ubuntu"],
      "env": {}
    },
    "brave-search": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "your-brave-api-key-here"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-postgres"],
      "env": {
        "POSTGRES_CONNECTION_STRING": "postgresql://username:password@localhost:5432/database"
      }
    },
    "sqlite": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-sqlite", "/home/ubuntu/databases"],
      "env": {}
    }
  }
}
```

### Step 6: Add MCP Servers via Command Line

```bash
# Add filesystem server
claude mcp add --transport stdio filesystem -- npx @modelcontextprotocol/server-filesystem /home/ubuntu

# Add git server
claude mcp add --transport stdio git -- npx @modelcontextprotocol/server-git /home/ubuntu

# List configured MCP servers
claude mcp list
```

## Specialized Settings

### Step 7: Create Web Development Settings

Create `claude-settings-web-dev.json`:

```json
{
  "model": "claude-3-5-sonnet-20241022",
  "fallbackModel": "claude-3-5-haiku-20241022",
  "systemPrompt": "You are a full-stack web developer with expertise in React, Node.js, TypeScript, and modern web technologies. Focus on clean code, responsive design, performance optimization, and security best practices.",
  "permissionMode": "acceptEdits",
  "verbose": true,
  "allowedTools": [
    "Bash",
    "Edit", 
    "Read",
    "WebSearch",
    "FileSystem",
    "Git"
  ],
  "disallowedTools": [],
  "mcpConfig": [
    {
      "name": "filesystem",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/ubuntu/projects"],
      "env": {}
    },
    {
      "name": "git",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-git", "/home/ubuntu/projects"],
      "env": {}
    }
  ],
  "agents": {
    "frontend": {
      "description": "Frontend development specialist",
      "prompt": "Focus on React, TypeScript, CSS, responsive design, and user experience best practices."
    },
    "backend": {
      "description": "Backend development specialist", 
      "prompt": "Focus on Node.js, APIs, databases, security, and server-side best practices."
    },
    "devops": {
      "description": "DevOps and deployment specialist",
      "prompt": "Focus on Docker, CI/CD, cloud deployment, and infrastructure best practices."
    }
  }
}
```

### Step 8: Create Data Science Settings

Create `claude-settings-data-science.json`:

```json
{
  "model": "claude-3-5-sonnet-20241022",
  "fallbackModel": "claude-3-5-haiku-20241022",
  "systemPrompt": "You are a data scientist with expertise in Python, pandas, numpy, scikit-learn, matplotlib, seaborn, and statistical analysis. Focus on data cleaning, analysis, visualization, and machine learning best practices.",
  "permissionMode": "acceptEdits",
  "verbose": true,
  "allowedTools": [
    "Bash",
    "Edit", 
    "Read",
    "WebSearch",
    "FileSystem",
    "Git"
  ],
  "disallowedTools": [],
  "mcpConfig": [
    {
      "name": "filesystem",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/ubuntu/data-projects"],
      "env": {}
    },
    {
      "name": "sqlite",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-sqlite", "/home/ubuntu/data-projects/databases"],
      "env": {}
    }
  ],
  "agents": {
    "data-analyst": {
      "description": "Data analysis specialist",
      "prompt": "Focus on exploratory data analysis, statistical analysis, and data visualization using pandas, numpy, and matplotlib."
    },
    "ml-engineer": {
      "description": "Machine learning specialist",
      "prompt": "Focus on machine learning models, feature engineering, model evaluation, and scikit-learn best practices."
    },
    "data-visualizer": {
      "description": "Data visualization specialist",
      "prompt": "Focus on creating clear, informative visualizations using matplotlib, seaborn, and plotly."
    }
  }
}
```

### Step 9: Create DevOps Settings

Create `claude-settings-devops.json`:

```json
{
  "model": "claude-3-5-sonnet-20241022",
  "fallbackModel": "claude-3-5-haiku-20241022",
  "systemPrompt": "You are a DevOps engineer with expertise in Docker, Kubernetes, CI/CD, cloud platforms (AWS, Azure, GCP), infrastructure as code, and automation. Focus on reliability, scalability, and security.",
  "permissionMode": "acceptEdits",
  "verbose": true,
  "allowedTools": [
    "Bash",
    "Edit", 
    "Read",
    "WebSearch",
    "FileSystem",
    "Git"
  ],
  "disallowedTools": [],
  "mcpConfig": [
    {
      "name": "filesystem",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/ubuntu/infrastructure"],
      "env": {}
    },
    {
      "name": "git",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-git", "/home/ubuntu/infrastructure"],
      "env": {}
    }
  ],
  "agents": {
    "containerization": {
      "description": "Container and orchestration specialist",
      "prompt": "Focus on Docker, Kubernetes, containerization best practices, and orchestration strategies."
    },
    "automation": {
      "description": "CI/CD and automation specialist",
      "prompt": "Focus on CI/CD pipelines, automation scripts, and deployment strategies."
    },
    "infrastructure": {
      "description": "Infrastructure as code specialist",
      "prompt": "Focus on Terraform, CloudFormation, infrastructure automation, and cloud best practices."
    }
  }
}
```

### Step 10: Create Security Settings

Create `claude-settings-security.json`:

```json
{
  "model": "claude-3-5-sonnet-20241022",
  "fallbackModel": "claude-3-5-haiku-20241022",
  "systemPrompt": "You are a cybersecurity expert with deep knowledge of security vulnerabilities, penetration testing, secure coding practices, and compliance standards. Always prioritize security and privacy.",
  "permissionMode": "default",
  "verbose": true,
  "allowedTools": [
    "Bash",
    "Edit", 
    "Read",
    "WebSearch",
    "FileSystem",
    "Git"
  ],
  "disallowedTools": [],
  "mcpConfig": [
    {
      "name": "filesystem",
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/ubuntu/security-projects"],
      "env": {}
    }
  ],
  "agents": {
    "vulnerability-scanner": {
      "description": "Security vulnerability specialist",
      "prompt": "Focus on identifying security vulnerabilities, OWASP Top 10, and security best practices in code."
    },
    "compliance": {
      "description": "Compliance and audit specialist",
      "prompt": "Focus on security compliance standards, audit requirements, and regulatory frameworks."
    },
    "secure-coding": {
      "description": "Secure coding practices specialist",
      "prompt": "Focus on secure coding practices, input validation, authentication, and authorization."
    }
  }
}
```

## Usage Examples

### Step 11: Basic Usage

```bash
# Start an interactive session
claude

# Use with a specific prompt
claude "help me write a Python function to sort a list"

# Use with print mode (non-interactive)
claude --print "explain this code" < myfile.py
```

### Step 12: Using Specialized Settings

```bash
# Use web development settings
claude --settings claude-settings-web-dev.json "help me build a React component"

# Use data science settings
claude --settings claude-settings-data-science.json "analyze this dataset"

# Use DevOps settings
claude --settings claude-settings-devops.json "help me create a Dockerfile"

# Use security settings
claude --settings claude-settings-security.json "review this code for vulnerabilities"
```

### Step 13: Using Custom Agents

```bash
# Use a specific agent
claude --agents '{"reviewer": {"description": "Code reviewer", "prompt": "Review for bugs and best practices"}}' "review this code"

# Use multiple agents
claude --agents '{"security": {"description": "Security expert", "prompt": "Focus on security"}, "performance": {"description": "Performance expert", "prompt": "Focus on performance"}}' "analyze this code"
```

### Step 14: Advanced Command Line Options

```bash
# Use specific model
claude --model claude-3-5-sonnet-20241022 "your prompt"

# Set system prompt
claude --system-prompt "You are a Python expert" "help with my code"

# Allow specific tools only
claude --allowed-tools "Bash Edit" "refactor this code"

# Disable specific tools
claude --disallowed-tools "WebSearch" "help with local code"

# Enable debug mode
claude --debug "help me debug this issue"

# Use verbose output
claude --verbose "explain this concept"
```

### Step 15: MCP Server Management

```bash
# List MCP servers
claude mcp list

# Add MCP server
claude mcp add --transport stdio my-server -- npx my-mcp-server

# Remove MCP server
claude mcp remove my-server

# Get MCP server details
claude mcp get my-server
```

### Step 16: Plugin Management

```bash
# List available plugins
claude plugin marketplace

# Install a plugin
claude plugin install my-plugin

# List installed plugins
claude plugin list

# Enable/disable plugins
claude plugin enable my-plugin
claude plugin disable my-plugin

# Uninstall plugin
claude plugin uninstall my-plugin
```

## Configuration Hierarchy

Claude Code loads configuration in the following order (later settings override earlier ones):

1. **Default built-in settings**
2. **Global configuration** (`~/.claude.json`)
3. **Project configuration** (`.claude.json` in project directory)
4. **Command-line arguments**
5. **Environment variables**

## Environment Variables

You can also set these environment variables for global configuration:

```bash
export CLAUDE_API_KEY="your-api-key"
export CLAUDE_DEFAULT_MODEL="claude-3-5-sonnet-20241022"
export CLAUDE_VERBOSE="true"
export CLAUDE_PERMISSION_MODE="acceptEdits"
```

## Troubleshooting

### Common Issues and Solutions

#### Issue: Permission denied during installation
**Solution:**
```bash
# Configure npm to use user directory
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

#### Issue: Command not found after installation
**Solution:**
```bash
# Check if PATH is set correctly
echo $PATH

# Manually add to PATH
export PATH=~/.npm-global/bin:$PATH

# Verify installation
which claude
```

#### Issue: MCP server not working
**Solution:**
```bash
# Check MCP server status
claude mcp list

# Reinstall MCP server
claude mcp remove server-name
claude mcp add --transport stdio server-name -- npx server-package
```

#### Issue: Configuration not loading
**Solution:**
```bash
# Check configuration file syntax
cat ~/.claude.json | jq .

# Validate JSON syntax
python -m json.tool ~/.claude.json
```

### Getting Help

```bash
# Get help for any command
claude --help
claude mcp --help
claude plugin --help

# Check version and installation
claude --version

# Run health check
claude doctor
```

## Best Practices

1. **Use project-specific configurations** for different types of projects
2. **Create specialized settings files** for different development scenarios
3. **Set up MCP servers** to extend Claude's capabilities
4. **Use custom agents** for specific tasks and expertise areas
5. **Keep configurations organized** with clear naming conventions
6. **Test configurations** before using in production environments
7. **Backup configuration files** before making major changes

## Conclusion

You now have a fully configured Claude Code setup with:
- ✅ Global configuration with custom agents and MCP servers
- ✅ Project-specific configuration capabilities
- ✅ Specialized settings for different development scenarios
- ✅ MCP server integration for extended functionality
- ✅ Custom agents for specialized tasks
- ✅ Comprehensive command-line options

This setup provides a powerful, customizable development assistant that can adapt to your specific workflow and project requirements.

## Next Steps

1. **Customize the configurations** to match your specific needs
2. **Add more MCP servers** as needed for your projects
3. **Create additional specialized settings** for other development areas
4. **Experiment with different agents** and prompts
5. **Integrate with your existing development workflow**

Happy coding with Claude Code! 🚀
