See: https://docs.docker.com/compose/rails/

Run: `docker-compose run web rails new . --force --database=postgresql`

Now that you’ve got a new Gemfile, you need to build the image again. (This, and changes to the Gemfile or the Dockerfile, should be the only times you’ll need to rebuild.)

`docker-compose build`

The app is now bootable, but you’re not quite there yet. By default, Rails expects a database to be running on localhost - so you need to point it at the db container instead. You also need to change the database and username to align with the defaults set by the postgres image.

Replace the contents of config/database.yml with the following:

```
default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: postgres
  password:
  pool: 5

development:
  <<: *default
  database: myapp_development

test:
  <<: *default
  database: myapp_test
```

You can now boot the app:

`docker-compose up`

Finally, you need to create the database. In another terminal, run:

`docker-compose run web rake db:create`

Go to http://localhost:3000 on a web browser to see the Rails Welcome.

- To stop the application, run `docker-compose down` in your project directory.
- To restart the application run `docker-compose up`
- If you make changes to the Gemfile or the Compose file to try out some different configurations, you need to rebuild. Some changes require only docker-compose up --build


