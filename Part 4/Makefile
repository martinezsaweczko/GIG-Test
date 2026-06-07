



# Setup environment for GIG Test - Part 4: AlmaLinux 9
.PHONY: setup
setup:
	@echo "Setting up environment for GIG Test - Part 4: AlmaLinux 9"
	vagrant up


.PHONY: clean
clean: delete_key
	@echo "Cleaning up environment for GIG Test - Part 4: AlmaLinux 9"
	vagrant destroy -f


.PHONY: delete_key
delete_key:
	@echo "Deleting SSH key for MySQL VM"
	ssh-keygen -f "/home/david/.ssh/known_hosts" -R "[127.0.0.1]:2222"

.PHONY: install_mysql
install_mysql:
	@echo "Installing MySQL on AlmaLinux 9 VM"
	cd ansible-mysql && ansible-playbook -i inventory playbook.yml
