# VSP - Quake 3 ExcessivePlus Community version

VSP Stats Game Log Processor - Quake 3 ExcessivePlus Community version 0.45-xp-1.1.2, based on the VSP Stats Processor 0.45 by Myrddin.

Related GitHub repository:
<https://github.com/evilru/quake3-vsp-stats>.

## How To run the Docker Compose Stack

1. Copy **docker-compose.yml** and **.env** to your disk into the same folder.
2. Configure the variables in **.env** to suit your needs.
3. Run the docker stack.

```sh
docker-compose up -d
```

The configured games.log will be checked every 15 minutes for new games.

## Configuration

The following configuration options need to be set in the **.env**.

### Timezone

```txt
TIMEZONE="America/Los_Angeles"
```

### Logfile Mount

```txt
LOGPATH="C:\Docker\q3\osp\logs\games.log:/vsp/games.log"
```

### VSP & DB configuration

```txt
SQLUSER="someUserName"
SQLPASS="somePassword1234"
DBNAME="vsp"
```

#### Server Details

```txt
SERVERTITLE="Duff Brewery - OSP"
SERVERNAMEIP="q3.monroec.com:27960"
SERVERGAMEMOD="OSP"
SERVERADMINS="duffman91"
SERVEREMAILIM="@duffman91 on Discord"
WEBSITEADDRESS="http://q3.monroec.com"
WEBSITENAME="Duff Brewery"
SERVERQUOTE="...sup?"
DEFAULTSKIN="evilsmurfs"
```

#### Additional Note
These are _slight_ modifications from the upstream project to suit my short term needs. 
  https://github.com/evilru/quake3-vsp-stats 
  https://hub.docker.com/r/evilru/quake3-vsp-stats