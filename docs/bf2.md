


## Game Status
 

###   Player
 class bf2.PlayerManager.Player

    An object of this class is created for each player in the game. When they are initialized, Player objects automatically instantiate a PlayerScore object and assign it to their score attribute.

    You must import bf2.PlayerManager if you wish to create objects of this class; normally, however, you will just access methods and attributes of already existing Player objects that are returned to you by other calls, which requires no special import statement.
    Instance creation

    x = bf2.PlayerManager.Player(index)

    index

        playerID for this player.

    score

        Current score for this player.

    isValid()

    isRemote()

    isAIPlayer()

    isAlive()

    isManDown()

    isConnected()

    getProfileId()

    isFlagHolder()

    getTeam()

    setTeam(t)

    getPing()

        Returns:

            The player’s ping (network transit time from player to server and back) in milliseconds

    getSuicide()

        Returns:

            1 if the player suicided (resets once the player spawns)

    setSuicide(t)

    getTimeToSpawn()

        Returns:

            The number of seconds until they are allowed to spawn
        Returns:

            0 when a player is spawned in; when a player is waiting to spawn

    setTimeToSpawn(t)

        Appears to generate an exception if used on a player that is already spawned in; if used on a player that that is waiting to spawn it changes the time until they are allowed to spawn.

    getSquadId()

        Squads for each team are independently numbered beginning at 1 and increasing thereafter. Players not on a squad, including team commanders, are assigned to squad 0.

        Returns:

            The player’s squad ID

    isSquadLeader()

        Returns:

            1 if player is a squad leader

    isCommander()

        Returns:

            1 if player is currently the commander

    getName()

        Returns:

            Player’s name

    setName(name)

        Sets a player’s name (at least, it changes what getName() returns), but the change doesn’t show up in-game–everything in the game still shows the player’s old name.

        It is working, but only sees that player who connected after the name change.

    getSpawnGroup()

    setSpawnGroup(t)

    getKit()

        Returns:

            The current player’s kit object

    getVehicle()

        Returns:

            The current player’s vehicle object
        Returns:

            The player’s soldier object if the player is not in a vehicle at the time

    getDefaultVehicle()

        Returns:

            The player’s soldier object, no matter what vehicle they are in

    getPrimaryWeapon()

        Returns:

            The weapon object for the player’s currently selected weapon

    getAddress()

        Returns:

            The player’s IP address

    setIsInsideCP(val)

    getIsInsideCP()


###  Player Manager
class bf2.PlayerManager

    This class is a wrapper around some player management functions in the BF2 engine, and also adds some simple calculations and logic to those functions. During its initialization the bf2 class instantiates this class as the singleton object bf2.playerManager.

    You must import bf2 to access the methods and attributes of this object.

    getNumberOfPlayers()

    getCommander(team)

    getPlayers()

    getPlayerByIndex(index)

    getNextPlayer(index)

    getNumberOfPlayersInTeam(team)

    getNumberOfAlivePlayersInTeam(team)

    enableScoreEvents()

        Enables PlayerScore events

    disableScoreEvents()

        Disables PlayerScore events


### Timer
class bf2.Timer

    (Not available in PR, use the realitytimer below.)

    Objects in this class are timers that can cause timer events to be generated when a fixed amount of time has elapsed.

    bf2.Timer(timerEventHandler, delta, alwaysTrigger, data)

            Note that even though data is optional when establishing a timer, the timerEventHandler must specify it as a parameter, or the handler won’t work.

            You must import bf2.Timer to create timer objects.

        Parameters:

                timerEventHandler – Handler to be called when delta seconds have elapsed since the creation of the timer.

                alwaysTrigger – Should be 1 (not sure what the alternative is)

                data – Optional item (typically a tuple) that will be passed to timerEventHandler.

    destroy()

        Destroys the associated game engine timer (but not the Python instance.)

    getTime()

        Returns:

            The wall time at which this timer will fire

    setTime(time)

        Changes the wall time at which this timer will fire.

    setRecurring(interval)

        Specifies this this timer should fire repeatedly, every interval seconds.

    onTrigger()

        For internal use only; calls timerEventHandler.

class realitytimer.py

    Project Reality timer (realitytimer.py) expands the default interface with the following:

        Exception catching with a debug message when an exception is not caught in the handler

            No need to worry about bad code crashing the server.

        Internal check to make sure timers don’t fire after bf2.Timer.destroy() was called

            Can happen when bf2.Timer.destroy() is called on the same tick

    fireOnce(targetFunc, delay, data=None)

        Class that can fire an event once after delay and then destroy itself. No need to store reference.

    fireNextTick(targetFunc, data=None)

        Class that will fire the event at the next game tick and then destroy itself. No need to store reference




### Game Status
    class bf2.GameStatus

    Playing

    EndGame

    PreGame

    Paused

    RestartServer

    NotConnected
Just used as a container for the constant values returned by callbacks from the GameStatusChanged event


