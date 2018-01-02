include .env

export $(shell sed 's/=.*//' .env)

RED=\033[0;31m
NOCOLOUR=\033[0m

.PHONY: init-symfony
init-symfony:
	if [ ! -d "../$(APP_NAME)" ]; then \
		mkdir ../$(APP_NAME); \
		printf "$(RED)folder <$(APP_NAME)> $(NOCOLOUR) will be created Symfony skeleton project will be initialised\n" ; \
		docker-compose -f docker-compose-dev.yml run --rm tools bash -ci 'composer create-project symfony/skeleton .'; \
	else \
		printf "$(RED)folder <$(APP_NAME)> already exists, no skeleton is initialised\n" ; \
	fi;

.PHONY: init-symfony-encore
init-symfony-encore:
	if [ ! -d "../$(APP_NAME)" ]; then \
		make init-symfony; \
	fi
	docker-compose -f docker-compose-dev.yml run --rm tools bash -ci '\
		npm init -y && npm install @symfony/webpack-encore --only=dev && \
		composer require encore && \
		yarn install && \
		rm -rf npm* && rm -rf .yarn-cache*'

.PHONY: start
start:
	docker-compose up -d

.PHONY: stop
stop:
	docker-compose stop

.PHONY: start
rebuild-and-start:
	docker-compose up --build -d
