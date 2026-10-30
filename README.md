name: Build Shawon Tech Android APK

on:
  workflow_dispatch:
  push:
    branches:
      - main

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source
        uses: actions/checkout@v4

      - name: Setup Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '21'

      - name: Setup Android SDK
        uses: android-actions/setup-android@v3

      - name: Install Android SDK
        run: |
          sdkmanager "platforms;android-36.1" \
                     "build-tools;36.0.0" \
                     "platform-tools"

      - name: Build Debug APK
        run: |
          chmod +x gradlew
          ./gradlew --no-daemon :app:assembleDebug

      - name: Upload APK
        uses: actions/upload-artifact@v4
        with:
          name: Shawon-Tech-APK
          path: app/build/outputs/apk/debug/app-debug.apk
          if-no-files-found: error