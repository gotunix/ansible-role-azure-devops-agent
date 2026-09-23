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

# Dependencies to install (set to true to enable) and their versions
azure_devops_install_dotnet: false
azure_devops_dotnet_version: "8.0"

azure_devops_install_java: false
azure_devops_java_version: "17"

azure_devops_install_node: false
azure_devops_node_version: "20.x"

azure_devops_install_php: false
azure_devops_php_version: "8.2"
```

## Example Playbook

```yaml
- hosts: agents
  become: yes
  vars:
    azure_devops_server_url: "https://dev.azure.com/my-org"
    azure_devops_pat: "my-secret-pat"
    azure_devops_agent_pool: "MyCustomPool"
    
    # Enable Node and set a custom version
    azure_devops_install_node: true
    azure_devops_node_version: "18.x"
    
    # Enable PHP and set a custom version
    azure_devops_install_php: true
    azure_devops_php_version: "8.1"
  roles:
    - azure-devops-agent
```
