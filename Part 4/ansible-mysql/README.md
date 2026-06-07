# MySQL 8 Ansible Playbook

This Ansible playbook automates the installation and hardening of MySQL 8 on Rocky Linux/CentOS systems.

## Structure

```
ansible-mysql/
├── playbook.yml              # Main playbook entry point
├── vars/
│   └── main.yml             # Centralized variables for MySQL configuration
├── inventory                # Host inventory file
├── ansible.cfg              # Ansible configuration
├── roles/
│   ├── os-management/
│   │   └── tasks/
│   │       └── main.yml     # OS-level tasks (user limits, directories)
│   ├── mysql-install/
│   │   ├── tasks/
│   │   │   └── main.yml     # MySQL installation tasks
│   │   ├── handlers/
│   │   │   └── main.yml     # Service handlers
│   │   └── templates/
│   │       └── my.cnf.j2    # MySQL configuration template
│   └── mysql-hardening/
│       ├── tasks/
│       │   └── main.yml     # Security hardening tasks
│       └── handlers/
│           └── main.yml     # Hardening handlers
└── README.md                # This file
```

## Roles Description

### 1. os-management
Manages OS-level configurations:
- Creates MySQL user and group
- Creates data and log directories
- Sets resource limits for MySQL user (file descriptors, processes, memory)
- Updates system packages
- Disables SELinux (optional)

### 2. mysql-install
Handles MySQL 8 installation:
- Adds MySQL repository
- Installs MySQL Server package
- Creates required directories
- Deploys configuration from template (if not already exists)
- Sets correct ownership and permissions
- Starts and enables MySQL service

### 3. mysql-hardening
Implements security best practices:
- Removes test database
- Removes anonymous users
- Removes root remote access
- Disables event scheduler
- Enables secure transport
- Enables skip_name_resolve
- Sets root to socket-only authentication
- Generates hardening report

## Variables

Edit `vars/main.yml` to customize:

```yaml
# Paths
mysql_data_dir: /var/lib/mysql
mysql_log_dir: /var/log/mysql

# Resources
mysql_open_files_limit: 65536
mysql_max_connections: 200

# Configuration
mysql_port: 3306
mysql_bind_address: "127.0.0.1"
mysql_innodb_buffer_pool_size: "1G"
```

## Usage

### 1. Configure Inventory
Edit `inventory` and add your target hosts:

```ini
[mysql_servers]
localhost ansible_connection=local
```

### 2. Run Full Playbook
```bash
ansible-playbook playbook.yml
```

### 3. Run Specific Roles
```bash
# Only OS management
ansible-playbook playbook.yml --tags os

# Only MySQL installation
ansible-playbook playbook.yml --tags install

# Only hardening
ansible-playbook playbook.yml --tags hardening
```

### 4. Run on Vagrant VM
```bash
# From Vagrantfile directory
vagrant up
vagrant ssh

# Then run playbook from the VM
ansible-playbook /vagrant/ansible-mysql/playbook.yml -i /vagrant/ansible-mysql/inventory
```

## Requirements

- Ansible 2.9+
- Target: Rocky Linux 9, CentOS 8/7, or similar RHEL-based systems
- SSH access or local connection

## Install Required Ansible Collections

```bash
ansible-galaxy collection install community.mysql
```

## Security Considerations

✓ Removes test database and anonymous users
✓ Disables remote root access
✓ Enables secure transport
✓ Sets socket-only authentication for root
✓ Configures resource limits
✓ Enables slow query logging
✓ Sets up binary logging for replication

## Troubleshooting

### MySQL won't start
- Check: `systemctl status mysqld`
- Logs: `/var/log/mysql/error.log`
- Verify data directory permissions

### Cannot connect to MySQL
- Ensure socket exists: `ls -la /var/run/mysqld/mysqld.sock`
- Check bind address in `/etc/my.cnf`
- Verify SELinux is disabled or configured properly

### Hardening tasks fail
- Ensure MySQL is fully started before hardening
- Check: `mysql -u root -e 'SELECT 1'`
- Install: `pip install PyMySQL` on control machine

## Additional Notes

- Root password is randomly generated and stored in `/tmp/mysql_root_password`
- Hardening report is available at `/var/log/mysql/HARDENING_REPORT.txt`
- Always backup before running on production
- Test in non-production environment first

## References

- [MySQL 8.0 Documentation](https://dev.mysql.com/doc/)
- [MySQL Security Best Practices](https://dev.mysql.com/doc/mysql-security-excerpt/)
- [Ansible MySQL Collection](https://docs.ansible.com/ansible/latest/collections/community/mysql/)
