
Codemagic - One-click build instructions (mobile-friendly)
1) Create a GitHub repo and push the contents of this folder (all files & folders).
2) Sign in to https://codemagic.io with "Sign in with GitHub" and allow access to your repo.
3) On Codemagic Dashboard -> Add application -> Select your GitHub repo 'IndyNova'.
4) In Build configuration choose:
   - Platform: Android
   - Build type: apk (or aab for Play Store)
   - Working directory: flutter-client
   - Build mode: release
5) To sign the APK (recommended for Play Store):
   - Create a keystore locally:
     keytool -genkey -v -keystore indynova.keystore.jks -alias indynova_key -keyalg RSA -keysize 2048 -validity 10000
   - base64 encode and add as a Codemagic environment variable (secure):
     base64 indynova.keystore.jks > keystore.b64
     Copy content and set CM env var: KEYSTORE_BASE64
     Also set: KEYSTORE_PASSWORD, KEY_ALIAS, KEY_PASSWORD as env vars.
6) In Codemagic UI under "Environment variables" add:
   - KEYSTORE_BASE64 (secure)
   - KEYSTORE_PASSWORD (secure)
   - KEY_ALIAS (secure)
   - KEY_PASSWORD (secure)
   - GOOGLE_SERVICES_JSON (optional: base64 of google-services.json) 
   - SERVICE_ACCOUNT_JSON (optional: base64 of service-account.json)
7) Add a pre-build script to decode keystore (example in Codemagic scripts):
   echo "$KEYSTORE_BASE64" | base64 --decode > /tmp/indynova.keystore
   mkdir -p android/app
   # if google services provided
   if [ -n "$GOOGLE_SERVICES_JSON" ]; then echo "$GOOGLE_SERVICES_JSON" | base64 --decode > android/app/google-services.json; fi
8) Start build -> download artifact from Artifacts tab.
Notes:
- Keep all secrets secure. Do not commit keystore or service account JSON to GitHub.
- Replace AdMob test IDs in flutter-client with your production Ad Unit IDs before publishing.
