# Ansible Configuration Management

A comprehensive Ansible playbook project for automating the configuration and deployment of applications infrastructure.

## Project Overview

This project uses Ansible to manage the configuration, installation, and setup of servers in a distributed infrastructure. It automates the deployment of essential packages, Docker, and related containerization tools across multiple host groups.

## Project Structure

```
ansible_config_management/
├── ansible.cfg           # Ansible configuration file
├── hosts                 # Inventory file defining host groups
├── setup.yml             # Main playbook file
├── docker/               # Docker installation and setup role
│   ├── handlers/
│   │   └── main.yml     # Docker service restart handler
│   └── tasks/
│       └── main.yml     # Docker installation tasks
├── pkgs/                 # Essential packages installation role
│   └── tasks/
│       └── main.yml     # Package installation tasks
└── README.md             # This file
```

## Prerequisites

- Ansible 2.9 or higher installed on the control machine
- SSH access to target hosts with sudo privileges
- Python 3 installed on target hosts
- Ubuntu/Debian-based systems (tested with Ubuntu 22.04 LTS and later)

## Host Groups

The `hosts` inventory file defines two host groups:

### Group 1

First group of servers for configuration management. Currently includes:

- `your-server` - Server instance in first group

### Group 2

Second group of servers for configuration management. Currently includes:

- `your-server` - Server instance in second group

## Roles

### 1. pkgs Role

Installs and configures essential system packages:

- **nginx** - Web server for hosting applications
- **ca-certificates** - Certificate authority certificates for SSL/TLS
- **curl** - Command-line tool for transferring data with URLs
- **gnupg** - GNU Privacy Guard for cryptographic operations
- **htop** - Interactive process viewer for system monitoring

### 2. docker Role

Installs Docker and related containerization tools:

- **Docker CE (Community Edition)** - Container runtime
- **Docker CLI** - Command-line interface for Docker
- **containerd.io** - Container runtime
- **docker-buildx-plugin** - Extended build capabilities
- **docker-compose-plugin** - Multi-container Docker applications

Additional configurations:

- Adds Docker's official GPG key and APT repository
- Adds the `ubuntu` user to the docker group for non-root access
- Ensures Docker service is started and enabled at boot

## Getting Started

### 1. Configure Inventory

Edit the `hosts` file to add your server IP addresses or hostnames:

```ini
[group1]
your-server-1

[group2]
your-server-2
```

### 2. Configure Ansible Settings

Review `ansible.cfg` to adjust settings as needed. The default configuration points to the `hosts` inventory file.

### 3. Run the Playbook

Execute the main setup playbook to configure all servers:

```bash
ansible-playbook setup.yml
```

### 4. Run Playbook Against Specific Group

To run the playbook against only Group 1:

```bash
ansible-playbook setup.yml -i hosts -l group1
```

To run against only Group 2:

```bash
ansible-playbook setup.yml -i hosts -l group2
```

## Usage Examples

### Install packages on all servers

```bash
ansible-playbook setup.yml
```

### Install packages on Group 1 only

```bash
ansible-playbook setup.yml -l group1
```

### Verify connectivity to all hosts

```bash
ansible all -m ping
```

### Run a specific role

```bash
ansible-playbook setup.yml --tags docker
ansible-playbook setup.yml --tags pkgs
```

## SSH Configuration

Ensure you have SSH configured properly for your target hosts:

```bash
# Test SSH connectivity
ssh -i /path/to/key ubuntu@your-server-ip

# Add SSH key to agent
ssh-add /path/to/key
```

## Handlers

The project includes a Docker handler that:

- Restarts the Docker daemon when changes require it
- Ensures Docker service is properly restarted with proper privileges

## Common Issues

### Permission Denied

- Ensure the SSH user has sudo privileges
- Verify SSH key permissions (should be 600)

### Docker Group Changes Not Applied

- The playbook automatically resets the SSH connection to apply user group changes
- Log out and log back in if needed

### Repository or GPG Key Issues

- Verify internet connectivity on target hosts
- Check that Ubuntu version is supported (20.04 LTS and later recommended)

## Future Enhancements

- Add Kubernetes deployment capabilities
- Include monitoring and logging roles
- Add automated backup and disaster recovery
- Implement security hardening roles

## Contributing

When adding new roles:

1. Create a new directory under the project root
2. Add `tasks/main.yml` for task definitions
3. Add `handlers/main.yml` if service restarts are needed
4. Include the role in `setup.yml` under the appropriate host group
5. Update this README with role documentation

## Author

Solo Shun

## License

MIT License
