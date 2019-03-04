---
layout: post
title: Simple In-App-Purchase validation service
tags: Cordova ionic node GitHub
update: 2019-03-04
---

Very basic validation service which can be used in combination
with [Cordova Purchase Plugin](https://github.com/j3k0/cordova-plugin-purchase).

It is build on top of [Express](https://expressjs.com/), using
[this excellent node.js module for in-app purchase](https://github.com/voltrue2/in-app-purchase).

<s>I also created a gist for this, so if you have any suggestions for improvement please change it right there and let me know!</s>

__Update:__ The script has been updated a bit and there is a repository now: [znegva/iap_validation_service](https://github.com/znegva/iap_validation_service).  
Please file issues and pull requests there.


Obsolete original content below

---

~~~js
/*
 * Example validations service to be used with j3k0/cordova-plugin-purchase
 * build with https://github.com/voltrue2/in-app-purchase and express
 */

const PURCHASE_EXPIRED_CODE = 6778003; //the cordova-purchase-plugin expects these
/*
 * prepare in-app-purchases
 */
var iap = require("in-app-purchase");
iap.config({
  /* Configurations for HTTP request */
  requestDefaults: {
    /*
     Please refer to the request module documentation here:
     https://www.npmjs.com/package/request#requestoptions-callback
     */
  },

  /*
   Please refer to the voltrue2/in-app-purchase documentation here:
   https://github.com/voltrue2/in-app-purchase
   */

  /* Configurations for Apple */
  appleExcludeOldTransactions: true, // if you want to exclude old transaction, set this to true. Default is false
  applePassword: "abcdefg...", // this comes from iTunes Connect (You need this to valiate subscriptions)

  /* Configurations for Google Play */
  googlePublicKeyPath: "path/to/public/key/directory/", // this is the path to the directory containing iap-sanbox/iap-live files
  googlePublicKeyStrSandBox: "publicKeySandboxString", // this is the google iap-sandbox public key string
  googlePublicKeyStrLive: "publicKeyLiveString", // this is the google iap-live public key string
  googleAccToken: "abcdef...", // optional, for Google Play subscriptions
  googleRefToken: "dddd...", // optional, for Google Play subscritions
  googleClientID: "aaaa", // optional, for Google Play subscriptions
  googleClientSecret: "bbbb", // optional, for Google Play subscriptions

  /* Configurations for Google Service Account validation: You can validate with just packageName, productId, and purchaseToken */
  googleServiceAccount: {
    clientEmail:
      "<client email from Google API service account JSON key file>,",
    privateKey:
      "<private key string from Google API service account JSON key file>"
  },

  /* Configurations all platforms */
  test: true, // For Apple and Googl Play to force Sandbox validation only
  verbose: true // Output debug logs to stdout stream
});
iap
  .setup()
  .then(() => {
    console.log(`*** Finished setting up in-app-purchase! ***`);
    // iap.validate(...) automatically detects what type of receipt you are trying to validate
  })
  .catch(error => {
    console.error(`*** Error during setup of in-app-purchase: ***`);
    console.error(error);
  });

/*
 * prepare the express server
 */
const express = require("express");
const app = express();
const port = 9001;

//json parsing
var bodyParser = require("body-parser");
app.use(bodyParser.json()); // to support JSON-encoded bodies
app.use(
  bodyParser.urlencoded({
    // to support URL-encoded bodies
    extended: true
  })
);
//CORS
var cors = require("cors");
app.use(cors());
//logging
var morgan = require("morgan");
app.use(
  morgan(
    ':method :url :status [HTTP/:http-version] [:res[content-length]] (":user-agent" / ":referrer")'
  )
);

//just answer get-requests
app.get("/", (req, res) => {
  res.send("<pre>Status: running</pre>");
});

/*
 * manage data offered by our apps, see
 * https://github.com/j3k0/cordova-plugin-purchase/blob/a1002c559686e555745de07bf531222c2dcb9e3a/www/store-ios.js#L1334
 */
app.post("/", (req, res) => {
  if (!req.body) {
    console.error("request was emtpy, return 400");
    res.status(400).end();
    return;
  }

  //we need to make sure our body is well-formed :D
  if (
    req.body &&
    req.body.id &&
    req.body.transaction &&
    req.body.transaction.type
  ) {
    //do noting... aka go on
  } else {
    console.error("malformed body, return 400 (Bad Request)");
    res.status(400).end();
    return;
  }

  //we only want to validate our own products...
  if (req.body.id.indexOf("com.example") != 0) {
    console.error("no example.com product, return 403 (Forbidden)");
    res.status(403).end();
    return;
  }

  var receipt;
  if (req.body.transaction.type == "ios-appstore") {
    console.log(`### Apple-Store-transaction for ${req.body.id} ###`);
    receipt = req.body.transaction.appStoreReceipt;
  } else if (req.body.transaction.type == "android-playstore") {
    console.log(`### Play-Store-transaction for ${req.body.id} ###`);
    receipt = req.body.transaction.receipt;
  } else {
    // we only have Android and iOS Apps, ignore all other!
    console.error("no Android or iOS product, return 403 (Forbidden)");
    res.status(403).end();
    return;
  }

  if (!receipt) {
    console.error(`Empty or undefined receipt for ${req.body.id}!`);
    //console.error(error);
    res.status(400).end();
    return;
  }

  iap
    .validate(receipt)
    .then(validatedData => {
      //console.log(validatedData);

      //convert the answer to something readable
      var purchaseData = iap.getPurchaseData(validatedData); //Array!!
      //console.log(purchaseData);

      //the only thing the cordova-plugin checks is if the validator returns PURCHASE_EXPIRED_CODE,
      //so this is the only thing we need to handle atm!
      var isExpired = false;
      purchaseData.forEach(purchase => {
        //only check/combine purchases/answers of the product this request is related to!
        if (purchase.productId == req.body.id) {
          isExpired = isExpired || iap.isExpired(purchase);
        }

        // we could also check for iap.isCanceled(purchase)); to see if it was canceled!
        //canceled means the user asked for a refund
        //this should be handled by the cordova-plugin, atm no need to handle it here too
      });

      if (isExpired) {
        console.log(`receipt for ${req.body.id} is valid BUT EXPIRED!`);
        res.json({
          ok: false,
          data: {
            code: PURCHASE_EXPIRED_CODE
          }
        });
      } else {
        console.log(`receipt for ${req.body.id} is valid!`);
        //if it is not exired, then it probably is valid...
        res.json({
          ok: true, //indicates the provided receipt was valid
          data: {} //as of j3k0/cordova-plugin-purchase@7.2.7 a data field is expected
        });
      }
    })
    .catch(error => {
      //some other error occured
      console.error(`Error during validation for ${req.body.id}!`);
      //console.error(error);
      res.status(400).end();
    });
});

//start server
app.listen(port, () => {
  console.log(
    `*** in-app-purchase-validation-server listening (port ${port}) ***`
  );
});
~~~

The related `package.json` to manage all used packages:

~~~json
{
  "name": "simple-validation-service",
  "version": "0.0.1",
  "description": "a simple app store validation service",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Martin Kausche",
  "license": "MIT",
  "dependencies": {
    "body-parser": "^1.18.3",
    "cors": "^2.8.4",
    "express": "^4.16.4",
    "in-app-purchase": "github:voltrue2/in-app-purchase",
    "morgan": "^1.9.1"
  }
}
~~~
