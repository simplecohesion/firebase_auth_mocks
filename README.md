# Firebase Auth Mocks

Mocks for [Firebase Auth](https://pub.dev/packages/firebase_auth). Use this package with [google_sign_in_mocks](https://pub.dev/packages/google_sign_in_mocks) to write unit tests involving Firebase Authentication.

[![pub package](https://img.shields.io/pub/v/firebase_auth_mocks.svg)](https://pub.dartlang.org/packages/firebase_auth_mocks)

## Usage

A simple usage example:

```dart
import 'package:firebase_auth_mocks/firebase_auth_mocks.dart';
import 'package:google_sign_in_mocks/google_sign_in_mocks.dart';

main() {
    // Mock sign in with Google.
    final googleSignIn = MockGoogleSignIn();
    final signinAccount = await googleSignIn.signIn();
    final googleAuth = await signinAccount.authentication;
    final AuthCredential credential = GoogleAuthProvider.getCredential(
      accessToken: googleAuth.accessToken,
      idToken: googleAuth.idToken,
    );
    // Sign in.
    final user = MockUser(
      isAnonymous: false,
      uid: 'someuid',
      email: 'bob@somedomain.com',
      displayName: 'Bob',
    );
    final auth = MockFirebaseAuth(mockUser: user);
    final result = await auth.signInWithCredential(credential);
    final user = await result.user;
    print(user.displayName);
}
```

## Features

- `MockFirebaseAuth` supports:
  - instantiating in a signed-in state or not: `MockFirebaseAuth(signedIn: true/false)`.
  - firing events on sign-in to `authStateChanges` and `userChanges`.
  - `signInWithCredential`, `signInWithEmailAndPassword`, `signInWithCustomToken`,
    `signInAnonymously` and `createUserWithEmailAndPassword` signs in.
  - `sendSignInLinkToEmail`, `confirmPasswordReset` and `verifyPasswordResetCode`.
  - `verifyPhoneNumber` resolves `codeSent`.
  - `signOut`
  - `sendPasswordResetEmail`
  - `currentUser`
  - the ability to throw exceptions using `authExceptions`:
  ```dart
  final auth = MockFirebaseAuth(
    authExceptions: AuthExceptions(
      signInWithCredential: FirebaseAuthException(code: 'invalid-credential'),
    ),
  );
  ```
- `UserCredential` contains the provided `User` with the information of your choice.
- `User` supports:
  - `updateDisplayName`
  - `reauthenticateWithCredential`
  - `updatePassword`
  - `delete`
  - `sendEmailVerification`
  - `getIdToken` and `getIdTokenResult`
  - the ability to throw exceptions.


## Compatibility table

| firebase_auth | firebase_auth_mocks |
|---------------|---------------------|
| 4.0.0         | 0.9.0               |
| 3.5.0         | 0.8.7               |

## Features and bugs

Please file feature requests and bugs at the issue tracker.
