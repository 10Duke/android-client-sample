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


## Building all dependencies and this project

Following shell-script can be used to clone the required repositories and build them to assemble the
sample project. The script also downloads and installs the Android SDK.

```console
# Base directory, where the github-repositories are cloned to
REPO_BASE=${HOME}/git/github/10duke

mkdir -p ${REPO_BASE}

# Android SDK is downloaded to ANDROID_BASE
ANDROID_BASE=${HOME}
export ANDROID_HOME=${ANDROID_BASE}/android-sdk-linux


#
# java-client-core
# ================
#
cd ${REPO_BASE}
git clone https://github.com/10Duke/java-client-core.git
cd java-client-core
mvn clean install


#
# java-client-idp
# ================
#
cd ${REPO_BASE}
git clone https://github.com/10Duke/java-client-idp.git
cd java-client-idp
mvn clean install


#
# android-client-idp-sso
# ======================
#
cd ${REPO_BASE}
git clone https://github.com/10Duke/android-client-idp-sso.git

# These versions are necessary for working build:
SDK_VERSION=24.4.1
SUPPORT_VERSION=24.2.1
API_LEVEL=24

# Some following scripts may use SRC, which should point to the project root
SRC=${REPO_BASE}/android-client-idp-sso

# Download, and install the initial Android SDK
mkdir -p ${ANDROID_BASE}
cd ${ANDROID_BASE}

curl -O https://dl.google.com/android/android-sdk_r${SDK_VERSION}-linux.tgz
sha1sum android-sdk_r${SDK_VERSION}-linux.tgz

tar -xzf android-sdk_r${SDK_VERSION}-linux.tgz

# Update and download the missing Android SDK pieces:
cd ${ANDROID_HOME}/tools

# List available packages (no need to execute)
#./android list sdk  --extended --all

# Install needed dependencies
# NOTE: These ask to accept license
./android update sdk --filter android-16,android-24,build-tools-24.0.1,extra-android-m2repository,platform-tools,sys-img-armeabi-v7a-android-16 --all --no-ui

# Support annotations
cd ${ANDROID_HOME}/extras/android/m2repository/com/android/support/support-annotations/${SUPPORT_VERSION}
mvn install:install-file \
  -DpomFile=support-annotations-${SUPPORT_VERSION}.pom \
  -Dfile=support-annotations-${SUPPORT_VERSION}.jar

# The SDK itself
cd ${ANDROID_HOME}/platforms/android-${API_LEVEL}
mvn install:install-file \
  -Dfile=android.jar \
  -DgroupId=com.android \
  -DartifactId=sdk \
  -Dversion=${API_LEVEL} \
  -Dpackaging=jar


# Build the maven-artifacts
cd ${SRC}
mvn clean install

# Build the SSO .aar and install it to the local Maven repository
cd com.tenduke.client.android.sso
./gradlew assembleDebug install

# Build the test
cd ../com.tenduke.client.android.tests
./gradlew assembleDebug


#
# android-client-sample
# =====================
#
cd ${REPO_BASE}
git clone https://github.com/10Duke/android-client-sample.git
cd android-client-sample/com.tenduke.client.android.sample
./gradlew assembleDebug
```
