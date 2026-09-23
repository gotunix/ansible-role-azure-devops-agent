# Ansible Role: Azure DevOps Agent

This role installs and configures the Azure DevOps agent on Linux. It can also conditionally install common build dependencies like .NET Core SDK, Java, Node.js, and PHP, allowing you to dynamically specify the versions.

## Role Variables

The following variables are available in `defaults/main.yml`:

```yaml
# Agent Configuration
azure_devops_agent_url: "https://vstsagentpackage.azureedge.net/agent/3.243.1/vsts-agent-linux-x64-3.243.1.tar.gz"
azure_devops_server_url: "https://dev.azure.com/your-organization"
azure_devops_pat: "" # Provide your Personal Access Token
azure_devops_agent_pool: "Default"
azure_devops_agent_name: "{{ ansible_hostname }}"
azure_devops_agent_dir: "/opt/azagent"
azure_devops_agent_user: "azagent"

# Toolset dependencies (e.g. dotnet, java, node, php)
# To add new toolsets later, just add a corresponding .yml file in the tasks/ directory
azure_devops_dependencies: []
```

## Example Playbook

```yaml
- hosts: agents
  become: yes
  vars:
    azure_devops_server_url: "https://dev.azure.com/my-org"
    azure_devops_pat: "my-secret-pat"
    azure_devops_agent_pool: "MyCustomPool"
    
    # Dynamically provide any dependencies to install
    azure_devops_dependencies:
      - name: node
        versions:
          - "18.x"
          - "20.x"
      - name: php
        versions:
          - "8.1"
          - "8.2"
      - name: dotnet
        versions:
          - "8.0"
  roles:
    - azure-devops-agent
```
