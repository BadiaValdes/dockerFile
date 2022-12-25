# MongoDB

## Test and Deploy

Use the built-in continuous integration in GitLab.

- Download the repository.
- Execute `docker-compose up -d` to deploy the database.
- The database should start running over **localhost:27017**
- The database client manager should start running over **localhost:8081**

***

## Setup

The container created automatically runs the following databases.

- security
- api_test_db
- api_dev_db

In order to update this behavior please modify the `mongo-init.js` file.

***
