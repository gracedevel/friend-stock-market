# Pretend Stock Market - Free / No-Billing Edition

This version is a fictional stock-market game. It never handles real money.
Players use 1,000 virtual **credits** to buy and sell custom stocks.

## What is different from the paid/backend version?

- No Cloud Functions.
- No Stripe, PayPal, bank account, card, deposit, withdrawal, or real-money functionality.
- Firebase Authentication + Cloud Firestore only.
- Designed to stay on Firebase's Spark no-cost plan.
- Maximum 10 members is enforced by the app/rules, but this version is not as hardened against a hostile user as the Cloud Functions edition. It is intended for a small trusted group.

## 1. Create the Firebase project

1. Go to https://console.firebase.google.com/
2. Create a new project.
3. Keep it on the **Spark** plan. Do not add a Cloud Billing account.
4. Open **Build -> Authentication -> Get started**.
5. Open **Sign-in method**, enable **Email/Password**, and save.
6. Open **Build -> Firestore Database -> Create database**.
7. Use Production mode and choose a nearby region.

## 2. Create a Web App

In Firebase:

**Project settings -> Your apps -> Web (`</>`)**

Register a web app and copy the Firebase config.

Open `public/index.html` and replace:

```js
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

with your actual Firebase web configuration.

## 3. Install the Firebase CLI

Install Node.js LTS from https://nodejs.org/.

Then in a terminal:

```bash
npm install -g firebase-tools
firebase login
```

Check that the Firebase project is visible:

```bash
firebase projects:list
```

## 4. Connect this folder to Firebase

From this project's root folder:

```bash
firebase use --add
```

Pick your Firebase project and use `default` as the alias.

## 5. Deploy Firestore rules and Hosting

```bash
firebase deploy --only firestore:rules,hosting
```

There is deliberately no `functions` directory in this free edition.

## 6. Add your custom stocks

In Firebase Console -> Firestore Database -> Data, create a collection named:

`stocks`

Use the stock symbol as the document ID.

Example document:

`SUN`

Fields:

```text
symbol: "SUN"
name: "Sunrise Snacks"
price: 25
previousPrice: 25
```

Another:

`NOVA`

```text
symbol: "NOVA"
name: "Nova Robotics"
price: 75
previousPrice: 75
```

When you edit a stock's `price`, currently signed-in players will see the change in real time.

## 7. Create the market state document

In Firestore create:

Collection: `market`

Document ID: `state`

Fields:

```text
memberCount: 0
maxMembers: 10
```

The app increments the member count when people register.

## 8. How the game works

Each member starts with:

`1,000 credits`

Credits are fictional game points only.

Example:

- SUN price = 25 credits
- Buy 4 shares = 100 credits
- Your credits become 900
- Your SUN holding becomes 4

Selling returns virtual credits.

No real currency or payment service is used anywhere.

## 9. Important limitation

Because this edition deliberately avoids Cloud Functions so the Firebase project can stay on Spark, the strongest server-side enforcement from the paid/backend edition is not present.

For a trusted private group of up to 10 people, this is a simple and inexpensive setup. Do not use it as a real financial application or as a security-critical trading system.
