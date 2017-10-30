CARLA Simulator
===============

Welcome to CARLA simulator.

This file contains the instructions to run the CARLA simulator binaries on
Linux.

Running the Python client
-------------------------

Requires Python 3 and the protobuf module installed, saving images to disk
requires the PIL module too.

    $ sudo apt-get install python3 python3-pip
    $ sudo pip3 install protobuf

A sample Python script is provided at `PythonClient/client_example.py`. The
script is well commented explaining how to use the client API.

The script can be run and provides basic functionality for controlling the
vehicle and saving images to disk. Run the help command to see options available

    $ ./client_example.py --help

A second Python script is provided at `PythonClient/client_manual_control.py`. The
script is pygame dependent and servers as an interactive example where the user controls
the car with a keyboard Run the help command to see options available

    $ ./client_manual_control.py --help

Running the server
------------------

IMPORTANT: By default the game starts in networking mode. It will hang until a
client is connected. See below how to run it without client.

The server can be started by running the `CarlaUE4.sh` script. When run in
networking mode (controlled by the CARLA client), it is highly recommended to
run it at fixed time-step

    $ ./CarlaUE4.sh /Game/Maps/Town01 -benchmark -fps=15

The arguments `-benchmark -fps=15` make the engine run at a fixed time-step of
1/15 seconds. In this mode, game-time decouples from real-time and the
simulation runs as fast as possible.

To run the game on the second city, just change the command to select the
"Town02"

    $ ./CarlaUE4.sh /Game/Maps/Town02 -benchmark -fps=15

To run the game windowed at a given resolution

    $ ./CarlaUE4.sh /Game/Maps/Town01 -benchmark -fps=15 -windowed -ResX=800 -ResY=600

The game can also be run without a client, this way you can drive around the
city just using the keyboard (in this mode is not recommended to use fixed
frame-rate to get a more realistic feeling)

    $ ./CarlaUE4.sh /Game/Maps/Town02 -carla-no-networking

#### CARLA command-line options

  * `-carla-settings=<ini-file-path>` Load settings from the given INI file. See Example.CarlaSettings.ini.
  * `-carla-world-port=<port-number>` Listen for client connections at <port-number>, agent ports are set to <port-number>+1 and <port-number>+2 respectively. Activates networking.
  * `-carla-no-networking` Disable networking. Overrides other settings.
  * `-carla-no-hud` Do not display the HUD by default.

#### In-game controls

The following key bindings are available during game play at the server window.
Note that vehicle controls are only available when networking is disabled.

    W            : throttle
    S            : brake
    AD           : steer
    Q            : toggle reverse
    Space        : hand-brake

    P            : toggle autopilot

    Arrow keys   : move camera
    PgUp PgDn    : zoom in and out
    mouse wheel  : zoom in and out
    Tab          : toggle on-board camera

    R            : restart level
    G            : toggle HUD
    C            : change weather/lighting

    Enter        : jump
    F            : use the force

Settings
--------

CARLA reads its settings from a "CarlaSettings.ini" file. This file controls
most aspects of the simulator, and it is loaded every time a new episode is
started (every time the level is loaded).

Settings are loaded following the next hierarchy, with values later in the
hierarchy overriding earlier values.

  1. `{ProjectFolder}/Config/CarlaSettings.ini`
  2. File provided by command-line argument `-carla-settings=<path-to-ini-file>`
  3. Other command-line arguments as `-world-port`, or `-carla-no-networking`
  4. Settings file sent by the client on every new episode.

Take a look at the Example.CarlaSettings.ini file for further details.

#### Weather presets

The weather and lighting conditions can be chosen from a set of predefined
settings. To select one, set the `WeatherId` key in CarlaSettings.ini. The
following presets are available

  * 0 - Default
  * 1 - ClearNoon
  * 2 - CloudyNoon
  * 3 - WetNoon
  * 4 - WetCloudyNoon
  * 5 - MidRainyNoon
  * 6 - HardRainNoon
  * 7 - SoftRainNoon
  * 8 - ClearSunset
  * 9 - CloudySunset
  * 10 - WetSunset
  * 11 - WetCloudySunset
  * 12 - MidRainSunset
  * 13 - HardRainSunset
  * 14 - SoftRainSunset

E.g., to choose the weather to be hard-rain at noon, add to CarlaSettings.ini

```
[CARLA/LevelSettings]
WeatherId=6
```