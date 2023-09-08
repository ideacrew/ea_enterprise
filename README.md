# ea_enterprise
Collection of containers to run the Enroll ecosystem in a development context

## Getting Started

Download and install Docker Desktop.

Clone the ea_enterprise repo:
```
git clone https://github.com/ideacrew/ea_enterprise.git
```

Copy the env.example to env.dev and change the values as needed (don't commit env file)
```
cp env.example env.dev
```

Ensure the other repositories are present on your local machine and at the same level as ea_enterprise:

- enroll
- fdsh_gateway
- medicaid_gateway
- medicaid_eligibility
- aca_entities
- polypress

the layout should look like this (you can change the *projects* folder)
```
~ /
└── projects/
    ├── ea_enterprise
    ├── enroll
    ├── fdsh_gateway
    ├── medicaid_gateway
    ├── aca_entities
    └── polypress

```

Be sure all repositories are up to date with their respective release branches before running any commands!

## Important concepts

- inside the container: this means we will run a command inside the container, for example, `docker-compose exec enroll /bin/bash` will run the command `/bin/bash` inside the container `enroll` from there we can execute any command that is available inside the container, for example, `rails c` will open a rails console inside the container

- outside the container: this means we will run a command on the "host", usually in the context of the developer's Mac.

## Known workarounds

- mongoid.yml:
The services `fdsh_gateway`, `enroll` has "localhost" as the "server" on config/mongoid.yml, the docker-compose configuration "patches" this by mounting anther configuration, the "patched" configuration is located at `config/mongoid.yml.docker` and is mounted on the container at `/{APP}/config/mongoid.yml`

- local aca_entities:
Local aca_entities: it is already mounted and can be changed on the Gemfile, the trick is just to restart the service you are working with (i.e., restart only Enroll if you are updating Enroll's Gemfile).

1. docker-compose up
2. wait
3. modify the gemfile point to a file path `gem 'aca_entities', path: "/aca_entities"`
4. restart just enroll via the ui or command line *(do not restart everything)*

- stimulus reflex on MG
Docker-compose will patch MG stimulus reflex initializer to ignore dev:cache not being enabled, this is done via a volume mount on the docker-compose.yml file, the file is located at `hotpatches/stimulus_reflex.rb` and is mounted on the container at `/{APP}/config/initializers/stimulus_reflex.rb`. 

## Basic Docker Compose Commands
Note: commands must be run in the terminal from inside the ea_enterprise directory.

- Start all services
```
docker-compose up
```

- Start **enroll** and any of its dependencies
```
docker-compose up enroll
```

- Start 2 services
```
docker-compose up enroll fdsh_gateway
```
- Rebuild all the containers  
```
docker-compose build
```

- Rebuild specific container (example; **enroll** container)
```
docker-compose build enroll
```
- Shell inside a container
```
docker-compose exec enroll /bin/bash
```
- Rubocop inside **enroll**
```
docker-compose exec enroll /enroll/rubocop_check_last_commit.sh
```
```
docker-compose exec enroll /enroll/rubocop_check_pre_commit.sh
```

- docker-compose "exec vs run"

The slight difference between exec and run is that run will create a new container, and exec will run the command on an existing container, examples;

1. this will execute /bin/bash under the *running* container enroll, if there is no enroll running, the command will fail

```
docker-compose exec enroll /bin/bash
```
2. this will create a new container and inside it will execute /bin/bash, if there is a container running, it will not be affected and create a new one in parallel, if there is no enroll running it will not fail

```
docker-compose run enroll /bin/bash
```

## Restoring a database 

Make sure Mongo is running

```
cd ea_enterprise
docker compose cp ~/projects/dumps/super_dump/ mongodb:/dump
docker-compose exec mongodb mongorestore
```

## Basic rails commands
- start a console (from outside, to the container)
```
docker-compose exec enroll rails c
```

- run tests (inside the container): 
```
RAILS_ENV=test bundle exec rspec components/financial_assistance/spec/
```

- run test outside the container (cd to ea_enterprise first):
```
docker-compose exec -e "RAILS_ENV=test" bundle exec enroll rspec components/financial_assistance/spec/
```

- open rails console (inside the container):
## When do you need to restart the container?

Very similar to the rules regarding when to restart a rails application 

- You changed something under /config 
- You need to switch branches

## When to trigger a rebuild 

- you added or deleted a gem
- you changed anything docker-compose.yml file
- you changed the Dockerfile of any container

## Cucumber

It is possible to run cucumber, however, there are 2 drawbacks; first is that the gem webdrivers have to be removed manually and the container restarted, and the second drawback is that it uses a "modified" env.rb that removes all the references to the webdriver gem, this is done automatically via a virtual volume on docker-compose (as end user you don't need to worry about this unless something big changes on cucumber).

These are the steps to enable cucumber 

1. On enroll edit the Gemfile and remove: `gem 'webdrivers', '~> 3.0'`
2. Trigger a restart for enroll with 
```
docker-compose restart enroll
```
3. run cucumber
    - from inside the container (attach a shell to the container first)
```
NODE_ENV=test RAILS_ENV=test bundle exec cucumber features/financial_assistance/view_eligibility.feature
```
    - from outside the container (inside the ea_enterprise directory): 
```
docker-compose exec -e "RAILS_ENV=test" enroll bundle exec cucumber features/financial_assistance/view_eligibility.feature

```

After rebuilding the container for the first time, only step 3 is needed

## Common issues and how to solve them
- rspec 
```
  uninitialized constant Mongoid::Matchers
```
you are not running the specs on test environment, run them with `RAILS_ENV=test bundle exec rspec`


## Recommendations 

Some people complain about "writing" speed, and it's true, it's slow, however on the "experimental features", there is a new option called "VirtioFS" and it's fast, close to native fast, the recommendation is to enable it

<img width="1090" alt="Screen Shot 2022-07-15 at 1 27 05 PM" src="https://user-images.githubusercontent.com/350422/179311143-090ab646-3b6d-441a-ba0b-cefc4d5de1d0.png">

## FAQ

- how do I enter the rails console? use `docker exec -it <container name>` /bin/bash, or via the UI there is a button to do this quickly

- why we are not executing bundle install on docker-compose? <- it's slower, and in theory, gems should not change that much

- why the shell inside the container doesn't respond to the arrows or any other shell nicety? (usually, this happens when using the docker UI) it's because it's executing /bin/sh, you can execute /bin/bash as soon as the terminal is open

## TODO

- add scripts to open the console quickly 
- add scripts to run rubocop/tests
- fix/add the Nginx in front of the services that have them on production (the one in Medicaid gateway is not fully configured yet)
