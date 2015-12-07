# android-ci-test
[![Build Status](https://drone.io/github.com/bladh/android-ci-test/status.png)](https://drone.io/github.com/bladh/android-ci-test/latest)

## Using drone.io to build android

I used the following configuration:

	export ANDROID_HOME=$(pwd)/android-sdk-linux
	export PATH=${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools:${PATH}
	
	# get Android SDK
	echo "preparing android sdk"
	
	wget -q http://dl.google.com/android/android-sdk_r24.3.3-linux.tgz
	tar xzf android-sdk_r24.3.3-linux.tgz
	
	# prepare build-tools
	android list sdk --all --extended > installsdk.log
	echo y | android update sdk --filter tools --all --no-ui --force >> installsdk.log
	echo y | android update sdk --filter platform-tools --all --no-ui --force >> installsdk.log
	echo y | android update sdk --filter android-$ANDROID_VERSION --all --no-ui --force >> installsdk.log
	echo y | android update sdk --filter build-tools-$BUILD_TOOLS_VERSION --all --no-ui --force >> installsdk.log
	echo y | android update sdk --filter extra-android-support --all --no-ui --force >> installsdk.log
	echo y | android update sdk --filter extra-android-m2repository --all --no-ui --force >> installsdk.log
	echo y | android update sdk --filter extra-google-m2repository --all --no-ui --force >> installsdk.log
	
	echo "preparing gradle..."
	
	./gradlew
	
	echo "building..."
	
	./gradlew build
