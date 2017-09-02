# Telegram Kraken Bot
Python bot to trade on Kraken via Telegram

## Overview
This script is a polling (not [webhook](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Webhooks)) based telegram bot. It can trade crypto-currencies on the [Kraken](http://kraken.com) marketplace and has a user friendly interface (custom keyboards with buttons).

### Features
- Bound to a specific Telegram user - only that user can use the bot
- No need to login to Kraken - start trading immediately, always
- Integrated update mechanism - to latest version on GitHub
- Notifies you once order is closed - trade successfully executed
- Fully usable with buttons - no need to enter commands manually
- Following Kraken functionality is implemented
    - Create a buy / sell order (type _limit_)
    - Lookup last trade price for currencies
    - Show your assets
    - Current market value of assets
    - Show / close open orders
- Supported currencies
	- Bitcoin
	- BitcoinClassic (no trading because of buggy Kraken API)
	- Ether
	- Litecoin
	- Ripple
	- Monero

## Configuration
Before starting up the bot you have to take care of some settings. You need to edit two files:

### config.json
This file holds the configuration for your bot. You have to at least edit the values for __user_id__ and __bot_token__.

- __user_id__: Your Telegram user ID. The bot will only reply to messages from this user. If you don't know your user ID, send a message to `userinfobot` and he will reply your ID

- __bot_token__: The token that identifies your bot. You will get this from 'BotFather' when you create your bot. If you don't know how to register your bot, follow [these instructions](https://core.telegram.org/bots#3-how-do-i-create-a-bot)

- __password_needed__: (_currently not used_) If you want to use the bot with a password, set this to `true`, otherwise to `false`

- __password_hash__: (_currently not used_) Will be set automatically once you enable the password protection and set a new password. __Please don't change this__

- __trade\_to\_currency__: The fiat currency you are using (for example `EUR`)

- __check_trade__: If `true` then every order (already existing or newly created) will be monitored by a job and if the status changes to `closed` (which means that the trade was successfully executed) then you will be notified with a message

- __check\_trade\_time__: Time in seconds to check for order status change (see setting `check_trade`)

- __update_url__: URL to the latest GitHub version of the script. This is needed for the update functionality. Per default this points to my repository and if you don't have your own repo with some changes then you can use the default value

- __update_hash__: Hash of the latest version of the script. __Please don't change this__. Will be set automatically after updating

### kraken.key
This file holds two keys that are necessary in order to communicate with Kraken. Both keys have to be considered secret and you should be the only one that knows them. If you don't know where to get / how to gererate the keys:

1. Login to Kraken
2. Click on `Settings`
3. Click on `API`
4. Click on `Generate New Key`
5. Enter `Telegram-Kraken-Bot` in `Key Description`
6. Enter `4` in `Nonce Window` (or just use the default value)
7. Select all available permissions at `Key Permissions`
8. Click on `Generate Key`

When you have your Kraken API keys, open the file `kraken.key` and replace `some_api_key` (first line) with the value of `API Key` and `some_private_key` (second line) with the value of `Private Key`.

## Installation
In order to run the bot you need to execute the script `telegram_kraken_bot.py`. If you don't have any idea where to host it, take a look at [Where to host Telegram Bots](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Where-to-host-Telegram-Bots). __Since you have to provide sensitive data (Kraken API keys) to use the bot, i would only host this script on a server that you own__. You can also run the script locally on your computer for testing purposes.

### Prerequisites
##### Python version
You have to use __Python 3.6__ to execute the script (because of enum method `auto()`). If you would like to use Python 3.4 or 3.5, you have to remove `auto` from imports and set the values in `WorkflowEnum` and `KeyboardEnum` yourself. Python 2.x is __not__ supported.

##### Needed modules
You need to install the following Python modules first:
```shell
pip3 install python-telegram-bot -U
pip3 install krakenex -U
pip3 install requests -U
```

##### Installing from `requirements.txt`
Do the above to install the newest versions of the needed Python modules (recommended) or if you run into issues, install a set of module-versions that is known to work together for sure with:
```shell
pip3 install -r requirements.txt
```

### Starting up
To start the script, execute
```shell
python3 telegram_kraken_bot.py &
```

## Usage
If you configured the bot correctly and execute the script, you should get a welcome message from the bot along with the information if you are using the latest version. There should also be a custom keyboard that shows you all the available commands. Click on a button to execute the command or type the command in manually.

### Available commands
##### Related to Kraken
- `/trade`: Start a workflow that leads to the creation of a new order of type _limit_ (buy or sell)
- `/orders`: Show all open orders (buy and sell) and close a specific one or all if desired
- `/balance`: Show all assets and the volume available to trade if open orders exist that block assets
- `/price`: Return last trade price for the selected crypto-currency
- `/value`: Show current market value of chosen crypto-currency or all assets, based on the last trade price
- `/bot`: Show options to check for update, update, restart or shutdown the bot
- `/chart`: Show a trading chart for the chosen currency
- `/history`: Show history of closed trades

##### Related to bot
- `/update`: Update the bot to the latest version on GitHub
- `/restart`: Restart the bot
- `/shutdown`: Shutdown the bot

## Development
I know that it is unconventional to have the whole source code in just one file. At some point i should have been switching to object orientation and multiple files but i kind of like the idea to have it all in just one file and object orientation would only blow up the code. This also makes the `/update` command much simpler :)

### Todo
##### Priority 1
- [X] Add command `/history` that shows executed trades
- [X] Add command `/chart` to show TradingView Chart Widget website
- [ ] Add command `/funding` to deposit / withdraw funds
- [ ] Add command `/alert` to be notified once a specified price is reached
- [ ] Add possibility to sell __all__ assets immediately to current market value
- [ ] Enable to trade every currency that Kraken supports

##### Priority 2
- [ ] Optimize code to call Kraken API les often
- [ ] Automatically check for updates (configurable timespan & changelog)
- [ ] Create webhook-version of this bot

##### Priority 3
- [ ] Add command `/stats` that shows statistics
- [ ] Notificaiton: Show win / loss if association between buy and sell order can be made

### Known bugs
- Background jobs that check order state do not send messages if `updater.idle()` is present (commented out `updater.idle()` for now)

## Disclaimer
I use this bot personally to trade on Kraken so i guess it's kind of stable but __if you use it, then you are doing this on your own responsibility__ !!! I can not be made responsible for lost coins or other stuff that might happen due to some fuckup within the code. Use at your own risk!