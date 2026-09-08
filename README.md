# Testing Website

This GitHub Pages site uses Firebase Authentication and includes a signed-in dashboard with:

- camera/photo selection with Firebase Storage upload and a browser-storage fallback;
- read-only Google Calendar access requested only when the user clicks Connect;
- an OpenStreetMap/Leaflet map with high-accuracy browser geolocation and manual map picking;
- an opt-in FingerprintJS v5 visitor identifier;
- a non-authentication browser-session cookie demonstration.

## Firebase Storage

Enable Firebase Storage for project `testwebsite-f8d84`, then deploy the included user-scoped rules:

```powershell
firebase deploy --only storage
```

Uploads are limited to signed-in users, their own `uploads/{uid}` path, image MIME types, and files smaller than 5 MB.

If Firebase Storage is unavailable, the page saves the photo in IndexedDB for the signed-in user. Those photos survive reloads in the same browser and device, but they are not synced to other devices.

## Google Calendar

Enable the Google Calendar API in the Google Cloud project that backs Firebase. The dashboard requests only:

`https://www.googleapis.com/auth/calendar.readonly`

The returned access token is kept in memory and is not written to cookies or browser storage.

## Session-cookie limitation

GitHub Pages is a static host and cannot create or validate a secure Firebase `HttpOnly` session cookie. The dashboard's `demo_session` cookie is only a session-cookie demonstration and is never trusted for authentication.

A production Firebase session cookie requires a backend endpoint using the Firebase Admin SDK to exchange a recent Firebase ID token for a signed session cookie, with CSRF protection and `HttpOnly; Secure; SameSite` attributes.

## Privacy

Camera access, location access, Calendar authorization, and fingerprint computation occur only after the corresponding user action. The open-source FingerprintJS result is displayed locally and is not uploaded by this code.
