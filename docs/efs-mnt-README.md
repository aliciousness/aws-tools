# EFS Mount Script

An interactive bash script for mounting AWS Elastic File System (EFS) filesystems using access points with a user-friendly selection interface.

## Prerequisites

- AWS CLI installed and configured
- `fzy` ([fuzzy finder](https://github.com/jhawthorn/fzy)) for interactive selection
- Root privileges (script must be run with `sudo`)
- AWS EFS utilities installed on the system
- A SSM session open on the EC2 (Bastion) host (Very important)

## Installation

1. Ensure the script is executable:

```bash
chmod +x efs-mnt
```

2. Install required dependencies (if not already installed on EC2 host):

```bash
# On macOS with Homebrew
brew install fzy

# On Ubuntu/Debian
sudo apt-get install fzy amazon-efs-utils

# On Amazon Linux
sudo yum install amazon-efs-utils
> Install fzy from the binaries and put it in your PATH manually
```

## Usage

```bash
sudo ./efs-mnt [options]
```

### Options

| Option       | Description                               |
| ------------ | ----------------------------------------- |
| `-y`         | Auto-run the mount command without prompt |
| `-h, --help` | Show help message and exit                |
| `help`       | Show help message and exit                |

### Examples

1. Interactive mode (default):

```bash
sudo ./efs-mnt
```

2. Auto-mount without confirmation:

```bash
sudo ./efs-mnt -y
```

3. Show help:

```bash
./efs-mnt --help
```

## Interactive Flow

The script provides an interactive workflow using `fzy`:

1. **Mount Point**: Specify local mount directory (default: `./efs`)
2. **AWS Region Selection**: Choose from all available AWS regions
3. **EFS Filesystem Selection**: Select from available EFS filesystems (shows ID and Name tag if available)
4. **Access Point Selection**: Choose from access points associated with the selected filesystem (shows ID and root directory path)
5. **Mount Confirmation**: Option to execute the mount command immediately or copy it for manual execution

## Features

- Interactive region and filesystem selection
- Automatic discovery of EFS filesystems and access points
- Support for EFS access points with custom root directories
- TLS encryption enabled by default
- Flexible mount point specification
- Optional auto-mount functionality
- Comprehensive error handling
- Clear display of mount commands before execution

## Mount Command Format

The script generates mount commands in the following format:

```bash
sudo mount -t efs -o tls,accesspoint=<access-point-id> <filesystem-id>:/ <mount-point>
```

Example:

```bash
sudo mount -t efs -o tls,accesspoint=fsap-12345678 fs-87654321:/ ./efs
```

## Example Session

```
Enter mount point [default: ./efs]: /mnt/myapp
Select AWS region > us-west-2
Select EFS filesystem > fs-12345678 (MyApp-EFS)
Select Access Point for fs-12345678 > fsap-87654321 → /app/data

🖇  Mount command:
   sudo mount -t efs -o tls,accesspoint=fsap-87654321 fs-12345678:/ /mnt/myapp

Do you want to run the mount command now? [y/N]: y
```

## Error Handling

The script includes error handling for:

- Missing AWS credentials
- Invalid region selection
- No EFS filesystems found
- No access points available for selected filesystem
- Mount command failures
- User cancellation at any step

## Troubleshooting

If you encounter mount issues:

1. Ensure EFS mount helper is installed (`amazon-efs-utils`)
2. Verify security group allows NFS traffic (port 2049)
3. Check that the EFS filesystem is in the same VPC/region
4. Confirm access point configuration allows your user/group
