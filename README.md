Cloud Foundry Meteor Buildpack
==============================

##Create a sample app with 'meteor'

```
% meteor create --example todos
todos: created.

To run your new app:
   cd todos
   meteor
```

##Initial deploy of your app

We need to do an initial deploy of our app to Cloud Foundry so the app environment and bound services can be modified in following steps.

```
% cf push todos -b https://github.com/cloudfoundry-community/cf-meteor-buildpack.git --no-start
```

The app will not start since we need to bind MongoDB to the app first.

##Starting your Cloud Foundry app

To bind a Mongo service to our app we will look into the Cloud Foundry marketplace, create a mongo service instance and then bind that instance to our app.

```
% cf marketplace
Getting services from marketplace in org cs-home / space development as [username]...
OK

service   plans   description
mongodb   100    MongoDB NoSQL database

% cf create-service mongodb 100 todos-mongodb
Creating service todos-mongodb in org cs-home / space development as [username]...
OK

% cf services
Getting services in org cs-home / space development as [username]...
OK

name               service   plan   bound apps
todos-mongodb   mongodb   100

% cf bind-service todos todos-mongodb
Binding service todos-mongodb to app todos in org [org] / space development as [username]...
OK
TIP: Use 'cf restage' to ensure your env variable changes take effect

% cf start todos
Starting app todos in org [org] / space development as [username]...
...
1 of 1 instances running

App started
...
```

Now the todos app should be running on Cloud Foundry. Go to your ROOT_URL, http://todos.[CF Domain URL] in a 2 browsers and watch updates show up on the other browser immediately via web sockets to Cloud Foundry app.
