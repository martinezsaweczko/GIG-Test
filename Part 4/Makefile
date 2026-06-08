



# Setup environment for GIG Test - Part 4: AlmaLinux 9
.PHONY: setup
setup:
	@echo "Setting up environment for GIG Test - Part 4: AlmaLinux 9"
	vagrant up


.PHONY: clean
clean: delete_key
	@echo "Cleaning up environment for GIG Test - Part 4: AlmaLinux 9"
	rm -f /tmp/mysql_root_password
	vagrant destroy -f


.PHONY: delete_key
delete_key:
	@echo "Deleting SSH key for MySQL VM"
	ssh-keygen -f "/home/david/.ssh/known_hosts" -R "[127.0.0.1]:2222"

.PHONY: install_mysql
install_mysql:
	@echo "Installing MySQL on AlmaLinux 9 VM"
	cd ansible-mysql && ansible-playbook -i inventory playbook.yml

.PHONY: populate_legacy_table
populate_legacy_table:
	@echo "Populating legacy transaction_logs_flat table (1 million rows)"
	cd ansible-mysql && ansible-playbook -i inventory populate-legacy-table.yml

.PHONY: migrate_to_partitioned
migrate_to_partitioned:
	@echo "Migrating data from flat to partitioned architecture"
	cd ansible-mysql && ansible-playbook -i inventory migration-and-archiving.yml
