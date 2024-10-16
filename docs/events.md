# Events in BF2 Engine

## Overview
The interaction between the BF2 engine and Python programs is largely event-driven. This means that when an event happens in a game, the BF2 engine checks if any Python programs have registered to be notified of that specific event. If they have, the programs are called (this is known as a "callback"). 

You can think of events as “hooks” into the game engine where you can attach your own Python code.

### How Events Work
- When an event occurs, the BF2 engine searches for any Python programs that have registered for that event.
- If registered callbacks exist, they are executed when the event happens.
- Multiple callbacks can be registered for a single event, meaning all registered callbacks will be executed sequentially when the event occurs.

### Example: PlayerConnect Event
For instance, if three different programs have registered for the “PlayerConnect” event, then when a new player connects, all three of those programs will be run. 

This allows for flexibility and modularity in how the game responds to various events.

### Registering Events
Python programs can register for specific events using the BF2 engine’s event system. By doing this, you can customize and extend the behavior of the game.

- **Benefits**:
  - Modular code: Different Python modules can independently handle different aspects of game behavior.
  - Flexibility: Multiple modules can respond to the same event, allowing for complex interactions and features.

### Summary
Events serve as important hooks into the BF2 engine, providing opportunities to extend and customize gameplay through Python callbacks. By understanding and utilizing these events, developers can create modular, responsive, and interactive game elements.



## Types of Events

### Standard Events

These are events that occur during the normal course of gameplay

   ```host.registerHandler('EventName', pythonEventHandler[, parameter])```

This registration will typically be done in the init() method of an object, but could be done anywhere in your code.



### Game Status Events

The BF2 engine generates these events when the status of the game changes.
You register to receive callbacks for these events

   `host.registerGameStatusHandler(pythonEventHandler)`

The possible states game status can be in (as enumerated in bf2.GameStatus) are:

    NotConnected

    PreGame

    Playing

    Paused

    EndGame

    RestartServer

Game status events are usually used to kick off pre-game initialization or post-game clean-up processing.



### Trigger Events

“Trigger” events activate when a vehicle comes within a specified distance of an object.
Triggers are created and registered for by” bf2.triggerManager.

    createHemiSphericalTrigger(object, triggerEventHandler, objectName, radius[, data])
    
    bf2.triggerManager.createRadiusTrigger(object, triggerEventHandler, objectName, radius[, data])

In standard Battlefield 2, trigger events are used to track scoring for flag captures (triggers activate whenever a player enters or leaves a fixed radius around each control point), as well as effects such as birds (birds are triggered to take flight whenever a player enter their trigger radius).


### Timer Events
    impimport game.realitytimer as rtimer

    rtimer.Timer(timerEventHandler, delta, alwaysTrigger,data)

Timer events activate when a certain amount of time has elapsed.
In standard Battlefield 2, timer events are used to cause periodic status checks related to scoring, ranking, punishing TKs, etc.
See the description of the bf2.Timer class for details.



### Descriptions of Events¶

EA/DICE have not published a list of events, but here is a list of events that have been discovered so far.

    host.EventName(handlerParameter1, handlerParameter2, ...)

The EventName is the name of the event to register for with host. registerHandler; the parameters are the parameters your handler should receive when called.

Parameters:

playerObject – An instance of bf2.PlayerManager.Player

playerID – As players connect to the server they are assigned playerID numbers from “1” up
squadID (int) – Squads for each side are independently numbered beginning at “1” and increasing thereafter. Players not on a squad, including team commanders, are assigned to squad “0”
 teamID (int) – 0 for US, 1 for China or MEC



## Player Events






### Player Connect
    
    host.PlayerConnect(playerObject)

Fires when a player connects to the game.


### Player Spawn

    
    host.PlayerSpawn(playerObject, soldierObject)¶

Occurs when a player spawns, and associates a playerObject “spirit” with a soldierObject “body”.


### Player Change Teams

    
    
    host.PlayerChangeTeams(playerObject, humanHasSpawned)

Parameters:

humanHasSpawned – 1 if the player the event is firing for is a human (as opposed to an AI). Does not fire due to team change from “setTeam”!





### Player Chang eWeapon

    
    host.PlayerChangeWeapon(playerObject, oldWeaponObject, newWeaponObject)¶

Player has changed which weapon they are holding.


### Player Changed Squad

    
  
  
    host.PlayerChangedSquad(playerObject, oldSquadID, newSquadID)

SquadID is 0 for players who are not members of any squad (including the team commander).




### Player Score

    
    host.PlayerScore(playerObject, difference)¶

Difference is change to player’s score (positive or negative).


### Player Heal Point

    
    host.PlayerHealPoint(givingPlayerObject, receivingSoldierObject)

Occurs whenever a player heals another player; there is no indication of how much health was restored.


### Player Repair Point

    

 
    host.PlayerRepairPoint(givingPlayerObject, receivingVehicleObject)

Occurs whenever a player repairs a vehicle; there is no indication of how much damage was repaired.




