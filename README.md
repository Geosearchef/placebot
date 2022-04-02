This is a fork for /r/placeDE of the oroginal bot located here: https://github.com/Geosearchef/placebot

to set the config file, you can either set the

`CONFIGURATION_FILE`  environment variable to the path to the config file  
append the path to the programm call as parameter  
or provide a file called `config.json`.

To run the programm you have to run `python src/placebot.py`  
Make sure, you have the dependencies installed.  

Pipenv and docker are not yet tested, so might not work!
  
Bot for the r/place event.
# placebot

This is a functional bot for the r/place 2022 event, supporting at the time of writing all (2) canvases.
This is a fork for /r/placeDE of the oroginal bot located here: https://github.com/Geosearchef/placebot

to set the config file, you can either set the

`CONFIGURATION_FILE`  environment variable to the path to the config file  
append the path to the programm call as parameter  
or provide a file called `config.json`.

To run the programm you have to run `python src/placebot.py`  
Make sure, you have the dependencies installed.  

Pipenv and docker are not yet tested, so might not work!
  
Bot for the r/place event.

Based on code from https://github.com/goatgoose/PlaceBot and https://github.com/rdeepak2002/reddit-place-script-2022.

## Functionality
- Convert input image to be drawn to target configuration, ignores transparent pixels
- Supports multiple accounts
- Supports obtaining the target configuration from a server (or local file) to prevent outdated templates
- Supports multiple canvases (2 at the time of writing)
- Goes to sleep when only few mismatched pixels remain

## Installation
Clone the repostory:
```
git clone https://github.com/Geosearchef/placebot.git
```

Create a virtual environment
```
python3 -m virtualenv .venv
```

and install the dependencies (or just install them globally):
```
python3 -m pip install -r requirements.txt
```

Source the environment (for bash):
```
source .venv/bin/activate
```
## Functionality
- does NOT use the reddit API
- Convert input image to be drawn to target configuration, ignores transparent pixels
- Supports multiple accounts
- Supports obtaining the target configuration from a server (or local file) to prevent outdated templates
- Supports multiple canvases (2 at the time of writing)
- Goes to sleep when only few mismatched pixels remain

## Installation
Clone the repostory:
```
git clone https://github.com/Geosearchef/placebot.git
```

Create a virtual environment
```
python3 -m virtualenv .venv
```

and install the dependencies (or just install them globally):
```
python3 -m pip install -r requirements.txt
```

The bot (run via placebot.py) can be configured using config.json. It logs into multiple accounts and pulls the target config from a server via http every 60 seconds.
Every 5 minutes (+5-25 secs), each account pulls the board and attempts to place a misplaced pixel.


## Docker-Compose

```yml
version: '2'
services:
  placebot:
    build: https://github.com/TheCataliasTNT2k/placebot.git#main
    restart: unless-stopped
    volumes:
     - ./config.json:/app/config.json
```
1. Add above as docker-compose.yml
2. Add `config.json` to the same folder
3. Run `docker-compose up -d
4. Profit.

Source the environment (for bash):
```
source .venv/bin/activate
```

The bot (run via placebot.py) can be configured using config.json. It logs into multiple accounts and pulls the target config from a server via http every 60 seconds.
Every 5 minutes (+5-25 secs), each account pulls the board and attempts to place a misplaced pixel.


## Docker-Compose

```yml
version: '2'
services:
  placebot:
    build: https://github.com/TheCataliasTNT2k/placebot.git#main
    restart: unless-stopped
    volumes:
     - ./config.json:/app/config.json
```
1. Add above as docker-compose.yml
2. Add `config.json` to the same folder
3. Run `docker-compose up -d
4. Profit.

### Generate target configuration
To paint an image you need a template in the form of a PNG that will be converted into a configuration file. 
```
python3 src/image_converter.py [filename] [x] [y]
```
The position specified is that of the top left pixel.
The generated file contains the coordinates and closest available place color for each pixel of the input image that hasn't set alpha to 0.

You can then put the generated file on a web server of your choosing.
Alternatively, just put it in the folder next to the local configuration file.

### Configure local
Copy the configuration template:
```
cp config.template.json config.json
```

Configure the accounts to be used in `config.json`.
Set the ```target_configuration_url``` to the URL of the web server serving the target configuration file you generated using the image converter above. In case you do not want to use a web server for coordination of multiple people, you can also enter a local file path.

You can then run the bot using:
```
python3 src/placebot.py
```

It should log into the specified accounts and start drawing. Every 5 minutes, each account pulls the current board and compares it to the target configuration. The target configuration is updated every minute from the specified webserver to keep all clients in sync. When it detects a mismatch, it places a pixel with the desired color.

## Placement order
When placing pixels, the bot will prioritize them in the order specified in the target configuration file as supplied by the server. By default, the `image_converter.py` generates pixels from top to bottom, but this can be change by exchanging the loops.

The bot will scan the image for mismatched pixels, order them according to priority and pick one from the first 8 randomly to avoid conflicting with other bots that happen to be placing at the same time.

## To be noted
The bot only pulls information for canvases specified in the target configuration. This is generated by the image converter automatically.

The bot goes to sleep when it detects that there are only a few pixels left to be placed, only refreshing every 120s (till it detects it should wake up again).

### Generate configuration
To paint an image you need a template in the form of a PNG. 
```
python3 src/converter/image_converter.py [filename] [x] [y]
```
The position specified is that of the top left pixel.
The generated file contains the coordinates and closest available place color for each pixel of the input image that hadn't set alpha to 0.

You can then put the generated file on a web server of your choosing.
Alternatively, just put it in the folder next to the local configuration file.

### Configure locally
Copy the configuration template:
```
cp config.template.json config.json
```

Configure the accounts to be used in `config.json`.
Set the ```target_configuration_url``` to the URL of the web server saving the target configuration file you generated using the image converter above. In case you do not want to use a web server for coordination of multiple people, you can also enter a local file path.

You can then run the bot using:
```
python3 src/placebot.py
```

It should log into the specified accounts and start drawing. Every 5 minutes, each account pulls the current board and compares it to the target configuration. The target configuration is updated every minute from the specified webserver to keep all clients in sync. When it detects a mismatch, it places a pixel with the desired color.

## To be noted
The bot only pulls information for canvases specified in the target configuration. This is generated by the image converter automatically.

The bot goes to sleep when it detects that there are only a few pixels left to be placed, only refreshing every 120s (till it detects it should wake up again).
