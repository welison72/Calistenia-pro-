
name: Gerar APK Calistenia Pro

on:
  workflow_dispatch:
  push:
    paths:
      - 'CalisteniaProAndroid.zip'
      - '.github/workflows/build.yml'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Baixar projeto
        uses: actions/checkout@v4

      - name: Preparar Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Preparar Android SDK
        uses: android-actions/setup-android@v3

      - name: Instalar ferramentas Android
        run: sdkmanager "platforms;android-35" "build-tools;35.0.0"

      - name: Instalar Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.7'

      - name: Extrair projeto
        run: unzip -q CalisteniaProAndroid.zip -d .

      - name: Compilar APK
        run: gradle -p CalisteniaProAndroid assembleDebug

      - name: Disponibilizar APK
        uses: actions/upload-artifact@v4
        with:
          name: Calistenia-Pro-APK
          path: CalisteniaProAndroid/app/build/outputs/apk/debug/app-debug.apk
          if-no-files-found: error
