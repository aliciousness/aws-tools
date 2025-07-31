# AWS Tools Collection

A collection of utility scripts for working with AWS services, designed to simplify common AWS operations through interactive command-line interfaces and direct scripting.

## 🚀 Quick Start

1. **Clone the repository:**

   ```bash
   git clone https://github.com/aliciousness/aws-tools.git
   cd aws-tools
   ```

2. **Set up Python environment:**

   ```bash
   ./pip-env
   ```

3. **Make scripts executable:**

   ```bash
   chmod +x bin/*
   ```

4. **Add to your PATH (optional):**
   ```bash
   echo 'export PATH=$PATH:'$(pwd)'/bin' >> ~/.bashrc
   source ~/.bashrc
   ```

## 📦 Available Tools

### Database Connection Tools

- **[db-session](docs/db-session-README.md)** - Direct database connection via SSM port forwarding
- **[db-session-helper](docs/db-session-helper-README.md)** - Interactive database connection with Parameter Store integration

### ECS Container Tools

- **[ecs-connect](docs/ecs-connect-README.md)** - Direct ECS container connection with specific parameters
- **[ecs-shell-helper](docs/ecs-shell-helper-README.md)** - Interactive ECS container shell access

### File System Tools

- **[efs-mnt](docs/efs-mnt-README.md)** - Interactive EFS filesystem mounting with access points

## 🛠️ Installation & Setup

### Prerequisites

- **Python 3.x** - Required for Python-based scripts
- **AWS CLI v2** - Required for all AWS operations
- **Configured AWS credentials** - Via `aws configure` or environment variables
- **Interactive tools** (for helper scripts):
  - `fzf` or `fzy` - Fuzzy finder for selections
  - `jq` - JSON processor for AWS CLI output

### Python Environment Setup

The repository includes a `pip-env` script that automatically sets up a Python virtual environment with all required dependencies:

```bash
./pip-env
```

**What `pip-env` does:**

1. Removes any existing `venv/` directory
2. Creates a new Python 3 virtual environment
3. Activates the virtual environment
4. Installs packages from `requirements.txt`
5. Updates `requirements.txt` with current package versions

**Manual activation** (if needed later):

```bash
source venv/bin/activate
```

### System Dependencies

Install fuzzy finders and JSON processor:

```bash
# macOS (Homebrew)
brew install fzf fzy jq

# Ubuntu/Debian
sudo apt-get install fzf jq
# Note: fzy may need to be installed from source on some distributions

# Amazon Linux/RHEL/CentOS
sudo yum install fzf jq
```

## 🔧 Usage Patterns

### Direct Usage Scripts

These scripts accept command-line arguments for direct operation:

- `db-session` - Specify endpoint, port, target instance
- `ecs-connect` - Specify cluster, service, container details

### Interactive Helper Scripts

These scripts provide guided workflows with resource discovery:

- `db-session-helper` - Interactive database connection setup
- `ecs-shell-helper` - Interactive ECS resource selection
- `efs-mnt` - Interactive EFS mounting

### Example Workflows

**Quick database connection:**

```bash
# Direct approach
./bin/db-session -e mydb.region.rds.amazonaws.com -p 5432 -t i-1234567890abcdef0

# Interactive approach
./bin/db-session-helper
```

**ECS container access:**

```bash
# Direct approach
./bin/ecs-connect -c my-cluster -s my-service -n web

# Interactive approach
./bin/ecs-shell-helper
```

## 🏗️ Architecture

```
aws-tools/
├── bin/                    # Executable scripts
│   ├── db-session         # Python script for SSM port forwarding
│   ├── db-session-helper  # Interactive bash script with Parameter Store
│   ├── ecs-connect        # Python script for ECS exec
│   ├── ecs-shell-helper   # Interactive bash script for ECS
│   └── efs-mnt           # Interactive bash script for EFS mounting
├── docs/                  # Individual script documentation
├── venv/                  # Python virtual environment (created by pip-env)
├── requirements.txt       # Python dependencies
├── pip-env               # Environment setup script
└── README.md             # This file
```

## 🔐 AWS Permissions

Each tool requires specific AWS permissions. Refer to individual documentation for detailed permission requirements:

**Common permissions needed:**

- `ec2:DescribeInstances` - For EC2 instance discovery
- `ssm:StartSession` - For SSM-based connections
- `ecs:*` - For ECS operations (varies by tool)
- `ssm:GetParameter` - For Parameter Store integration
- `elasticfilesystem:*` - For EFS operations

## 🐛 Troubleshooting

### Common Issues

1. **"Command not found" errors:**

   - Ensure scripts are executable: `chmod +x bin/*`
   - Check if tools are in your PATH

2. **AWS credential errors:**

   - Verify: `aws sts get-caller-identity`
   - Configure: `aws configure`

3. **Interactive selection not working:**

   - Install required tools: `fzf`, `fzy`, `jq`
   - Check tool availability: `which fzf fzy jq`

4. **Python import errors:**
   - Run the environment setup: `./pip-env`
   - Manually activate venv: `source venv/bin/activate`

### Debug Mode

Several scripts support debug flags for troubleshooting:

```bash
./bin/db-session-helper --debug
```

## 📝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make changes and test thoroughly
4. Update relevant documentation in `docs/`
5. Submit a pull request

### Adding New Tools

1. Create the script in `bin/`
2. Make it executable: `chmod +x bin/script-name`
3. Add documentation: `docs/script-name-README.md`
4. Update this main README with tool description
5. Test the tool thoroughly

## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## 🤝 Acknowledgments

- Built for AWS cloud operations and administration
- Designed with interactive workflows for ease of use
- Focuses on secure connection patterns and best practices
