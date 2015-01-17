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

##Initial deploy of your app

We need to do an initial deploy of our app to Cloud Foundry so the app environment and bound services can be modified in following steps.

```
% cf push wordplay -b https://github.com/csterwa/cf-meteor-buildpack.git
```

This will not start successfully since we have not setup environment or bound a Mongo service to the app.

##Restage your Cloud Foundry app

To bind a Mongo service to our app we will look into the Cloud Foundry marketplace, create a mongo service instance and then bind that instance to our app.

```
% cf marketplace
Getting services from marketplace in org cs-home / space development as [username]...
OK

service   plans   description
mongodb   free    MongoDB NoSQL database

% cf create-service mongodb free wordplay-mongodb
Creating service wordplay-mongodb in org cs-home / space development as [username]...
OK

% cf services
Getting services in org cs-home / space development as [username]...
OK

name               service   plan   bound apps
wordplay-mongodb   mongodb   free

% cf bind-service wordplay wordplay-mongodb
Binding service wordplay-mongodb to app wordplay in org [org] / space development as [username]...
OK
TIP: Use 'cf restage' to ensure your env variable changes take effect

% cf restage wordplay
Restaging app wordplay in org [org] / space development as [username]...
...
1 of 1 instances running

App started
...
```

Now the wordplay app should be running on Cloud Foundry. Go to your ROOT_URL, http://wordplay.[CF Domain URL] in a 2 browsers and watch updates show up on the other browser immediately via web sockets to Cloud Foundry app.
