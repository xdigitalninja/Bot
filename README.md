# Deploy [Zeppelin](https://zeppelin.gg) on [Railway](https://railway.app)
### Create a [Railway account](https://railway.app?referralCode=nebula) using GitHub for login
### Link your [Discord account](https://discord.com) to Railway from account settings **[REQUIRED]**
### Run the `/beta` slash command in the Railway Discord

## Set up project on Railway
### Create a new project, provisioned with the MySQL Plugin ![Provision MySQL](/assets/images/provision_mysql.png "Provision MySQL")
### Run Control/Command + K and select Metrofy
### Fork this repo, we're gonna deploy it next

## Create the API service
### Select New Service and pick GitHub Repo, selecting your fork
### Once the service has deployed (and failed to build), select it and go to Settings.
### Rename the service if you would like and set the root directory to `/api`
### Create a variable named `KEY` and set it to [32 random characters](https://passwordsgenerator.net/?length=32&symbols=0&numbers=1&lowercase=1&uppercase=1&similar=0&ambiguous=0&client=1&autoselect=0)
```bash
# Example (don't use this):

KEY=eilegiluegoigefiugsdzdiuggfweaiug
```
### Create a variable named `STAFF` and set it to your Discord Snowflake
```bash
# Replace with your snowflake

STAFF=524722785302609941
```
### Copy the following variables into the api service (tip: Bulk Import)
```bash
DB_PASSWORD=${{ MYSQLPASSWORD }}
DB_USER=${{ MYSQLUSER }}
DB_HOST=${{ MYSQLHOST }}
DB_PORT=${{ MYSQLPORT }}
DB_DATABASE=${{ MYSQLDATABASE }}
PROFILING=false
PORT=8800
OAUTH_CALLBACK_URL=https://${{ RAILWAY_STATIC_URL }}/auth/oauth-callback
```
### These variables will require you to create a [Discord Application](https://discord.dev)
### Go to the OAuth tab and copy these values
```bash
CLIENT_SECRET=<OAuth Client Secret>
CLIENT_ID=<OAuth Client ID>
```

## Create the Bot service
### Deploy your fork again by creating a new service, like you did for the api
### Rename the service if you would like and set the root directory to `/bot`
### Create the following variables
```bash
# Pick a root server to add the bot to

SERVER_NAME=<Server Name>
SERVER_ID=<Server Snowflake>
OWNER_ID=<Server Owners User Snowflake>

# General vars

ACCOUNT_ID=<Your User Snowflake (from api step)>
TOKEN=<Bot Token>
```
## Initialize the Database
### Clone your fork of this repo, make sure Node.js and Railway CLI are installed
### Open a terminal and enter the `init-db` folder
### Run `npm ci` to install deps
### Run `railway run node .` to set up the database, Control + C once it stops logging (i'll fix it later ok)

## Create the Dashboard service
### Deploy your fork again by creating a new service, like you did for the bot
### Rename the service if you would like and set the root directory to `/dashboard`
### Create a variable named `API_URL` and set it to the url Railway generated for the api service 
```bash
# Make sure there is no trailing slash

API_URL=https://example.up.railway.app

# Also add PORT

PORT=80
```

## Connect Dashboard and API
### Go back to the api service and add a variable named `DASHBOARD_URL` set to the generated url for the dashboard
```bash
# Make sure there is no trailing slash

DASHBOARD_URL=https://example.up.railway.app
```

## Set OAuth Callback
### Go your bot's [Discord Application](https://discord.dev)
### Under OAuth, add a callback url set to:
```bash
$API_URL + '/auth/oauth-callback'

EXAMPLE:
API_URL=https://example.up.railway.app

CALLBACK=https://example.up.railway.app/auth/oauth-callback
```