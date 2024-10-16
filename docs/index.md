# How Battlefield 2 Hooks Python


# BF2 Engine Interaction with Python

## Overview
This section describes how the BF2 engine interacts with the embedded Python interpreter, detailing the initialization process, module imports, and event handling.

## Python Initialization
When the BF2 engine starts, it initializes its embedded Python interpreter with its `sys.path` set to include the following directories:

1. **Python Standard Library**:
   - Located in the file: `Battlefield 2 Server/pylib-2.3.4.zip`
   
2. **Battlefield 2 Server Python Directory**:
   - The directory: `Battlefield 2 Server/python`
   
3. **Python Directory of the Current Mod**:
   - Example: `Battlefield 2 Server/mods/bf2/python`
   - If the server switches mods, this path will change accordingly. Note that the BF2 game engine does not switch mods automatically between rounds. A mod remains loaded until the server is manually stopped, reconfigured, and restarted.

4. **Admin Directory**:
   - The directory: `Battlefield 2 Server/admin`

## Importing the `bf2` Package
The BF2 engine imports the `bf2` package, which is located in the `Battlefield 2 Server/python` directory. During this process:

- The `__init__.py` module in the `bf2` package is executed. 
- This module imports and instantiates the basic code and objects required for the game, including `game.scoringCommon` from the active mod’s Python directory (this is the only mod-specific Python code that gets pulled in).

## Engine Initialization Call
The BF2 engine calls the `bf2.init_module()` method:

- This method imports the stats modules.
- The reason for this separation (instead of putting everything in the `__init__` module) is unclear.

## Configuration File Processing
The BF2 engine processes the configuration file located at `Battlefield 2 Server/mods/bf2/settings/ServerSettings.con`. 

- By default, this file contains the directive: `sv.adminScript "default"`. 
- This directive causes the default module in the `Battlefield 2 Server/admin` directory to be imported. 
  - This module primarily handles the RCon interface but includes an `init()` function that, when called, imports the `admin.standard_admin` package.
  - This package handles features such as team autobalancing and team-kill punishment. (See the [Admin Module](#admin-module) for more details.)

## Event Registration During Startup
Throughout the startup process, as packages and modules are imported into the embedded Python interpreter, they register themselves to receive callbacks when specific events occur in the BF2 engine.

### Pre-Game Initialization
Some modules require initialization before each round begins. These modules:

- Register to receive callbacks for `GameStatusChangedEvents`.
- When the callback indicates that the game status is “PreGame,” the modules execute their initialization code.

## Event-Driven Execution During Gameplay
Once the setup is complete and the game is running, the initialized Python code operates in an event-driven mode:

- The game engine dispatches execution to Python handlers when registered events occur. 
  - For instance, when a player is killed, a `PlayerKilled` event is fired.
  - Any modules that registered to receive callbacks for this event (e.g., a scoring module) are triggered.

## End-of-Round Behavior
When a game round ends:

- The game engine does **not** restart, and neither does the embedded Python interpreter. The `bf2` packages are **not** reimported or reinitialized.
- Modules needing end-of-round cleanup:
  - Register to receive callbacks for `GameStatusChangedEvents`.
  - When the game status changes to “EndGame,” these modules perform their cleanup activities.

The game engine then returns to the “PreGame” state, and pre-game initialization callback handlers are called to start the cycle again.





## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.
