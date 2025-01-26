# Telegram bot for predicting cryptocurrency prices

## What is this bot for

The bot predicts cryptocurrency prices (for the next 3 weeks), which are added to the user's favorites (by default, there are a maximum of 3 positions).

For the selected cryptocurrencies, you can find out:

- price forecast **for the next 2 weeks**;
- "_Low_" support levels (with a visual graph for the last year. If there is data);
- "_High_" support levels (with a visual graph for the last year. If there is data).

> Support levels are needed to make the right decision to buy \ sell.

The bot has a system for adding \ removing and storing a list of selected shares.
And also a convenient navigation menu - it is adaptive and can "stretch" depending on the number of shares in the favorites list (displayed as buttons).

Here is a visual example of what the bot is:

![TG_CryptoBot](_assets/TG-BOT.gif)

## Technologies used

- **investpy** = For obtaining historical data on cryptocurrency prices
- **pandas** = For working with historical data
- **beatifulsoup4** = For parsing predictive prices on cryptocurrencies
- **python-telegram-bot** = For working with telegram as a bot (sending messages, charts)
- **matplotlib** = For drawing charts of cryptocurrency prices in an annual period
- **sqlite3** = For storing a list of cryptocurrencies and favorite cryptocurrencies for users

## Preparing for launch

Before launching, you need to prepare a special file `.env` (virtual environment variables).

This file will store two **mandatory** variables (Token and chat ID).

Create a `.env` file, fill in the variables and place the file in the `config/` directory

```
TOKEN=<Your telegram bot token>
CHAT_ID=<Chat to which the bot will send graphs (you yourself)>
```

## Launch

### Standard launch

When the preparatory work is completed, you can proceed to launch.

First, create a virtual environment:

```bash
python -m venv venv
```

And install all the necessary project dependencies:

```bash
pip install -r requirements.txt
```

Now you can launch the project:

```bash
python main.py
```

All that remains is to go to telegram and use the bot as intended.

### Launch with Docker

The project is also configured to run in Docker

> You must have docker and docker-compose installed
> Also make sure that docker itself is running: `sudo systemctl status docker`

Build and run in Docker with the command:

```bash
docker-compose up --build
```

If everything is done correctly, you will be greeted by:

![Docker](_assets/Docker-done.png)

You can use the bot.

## Additional functionality

The project implements GitHub Actions (Workflow) ('.github/worflows/main.yml').
You can use it in your project when cloning this repository.

Activated when sending changes to GiHub repository (git push)

GitHub Actions consists of 3 jobs:

- **formatting_and_test** = Checking code with Linter (flake8) and formatter (black), and performing Unittest testing.
(workflow tests for receiving data from investpy fail because GiHub blocks third-party connections)
- **build_and_push_to_docker_hub** = Build and publish an image on DockerHub
(to do this, add DOCKER*USERNAME, DOCKER_PASSWORD and NAME_REPO in the GitHub settings in the \_secrets* item)
- **deploy** = upload the image to the server and activate it
(this job is disabled, but you can familiarize yourself with its settings in the file '.github/worflows/main.yml')

The project also has built-in logging (Logging library), which can help you if something suddenly doesn't work out when launching the project.

The logs themselves are stored in the `config/logging/` directory.

By default, only _ERROR_ level logs are collected, but you can change the logging level yourself in the `settings.py` file

## How to improve the bot

A few words about how to improve the bot:

- Refactor the logging system and catch only the necessary errors. Using Exception everywhere is Bad Practice.
- Rewrite parsing using an asynchronous method (asyncio and aiohttp libraries). Will reduce the wait for crypto price results.
- Rewrite the rendering of graphs. For example, instead of writing \ reading a png file, send an image to the bot via base64.