### Player Give Ammo Point

   
    host.PlayerGiveAmmoPoint(givingPlayerObject, receivingPhysicalObject)

Occurs whenever a player replenishes the ammo in another player or vehicle. 
There is no indication of how much ammo was replenished.


### Player Team Damage Point

   
    host.PlayerTeamDamagePoint(playerObject, victimSoldierObject)¶

Occurs whenever a player damages another player on their own team.
There is no indication of how much damage was done.

### Player Killed

    
    host.PlayerKilled(victimPlayerObject, attackerPlayerObject, weaponObject, assists, victimSoldierObject)

This event occurs when a player is “critically injured”, but still capable of being revived.
If any players assisted in the kill,
then “assists” will be a tuple of tuples: the top-level tuple will contain one lower-level tuple for each assisting player; the lower-level tuples will each have two elements:
a playerObject for one of the assisting players, and a number indicating the type of assist
(1=targeting assist, 2=kill damage assist, 3=driver assist).

### Player Revived



    host.PlayerRevived(revivedPlayerObject, medicPlayerObject)

Player has been revived from a “critically injured” condition to full health by a medic using “shock paddles”. Players can only be revived in the interval between a PlayerKilled() event and a subsequent PlayerDeath() event.

### Player Death


    host.PlayerDeath(playerObject, soldierObject)

This event occurs when a player is decisively dead, and cannot be revived; they will only return to the game by respawning. soldierObject is the soldier “body” (which is now discarded) for the playerObject “spirit” (which will persist through the next spawn).


### Player Banned

     host.PlayerBanned(playerObject, time, type)¶

### Player Kicked
    host.PlayerKicked(playerObject)

    Player has been kicked off the server.


### Player Disconnect
    host.PlayerDisconnect(playerObject)

    Player has disconnected from the server.


## Vehicle and Kit Events

### Enter Vehicle

    host.EnterVehicle(playerObject, vehicleObject[, freeSoldier])

Player represented by playerObject has entered the vehicle represented by vehicleObject. freeSoldier is 1 if the player is a passenger of the vehicle and is able to use weapons/objects of his kit. i.e. seat 4(+) of a black hawk.

### Exit Vehicle

    host.ExitVehicle(playerObject, vehicleObject)

Player has exited a vehicle.
    
### Vehicle Destroyed

    host.VehicleDestroyed(vehicleObject, attackerObject)

vehicleObject has been destroyed by attackerObject.
    
### Pickup Kit

    host.PickupKit(playerObject, kitObject)

Player has picked up a kit. This occurs both when a player physically picks up a kit, as well as when a player spawns.

### Drop Kit

    ost.DropKit(playerObject, kitObject)

Player has dropped a kit; can occur either because a player has died, or because a player has picked up a different kit.



## Team Events

###  Reset
    host.Reset(data)

TBD

### Changed Commander
    host.ChangedCommander(teamID, oldCommanderPlayerObject, newCommanderPlayerObject)

Occurs when a team changes commanders; the old or new commander may be “None”.

### Changed Squad Leader
    host.ChangedSquadLeader(squadID, oldLeaderPlayerObject, newLeaderPlayerObject)

 Occurs when a squad changes squad leaders; the old or new squad leader may be “None”.



## Game Events

### Control Point ChangedOwner
host.ControlPointChangedOwner(controlPointObject, attackingTeamID)

TBD

### Time Limit Reached
host.TimeLimitReached(value)

Event fires when the fixed time limit for a round expires (not sure what “value” is)

### Ticket Limit Reached
host.TicketLimitReached(team, limitID)

TBD



## Command Events

### Console Send Command
    host.ConsoleSendCommand(command, args)

