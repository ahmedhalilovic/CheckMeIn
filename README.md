# CheckMeIn

Lightweight iOS app (Swift/SwiftUI) for recording checkins using Firebase (Auth, Firestore, Storage).

## Overview

CheckMeIn records user checkins (QR / NFC / simulated) and writes `checkins` documents to Firestore. User profiles are stored in `users/{uid}` so checkins can include `firstName` / `lastName`.

## Key features

- Email/password sign up (writes `users/{uid}` with `firstName` / `lastName`)
- Create checkins (QR / NFC / simulated)
- Upload selfie image for a checkin to Firebase Storage
- Fetch shifts and recent checkins

## Requirements

- macOS with Xcode (use latest stable)
- iOS deployment target supported by your Xcode
- Firebase iOS SDK configured
- `GoogleService-Info.plist` added to the app target

## Quick setup

1. Create a Firebase project and add an iOS app.
2. Download `GoogleService-Info.plist` and add it to the Xcode app target.
3. Enable Authentication → Email/Password.
4. Enable Firestore and Storage.
5. Install dependencies (if using Swift Package Manager, add Firebase packages in Xcode).
6. Open the project in Xcode and build.

## Firestore rules (development)

For quick local testing you can use permissive rules (replace with proper rules before production):

\`\`\`text
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
\`\`\`

## How to run & test checkins

1. Build and run the app in Simulator or device.
2. Sign up using the app UI and provide first and last name.
   - This writes a user doc at `users/{uid}` (fields: `displayName`, `firstName`, `lastName`, `email`, `updatedAt`).
3. Trigger a checkin:
   - Tap the NFC simulate button or the QR button.
4. Inspect Firestore:
   - `users/{uid}` — verify `firstName` / `lastName` present.
   - `checkins/{docId}` — verify `firstName`, `lastName`, `displayName`, `userId`, `timestamp`, etc.
5. Check Xcode console for debug logs from the attendance service:
   - Logs are prefixed with `[FirestoreAttendanceService]` and show the payload being written.

## Files to inspect

- `CheckMeIn/Services/FirestoreAttendanceService.swift` — builds checkin payload, sanitizes optional fields, writes to `checkins`.
- `CheckMeIn/SessionStore.swift` — creates Firebase Auth users and writes `users/{uid}` on signup.
- `CheckMeIn/MainTabView.swift` — constructs simulated NFC/QR `CheckinEvent` (includes splitting display name into first/last).
- `CheckMeIn/Models/TimeEntry.swift` — `CheckinEvent` model (may include optional `firstName` / `lastName`).

## Troubleshooting

- Names are null in Firestore:
  - Confirm `users/{uid}` exists and contains `firstName` / `lastName`.
  - If a checkin is created immediately after signup, client now sends name in the event to avoid race — verify the UI path that constructs events includes names.
  - Check console logs (`[FirestoreAttendanceService]`) to see which name source was used (`event`, `users_doc`, or `auth_displayName`) and the final payload.
  - Ensure Firestore rules allow reads if the app reads `users/{uid}`.

## Contributing

- Open issues or PRs.
- Keep changes minimal and add tests where applicable.

## License

This project is available under the MIT License. See `LICENSE` for details.
