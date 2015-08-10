Here is the page where all of the internal chatBot.js function documentation will be stored. This page will also show the template for creating your own trigger. Event handlers (functions that start with a `_` will not be listed). Anything having to do with a trigger will not be listed. An output of `None` means that the method doesn't return anything.

Here are the internal methods that SHOULD NOT be used outside of chatbot (only use connect in config file, NEVER use logOn unless you want the bot to crash).

| Method name | Parameters | Usage | Output 
--------------|------------|-------|-------
| logOn       | None       | Logs the bot on to steam, won't work if bot isn't connected via chatBot.connect | None
| connect     | None       | Connects the bot to steam | None

That's really it for internal methods. Now listed are the methods that can be used in triggers via `this/that.chatBot.<method>`.

| Method name | Parameters | Usage | Output
--------------|------------|-------|-------
mute          | None       | Mutes the bot so it won't talk in a group chat | None
unmute        | None       | Unmutes the bot from a muted state so it can talk | None
sendMessage   | steamID64, message | Sends a message to steamID64 over steam | None
joinGame, setGames, setPrimaryGame      | appID      | Plays appID provided the bot has it in its library | None
send          | Header object (steam EMsg, protobuf object), body buffer, optional callback | Sends a protobuf message to steam | None
joinChat      | roomID, autoJoinAfterDisconnect | Joins roomID and reconnects after autoJoinAfterDisconnect ms | None
leaveChat     | roomID | Leaves chat roomID | None
addFriend     | steamID | Adds steamID on steam | None
removeFriend | steamID | Removes steamID on steam | None
setPersonaName | name | Sets your name to name | None
setPersonaState | state (int) | Sets your personastate (online, away, etc) to state | None
lockChat | roomID | Locks the room so no one can leave/talk? | None
unlockChat | roomID | Unlocks the room so people can leave/talk again? | None
setModerated | roomID | Sets the chat to "moderated" | None
setUnmoderated | roomID | Removes the "moderated" state | None
kick | roomID, steamID | Kicks steamID from roomID | None
ban | roomID, steamID | Bans steamID from roomID | None
unban | roomID, steamID | Unbans steamID from roomID | None
users | None | Returns all the users that the bot has interacted with | List of users
rooms | None | Returns all the chat rooms that the bot has interacted with | List of chat rooms
friends | None | Returns all of the bot's friends | List of friends
groups | None | Returns all of the bot's groups | List of groups
logOff | None | Logs the bot off of steam | None
chatInvite | chatID, steamID | Invites steamID to chatID | None
getSteamLevel | Array of steamIDs | Gets the steam level of steamIDs | Levels object 
setIgnoreFriend | steamID, setIgnore bool | Blocks(?) steamID | None
trade | steamID | Invites steamID to a trade | None
respondToTrade | tradeID, bool | bool ? accept trade from user : deny trade (tradeID should ALWAYS be from chatBot.steamTrading.on('tradePropsed', function(tradeID, steamID) { ... }); | None
cancelTrade | steamID | Cancels trade invite to steamID | None
steamApi | interface, method, version, request type, options object | Queries the steamapi (omit leading 0s in version | Promise


That's it for the chatBot.js methods. The template for creating a trigger is as follows:
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
         * View baseTrigger.js for a whole list of them, you can also define your own
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


That's it for the guide. If you need help, please feel free to join our [chatbot development chat (aka botopia)[http://steamcommunity.com/groups/steam-chat-bot]. Thanks for reading, and have fun developing your own triggers!