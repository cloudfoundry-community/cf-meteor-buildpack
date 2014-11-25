# Cloud Foundry buildpack for Meteor 0.9.0+

## Usage

```
% cf push <appname> -b https://github.com/rajit/heroku-buildpack-meteor.git
```

## Example

Create a sample app with 'meteor'

```
% meteor create --example wordplay
wordplay: created.

To run your new app:
   cd wordplay
   meteor
```

Push your Cloud Foundry app

```
% cf push wordplay -b https://github.com/csterwa/cf-meteor-buildpack.git
```

Enjoy!
