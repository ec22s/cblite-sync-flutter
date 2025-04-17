# cblite-sync-flutter
A simple app to sync data with Couchbase server and client

original: https://github.com/couchbaselabs/flutter_cblite_userprofile_sync

<br>

<img width=256 src=https://github.com/user-attachments/assets/72f5c86c-1b1f-4692-bd03-e029ff51078d>
　<img width=256 src=https://github.com/user-attachments/assets/ac600b5c-c818-4927-b165-95bdf4db9b1b>
　<img width=256 src=https://github.com/user-attachments/assets/3840d990-577d-44cc-8ca7-84845e48a82a>

<br>
<br>

### 動作確認 (アプリ起動と端末内データ保存まで)
```
$ flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[!] Flutter (Channel [user-branch], 3.22.3, on macOS 14.7.1 23H222 darwin-x64, locale en-JP)
    ! Flutter version 3.22.3 on channel [user-branch] at /usr/local/Caskroom/flutter/3.29.2/flutter
      Currently on an unknown channel. Run `flutter channel` to switch to an official channel.
      If that doesn't fix the issue, reinstall Flutter by following instructions at https://flutter.dev/docs/get-started/install.
    ! Upstream repository unknown source is not a standard remote.
      Set environment variable "FLUTTER_GIT_URL" to unknown source to dismiss this error.
[✓] Android toolchain - develop for Android devices (Android SDK version 36.0.0)
[✓] Xcode - develop for iOS and macOS (Xcode 16.2)
[✓] Chrome - develop for the web
[✓] Android Studio (version 2024.3)
[✓] VS Code (version 1.99.2)
[✓] Connected device (6 available)
[✓] Network resources

! Doctor found issues in 1 category.
```

✅ macOS 14.7.1 (x64)

✅ iOS 15.8.4 (実機: iPhone7)

✅ iOS 18.2.1 (実機: iPad 10th)

✅ Android 10 (実機: HUAWEI PLE-701L, LineageOS 17.1)

<br>

### 補足

- アプリ名は `flutter_cblite_userprofile_sync` (オリジナルのまま)

- iOS端末でのアプリ起動時、LAN接続を許可するか確認モーダルが出る

- 起動時に任意の `Username` `Password` を設定し、次画面でユーザ情報を設定する. その情報が端末に保存される

- macOSでのプロフィール画像操作は未完 (画像選択ダイアログ開かず)

- サーバとのデータ同期には [Couchbase Capella](https://cloud.couchbase.com) の設定が必要、今後やる

<br>

以下、オリジナルのREADME

---

# Quickstart for Couchbase Lite Data Sync with Flutter

#### Build a Flutter App with Couchbase Lite

Find the video of this demo: https://drive.google.com/drive/folders/1yN8lzSmoEZ6OxUQ14AS5___9ijHAWf5q

You can find more information about the Couchbase Lite for Dart/Flutter SDK
[here](https://cbl-dart.dev/)

Couchbase Sync Gateway is a key component of the Couchbase Mobile stack. It is
an internet-facing synchronization mechanism that securely syncs data across
devices as well as between devices and the cloud. Couchbase Mobile is built upon
a websocket based
[replication protocol](https://blog.couchbase.com/data-replication-couchbase-mobile/).

The core functions of the Sync Gateway includes:

- Data Synchronization across devices and the cloud
- Authorization
- Access Control
- Data Validation

> This repo is designed to show you an app that allows users to log in and make
> changes to their user profile information. User profile information is
> persisted as a Document in the local Couchbase Lite Database. When the user
> logs out and logs back in again, the profile information is loaded from the
> Database.

Full documentation can be found on the
[Couchbase Developer Portal](https://developer.couchbase.com/tutorial-quickstart-android-java-sync/).

## Prerequisites

To run this prebuilt project, you will need flutter installed on your machine,
please refer to: https://docs.flutter.dev/get-started/install and install the
Flutter SDk and all required dependencies (e.g. XCode on iOS).

## Try it out

The project supports iOS, Android and macOS.

The easiest way to get started with Data Sync is to use a free
[Couchbase Capella](https://www.couchbase.com/products/capella) trial cluster.

After you have created a cluster, create a bucket and setup an App Service
endpoint that uses that bucket.

Also configure the App Service endpoint with the following access control function:

```javascript
function (doc, oldDoc, meta) {
  requireUser(doc.username);
  var userChannel = 'user.' + doc.username;
  channel(userChannel);
  access(doc.username, userChannel);
}
```

Create an app user and note the username and password so you can log in with
that user later.

Open the editor of your choice (I suggest VS Code with Flutter plugin) and
modify the string `CouchbaseLiteManager._syncGatewayUrl` in the
`lib/data/couchbase_lite_manager.dart`, pointing it to the App Services endpoint
(should be something like "wss://xxxx.apps.cloud.couchbase.com/userprofile").

To run the project on your preferred emulator or device simply type
`flutter run` from the project root folder.

## Conclusion

This code is an example of how to App Services to synchronize data between
Couchbase Lite enabled clients. The
[Couchbase Developer Portal](https://developer.couchbase.com/tutorial-quickstart-android-java-sync/)
tutorial will discuss how to configure your App services to enforce relevant
access control, authorization and data routing between Couchbase Lite enabled
clients.
