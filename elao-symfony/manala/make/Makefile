.DEFAULT_GOAL := help
.PHONY: help confirm manala

DIR      = $(shell pwd -P)
HOSTNAME = $(shell hostname -f)

# Manala
MANALA_DIR = $(patsubst %/make/,%,$(dir $(abspath $(lastword $(MAKEFILE_LIST)))))

# Colors
COLOR_RESET   = \033[0m
COLOR_ERROR   = \033[31m
COLOR_INFO    = \033[32m
COLOR_WARNING = \033[33m
COLOR_COMMENT = \033[36m

# Log
define log
    @printf "\n[$(COLOR_COMMENT)$(shell date +"%T")$(COLOR_RESET)] $(COLOR_INFO)$(1)$(COLOR_RESET)\n\n"
endef

# Log - Warning
define log_warning
    @printf "\n[$(COLOR_COMMENT)¯\_(ツ)_/¯$(COLOR_RESET)] $(COLOR_WARNING)$(1)$(COLOR_RESET)\n\n"
endef

# Log - Error
define log_error
    @printf "\n[$(COLOR_COMMENT)(╯°□°)╯︵ ┻━┻$(COLOR_RESET)] $(COLOR_ERROR)$(1)$(COLOR_RESET)\n\n"
endef

# Link directory
define link_directory
    [ -d $(1) ] && rm -rf $(1) ; ln -sfnv $(2) $(1)
endef

# Help
HELP = \
	$(COLOR_COMMENT)Usage:$(COLOR_RESET)\
	\n  make [target]\n\
	\n$(COLOR_COMMENT)Help:$(COLOR_RESET)\
	\n  $(COLOR_INFO)help:$(COLOR_RESET) This help\
	\n

# Includes
ifneq ($(wildcard manala/.),)
include $(MANALA_DIR)/make/Makefile.vm
else ifeq ($(DIR),$(MANALA_DIR))
include $(MANALA_DIR)/make/Makefile.vm
endif
include $(MANALA_DIR)/make/Makefile.lint

########
# Help #
########

help:
	@printf "$(HELP)"
ifneq ($(DIR),$(MANALA_DIR))
	@printf "\n$(COLOR_COMMENT)Application:$(COLOR_RESET)\n"
	@awk '/^[a-zA-Z\-\_0-9\.@]+:/ { \
		helpMessage = match(lastLine, /^## (.*)/); \
		if (helpMessage) { \
			helpCommand = substr($$1, 0, index($$1, ":")); \
			helpMessage = substr(lastLine, RSTART + 3, RLENGTH); \
			printf "  $(COLOR_INFO)%-18s$(COLOR_RESET) %s\n", helpCommand, helpMessage; \
		} \
	} \
	{ lastLine = $$0 }' $(MAKEFILE_LIST)
endif
	@printf "\n"

###########
# Confirm #
###########

confirm:
	@printf "\n${COLOR_INFO} ༼ つ ◕_◕ ༽つ ${COLOR_WARNING}Please confirm (y/N)${COLOR_RESET}: "
	@read CONFIRM ; if [ "$$CONFIRM" != "y" ]; then exit 1; fi

##########
# Manala #
##########

manala:
	@curl -s -L http://bit.ly/10hA8iC | bash