### Game Logic

    class bf2.GameLogic.GameLogic
    
    methods :

    getModDir()

    getMapName()

    getWorldSize()

    getTeamName(team)

    isAIGame()

    sendClientCommand(playerId, command, args)

    sendGameEvent(playerObject, event, data)

    sendMedalEvent(playerObject, type, value)

    sendRankEvent(playerObject, rank, score)

    sendHudEvent(playerObject, event, data)

    sendServerMessage(playerId, message)

    getTicketState(team)

    setTicketState(team, value)

    getTickets(team)

    setTickets(team, value)

    getDefaultTickets(team)

    getTicketChangePerSecond(team)

    setTicketChangePerSecond(team, value)

    getTicketLimit(team, id)

    setTicketLimit(team, id, value)

    getDefaultTicketLossPerMin(team)

    getDefaultTicketLossAtEndPerMin()

    getWinner()

    getVictoryType()

    setHealPointLimit(value)

    setRepairPointLimit(value)

    setGiveAmmoPointLimit(value)

    setTeamDamagePointLimit(value)

    setTeamVehicleDamagePointLimit(value)
    
A wrapper around lots of BF2 engine stuff accessible through host, apparently to make it more Pythonic. During its initialization the bf2 class instantiates this class as the singleton object bf2.gameLogic.
You must import bf2 to access the methods and attributes of this object.


### Server Settings

Another wrapper around more BF2 engine stuff that’s accessible through host; this class makes it accessing these things cleaner and more Pythonic. During its initialization the bf2 class instantiates this class as the singleton object bf2.serverSettings.

    class bf2.GameLogic.ServerSettings

   
    You must import bf2 to access the methods and attributes of this object.

    getTicketRatio()

    getTeamRatioPercent()

    getMaxPlayers()

    getGameMode()

    getMapName()

    getTimeLimit()

    getScoreLimit()

    getAutoBalanceTeam()

    getTKPunishEnabled()

    getTKNumPunishToKick()

    getTKPunishByDefault()

    getUseGlobalRank()

    getUseGlobalUnlocks()

You cannot find out server name, port and other similar information this way. Instead, use f.e. host.rcon_invoke('sv.serverName') to get the server name.


### ObjectManager

 class bf2.ObjectManager.ObjectManager

    getObjectsOfType(object type)

    getObjectsOfTemplate(object template)

During its initialization the bf2 class instantiates this class as the singleton object bf2.objectManager. This object can be used by Python to get access to internal game engine C++ objects. A list of the available object types can be found here.

You must import bf2 to access the methods and attributes of this object.



### Player Manager

    class bf2.PlayerManager.PlayerScore

Objects of this class maintain a long list of player score attributes. They are used inside of objects of the Player class; for any Player object x, x.score is an object of class PlayerScore.

You will not normally create objects of this class; they are created automatically as part of the Player class when Player objects are created. No special imports are necessary to access methods and attributes of these objects.

There is also a separate player.stats object, which tracks different information. You can use the following code fragment to see all of the variables in the player.stats object.

    for s in vars(player.stats):
       print str(s)

    reset()

        Resets all score attributes stored within the object itself.

    index

    heals

    ammos

    repairs

    damageAssists

    passengerAssists

    driverAssists

    targetAssists

    driverSpecials

    revives

    teamDamages

    teamVehicleDamages

    cpCaptures

    cpDefends

    cpAssists

    suicides

    cpNeutralizes

    cpNeutralizeAssists

    rplScore

        This attribute may not be in all versions of BF2

    skillScore

    cmdScore

   

Class attributes stored in the BF2 engine
                deaths,
                kills,
                TKs,score,
                skillScore,
                rplScore,
                cmdScore,
                fracScore,
                rank,
                firstPlace,
                secondPlace,
                thirdPlace,
                bulletsFired

Gives a tuple, each element of which is a 2-tuple consisting of the name of a weapon the player has fired, and the number of shots they fired from that weapon. As the player uses more weapons, more of the 2-tuples are added to the list. An example tuple returned:

(("uspi-m16", 30), ("knife", 3))

Before the first weapon is fired, this may be None or an empty tuple. The first weapon fired will not always be the first 2-tuple on the list returned.

bulletsGivingDamage

Same as above, but only with bullets giving damage

bulletsFiredAndClear

The “AndClear” resets the engine counter. polling this will only give new bullets. However having more than one module polling them is not a good idea.

bulletsGivingDamageAndClear

dkRatio



### Trigger Manager
    class bf2.TriggerManager.TriggerManager

This class is a wraper around some player management functions in the BF2 engine. During its initialization the bf2 class instantiates this class as the singleton object bf2.triggerManager. This object is used to manage “triggers”, which are events that are fired when a PCO enters a defined spherical or hemispherical volume surrounding an object.

 You must import bf2 to access the methods and attributes of this object.

    createRadiusTrigger(object, callback, objName, radius, data=None)

Creates a trigger that causes callback to be called if a player enters a spherical region of radius radius centered on object, passing data as an argument.

    createHemiSphericalTrigger(object, callback, objName, radius, data=None)

Same as createRadiusTrigger(), except that instead of a spherical trigger region, the trigger region is a flat circle lying along the ground (yes, it’s badly named).

    destroyAllTriggers()

        Destroys all registered triggers.

    destroy(trig_id)

        Destroys a specific trigger.

    getObjects(trig_id)

        Returns:

            A tuple containing all objects currently within the specified trigger region


### constants

    class bf2.stats.constants

This module appears intended to be imported with something like…

from bf2.stats.constants import *

so that everything in it is loaded into the local namespace, rather than being a part of any object. The module includes a lot of constants and dictionaries, as well as some utility functions.

    getVehicleType(templateName)

    getWeaponType(templateName)

    getKitType(templateName)

    getArmy(templateName)

    getMapId(mapName)

    getGameModeId(gameMode)

    getRootParent(physicalObject)

        Traverses the containment for physicalObject all the way to the top