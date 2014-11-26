Cloud Foundry Meteor Buildpack
==============================

##Create a sample app with 'meteor'

```
% meteor create --example wordplay
wordplay: created.

To run your new app:
   cd wordplay
   meteor
```

##Workaround For Now (Ugh)

Until scripts pull VCAP_SERVICES data (MongoDB URL and app URI), we must set specific environment variables for the Meteor app to start.

```
% cf env wordplay
...
System-Provided:
{
 "VCAP_SERVICES": {
  "mongodb-2.2": [
   {
    "credentials": {
     "db": "db",
     ...
     "url": "mongodb://<guid-user-id>@<ip address>:<port>/db",
     ...
    },
    ...
   }
  ]
 }
}

% cf set-env wordplay MONGO_URL <mongodb-2.2 url from above>

% cf domains
Getting domains in org cs-home as <username>...
name                  status
<CF Domain URL>       shared

% cf set-env wordplay ROOT_URL http://wordplay.<CF Domain URL>
```

To verify you set the environment variables for the app, run the `cf env` command again and look for the "User-Provided" section.

```
% cf env wordplay
...
User-Provided:
MONGO_URL: <mongodb-2.2 url from above>
ROOT_URL: http://wordplay.<CF Domain URL>
```

##Push your Cloud Foundry app

Once we have the environment variables set then we can make an initial push of our app using the cf-meteor-buildpack.

```
% cf push wordplay -b https://github.com/csterwa/cf-meteor-buildpack.git
```

This will not start successfully since we have not bound a Mongo service to our app. To bind a Mongo service to our app we will look into the Cloud Foundry marketplace, create a mongo service instance and then bind that instance to our app.

```
% cf marketplace
Getting services from marketplace in org cs-home / space development as <username>...
OK

service   plans   description
mongodb   free    MongoDB NoSQL database

% cf create-service mongodb free wordplay-mongodb
Creating service wordplay-mongodb in org cs-home / space development as <username>...
OK

% cf services
Getting services in org cs-home / space development as <username>...
OK

name               service   plan   bound apps
wordplay-mongodb   mongodb   free

% cf bind-service wordplay wordplay-mongodb
Binding service wordplay-mongodb to app wordplay in org <org> / space development as <username>...
OK
TIP: Use 'cf restage' to ensure your env variable changes take effect

% cf restage wordplay
Restaging app wordplay in org <org> / space development as <username>...
...
1 of 1 instances running

App started
...
```

Now the wordplay app should be running on Cloud Foundry.
