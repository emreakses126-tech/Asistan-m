# Asistan-m
Muhasebe 
name: APK
on: [push, workflow_dispatch]
permissions:
  contents: write
jobs:
  apk:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: for z in *.zip; do unzip -o "$z" -d proje; done
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.11.1'
      - run: cd proje && gradle assembleDebug --no-daemon
      - run: cp proje/app/build/outputs/apk/debug/app-debug.apk MuhasebeAsistani.apk
      - uses: softprops/action-gh-release@v2
        with:
          tag_name: surum-${{ github.run_number }}
          files: MuhasebeAsistani.apk
