
name: Build Android APK

on:
  workflow_dispatch:
  push:
    branches: [ main ]

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - uses: android-actions/setup-android@v3

      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Extract Android project
        run: unzip -o UzbekCallerID-source.zip

      - name: Build APK
        run: gradle -p UzbekCallerID assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: UzbekCallerID-APK
          path: UzbekCallerID/app/build/outputs/apk/debug/app-debug.apk
          
