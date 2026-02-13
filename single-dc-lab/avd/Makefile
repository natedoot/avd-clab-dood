.PHONY: avd-validate
avd-validate: ## Validate configurations using AVD
	ansible-playbook playbooks/avd-validate.yml -i inventory.yml \
		-e ansible_user=$(ANSIBLE_USERNAME) \
		-e ansible_password=$(ANSIBLE_PASSWORD)

.PHONY: anta-validate
anta-validate: ## Validate configurations using ANTA
	ansible-playbook playbooks/anta-validate.yml -i inventory.yml \
		-e ansible_user=$(ANSIBLE_USERNAME) \
		-e ansible_password=$(ANSIBLE_PASSWORD)
