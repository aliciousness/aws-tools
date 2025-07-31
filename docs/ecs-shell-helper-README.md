# ECS Shell Helper Script

An interactive bash script that provides a guided interface for connecting to AWS ECS containers using ECS Exec functionality with automated resource discovery.

## Prerequisites

- AWS CLI v2 installed and configured
- `fzy` ([fuzzy finder](https://github.com/jhawthorn/fzy)) for interactive selection
- `jq` for JSON processing
- Valid AWS credentials with appropriate ECS permissions
- ECS tasks must have ECS Exec enabled
- Tasks must be running on EC2 instances or Fargate with the required IAM roles

## Installation

1. Ensure the script is executable:

```bash
chmod +x ecs-shell-helper
```

2. Install required dependencies:

```bash
# On macOS with Homebrew
brew install fzy jq

# On Ubuntu/Debian
sudo apt-get install fzy jq

# On Amazon Linux/RHEL/CentOS
Install fzy from the binaries and put it in your PATH manually
```

## Usage

```bash
./ecs-shell-helper
```

The script runs in interactive mode only - no command line arguments are required.

## Interactive Flow

The script provides a step-by-step interactive workflow using `fzy`:

1. **AWS Profile Selection**: Choose from available AWS profiles
2. **ECS Cluster Selection**: Select from available ECS clusters
3. **ECS Service Selection**: Choose from services in the selected cluster
4. **Task Selection**: Pick from running tasks in the selected service
5. **Container Selection**: Choose from containers in the selected task
6. **Shell Selection**: Select shell type (sh, bash, zsh)

## Features

- Fully interactive workflow with fuzzy finder
- Automatic discovery of ECS resources
- Support for multiple AWS profiles
- Multiple shell options (sh, bash, zsh)
- Clean error handling with graceful exits
- No complex command-line arguments needed

## Example Session

```
Selected profile: dev-profile
Selected cluster: arn:aws:ecs:us-west-2:123456789012:cluster/my-cluster
Selected service: arn:aws:ecs:us-west-2:123456789012:service/my-cluster/my-service
Selected task: arn:aws:ecs:us-west-2:123456789012:task/my-cluster/1234567890abcdef0
Selected container: web
Selected shell: bash

Starting session with ECS Exec...
```

## Requirements for ECS Exec

For the script to work properly, ensure:

1. **ECS Task Definition**: Must include `"enableExecuteCommand": true`
2. **ECS Service**: Must be created or updated with execute command capability enabled
3. **IAM Permissions**: The user/role must have permissions for:
   - `ecs:ExecuteCommand`
   - `ecs:DescribeTaskDefinition`
   - `ecs:DescribeTasks`
   - `ecs:ListTasks`
   - `ecs:ListServices`
   - `ecs:ListClusters`

## Error Handling

The script handles various error conditions:

- No AWS profiles available
- Empty cluster, service, or task lists
- User cancellation at any selection step
- AWS CLI command failures
- Missing ECS Exec capabilities

## Comparison with ecs-connect

While `ecs-connect` requires specific command-line arguments, `ecs-shell-helper` provides a fully guided interactive experience:

- **ecs-connect**: Direct command with specific parameters
- **ecs-shell-helper**: Interactive discovery and selection of all resources
