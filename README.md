# ea_enterprise
Collection of containers to run Enroll in development context

## Getting Started

Download and install Docker Desktop.

Clone the ea_enterprise repo:

`git clone https://github.com/ideacrew/ea_enterprise.git`

Ensure the other repositories are present on your local machine and at the same level as `ea_enterprise`:

- enroll
- fdsh_gateway
- medicaid_gateway
- medicaid_eligibility
- polypress

the layout should look like this (you can change *projects*)
```
~ /
└── projects/
    ├── ea_enterprise
    ├── enroll
    ├── fdsh_gateway
    ├── medicaid_gateway
    └── polypress

```

Be sure all repositories are up to date with their respective release branches before running any commands!

## Known issues

The services fdsh_gateway, enroll has "localhost" as the "server" on config/mongoid.yml, you need to change it to "mongodb" on line 12 and 69 (16 and 175 for fdsh_gateway), but do not commit this change.
For some reason, medicaid_gateway and polypress have the correct name on config/mongoid.yml. No need to update there.


## Basic Docker Compose Commands
Note: commands must be run in the terminal from inside the ea_enterprise directory.

- Start all services
```
docker-compose up
```

- Start enroll and any of its dependencies
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

- Rebuild specific container (example; enroll container)
```
docker-compose build enroll
```
- Shell in a container
```
docker-compose run enroll /bin/bash
```
- Rubocop on enroll
```
docker-compose run enroll /enroll/rubocop_check_last_commit.sh
```
```
docker-compose run enroll /enroll/rubocop_check_pre_commit.sh
```
## Basic rails commands
- run tests (inside the container): 
```
RAILS_ENV=test bundle exec rspec components/financial_assistance/spec/
```

- run test outside the container (cd to ea_enterprise first):
```
docker-compose run -e "RAILS_ENV=test" enroll bundle exec rspec components/financial_assistance/spec/
```

- open rails console (inside the container):
```
bin/rails c
``

## when you need to restart the container

Very similar to the rules regarding when to restart a rails application 

- You changed something under /config 
- You need to switch branches

## when to trigger a rebuild 

- you added or deleted a gem
- you changed anything docker-compose.yml file
- you changed the Dockerfile of any container

## FAQ

- how do I enter rails console? use docker exec -it <container name> /bin/bash, or via the UI there is a button to do this quickly

- why we are not executing bundle install again? <- it's slower, and in theory, gems should not change that much

- why the shell inside the container doesn't respond to the arrows or any other shell nicety? (usually, this happens when using the docker UI) it's because it's executing /bin/sh, you can execute /bin/bash as soon as the terminal is open

## TODO

- add scripts to open console quickly 
- add scripts to run rubocop/tests
- fix/add the nginx in front of the services that has them on production
