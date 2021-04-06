# VSP Stats Processor - ExcessivePlus Community version 0.45-xp-1.1.2

The Purpose of this repository is to fix php/mysql compatibility issues of the Quake 3 ExcessivePlus Community version 0.45-xp-1.1.2 which is based on the VSP Stats Processor 0.45 by Myrddin.

Right now, it works with:

* **PHP 7.4.16**
* **MySQL 8.0.23** / **MariaDB 10.5.9**

If you find a bug, you are welcome to contribute and keep this version in a working state.

To get more information about the ExcessivePlus Community version you might visit [goquake.com](https://www.goquake.com/).

**This version only supports Quake III Arena!**

## Usage

Follow the setup proceduere described in [readme.txt](./readme.txt) or run the provided [docker-compose.yml](./docker-compose.yml).

## Docker

https://hub.docker.com/r/evilru/quake3-vsp-stats

1. install docker
1. copy [docker-compose.yml](./docker-compose.yml) and [docker-compose.override.yml](./docker-compose.override.yml) to your disk into the same folder
1. configure the container (see below)
1. run the docker stack

```sh
docker-compose up -d
```

The configured games.log will be checked every 5 minutes for new games.

The following configuration options need to be done in the [docker-compose.yml](./docker-compose.yml).

### Logfile

Mount your games.log.  
Needs to be placed in the _volumes_ section of the _web_ service.

```yaml
    volumes:
        - /path/to/your/games.log:/vsp/games.log
```

### VSP configuration

The parser can be configured with the following ENV variabels. They need to be placed in _evnironment_ section of the _web_ service.

#### LOGTYPE

default: _q3a-osp_  
The parser supports the following values:

```txt
q3a              Quake 3 Arena
q3a-battle       Quake 3 Arena BattleMod
q3a-cpma         Quake 3 Arena CPMA (Promode)
q3a-freeze       Quake 3 Arena (U)FreezeTag etc.
q3a-lrctf        Quake 3 Arena Lokis Revenge CTF
q3a-osp          Quake 3 Arena OSP
q3a-ra3          Quake 3 Arena Rocket Arena 3
q3a-threewave    Quake 3 Arena Threewave
q3a-ut           Quake 3 Arena UrbanTerror
q3a-xp           Quake 3 Arena Excessive Plus
```

#### TABLE_PREFIX

default: _vsp\__  
The tableprefix used in the database

#### DB_HOSTNAME

default: _host.docker.internal_  
This is the internal hostname of the dockerhost, no need to change that

#### DB_NAME

default: _vsp_  
If not available, this database will be created on the first run.  
Should be the same as _MYSQL_DATABASE_.

#### DB_USERNAME

Username used to connect to the database.  
Should be the same as _MYSQL_USER_.

#### DB_PASSWORD

Password used to connect to the database.  
Should be the sames as _MYSQL_PASSWORD_.

#### VSP_WEB_PASSWORD

No default value, to enable this feature, set a password with at least 6 characters.

All VSP commands can be called from the commandline and from the browser. The docker image comes with the following [cronjob](./docker/import-cron).

The webinterface is available under the following url: http://yourserver.com/vsp.php.

### VSP Header configuration

The VSP Header can be configured with the following ENV variabels. They need to be placed in _evnironment_ section of the _web_ service.

#### SERVER_TITLE

default: _HERE GOES YOUR SERVER TITLE_

#### SERVER_NAME_IP

default: _Your Server Name and IP goes here_

#### SERVER_GAME_MOD

default: _Your Game and Mod type goes here_

#### SERVER_ADMINS

default: _List your admins here_

#### SERVER_EMAIL_IM

default: _List your E-Mail and/or IM account here_

#### WEB_SITE_ADDRESS

default: _http://my.web_site_goes_here.com_

#### WEB_SITE_NAME

default: _My web site name goes here_

#### SERVER_QUOTE

default: _My quote goes here_

#### Headerimages

Mount your own images.  
Needs to be placed in the _volumes_ section of the _web_ service.

```yaml
    volumes:
      - /path/to/your/server.gif:/vsp/pub/images/server.gif
      - /path/to/your/logo.gif:/vsp/pub/images/logo.gif
```

## Extended configuration with multiple mods / servers

1. Create a copy of [pub/configs/cfg-default.php](./pub/configs/cfg-default.php), update the database configuration and set a different table_prefix. Mount it into the image.

```yaml
    volumes:
      - /path/to/your/cfg-ra3.php:/vsp/pub/configs/cfg-ra3.php
```

2. Mount your games.log

```yaml
    volumes:
        - /path/to/your/ra3.log:/vsp/ra3.log
```

3. Add a your configuration to the [import script](docker/import.sh)

```sh
php /vsp/vsp.php -c /vsp/pub/configs/cfg-ra3.php -l q3a-ra3 -p savestate 1 ra3.log
```

4. Call it from the browser

http://yourserver.com/pub/themes/bismarck/index.php?config=cfg-ra3.php

---

## From the VSP Readme

```txt
================================================================================
vsp stats processor - vsp(c) 2004-2005 by myrddin(myrddin8 <AT> gmail <DOT> com)
================================================================================

vsp stats processor is a multi game - log analyzer/stats generator/log parser. 
It can process log files from several games including Quake 3 Arena (q3a), 
Halflife 1, 2, Counter Strike Source (hl), Wolfenstein: Enemy Territory (wet), 
Return to Castle Wolfenstein (rtcw), Call Of Duty, COD: United Offensive (cod), 
Medal of Honor Allied Assault, MOHAA: Spearhead, MOHAA: BreakThrough (moh), 
Soldier of Fortune 2 (sof2), etc. It reads the log files generated by a game 
and displays various game statistics. vsp also supports both server logs (ex:- 
games.log for Q3A) and client logs (ex:- qconsole.log for Q3A).
```

For more details I suggest to take a look at the readme files:

* [readme.txt](./readme.txt)
* [readme-xp.txt](./readme-xp.txt)

## Credits

```txt
vsp 0.45: 
--------
             Lead Programming/Design : myrddin (myrddin8 <AT> gmail <DOT> com)
                         Programming : react
                             Hosting : gouki
                             Website : http://www.clanavl.com/vsp/
                                       http://www.clanavl.com
                                 Irc : #vsp on irc.enterthegame.com

xp 1.0:
------
            WaspBeast - http://forums.excessiveplus.net/profile.php?mode=viewprofile&u=13161
```
