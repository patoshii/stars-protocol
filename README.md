## STARS (Smart Transaction API Reoccuring Service) Protocol ##

Version 1.0 - (Alpha - Experimental POC)

Cryptocurrency is based off of a push payment system, meaning users control their own private keys and only send out when they need to. The typical financial system is a "pull" system where users have semi-control over their funds. Merchants and service providers are authorized to "pull" funds out of users accounts when paying with a credit / debit card. We've built something similar for the cryptocurrency world, users still control their private keys, but gives the merchants the ability to "pull" funds out of their private keys when needed. This opens up many new business use cases for Bitcoin and the cryptocurrency ecosystem.

Now you can process bitcoin private keys and send coins from one address to another based on a condition such as time, price of a stock, or some other external variable. We've built an API service using a shared private key system. Using the condition of an external variable such as time, we can forward payments into a future date without the user losing control of their funds. As a 3rd party we enable "smart transactions" such as: external condition criteria's, time delay and repeating payments. These "smart contract" like features are not possible on the blockchain itself as it requires a 3rd party to interpret and process this type of data.

### Features: ###
- **Private Key Sweeping** Input a private key and extract coins to send to another address.
- **Delay Transactions** Create a transaction to be sent in the future without needing to lock funds.
- **Repeating payments** Send 1 BTC every 24 hours for 14 days. 
- **Noncustodial of coins** Uses shared private key system.
- **Smart Sending** Detects an external variable before sending coins.
- **Smart Send URL** Send coins to any URL on the internet. Claiming of these coins requires the URL owner to be able to prove they can edit the web page. 

## Compatible Payments ##
**Working:** Bitcoin
**Work in progress:** ETH, Stablecoins, Monero, EOS, and Ardor Ignis. Message us to support your coin.
**Fiat Bridges:** Work in Progress: Paypal, Venmo, Wechat Pay, AliPay, and Zelle. 

### Project Phase 1: ###
- Receive API calls via distributed blockchain protocol
- Process Bitcoin Private keys.

### Project Phase 2: ###
- Delay sending of bitcoin transactions.
- Repeating bitcoin transactions. 

### Project Phase 3: ###
- Processing of Ethereum tokens from private key. 
- Processing of EOS private keys and account name
- Repeating payments on Bitcoin, Monero, and Ethereum Tokens

### Project Phase 4: ###
- Fiat gateway testing

-----------------------------------------------------

### Frequently Asked Questions: ###
- **Do you hold our coins?** No we do not, you still have access to your coins until they are sent. The only thing we have is your private key that is only used during transaction processing. Hence, this is not a custodial type of service, but a shared private key system we've built. It's the same concept as a credit card, merchants have access to charge your card but you also have the ability to remove your funds too. This avoids the "not your keys, not your coins" problem.
- **How do you protect the private keys?** All private keys are encrypted before sending it over the internet for processing. This data is encrypted and then we broadcast it through Ardor testnet nodes. Hence our servers have no front facing internet address.
- **Why are you using the testnet Ardor blockchain and not mainnet?** In our testing phase we're using testnet for now as coins are free, and all of this data doesn't need to be stored forever. We do plan on going mainnet when this service is fully operational.  
- **Can I use this service locally?** You can either send the api calls through an online Ardor testnet node such as https://testardor.jelurida.com, or you can install the Ardor testnet node on your computer and send the same api calls to http://localhost:26876
- **What format does your api service accept?** Send a single line encrypted message in JSON format to account: ARDOR-XXXX-XXXX-5496-B3YAC - A fail or success response will be returned after 2-3 minutes.
- **How do I get a status on my transaction?** We send out failure or success messages to your Ardor testnet account.
- **What if I lose coins from my wallet?** You are responsible of keep track of your own private keys and only store what is necessary and not use this shared private key for anything else. If you suspect your keys are compromised, use another private key. Keep in mind this service is experimental and use at your own risk. 
- **Is there a fee to use this service?** Currently in our alpha phase, there will be no fee. The only fee you will be paying is the transaction fee on the blockchain. Future proposed fees would be based on a freemium model.
- **Why is there no IP or domain URL to access this service?** By using an existing blockchain distributed node system, we're able to limit the attack surface of our service substantially. We chose the Ardor blockchain, due to the technological advantages compared to other systems such as the ability to: encrypt messages, access to distributed nodes, and provide feeless transactions.
- **Is there a limit of how many transactions an account can create?** Currently there is no limit while testing, but we will most likely add in a rate limit for those passing in large amounts of transactions. 

---------------------

### How to access this forwarding service? ###
In order to prevent hacks, spam, and being distributed, we require all API calls sent via the Testnet Ardor messaging system. There is no direct IP address to our forwarding service as it is hidden behind blockchain nodes. You can send an encrypted message using a public Ardor testnet node such as https://testardor.jelurida.com or host one locally by installing a testnet Ardor node (ardorplatform.com). The following JSON format must be sent as an encrypted message to the processing Ardor account: ARDOR-XXXX-XXXX-5496-B3YAC in the following format in a single line:

**Beautified Version**
```
{
"nxtAddress" : "YOUR_ARDOR_ADDRESS",
"coinType" : "btc",
"senderPub" : "1xxxxxxx",
"senderPrv" : "5xxxxxx",
"recipient" : "1xxxxxxx",
"amount" : "9.00000009",
"delay" : "0",
"url" : "https://www.google.com/test",
}
```

## Accepted API Parameters ##

**nxtAddress:** -- OPTIONAL
- not-empty = An Ardor address to receive a status update on the transaction.
- empty = If empty, it will default to the sender's account.

**coinChosen:** -- REQUIRED
- not-empty = the coin type of the transaction btc/eth/ltc/eos/xmr/ignis
- empty = *Fail. Log this error and notify sender.

