# DB Session Helper Script

An interactive bash script that helps establish secure database connections through AWS Systems Manager (SSM) port forwarding with automated parameter selection through Parameter Store.

## Prerequisites

- AWS CLI installed and configured
- `fzy` ([fuzzy finder](https://github.com/jhawthorn/fzy)) for interactive selection
- `jq` for JSON processing
- Valid AWS credentials with appropriate permissions for:
  - EC2 (describe-instances)
  - SSM (start-session, get-parameter, describe-parameters)
- Target EC2 instance must have AWS SSM agent installed
- RDS endpoints stored in AWS Systems Manager Parameter Store

## Installation

1. Ensure the script is executable:

```bash
chmod +x db-session-helper
```

2. Install required dependencies:

```bash
# On macOS with Homebrew
brew install fzy jq

# On Ubuntu/Debian
sudo apt-get install fzy jq

# On Amazon Linux/RHEL/CentOS
sudo yum install fzy jq
```

## Usage

```bash
./db-session-helper [--debug]
```

### Options

| Option    | Description                    |
| --------- | ------------------------------ |
| `--debug` | Enable debug output (optional) |

## Interactive Flow

The script provides an interactive workflow using `fzy`:

1. **AWS Profile Selection**: Choose from available AWS profiles
2. **Region Selection**: Select AWS region (us-west-2, us-east-1, us-east-2, ap-southeast-1, eu-west-1)
3. **Target EC2 Instance**: Select from running EC2 instances (displays Instance ID, Name, and Private IP)
4. **RDS Endpoint Parameter**: Choose from Parameter Store parameters ending with `RDS_ENDPOINT`
5. **Database Port**: Select database port (3306 for MySQL, 5432 for PostgreSQL)
6. **Local Port**: Specify local port (defaults to same as remote port)

## Features

- Interactive selection using fuzzy finder (`fzy`)
- Automatic discovery of running EC2 instances
- Integration with AWS Systems Manager Parameter Store
- Support for multiple AWS profiles and regions
- Debug mode for troubleshooting
- Graceful error handling and validation
- Clear status reporting throughout the process

## Parameter Store Integration

The script expects RDS endpoints to be stored in AWS Systems Manager Parameter Store with names ending in `RDS_ENDPOINT`. For example:

- `/myapp/prod/RDS_ENDPOINT`
- `/database/staging/RDS_ENDPOINT`

## Example Session

```
Selected profile: prod-profile
Selected region: us-west-2
Fetching active EC2 instances...
Selected target: i-1234567890abcdef0
Fetching RDS endpoints from Parameter Store...
Selected RDS endpoint parameter: /myapp/prod/RDS_ENDPOINT
RDS Endpoint: mydb.123456789012.us-west-2.rds.amazonaws.com
RDS Port: 5432
Local port: 5432

Starting SSM session with port forwarding...
Target: i-1234567890abcdef0
Endpoint: mydb.123456789012.us-west-2.rds.amazonaws.com
Remote Port: 5432
Local Port: 5432
Region: us-west-2
Profile: prod-profile

Press Ctrl+C to terminate the session.
```