This event is triggered by someone using the pythonHost.sendCommand console command. Unfortunately, this command/event mechanism appears to be disabled in non-ranked servers. (As a workaround, you can achieve a similar effect creating and registering your own custom RCon commands.

### Remote Command
    host.RemoteCommand(playerId, cmd)

Event fires when a game client gives an RCon (remote console) command. cmd is the command the player gave. This event is used by the admin module to receive and process RCon commands from players; RCon commands from TCP connections are received by the admin module directly, and do not fire this event.

### Client Command
    host.ClientCommand(command, issuerPlayerObject, args)

This event occurs when a game client issues certain in-game commands. The only such commands identified so far are: command=100 means player wants to punish teammate for teamkill; command=101 means player wants to forgive. In both cases, args=1. This response is generated by the player responding to an on-screen prompt caused by a call to the bf2.gameLogic.sendClientCommand method.



## Miscellaneous Events

### Player Unlocks Response
    host.PlayerUnlocksResponse(succeeded, player, unlocks)

Unlocks are requested and received asynchronously. This event is triggered when the response for player (which is an object of type Player) are received. If the ranking server did not successfully find the player in question, succeeded is set to false, else true. unlocks is a list of kit ids, which are 11, 22, 33 .. 77.

### Player Awards Response
host.PlayerAwardsResponse()

    TBD

###  Chat Message
    host.ChatMessage(playerId, text, channel, flags)

channel is Squad, Team, Global, ServerTeamMessage or ServerMessage. Text is the text of the message sent, prefixed by HUD_TEXT_CHAT_TEAM or HUD_TEXT_CHAT_SQUAD if the message is sent to the Team or Squad channel; there is no prefix if it is sent to the Global channel. playerId is -1 for all server messages. If the player sending the message is dead, their message is also prefixed by HUD_CHAT_DEADPREFIX. (Strings such as HUD_TEXT_CHAT_TEAM, HUD_CHAT_DEADPREFIX, and so on, are used as keys by the game engine to match and replace with localized text.)

### Player StatsResponse
    host.PlayerStatsResponse(succeeded, player, response)

TBD

### Update
    host.Update(data)

TBD



## Non-Standard Events

### Game Status Changed Event

    host.GameStatusChangedEvent(status)

This type of event is registered for with host.registerGameStatusHandler(). status is an integer status code enumerated by bf2.GameStatus.

### Trigger Event

    host.TriggerEvent(triggerID, object, vehicle, enter, userData)

This type of event is registered for with bf2.triggerManager.createHemiSphericalTrigger or bf2.triggerManager.createRadiusTrigger; trigger triggerID is activated when vehicle comes within the preset radius of object.

### Timer Event

    host.TimerEvent(data)

This type of event fires when a timer established with bf2.Timer expires. “data” is an optional piece of information (of any type) that can be set when the timer is established.


## Mysterious Events

### read me
These strings were found in the server executable, and appear to be internal game engine events. They seem to not be accessible from Python, except in a few cases, where there is a correspondence between these “events” and Python events (e.g. “BeginRoundEvent” may be the same as “BeginRound”). For more information, see BF2 Internals.

### Remove Spawn Group Event
    host.RemoveSpawnGroupEvent()

### Create Spawn Group Event
    host.CreateSpawnGroupEvent()

### Content Check Event
    host.ContentCheckEvent()

### UAV Event
    host.UAVEvent()

### Missile Init Event
    host.MissileInitEvent()

### Unlock Event
    host.UnlockEvent()

### Medal Event
    host.MedalEvent()

### Toggle Free Camera Event
    host.ToggleFreeCameraEvent()

### Voip Session Event
    host.VoipSessionEvent()

### Request Event
    host.RequestEvent()

### Python Command Event
    host.PythonCommandEvent()

### End Of Round Event
    host.EndOfRoundEvent()

### Begin Round Event
    host.BeginRoundEvent()

### Target Direction Event
    host.TargetDirectionEvent()

### Vote Event
    host.VoteEvent()

### Voip Player Mute Event
    host.VoipPlayerMuteEvent()

### Supply Drop Event
    host.SupplyDropEvent()

### Commander Cam Event
    host.CommanderCamEvent()

### Voip On/Off Event
    host.VoipOnOffEvent()

### Ambient Effect Area Event
    host.AmbientEffectAreaEvent()

### Sticky Projectile Event
    host.StickyProjectileEvent()

### Artillery Event
    host.ArtilleryEvent()

### Spotted Event
    host.SpottedEvent()

### Set Squad Leader Event
    host.SetSquadLeaderEvent()

### Set Accept Order Event
    host.SetAcceptOrderEvent()

### Kick/Ban Event
    host.KickBanEvent()

### Rank Event
    host.RankEvent()

### Invite Event
    host.InviteEvent()

### Issue Squad Order Event
    host.IssueSquadOrderEvent()

### Set Private Squad Event
    host.SetPrivateSquadEvent()

### Change Squad Name Event
    host.ChangeSquadNameEvent()

### Killed By Event
    host.KilledByEvent()

### Radio Message Event
    host.RadioMessageEvent()

### Commander Event
    host.CommanderEvent()

### Leave Squad Event
    host.LeaveSquadEvent()

### Join Squad Event
    host.JoinSquadEvent()

### String Block Event
    host.StringBlockEvent()

### Handle Drop Event
    host.HandleDropEvent()

### Handle Pickup Event
    host.HandlePickupEvent()

### Change Player Name Event
    host.ChangePlayerNameEvent()

### Post Remote Event
    host.PostRemoteEvent()

### Exit Vehicle Event
    host.ExitVehicleEvent()

### Enter Vehicle Event
    host.EnterVehicleEvent()

### Destroy Player Event
    host.DestroyPlayerEvent()

### Destroy Object Event
    host.DestroyObjectEvent()

### Create Kit Event
    host.CreateKitEvent()

### Create Object Event
    host.CreateObjectEvent()

### Create Player Event
    host.CreatePlayerEvent()

### Data Block Event
    host.DataBlockEvent()

### Connection Type Event
    host.ConnectionTypeEvent()

### Challenge Response Event
    host.ChallengeResponseEvent()

### Challenge Event
    host.ChallengeEvent()