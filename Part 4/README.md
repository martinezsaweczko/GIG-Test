
# Database Architect Hands-On Challenge

## Task A:


1. Startup Vagrant VM
```bash
cd /home/david/Externo/GIG-Test/Part 4
make setup
```

Output should be similar to:

```
david@HardKernel:~/Externo/GIG-Test/Part 4$ make setup
Setting up environment for GIG Test - Part 4: AlmaLinux 9
vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'almalinux/9'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'almalinux/9' version '9.7.20260518' is up to date...
==> default: Setting the name of the VM: Rocky-Linux-10
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default:
    default: Guest Additions Version: 7.1.18
    default: VirtualBox Version: 7.0
==> default: Setting hostname...
==> default: Configuring and enabling network interfaces...
==> default: Mounting shared folders...
    default: /vagrant => /home/david/Externo/GIG-Test/Part 4
```




2. Install MySQL Playbook
```bash
cd /home/david/Externo/GIG-Test/Part 4
make install_mysql
```

Output should be similar to:

```
david@HardKernel:~/Externo/GIG-Test/Part 4$ make install_mysql
Installing MySQL on AlmaLinux 9 VM
cd ansible-mysql && ansible-playbook -i inventory playbook.yml
[WARNING]: Collection community.general does not support Ansible version 2.14.18

PLAY [MySQL 8 Installation and Configuration] **********************************************************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************************************************************************
ok: [default]

TASK [os-management : Install Python and PyMySQL for Ansible MySQL modules] ****************************************************************************************************************************************************
changed: [default]

TASK [os-management : Install PyMySQL via pip] *********************************************************************************************************************************************************************************
changed: [default]

TASK [os-management : Install required packages] *******************************************************************************************************************************************************************************
changed: [default]

TASK [os-management : Create MySQL user] ***************************************************************************************************************************************************************************************
changed: [default]

TASK [os-management : Create MySQL group] **************************************************************************************************************************************************************************************
ok: [default]

TASK [os-management : Create data directory] ***********************************************************************************************************************************************************************************
changed: [default]

TASK [os-management : Create log directory] ************************************************************************************************************************************************************************************
changed: [default]

TASK [os-management : Configure MySQL user limits - soft] **********************************************************************************************************************************************************************
changed: [default]

TASK [os-management : Configure MySQL user limits - hard] **********************************************************************************************************************************************************************
changed: [default]

TASK [os-management : Configure MySQL user limits - soft memlock] **************************************************************************************************************************************************************
changed: [default]

TASK [os-management : Configure MySQL user limits - hard memlock] **************************************************************************************************************************************************************
changed: [default]

TASK [os-management : Configure MySQL user limits - soft nproc] ****************************************************************************************************************************************************************
changed: [default]

TASK [os-management : Configure MySQL user limits - hard nproc] ****************************************************************************************************************************************************************
changed: [default]

TASK [mysql-install : Add MySQL repository (Rocky Linux/CentOS)] ***************************************************************************************************************************************************************
changed: [default]

TASK [mysql-install : Install MySQL Server] ************************************************************************************************************************************************************************************
changed: [default]

TASK [mysql-install : Create MySQL socket directory] ***************************************************************************************************************************************************************************
ok: [default]

TASK [mysql-install : Check if MySQL config exists] ****************************************************************************************************************************************************************************
ok: [default]

TASK [mysql-install : Deploy MySQL configuration file] *************************************************************************************************************************************************************************
changed: [default]

TASK [mysql-install : Ensure MySQL log directory exists with correct permissions] **********************************************************************************************************************************************
changed: [default]

TASK [mysql-install : Pre-create error.log file with correct permissions] ******************************************************************************************************************************************************
changed: [default]

TASK [mysql-install : Set SELinux context for MySQL log directory (if needed)] *************************************************************************************************************************************************
ok: [default]

TASK [mysql-install : Start MySQL service] *************************************************************************************************************************************************************************************
changed: [default]

TASK [mysql-hardening : Check if MySQL hardening has already been completed] ***************************************************************************************************************************************************
ok: [default]

TASK [mysql-hardening : Set hardening_needed flag] *****************************************************************************************************************************************************************************
ok: [default]

TASK [mysql-hardening : Display hardening status] ******************************************************************************************************************************************************************************
ok: [default] => {
    "msg": "MySQL hardening needed: True"
}

TASK [mysql-hardening : Wait for MySQL to be ready] ****************************************************************************************************************************************************************************
Pausing for 5 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
ok: [default]

TASK [mysql-hardening : Remove old root password file] *************************************************************************************************************************************************************************
ok: [default]

TASK [mysql-hardening : Check if MySQL is running] *****************************************************************************************************************************************************************************
ok: [default]

TASK [mysql-hardening : Get MySQL root temporary password] *********************************************************************************************************************************************************************
ok: [default]

TASK [mysql-hardening : Generate random root password] *************************************************************************************************************************************************************************
ok: [default]

TASK [mysql-hardening : Display random root password generation confirmation] **************************************************************************************************************************************************
ok: [default] => {
    "msg": "Random root password: XAbD-2ipyKYr75gz"
}

TASK [mysql-hardening : Reset root password using temporary password via shell] ************************************************************************************************************************************************
ok: [default]

TASK [mysql-hardening : Create default application user] ***********************************************************************************************************************************************************************
changed: [default] => (item=localhost)
changed: [default] => (item=%)

TASK [mysql-hardening : Remove test database] **********************************************************************************************************************************************************************************
ok: [default]

TASK [mysql-hardening : Remove anonymous MySQL users] **************************************************************************************************************************************************************************
ok: [default] => (item=localhost)
ok: [default] => (item=%)

TASK [mysql-hardening : Remove root user remote access] ************************************************************************************************************************************************************************
ok: [default] => (item=::1)
ok: [default] => (item=%)

TASK [mysql-hardening : Remove event scheduler (unless needed)] ****************************************************************************************************************************************************************
changed: [default]

TASK [mysql-hardening : Set require_secure_transport to ON] ********************************************************************************************************************************************************************
changed: [default]

TASK [mysql-hardening : Flush MySQL privileges] ********************************************************************************************************************************************************************************
ok: [default]

TASK [mysql-hardening : Create hardening marker file] **************************************************************************************************************************************************************************
changed: [default]

TASK [mysql-hardening : Display hardening completion message] ******************************************************************************************************************************************************************
ok: [default] => {
    "msg": "MySQL hardening completed! Marker file created at /var/lib/mysql/.hardened"
}

TASK [mysql-hardening : Display already hardened message] **********************************************************************************************************************************************************************
skipping: [default]

TASK [mysql-db-creation : Create MySQL database] *******************************************************************************************************************************************************************************
changed: [default]

TASK [mysql-db-creation : Display database creation status] ********************************************************************************************************************************************************************
ok: [default] => {
    "msg": "Database 'GIG_REFACTOR_LAB' created successfully"
}

TASK [mysql-db-creation : Display database already exists] *********************************************************************************************************************************************************************
skipping: [default]

TASK [mysql-db-creation : Grant app user privileges on database] ***************************************************************************************************************************************************************
changed: [default] => (item=localhost)
changed: [default] => (item=%)

RUNNING HANDLER [mysql-install : start-mysql] **********************************************************************************************************************************************************************************
ok: [default]

RUNNING HANDLER [mysql-install : restart-mysql] ********************************************************************************************************************************************************************************
changed: [default]

TASK [Verify MySQL is running] *************************************************************************************************************************************************************************************************
ok: [default]

TASK [Display MySQL version] ***************************************************************************************************************************************************************************************************
ok: [default]

PLAY RECAP *********************************************************************************************************************************************************************************************************************
default                    : ok=49   changed=25   unreachable=0    failed=0    skipped=2    rescued=0    ignored=0

```


