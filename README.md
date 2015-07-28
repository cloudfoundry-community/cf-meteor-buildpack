Cloud Foundry Meteor Buildpack
==============================

##Create a sample app with 'meteor'

```
% meteor create --example leaderboard
leaderboard: created.

To run your new app:
   cd leaderboard
   meteor
```

##Initial deploy of your app

We need to do an initial deploy of our app to Cloud Foundry so the app environment and bound services can be modified in following steps.

```
% cf push leaderboard -b https://github.com/cloudfoundry-community/cf-meteor-buildpack.git --no-start
```

This will not start the app, it will only upload the app.  We will need to create a database to make the example app work.

##Start your Cloud Foundry app

To bind a Mongo service to our app we will look into the Cloud Foundry marketplace, create a mongo service instance and then bind that instance to our app.

There are two options, you can use MongoDB or Mongolab.

###MongoDB
```
% cf marketplace
Getting services from marketplace in org cs-home / space development as [username]...
OK

service   plans   description
mongodb   100    MongoDB NoSQL database

% cf create-service mongodb 100 leaderboard-mongodb
Creating service leaderboard-mongodb in org cs-home / space development as [username]...
OK

% cf services
Getting services in org cs-home / space development as [username]...
OK

name               service   plan   bound apps
leaderboard-mongodb   mongodb   100

% cf bind-service leaderboard leaderboard-mongodb
Binding service leaderboard-mongodb to app leaderboard in org [org] / space development as [username]...
OK
TIP: Use 'cf restage' to ensure your env variable changes take effect

% cf start leaderboard
Starting app leaderboard in org [org] / space development as [username]...
...
1 of 1 instances running

App started
...
```

###MongoLab
```
% cf marketplace
Getting services from marketplace in org cs-home / space development as [username]...
OK

service   plans   description
mongolab  sandbox    MongoDB NoSQL database

% cf create-service mongolab sandbox leaderboard-mongolab
Creating service leaderboard-mongolab in org cs-home / space development as [username]...
OK

% cf services
Getting services in org cs-home / space development as [username]...
OK

name               service   plan   bound apps
leaderboard-mongolab   mongolab   sandbox

% cf bind-service leaderboard leaderboard-mongolab
Binding service leaderboard-mongolab to app leaderboard in org [org] / space development as [username]...
OK
TIP: Use 'cf restage' to ensure your env variable changes take effect

% cf start leaderboard
Starting app leaderboard in org [org] / space development as [username]...
...
1 of 1 instances running

App started
...
```

Now the leaderboard app should be running on Cloud Foundry. Go to your ROOT_URL, http://leaderboard.[CF Domain URL] in a 2 browsers and watch updates show up on the other browser immediately via web sockets to Cloud Foundry app.
