# ECS Connect Script

A Python utility script for connecting to AWS ECS containers using ECS Exec functionality.

## Prerequisites

- Python 3.x
- AWS CLI v2
- Configured AWS credentials
- `boto3` Python package

## Installation

1. Ensure you have the required dependencies installed:

```bash
pip install boto3
```

2. Make the script executable:

```bash
chmod +x ecs-connect
```

## Usage

```bash
./ecs-connect -c CLUSTER_NAME -s SERVICE_NAME [-n CONTAINER_NAME] [-cmd COMMAND] [-p AWS_PROFILE] [-r AWS_REGION]
```

### Arguments

| Argument               | Description             | Required | Default     |
| ---------------------- | ----------------------- | -------- | ----------- |
| `-c, --cluster_name`   | Name of the ECS cluster | Yes      | -           |
| `-s, --service_name`   | Name of the ECS service | Yes      | -           |
| `-n, --container_name` | Name of the container   | No       | 'app'       |
| `-cmd, --command`      | Command to execute      | No       | '/bin/bash' |
| `-p, --profile`        | AWS profile to use      | No       | -           |
| `-r, --region`         | AWS region              | No       | 'us-west-2' |

### Example

```bash
./ecs-connect -c my-cluster -s my-service -n web -cmd "/bin/sh"
```

## Features

- Automatic task discovery for specified service
- AWS credentials verification
- Region selection support
- Custom command execution
- Multiple AWS profile support
- Graceful SIGINT handling

## Error Handling

The script includes comprehensive error handling for:

- Missing AWS credentials
- Invalid cluster/service names
- Connection failures
- Command execution errors