3. Remove Vagrant VM
```
make clean
``

Output should be similar to:

```
david@HardKernel:~/Externo/GIG-Test/Part 4$ make clean
Deleting SSH key for MySQL VM
ssh-keygen -f "/home/david/.ssh/known_hosts" -R "[127.0.0.1]:2222"
Host [127.0.0.1]:2222 not found in /home/david/.ssh/known_hosts
Cleaning up environment for GIG Test - Part 4: AlmaLinux 9
vagrant destroy -f
==> default: Forcing shutdown of VM...
==> default: Destroying VM and associated drives...
``

Requisites

* Virtualbox (as hypervisor provider)
* Vagrant (for VM management)


# Task B

How to create and insert data

```bash
make populate_legacy_table
```

output should be similar to:

```
david@HardKernel:~/Externo/GIG-Test/Part 4$ make populate_legacy_table
Populating legacy transaction_logs_flat table (1 million rows)
cd ansible-mysql && ansible-playbook -i inventory populate-legacy-table.yml

PLAY [Populate Legacy Transaction Logs Table] **********************************************************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************************************************************************
ok: [default]

TASK [legacy-table-creation : Create transaction_logs_flat table] **************************************************************************************************************************************************************
changed: [default]

TASK [legacy-table-creation : Check row count in transaction_logs_flat] ********************************************************************************************************************************************************
ok: [default]

TASK [legacy-table-creation : Set row count variable with proper type conversion] **********************************************************************************************************************************************
ok: [default]

TASK [legacy-table-creation : Set row count to 0 if table doesn't exist] *******************************************************************************************************************************************************
skipping: [default]

TASK [legacy-table-creation : Display current row count] ***********************************************************************************************************************************************************************
ok: [default] => {
    "msg": "Current row count in transaction_logs_flat: 0"
}

TASK [legacy-table-creation : Calculate rows to insert] ************************************************************************************************************************************************************************
ok: [default]

TASK [legacy-table-creation : Display rows to insert] **************************************************************************************************************************************************************************
ok: [default] => {
    "msg": "Rows to insert: 1000000"
}

TASK [legacy-table-creation : Populate transaction_logs_flat table with randomized data ( using cross join))] ******************************************************************************************************************
ASYNC POLL on default: jid=j523623418616.14232 started=1 finished=0
ASYNC POLL on default: jid=j523623418616.14232 started=1 finished=0
ASYNC POLL on default: jid=j523623418616.14232 started=1 finished=0
ASYNC POLL on default: jid=j523623418616.14232 started=1 finished=0
ASYNC OK on default: jid=j523623418616.14232
changed: [default]

TASK [legacy-table-creation : Verify final row count] **************************************************************************************************************************************************************************
ok: [default]

TASK [legacy-table-creation : Display final row count] *************************************************************************************************************************************************************************
ok: [default] => {
    "msg": "Final row count in transaction_logs_flat: 1000000"
}

TASK [legacy-table-creation : Display skip message if table already populated] *************************************************************************************************************************************************
skipping: [default]

PLAY RECAP *********************************************************************************************************************************************************************************************************************
default                    : ok=10   changed=2    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
```

