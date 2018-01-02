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

.PHONY: require
sy-require:
	if [ "$(PACK)" = "null" ]; then \
		printf "$(RED)Package needs to be passed as argument: PACK \n" ; \
	else \
		printf "Package: $(RED) <$(PACK)> $(NOCOLOUR) is being required \n" ; \
		docker-compose run --rm php composer require $(PACK); \
	fi

.PHONY: remove
sy-remove:
	if [ "$(PACK)" = "null" ]; then \
		printf "$(RED)Package needs to be passed as argument: PACK \n" ; \
	else \
		printf "Package: $(RED) <$(PACK)> $(NOCOLOUR)is being removed \n" ; \
		docker-compose run --rm php composer remove $(PACK); \
	fi
