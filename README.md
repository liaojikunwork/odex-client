# ODEX client library

Use this library to write trading bots that trade on [ODEX](https://odex.ooo).

## Requirements

node.js 8+

## Install
Add odex-client as a dependency in your package.json:
```json
	"dependencies": {
		"odex-client": "git+https://github.com/byteball/odex-client.git"
	}
```
Install
```sh
npm install
```

## Usage
```js
const odex = require('odex-client');
let { orders, ws_api, rest_api, exchange, account, chat } = odex;

async function start() {
	await odex.start();

	// subscribe to new orders and trades on a pair
	await ws_api.subscribeOrdersAndTrades('GBYTE/USDC');
	ws_api.on('orders', (type, payload) => {
		console.error('received orders', type, payload);
	});
	ws_api.on('trades', (type, payload) => {
		console.error('received trades', type, payload);
	});

	// this will automatically update orders.assocMyOrders object which holds all my orders keyed by order hash. New orders will be automatically added, filled and cancelled orders will be automatically removed
	await orders.trackMyOrders();

	// place a new order, side is 'BUY' or 'SELL'
	await orders.createAndSendOrder('GBYTE/USDC', side, amount, price);

	// cancel an order
	await orders.createAndSendCancel(order_hash);
}

start();
```

## Configuration

Config options are read from conf.js in your project root or conf.json in its data folder.

To start with, copy conf.js from odex-client (this repo) to your project root and edit if necessary.

## Trading balances

The library includes an Obyte wallet that holds your trading balances. You need to deposit funds to this wallet before trading.

To manage the wallet, first edit your conf.js or conf.json and add the device address of your GUI Obyte wallet in `control_addresses` array in conf. 
```js
exports.control_addresses = ['your device address here'];
```
```json
"control_addresses": ["your device address here"],
```
You can find the device address of your GUI wallet under burger menu / Settings.

Next, start your bot that includes this library, it will print the wallet's paring code to the screen:
```
====== my pairing code: Aj2No4YghYNcHjzPqobfp5gSJ4jyaBqnhF2dK7lEsMZ+@obyte.org/bb#0000
```
Use this code to pair your GUI Obyte wallet to the bot and manage it. The bot recognizes the following commands:
* **exchange balance** - query the bot's balance on the exchange;
* **deposit \<amount> \<token>** - deposit tokens;
* **withdraw \<amount> \<token>** - withdraw tokens;
* **withdraw all \<token>** - withdraw the entire balance of a token;
* **help** - print the list of commands.

Refer to [headless wallet documentation](https://github.com/byteball/headless-obyte#remote-control) for other chat commands you can give to the bot.


## Help

\#tech channel on discord https://discord.obyte.org.

