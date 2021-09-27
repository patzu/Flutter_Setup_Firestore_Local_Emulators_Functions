# Setup Firestore local emulators and call a sample function in Flutter




* https://firebase.google.com/docs/cli#install_the_firebase_cli
* https://firebase.google.com/docs/cli#windows-npm
* https://firebase.google.com/docs/cli#sign-in-test-cli
* https://firebase.google.com/docs/emulator-suite
* https://firebase.google.com/docs/emulator-suite/install_and_configure
* https://firebase.google.com/docs/functions/local-emulator#windows
* https://firebase.flutter.dev/docs/firestore/usage/#emulator-usage

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
```
firebase emulators:start --only "functions,auth,firestore"
```

4- Sample function:
-------------------------------
* Copy and Paste below function inside `index.js` file in `functions/index.js` path in your Flutter project.
```
exports.listFruit = functions.https.onCall((data, context) => {
  return ["Apple", "Banana", "Cherry", "Date", "Fig", "Grapes"]
});
```

5- Setting the app to use simulator instead of cloud firestore and calling the above mentioned function
--------------------------
Call this method in onPressed callback of button:
```
void fruit() async {
    String host = Platform.isAndroid ? '10.0.2.2' : 'localhost';
    int port = Platform.isAndroid ? 5001 : 8080;
    FirebaseFirestore.instance.useFirestoreEmulator(host, port);
    
    HttpsCallable callable = FirebaseFunctions.instance.httpsCallable('listFruit');
      final results = await callable();
      
      List fruit = results.data;
      print(fruit);
  }
```

6-1 Sample 2
---
```
 exports.hello = functions.region(deploymentRegion).https.onCall((data, context) => {
  return {
      response : "hello " + data.message,
  }
});
```


6-2: calling the firebase function in flutter:
-----
```
 Future<void> checkZipCode() async {
    FirebaseFunctions firebaseFunctions =
        FirebaseFunctions.instanceFor(region: "europe-west1");
    String host = Platform.isAndroid ? '10.0.2.2' : 'localhost';
    int port = Platform.isAndroid ? 5001 : 8080;
    firebaseFunctions.useFunctionsEmulator(host, port);

    HttpsCallable callable = firebaseFunctions.httpsCallable('hello');

    try {
      final HttpsCallableResult result = await callable.call(
        <String, dynamic>{
          'message': 'Patzu!',
        },
      );
      print(result.data['response']);
    } on Exception catch (e) {
      print('caught firebase functions exception');
      print(e);
    }
  }
```


----------------

Configuring flutter app to work with firebase authentication in local emulators.

First:

Add one of the below lines before authentication function call see which one working for me loopback IP worked:

 firebaseAuth.useAuthEmulator('127.0.0.1',9099);
 //firebaseAuth.useAuthEmulator('http://localhost',9099);
Sample:

```
 Future<LocalUser?> signInAnonymously({required BuildContext context}) async {
    firebaseAuth.useAuthEmulator('127.0.0.1',9099);
    //firebaseAuth.useAuthEmulator('http://localhost',9099);

    UserCredential userCredential = await firebaseAuth.signInAnonymously();
    return LocalUser(user: userCredential.user);
  }
 ```
 
If you get network error do as follow.

Second-1: For Android.

Add android:usesCleartextTraffic="true" in AndroidManifest.xml path: android/app/src/main/AndroidManifest.xml.

```
 <application
    android:usesCleartextTraffic="true"      //<========
    android:name="io.flutter.app.FlutterApplication"
    android:label="firebaseauthexample"
    android:icon="@mipmap/ic_launcher">
```
    
Second-2: For IOS.

Add bellow code here ios/Runner/Info.plist
```
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>

```











