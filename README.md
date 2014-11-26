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
Getting domains in org cs-home as chris.sterling@gmail.com...
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

Once we have the environment variables set then we can just push our app using the cf-meteor-buildpack.

```
% cf push wordplay -b https://github.com/csterwa/cf-meteor-buildpack.git
```
