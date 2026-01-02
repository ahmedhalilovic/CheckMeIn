```markdown
# Worker Check-in App — Firestore-first (Option A)

This repo contains:
- SwiftUI app skeleton using Firebase Auth + Firestore + Storage.
- SessionStore (Google sign-in via Firebase).
- FirestoreAttendanceService to record checkins and upload selfies.
- Cloud Function example to export shifts to Google Sheets.

Minimum requirements
- Xcode 14+ (use latest available)
- Swift 5.8+
- iOS Deployment Target: 15.0 (recommended)

Packages (use Swift Package Manager)
- Firebase (Firestore, Auth, Storage) — add via SPM: https://github.com/firebase/firebase-ios-sdk
  - Add packages: FirebaseAuth, FirebaseFirestore, FirebaseStorage, FirebaseCore
- GoogleSignIn (for Google OAuth on iOS) — SPM: https://github.com/google/GoogleSignIn-iOS

Project setup (brief)
1. Create Firebase project in console.firebase.google.com.
2. Add iOS app with your Bundle ID and download GoogleService-Info.plist — add to Xcode (ensure it's in app target).
3. Add SPM packages listed above.
4. In Xcode:
   - Enable NFC capability if you will use CoreNFC.
   - Add Camera usage description and NFC usage description to Info.plist:
     - Privacy - Camera Usage Description
     - Privacy - NFC Scan Usage Description
   - Add URL type for Google SignIn (the reversed client id from GoogleService-Info.plist).
5. Configure Firebase at app start (see `SessionStore.configure()`).

Important Info.plist entries
- Privacy - Camera Usage Description (for QR / selfies)
- Privacy - NFC Scan Usage Description (for NFC)
- URL Types: Add an item with the reversed client id for Google Sign-In.

```
