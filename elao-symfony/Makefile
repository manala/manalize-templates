.DEFAULT_GOAL := help

# Colors
COLOR_RESET   = \033[0m
COLOR_INFO    = \033[32m
COLOR_COMMENT = \033[33m

# Log
define log
    @printf "[${COLOR_COMMENT}${shell date +"%T"}${COLOR_RESET}] ${COLOR_INFO}${1}${COLOR_RESET}\n"
endef

# Vagrant
export VAGRANT_CWD         = manala
export VAGRANT_FORCE_COLOR = 1

# Includes
include manala/Makefile.*

########
# Help #
########

## Help
help:
	@printf "${COLOR_COMMENT}Usage:${COLOR_RESET}\n"
	@printf " make [target]\n\n"
	@printf "${COLOR_COMMENT}Available targets:${COLOR_RESET}\n"
	@awk '/^[a-zA-Z\-\_0-9\.@]+:/ { \
		helpMessage = match(lastLine, /^## (.*)/); \
		if (helpMessage) { \
			helpCommand = substr($$1, 0, index($$1, ":")); \
			helpMessage = substr(lastLine, RSTART + 3, RLENGTH); \
			printf " ${COLOR_INFO}%-16s${COLOR_RESET} %s\n", helpCommand, helpMessage; \
		} \
	} \
	{ lastLine = $$0 }' $(MAKEFILE_LIST)

########
# Main #
########

## Up
up:
	$(call log,Vagrant up...)
	@vagrant up

## Ssh
ssh:
	$(call log,Vagrant ssh...)
	@vagrant ssh

## Halt
halt:
	$(call log,Vagrant halt...)
	@vagrant halt

## Destroy
destroy:
	$(call log,Vagrant destroy...)
	@vagrant destroy --force

	$(call log,Remove vagrant dotfile...)
	@rm -Rf manala/.vagrant