How to test the data is there:

```
david@HardKernel:~/Externo/GIG-Test/Part 4$ vagrant ssh -c "mysql -h localhost -uapp_user --p*****"
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 43
Server version: 8.0.46 MySQL Community Server - GPL

Copyright (c) 2000, 2026, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> select count(*) from GIG_REFACTOR_LAB.transaction_logs_flat;
+----------+
| count(*) |
+----------+
|  1000000 |
+----------+
1 row in set (0.24 sec)

mysql>
```


# Task C

How to migrate data from legacy table to new partitioned table:

```bash
make migrate_to_partitioned
```

The strategy used for migrating data is just divide the number of records I have to migrate by the batch size and then execute the migration in batches with a delay between each batch. Also we have a small delay between batches execution.

If we want to reduce the number of IOPS we can relax the ACID setting of the server.

*IMPROVEMENT POINT: With the first option and the OFFSET, RDBMS have to do the same query again and again and each time it has to skip the number of records defined by the OFFSET, so as the number of batches increases. With this new version, we keep track of the last ID migraed and we use it on the next iteration to migrate the next batch, so we avoid the performance degradation caused by the OFFSET.*


output should be similar to:

```
david@HardKernel:~/Externo/GIG-Test/Part 4$ make migrate_to_partitioned
Migrating data from flat to partitioned architecture
cd ansible-mysql && ansible-playbook -i inventory migration-and-archiving.yml

PLAY [Migration and Archiving Simulation] **************************************************************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************************************************************************************
ok: [default]

TASK [migration-and-archiving : Check if source table exists] ******************************************************************************************************************************************************************
ok: [default]

TASK [migration-and-archiving : Exit if source table does not exist] ***********************************************************************************************************************************************************
skipping: [default]

TASK [migration-and-archiving : Display source table found] ********************************************************************************************************************************************************************
ok: [default] => {
    "msg": "✓ Source table transaction_logs_flat found. Proceeding with migration."
}

TASK [migration-and-archiving : Display migration parameters] ******************************************************************************************************************************************************************
ok: [default] => {
    "msg": "Migration Configuration:\n- Source: transaction_logs_flat\n- Target: transaction_logs_modern\n- Database: GIG_REFACTOR_LAB\n- Batch Size: 25000 rows\n- Delay Between Batches: 5 seconds\n- Migration Window: Last 3 months\n"
}

TASK [migration-and-archiving : Create partitioned transaction_logs_modern table] **********************************************************************************************************************************************
changed: [default]

TASK [migration-and-archiving : Create index on legacy table for migration performance] ****************************************************************************************************************************************
changed: [default]

TASK [migration-and-archiving : Get total rows in last 3 months from flat table] ***********************************************************************************************************************************************
ok: [default]

TASK [migration-and-archiving : Set source row count] **************************************************************************************************************************************************************************
ok: [default]

TASK [migration-and-archiving : Display rows to migrate] ***********************************************************************************************************************************************************************
ok: [default] => {
    "msg": "Total rows to migrate from last 3 months: 251809"
}

TASK [migration-and-archiving : Calculate number of batches] *******************************************************************************************************************************************************************
ok: [default]

TASK [migration-and-archiving : Display batch information] *********************************************************************************************************************************************************************
ok: [default] => {
    "msg": "Will migrate 251809 rows in 11 batches of 25000 rows"
}

TASK [migration-and-archiving : Migrate data in batches with throttling] *******************************************************************************************************************************************************
ASYNC POLL on default: jid=j402540525628.13702 started=1 finished=0
ASYNC POLL on default: jid=j402540525628.13702 started=1 finished=0
ASYNC POLL on default: jid=j402540525628.13702 started=1 finished=0
ASYNC OK on default: jid=j402540525628.13702
changed: [default]

TASK [migration-and-archiving : Get row count in target table] *****************************************************************************************************************************************************************
ok: [default]

TASK [migration-and-archiving : Set target row count] **************************************************************************************************************************************************************************
ok: [default]

TASK [migration-and-archiving : Display migration verification] ****************************************************************************************************************************************************************
ok: [default] => {
    "msg": "Migration Verification:\n- Rows to migrate: 251809\n- Rows migrated: 251809\n- Status: SUCCESS\n"
}

TASK [migration-and-archiving : Verify migration success] **********************************************************************************************************************************************************************
ok: [default] => {
    "changed": false,
    "msg": "Migration successful! All 251809 rows have been migrated."
}

TASK [migration-and-archiving : Display archive warning] ***********************************************************************************************************************************************************************
ok: [default] => {
    "msg": "⚠  WARNING: About to drop legacy table transaction_logs_flat\nThis action is irreversible!\nProceeding with archival in 10 seconds...\n"
}

TASK [migration-and-archiving : Wait before dropping table] ********************************************************************************************************************************************************************
Pausing for 10 seconds
(ctrl+C then 'C' = continue early, ctrl+C then 'A' = abort)
Press 'C' to continue the play or 'A' to abort
ok: [default]

TASK [migration-and-archiving : Drop legacy flat table] ************************************************************************************************************************************************************************
changed: [default]

TASK [migration-and-archiving : Display final status] **************************************************************************************************************************************************************************
ok: [default] => {
    "msg": "✓ Migration and Archival Complete!\n- Partitioned table transaction_logs_modern created successfully\n- Migrated 251809 rows from the last 3 months\n- Legacy table transaction_logs_flat has been archived (dropped)\n- System is now running on the modern partitioned architecture\n"
}

PLAY RECAP *********************************************************************************************************************************************************************************************************************
default                    : ok=20   changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```