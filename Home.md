# Steam Chat Bot
##### Simplified Interface for a steam chat bot, by [several people](https://github.com/Efreak/node-steam-chat-bot/graphs/contributors)

## Installing & Running



1. Install NPM on your system. We probably won't help you with this beyond the following:
 * Linux: Install it from your package manager, or better, use [this script](https://gist.github.com/TooTallNate/3288316)
 * Windows, Mac, or Linux: Install from [here](https://nodejs.org/download/)
2. Install the bot: `npm install node-steam-chat-bot`
 * Alternatively, if you want to run the version in git, `npm install Efreak/node-steam-chat-bot` (or whichever fork you want)
3. Edit the config file *./node_modules/steam-chat-bot/example.js* and save it as *./mybot.js*
4. `node mybot.js`

## TODO:

### [Alternative Database Config](https://github.com/Efreak/node-steam-chat-bot/issues/40)

##### Ideas to improve the bot and avoid heroku's limitations
* Move logs into the database
* Abstract the database functionality away from firebase and make it available to other plugins (logs, infobot). Firebase is not enough for chat logs, so provide other options
    * [Firebase hacker plan](https://www.firebase.com/pricing.html) (100mb) is definitely plenty for just configuration, and probably for the infobot stuff as well. No good for logs, unless you only want error logs, or you don't want to keep full history (my logs are >200mb)
    * [Heroku's PostgreSQL hobby-dev plan](https://addons.heroku.com/heroku-postgresql)'s 10k rows is probably about the same as Firebase's hacker plan.
    * [MongoLab](https://mongolab.com/signup/) offers 500mb for free. This is probably the best free storage you can get. This is probably the same as [Heroku's MongoLab sandbox plan](https://addons.heroku.com/mongolab) (496mb) and [AppHarbor's MongoLab Free Plan](https://appharbor.com/addons/mongolab) (0.5gb)
    * [Scalingo](https://scalingo.com) has perhaps better offerings than Heroku, with free 512mb offerings for [PostgreSQL](https://scalingo.com/addons/scalingo-postgresql) and [MongoDB](https://scalingo.com/addons/scalingo-mongodb)
    * OpenShift is probably the best of all, as it allows you to have a "DIY" cartridge with 1gb of persistent data. The only downside is that you'll have to do all the setup yourself.
* Perhaps add options for other providers than heroku as well, allowing the bot to be easily run on Scalingo, red hat's openshift (1gb persistent storage on free plan *and* custom domains), others. (basically specify the environment variable containing certain information in the config file).
* Moving logs, etc into the database will require some kind of caching mechanism, as most providers don't guarantee uptime for the databases. For example, heroku guarantees "less than four hours" of database downtime per month for the free database, meaning that we'll have to accept it not being available.

### Test core functions

- See [#37](https://github.com/Efreak/node-steam-chat-bot/issues/37)

### Pushbullet

I'd like to get some kind of notification when someone messages the bot, or says my name in chat. This could probably be extended rather easily to allow users to give the bot their own Pushbullet API keys so they can get similar notifications when someone wants them.

### [Requests & Bugs](https://github.com/Efreak/node-steam-chat-bot/issues)

- [Antiflood plugin](https://github.com/Efreak/node-steam-chat-bot/issues/13)