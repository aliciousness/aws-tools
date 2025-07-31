# DB Session Script

A Python utility script to establish secure database connections through AWS Systems Manager (SSM) port forwarding.

## Prerequisites

- Python 3.x
- AWS CLI installed and configured
- `boto3` Python package
- Valid AWS credentials with appropriate SSM permissions
- Target instance must have AWS SSM agent installed

## Installation

1. Ensure you have the required dependencies installed:

```bash
pip install boto3
```

2. Make the script executable:

```bash
chmod +x db-session
```

3. Place the script in your PATH or run it directly from its location.

## Usage

Basic usage:

```bash
db-session -e your-db-endpoint.region.rds.amazonaws.com -p 5432 -t i-123456789abcdefgh
```

### Arguments

| Argument         | Required | Default        | Description                  |
| ---------------- | -------- | -------------- | ---------------------------- |
| `-e, --endpoint` | Yes      | -              | RDS endpoint                 |
| `-p, --port`     | Yes      | -              | Database port number         |
| `-t, --target`   | Yes      | -              | SSM bastion host instance ID |
| `--local-port`   | No       | Same as --port | Local port for forwarding    |
| `-r, --region`   | No       | us-west-2      | AWS region                   |
| `-P, --profile`  | No       | default        | AWS profile name             |

### Examples

1. Connect to PostgreSQL using default ports:

```bash
db-session -e mydb.123456789012.us-west-2.rds.amazonaws.com -p 5432 -t i-123456789abcdefgh
```

2. Use a different local port:

```bash
db-session -e mydb.123456789012.us-west-2.rds.amazonaws.com -p 5432 --local-port 6543 -t i-123456789abcdefgh
```

3. Specify a different region and profile:

```bash
db-session -e mydb.123456789012.us-west-2.rds.amazonaws.com -p 5432 -r us-east-1 -P prod-profile -t i-123456789abcdefgh
```
