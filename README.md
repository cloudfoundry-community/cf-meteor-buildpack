# Cloud Foundry Meteor Buildpack

## Usage

```
% cf push <appname> -b https://github.com/csterwa/cf-meteor-buildpack.git
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
=======
cf-meteor-buildpack
===================

