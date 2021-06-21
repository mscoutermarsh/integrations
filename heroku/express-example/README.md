## Express.js + PlanetScale example

Example Express.js/Node app using PlanetScale.

## Usage

### Setup your database
1. [Install the PlanetScale CLI](https://planetscale.com/cli).
2. Authenticate the CLI.
```
pscale auth login
```

3. Create a new database.
```
pscale database create your-db-name
```

### Running the example app

1. Clone this repository.
2. run `git clone https://github.com/planetscale/express-example.git`
3. `cd express-example`
4. run `npm install`.
5. Use pscale to create a connection to your database and start up the app

```
pscale connect your-db-name main --execute 'node app.js'
```

Running pscale connect with `execute` will pass a `DATABASE_URL` environment variable to the node application, enabling it to connect to PlanetScale. Check `app.js` to see how the `DATABASE_URL` is used to establish the connection.

## Deploying to Heroku
We'll use the pscale CLI on Heroku to establish a secure connection to your database.

1. Create a PlanetScale service token

```
pscale service-token create
```

Take note of the service token name and value it returns to you.

2. Grant that token access to your database.
```
pscale service-token add-access your-token-name connect_production_branch --database your-db-name
```

3. Set config vars for your Heroku application. You'll use your service token to authenticate to your database.

```
heroku config:set PLANETSCALE_ORG=your-org-name
heroku config:set PLANETSCALE_SERVICE_TOKEN_NAME=your-token-name
heroku config:set PLANETSCALE_SERVICE_TOKEN=your-token-value
```

4. Add the PlanetScale buildpack to your Heroku application:

```
heroku buildpacks:add https://github.com/planetscale/heroku-buildpack-planetscale
```

This command will install the latest `pscale` CLI into your Heroku application. You can specify a CLI version by setting `PLANETSCALE_CLI_VERSION`

_Note:_ Find the latest [CLI releases here.](https://github.com/planetscale/cli/releases).

5. Add a Procfile that initiates your app using pscale. This is the command Heroku will run when starting the app.

```
web: pscale connect your-db-name main --execute 'node app.js'
```

6. That's it! Push to Heroku and you'll have a working app.
