# Setup Firestore local emulators and call a sample function in Flutter




https://firebase.google.com/docs/cli#install_the_firebase_cli
https://firebase.google.com/docs/cli#windows-npm
https://firebase.google.com/docs/cli#sign-in-test-cli
https://firebase.google.com/docs/emulator-suite
https://firebase.google.com/docs/emulator-suite/install_and_configure
https://firebase.google.com/docs/functions/local-emulator#windows
https://firebase.flutter.dev/docs/firestore/usage/#emulator-usage

1- Installing firebase CLI:
------------------------------
```
npm install -g firebase-tools
```
```
firebase login
```
```
firebase projects:list
```

2- Initializing firebase emulators
-----------------------------------
```
firebase --version
```
```
firebase init
```
```
firebase init emulators
```

3- Run emulators
-----------------------------
```
firebase emulators:start
```
```
firebase emulators:start --only functions
```

4- Sample function:
-------------------------------
* Paste this function to `index.js` file in `functions/index.js` path in your Flutter project.
```
exports.listFruit = functions.https.onCall((data, context) => {
  return ["Apple", "Banana", "Cherry", "Date", "Fig", "Grapes"]
});
```

5- Setting the app to use function simulator instead of firestore cloud and calling the above function
--------------------------

```

```

















