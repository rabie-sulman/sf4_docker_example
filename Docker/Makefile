include .env

export $(shell sed 's/=.*//' .env)

RED=\033[0;31m
GREEN=\033[0;32m
NOCOLOUR=\033[0m
PACK=null

.PHONY: all
all: 
	make usage

.PHONY: usage
usage: 
	@printf "$(GREEN)usage:\n" ;
	@printf "$(GREEN)init-symfony: $(NOCOLOUR) initialise a Symfony project\n" ;
	@printf "$(GREEN)init-symfony-front: $(NOCOLOUR) initialise a Symfony front end Encore and installs Assets\n" ;
	@printf "$(GREEN)start: $(NOCOLOUR) starts the docker dev env from the $(GREEN)docker-compose.yml \n" ;
	@printf "$(GREEN)stop: $(NOCOLOUR) stops the docker containers in the $(GREEN)docker-compose.yml \n" ;
	@printf "$(GREEN)rebuild-and-start: $(NOCOLOUR) rebuilds and starts the docker containers in the $(GREEN)docker-compose.yml \n" ;
	@printf "$(GREEN)sy-require PACK=package-name $(NOCOLOUR): run composer require inside the php container\n" ;
	@printf "$(GREEN)sy-remove PACK=package-name $(NOCOLOUR): run composer remove inside the php container\n" ;
	@printf "$(GREEN)sy-run C=cache:clear $(NOCOLOUR):run the symfony console inside the php container\n" ;

.PHONY: init-symfony
init-symfony:
	@if [ ! -d "../$(APP_NAME)" ]; then \
		mkdir ../$(APP_NAME); \
		printf "$(RED)folder <$(APP_NAME)> $(NOCOLOUR) will be created Symfony skeleton project will be initialised\n" ; \
		docker-compose -f docker-compose-dev.yml run --rm tools bash -ci 'composer create-project symfony/skeleton .'; \
	else \
		printf "$(RED)folder <$(APP_NAME)> already exists, no skeleton is initialised\n" ; \
	fi;

.PHONY: init-symfony-encore
init-symfony-front:
	@if [ ! -d "../$(APP_NAME)" ]; then \
		make init-symfony; \
	fi
	docker-compose -f docker-compose-dev.yml run --rm tools bash -ci '\
		npm init -y && npm install @symfony/webpack-encore --only=dev && \
		composer require asset && \
		composer require encore && \
		yarn install && \
		rm -rf npm* && rm -rf .yarn-cache*'

.PHONY: start
start:
	docker-compose up -d

.PHONY: stop
stop:
	docker-compose stop

.PHONY: rebuild-and-start
rebuild-and-start:
	docker-compose up --build -d

.PHONY: require
sy-require:
	@if [ "$(PACK)" = "null" ]; then \
		printf "$(RED)Package needs to be passed as argument: PACK \n" ; \
	else \
		printf "Package: $(RED) <$(PACK)> $(NOCOLOUR) is being required \n" ; \
		docker-compose run --rm php composer require $(PACK); \
	fi

.PHONY: remove
sy-remove:
	@if [ "$(PACK)" = "null" ]; then \
		printf "$(RED)Package needs to be passed as argument: PACK \n" ; \
	else \
		printf "Package: $(RED) <$(PACK)> $(NOCOLOUR)is being removed \n" ; \
		docker-compose run --rm php composer remove $(PACK); \
	fi

.PHONY: sy-run
sy-run:
	@printf "Running Symfony command: <$(RED)$(C)$(NOCOLOUR)> \n" ; \
	docker-compose run --rm php bin/console $(C); \
