Here is the page where all of the internal chatBot.js function documentation will be stored. This page will also show the template for creating your own trigger. Event handlers (functions that start with a `_` will not be listed). Anything having to do with a trigger will not be listed. An output of `None` means that the method doesn't return anything.

Here are the internal methods that SHOULD NOT be used outside of chatbot (only use connect in config file, NEVER use logOn unless you want the bot to crash). All of these methods/properties can be accessed inside of triggers via `this/that.chatBot.property/method`

### logOn()
- Logs the bot on to steam, must be connected via `connect`

### connect()
- Connects the bot to steam, use in config file to connect


# Properties

These are static properties of the chat bot.

### users
- Returns all of the users that the bot has interacted with on steam
- Object that matches the following:
    - [CMsgClientPersonaState.Friend](https://github.com/SteamRE/SteamKit/blob/master/Resources/Protobufs/steamclient/steammessages_clientserver.proto#L445)

### rooms
- Returns all of the chat rooms that the bot has interacted with
- Object that matches the following:
    - steamID of the chat
        - steamID of a user
            - rank: [EClanPermissions](https://github.com/SteamRE/SteamKit/blob/master/Resources/SteamLanguage/enums.steamd#L115)
            - permissions: Bitset of values from [EClanPermissions](https://github.com/SteamRE/SteamKit/blob/master/Resources/SteamLanguage/enums.steamd#L115)
        - other users
    - other chat rooms

### friends
- Returns a value that matches a [EFriendRelationship](https://github.com/SteamRE/SteamKit/blob/master/Resources/SteamLanguage/enums.steamd#L66) with a steamID
- Can be accessed by `chatBot.friends[steamID]`

### groups
- Returns a value that matches a [EClanRelationship](https://github.com/SteamRE/SteamKit/blob/master/Resources/SteamLanguage/enums.steamd#L366) with a groupID
- Can be accessed by `chatBot.groups[groupID]`


# Methods

These are all methods that can make the bot do something.

### mute()
- Mutes the bot so it can't talk in a chat room

### unmute()
- Reverses `mute` so the bot can talk again

### sendMessage(steamID, message)
- Sends a message over steam
- steamID is a steamid64 string
- message is a message string

### joinGame(appID)/setGames(appIDs)/setPrimaryGame(appID)
- Plays a game on steam
- appID is an appID as an integer (http://store.steampowered.com/app/appID)

### send(header, body, [callback])
- Sends a protobuf message
- Header is an object that matches the following:
    - msg: [Steam EMsg](https://github.com/SteamRE/SteamKit/blob/master/Resources/SteamLanguage/emsg.steamd)
    - proto: [CMsgProtoBufHeader](https://github.com/SteamRE/SteamKit/blob/master/Resources/Protobufs/steamclient/steammessages_base.proto#L11)
- Body is a buffer containing the message

### joinChat(roomID, autoJoinAfterDisconnect)
- Joins a room 
- roomID is the steamID of the chat room
- autoJoinAfterDisconnect is a boolean that determines whether your bot will rejoin if it's disconnected

### leaveChat(roomID)
- Leaves a chat room
- roomID is the steamID of the chat room

### addFriend(steamID)
- Sends a friend request on steam
- steamID is the steamID of the user you want to add

### removeFriend (steamID)
- Removes a friend on steam
- steamID is the steamID of the user you want to remove

### setPersonaName(name)
- Sets your display name on steam
- name is the string of your new name

### setPersonaState(state)
- Sets your persona state on steam
- State matches one of [Steam.EPersonaState](https://github.com/SteamRE/SteamKit/blob/master/Resources/SteamLanguage/enums.steamd#L34)

### lockChat(roomID)
- Locks the chat
- roomID is the steamID of the chat room

### unlockChat(roomID)
- Unlocks the chat
- roomID is the steamID of the chat room

### setModerated(roomID) 
- Makes the chat room moderated
- roomID is the steamID of the chat room

### setUnmoderated(roomID)
- Makes the chat room unmoderated
- roomID is the steamID of the chat room

### kick(roomID, steamID)
- Kicks a user from a chat room
- roomID is the steamID of the chat room
- steamID is the steamID of the user that you want to kick

### ban(roomID, steamID)
- Bans a user from a chat room
- roomID is the steamID of the chat room
- steamID is the steamID of the user that you want to ban

### unban(roomID, steamID)
- Unbans a user from a chat room
- roomID is the steamID of the chat room
- steamID is the steamID of the user that you want to unban

### logOff()
- Logs the bot off from steam

### chatInvite(chatID, steamID)
- Invites a user to chat
- chatID is the steamID of the chat
- steamID is the steamID of the user you want to invite

### getSteamLevel(steamIDs)
- Returns the level of the users in the array of steamIDs
- Object that matches the following:
    - steamID: level

### setIgnoreFriend(steamID, setIgnore)
- Blocks a friend on steam
- steamID is the steamID of the user
- setIgnore is a boolean of whether you want to block or unblock (true is block, false is unblock)

### trade(steamID)
- Invites a user to trade
- steamID is the user you wish to trade with

### respondToTrade(tradeID, respond)
- Accepts or declines a trade
- tradeID is the steamID of the trade (not the user)
- request is a boolean, true is accept, false is declines

### cancelTrade(steamID)
- Cancels a trade request
- steamID is the user you wish to cancel

### steamApi(interface, method, version, request, options)
- Sends an HTTP request to the steam API
- interface is the interface name (I something, ex: 'ISteamUser')
- method is the method, ex: 'GetPlayerDetails'
- version is the version number, omit the leading 0s, ex: 1
- request is the type of request, either 'get' or 'post'
- options is an object, will be automatically serialized, matches the following:
    - json: 'true',
    - steamids: steamID
- Returns a promise, call .then to use it  


That's it for the chatBot.js methods and properties. Now that you know how chatBot works internally, lets look at creating triggers.. The template for creating a trigger is as follows:

```javascript
var util = require("util");
var BaseTrigger = require("./baseTrigger.js").BaseTrigger;

var NewTrigger = function() {
	NewTrigger.super_.apply(this, arguments);
};

util.inherits(NewTrigger, BaseTrigger);

var type = "NewTrigger";
exports.triggerType = type;
exports.create = function(name, chatBot, options) {
	var trigger = new NewTrigger(type, name, chatBot, options);
        /*
         * Create your own options here, heres an example, this will determine whether it respects chatBot.mute
         * View baseTrigger.js for a whole list of them, you can also define your own to be used in the trigger
         */
        trigger.options.respectsMute = trigger.options.respectsMute || false
	return trigger;
};

NewTrigger.prototype._respondToChatMessage = function(roomId, chatterId, message) { //responds to a group chat message
	return this._respond(roomId, chatterId, message);
}

NewTrigger.prototype._respondToFriendMessage = function(userId, message) { //responds to a friend message
       return this._respond(userId, userId, message);
}

NewTrigger.prototype._respond = function(toId, userId, message) {
    //Make the magic happen, this is the main method of your trigger
    //You are going to want to use returns in here to make the trigger stop when it needs to
    //Use this/that._sendMessageAfterDelay(toId, whatevermessage); to send a message to the id of toId
}
```

That's it for the guide. If you need help, please feel free to join our [chatbot development chat (aka botopia)](http://steamcommunity.com/groups/steam-chat-bot). Thanks for reading, and have fun developing your own triggers!