**coinPublic:** -- REQUIRED
- not-empty = Senders cryptocurrency address
- empty = *Fail. Log this error and notify sender. 

**coinPrivate:** -- REQUIRED
- not-empty = Senders cryptocurrency private key
- empty = *Fail. Log this error and notify sender.

**amount:** -- OPTIONAL
- -1 = Sending -1 value means cancel the transaction.
- 0 or EMPTY = Send the entire balance of the senders address to the recipient. Useful when a private key is submitted with a positive delay time. Ex. User creates a forward transaction to execute 30 days from now. Within the 30 days, the user accumulates bitcoins every week in that address. At end of 30 days, all the funds are swept and sent. 
-  0+ = Set amount of coins are sent to the recipient address. 

**delay:** -- OPTIONAL
- 0 or EMPTY= Send immediately.
- -1 = Coins are never sent. For use of not sending the coins unless condition X is met. (future development). 
- 0+ = Value is the number of hours to delay the transaction by. This decreases as the *hourly* until it reaches 0 on the backend.
**Repeating Delay** = Add a , comma to the delay. Example: 2,3 --- send every 2 hours for 3 times. 

**recipient:** -- REQUIRED only if URL is empty
- not-empty = Address of where to send coins
- empty = If this is empty, that means this transaction is associated to a URL and awaiting a recipient address. *If both recipient + URL is empty, transaction is deleted.*

**url:** -- OPTIONAL
- not-empty = URL of a website. If this is set, it will override the recipient value, and take the recipient value from the asset property.
- empty = If this is empty, recipient address must exist. Otherwise transaction is ignored and deleted.

**urlHash:** -- OPTIONAL
- not-empty = the hash of the URL that gets stored in the data cloud. hashed with sha256.
- empty = If empty and url exist. This transaction will be ignored and deleted.

**accountName:** -- OPTIONAL
- not-empty = eos account name here only if the coinChosen option is set as EOS.
- empty = If cointype is eos, and this is empty. This transaction will be deleted.

**pledgeNote:** -- OPTIONAL
- not-empty = a note that is attached to a url. Cannot be used alone. Ignored, if URL is not set.
- empty = Nothing happens.


---------
## Javascript Code Samples ##

```javascript
const util = {
  // API input
  node : "https://testardor.jelurida.com",
  secretPhrase : "a",// Ardor account with at least 10 ARDOR
  transactionLabel: 'api',

  send: function (data) {
    const params = {
      "requestType": "sendMessage",
      "chain": "ignis",
      "recipient": "ARDOR-XXXX-XXXX-5496-B3YAC",
      "secretPhrase": this.secretPhrase,
      "messageToEncrypt": JSON.stringify(data),
      "encryptedMessageIsPrunable": true,
      "messageToEncryptIsText": true,
      "message": this.transactionLabel,
      "referencedTransaction": "2:dfe36cf9f8dff41f30f1cde92717ee6f1ac2c5342bba7d691b5e0b75a6bc5204", //Reference a transaction that doesnt exist so it never confirms.
      "feeNQT": 0, //Burning Message - 0 fee paid. Never confirms.
      "deadline": 2,
      "broadcast": true
    };

    let paramString = Object.keys(params).map(index => (`${index}=${params[index]}&`)).join('');
    paramString = paramString.slice(0, -1);

    fetch(`${this.node}/nxt`, {
      method: "POST",
      body: paramString,
      headers: { 'Content-type': 'application/x-www-form-urlencoded' }
    })
      .then(r => r.json())
      .then(res => console.log(res));
  },

  hashUrl: async function (url) {
    let buffer = await crypto.subtle.digest('SHA-256', new TextEncoder().encode(url));

    let hashArray = Array.from(new Uint8Array(buffer));
    let base64 = hashArray.map(b => ('00' + b.toString(16)).slice(-2)).join('');

    return base64.replace(/[^a-z0-9]/gi, '_');
  }
}

let senderPublicKey = "_SENDER_COIN_PUBLIC_KEY_";
let senderPrivateKey = "_SENDER_COIN_PRIVATE_KEY_";
let senderArdorAddress = "_SENDER_ARDOR_PUBLIC_ADDRESS_";
let recipient = "_RECIEVER_COIN_PUBLIC_KEY_";


/*
 * Create a Sweep Transaction
 *******************/
const sweepTransaction = {
  "coinPublic": senderPublicKey,
  "coinPrivate": senderPrivateKey,
  "coinChosen": "btc", //btc, eos, eth, xmr, ltc
  "recipient": recipient,
  "nxtAddress": senderArdorAddress
};

// util.send(sweepTransaction);


/*
 * Create a Delay Transaction -- Returns: http://prntscr.com/pi4eed
 *******************/
const delayTransaction = {
  "coinPublic": senderPublicKey,
  "coinPrivate": senderPrivateKey,
  "coinChosen": "btc",
  "amount": 0.1,
  "recipient": recipient,
  "nxtAddress": senderArdorAddress,
  "delay" : 0 //3 hour
};

//util.send(delayTransaction);


/*
 * Create a Pledge Transaction
 *******************/
(async function() {
  const url = "https://gofundme.com/xcubicle";
  const urlHash = await util.hashUrl(url);

  const pledgeToUrL = {
    "coinPublic": senderPublicKey,
    "coinPrivate": senderPrivateKey,
    "coinChosen": "btc",
    "amount": 0.1,
    "nxtAddress": senderArdorAddress,
    "url": url,
    "urlHash": urlHash
  };

  //util.send(pledgeToUrL);
})()

```


---------------------


Copyright 2020 XCUBICLE

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
