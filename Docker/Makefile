include .env

export $(shell sed 's/=.*//' .env)

.PHONY: init-symfony
init-symfony:
	if [ ! -d "../$(APP_NAME)" ]; then \
		mkdir ../$(APP_NAME); \
	fi
	cd ../$(APP_NAME)
	docker-compose -f docker-compose-dev.yml run --rm tools bash -ci 'composer create-project symfony/skeleton .'

.PHONY: init-symfony-encore
init-symfony-encore:
	if [ ! -d "../$(APP_NAME)" ]; then \
		make init-symfony; \
	fi
	docker-compose -f docker-compose-dev.yml run --rm tools bash -ci 'npm init -y && npm install @symfony/webpack-encore --only=dev'
	docker-compose -f docker-compose-dev.yml run --rm tools bash -ci 'composer require encore'
	docker-compose -f docker-compose-dev.yml run --rm tools bash -ci 'yarn install'
	docker-compose -f docker-compose-dev.yml run --rm tools bash -ci 'rm -rf npm* && rm -rf .yarn-cache*'

