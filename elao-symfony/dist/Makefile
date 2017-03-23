.SILENT:
.PHONY: install build test

##########
# Manala #
##########

# Lint
LINT_PHP = src tests
LINT_JS  = app/Resources/assets/js
LINT_CSS = app/Resources/assets/sass

include manala/make/Makefile

################
# Environments #
################

# Testing
%@testing: export SYMFONY_ENV = test
%@testing: export NODE_ENV    = test

# Staging
%@staging: export SYMFONY_ENV = prod
%@staging: export NODE_ENV    = production

# Production
%@production: export SYMFONY_ENV = prod
%@production: export NODE_ENV    = production

#########
# Setup #
#########

setup@development: install build

###########
# Install #
###########

## Install application
install: $(call proxy,install)
install@development:
	$(call log,Install - Link directories)
	$(call link_directory, var/logs,     $(APP_LOG_DIR))
	$(call link_directory, var/cache,    $(APP_CACHE_DIR))
	$(call link_directory, var/sessions, $(APP_SESSIONS_DIR))

	$(call log,Install - Composer)
	composer install --ansi --verbose

	$(call log,Install - Database)
	bin/console doctrine:database:create --ansi --if-not-exists
	bin/console doctrine:schema:update --ansi --force
	bin/console doctrine:fixtures:load --ansi --no-interaction

	$(call log,Install - Database - Test)
	bin/console doctrine:database:create --ansi --if-not-exists --env=test
	bin/console doctrine:schema:update --ansi --force --env=test
	bin/console doctrine:fixtures:load --ansi --no-interaction --env=test

	$(call log,Install - Yarn)
	yarn install --color

install@testing:
	$(call log,Install - Composer)
	composer install --ansi --verbose --no-progress --no-interaction

	$(call log,Install - Database)
	bin/console doctrine:database:drop --ansi --force --if-exists
	bin/console doctrine:database:create --ansi --if-not-exists
	bin/console doctrine:schema:update --ansi --force
	bin/console doctrine:fixtures:load --ansi --no-interaction

	$(call log,Install - Yarn)
	yarn install --color

install@staging:
	$(call log,Install - Composer)
	composer install --verbose --no-progress --no-interaction --prefer-dist --optimize-autoloader

	$(call log,Install - Symfony cache)
	bin/console cache:warmup

	$(call log,Install - Yarn)
	yarn install

install@production:
	$(call log,Install - Composer)
	composer install --verbose --no-progress --no-interaction --prefer-dist --optimize-autoloader --no-dev

	$(call log,Install - Symfony cache)
	bin/console cache:warmup --no-debug

	$(call log,Install - Yarn)
	yarn install

#################
# Build / Watch #
#################

## Build application
build: $(call proxy,build)
build@development:
	$(call log,Build - Webpack)
	node_modules/webpack/bin/webpack.js --progress --color --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js

build@testing:
	$(call log,Build - Webpack)
	node_modules/webpack/bin/webpack.js --color --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js

build@staging:
	$(call log,Build - Webpack)
	node_modules/webpack/bin/webpack.js --color --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js

build@production:
	$(call log,Build - Webpack)
	node_modules/webpack/bin/webpack.js --color --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js

## Watch application
watch: $(call proxy,watch)
watch@development:
	$(call log,Watch - Webpack)
	node_modules/webpack/bin/webpack.js --watch --watch-poll --progress  --color --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js

########
# Test #
########

## Test
test: test-php

## Test Php
test-php: phpunit behat

## Run phpunit tests
phpunit: $(call proxy,phpunit)
phpunit@development:
	$(call log,Test - Php - PHPUnit)
	-vendor/bin/phpunit

phpunit@testing:
	$(call log,Test - Php - PHPUnit)
	vendor/bin/phpunit --log-junit build/test/phpunit.xml

## Run behat tests
behat: $(call proxy,behat)
behat@development:
	$(call log,Test - Php - Behat)
	-vendor/bin/behat

behat@testing:
	$(call log,Test - Php - Behat)
	mkdir -p build/test && vendor/bin/behat --format=junit --out=build/test/behat

#######
# Ssh #
#######

## Ssh (staging)
ssh@staging:
	ssh jarvis-2.elao.elao.ninja.local

## Ssh (production)
ssh@production:
	ssh jarvis-2.elao.elao.local

##########
# Deploy #
##########

## Deploy application (staging)
deploy@staging:
	$(call log,Deploy - Ansible playbook)
	ansible-playbook manala/deploy/playbook.yml --inventory-file=manala/deploy/hosts.yml --limit=staging

## Deploy application (production)
deploy@production:
	$(call log,Deploy - Ansible playbook)
	ansible-playbook manala/deploy/playbook.yml --inventory-file=manala/deploy/hosts.yml --limit=production

#######
# App #
#######