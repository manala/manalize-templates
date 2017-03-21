.SILENT:
.PHONY: install build test

##########
# Manala #
##########

# Lint
LINT_PHP = .
LINT_JS  = ./app/Resources/assets/js
LINT_CSS = ./app/Resources/assets/sass

# Security
SECURITY_PHP = .

include manala/Makefile

################
# Environments #
################

# Test
%@test: export SYMFONY_ENV = test
%@test: export NODE_ENV    = test

# Staging
%@staging: export SYMFONY_ENV = prod
%@staging: export NODE_ENV    = production

# Production
%@production: export SYMFONY_ENV = prod
%@production: export NODE_ENV    = production

###########
# Install #
###########

## Install application
install:
$(call log,Link directories)
$(call link_directory, var/logs,     $(APP_LOG_DIR))
$(call link_directory, var/cache,    $(APP_CACHE_DIR))
$(call link_directory, var/sessions, $(APP_SESSIONS_DIR))

$(call log,Composer)
composer install --verbose

$(call log,Database)
bin/console doctrine:database:create --if-not-exists
bin/console doctrine:schema:update --force
bin/console doctrine:fixtures:load --no-interaction

$(call log,Database - Test)
bin/console doctrine:database:create --if-not-exists --env=test
bin/console doctrine:schema:update --force --env=test
bin/console doctrine:fixtures:load --no-interaction --env=test

$(call log,Yarn)
yarn install

install@test:
$(call log,Composer)
composer install --verbose --no-progress --no-interaction

$(call log,Database)
bin/console doctrine:database:drop --force --if-exists
bin/console doctrine:database:create --if-not-exists
bin/console doctrine:schema:update --force

$(call log,Yarn)
yarn install

install@staging:
$(call log,Link directories)
$(call link_directory, var/logs,     $(APP_LOG_DIR))
$(call link_directory, var/cache,    $(APP_CACHE_DIR))
$(call link_directory, var/sessions, $(APP_SESSIONS_DIR))

$(call log,Composer)
composer install --verbose --no-progress --no-interaction --prefer-dist --optimize-autoloader

$(call log,Symfony cache)
bin/console cache:warmup

$(call log,Yarn)
yarn install

install@production:
$(call log,Link directories)
$(call link_directory, var/logs,     $(APP_LOG_DIR))
$(call link_directory, var/cache,    $(APP_CACHE_DIR))
$(call link_directory, var/sessions, $(APP_SESSIONS_DIR))

$(call log,Composer)
composer install --verbose --no-progress --no-interaction --prefer-dist --optimize-autoloader --no-dev

$(call log,Symfony cache)
bin/console cache:warmup --no-debug

$(call log,Yarn)
yarn install

#################
# Build / Watch #
#################

## Build application
build:
$(call log,Webpack)
node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js

build@production: build

## Watch application
watch:
$(call log,Webpack)
node_modules/webpack/bin/webpack.js --watch --watch-poll --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js

########
# Test #
########

## Run tests
test:
$(call log,PHPUnit)
vendor/bin/phpunit

$(call log,Behat)
bin/console cache:clear --env=test && vendor/bin/behat

test@test:
$(call log,PHPUnit)
rm -Rf build/phpunit && mkdir -p build/phpunit
stty cols 80 && vendor/bin/phpunit --log-junit build/phpunit/junit.xml --coverage-clover build/phpunit/clover.xml --coverage-html build/phpunit/coverage

$(call log,Behat)
rm -Rf build/behat && mkdir -p build/behat
bin/console cache:clear && vendor/bin/behat --format=junit --out=build/behat --no-interaction

##########
# Deploy #
##########

## Deploy application (staging)
deploy@staging:
$(call log,Ansible playbook)
ansible-playbook ansible/deploy.yml --inventory-file=ansible/hosts --limit=deploy_demo

## Deploy application (production)
deploy@production:
$(call log,Ansible playbook)
ansible-playbook ansible/deploy.yml --inventory-file=ansible/hosts --limit=deploy_prod

#######
# App #
#######

