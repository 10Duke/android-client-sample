# Android Sample for 10Duke SSO and IdP Client

This repository provides a sample Android application, which demonstrates how to use the SSO and
REST-API clients.


## Dependencies

Clone and build following projects:
* [Java-client core libraries (java-client-core)](https://github.com/10Duke/java-client-core)
* [Java-client for 10Duke Identity Provider REST API (java-client-idp)](https://github.com/10Duke/java-client-idp)
* [Android client for 10Duke Identity Provider SSO (android-client-idp-sso)](https://github.com/10Duke/android-client-idp-sso)

Follow the instructions in above projects.


## Building the project

Use Android studio or run following from console:

```console
cd com.tenduke.client.android.sample
./gradlew assembleDebug
```

## Things to look for

* How to declare dependencies? See [`build.gradle` in module `app`](./com.tenduke.client.android.sample/app/build.gradle)
* How to initialize the APIs? See [class `com.tenduke.client.android.sample.singletons.Apis`](./com.tenduke.client.android.sample/app/src/main/java/com/tenduke/client/android/sample/singletons/Apis.java)
* How to use Login/Logout? See methods `login()`, `onActivityResult()` and `handleLoginResult()` in
  [class `BaseActivity`](./com.tenduke.client.android.sample/app/src/main/java/com/tenduke/client/android/sample/BaseActivity.java)
* How to use the REST APIs? These are slightly invisible, as the simple crud-framework does most of
  the work, but see, e.g. methods `com.tenduke.client.android.sample.users.UserListFragment.Adapter.populate()`
  and `com.tenduke.client.android.sample.users.UserDetailFragment.create()`


