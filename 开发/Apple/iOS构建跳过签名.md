
https://stackoverflow.com/questions/40730051/how-to-build-an-ipa-without-signing-in-xcode-8


1.
    ```shell
    xcodebuild -workspace (or -project) [workspacename.xcworkspace] -scheme [Schemename] -sdk iphoneos -configuration Release CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO
    ```

2.
    Open the location from Step 1 (derived data) and navigate to >your app>build>products>Release-iphoneOS

3.
    Copy the .app file into a new subfolder named Payload (this folder name is case sensitive and much match verbatim)

4.
    Compress Payload folder and rename it to app_name-version_number.ipa
