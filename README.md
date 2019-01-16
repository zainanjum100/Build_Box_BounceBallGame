1.Start Android Studio

2.Click Import project (Eclipse ADT, Gradle, etc.)
3.Copy gradle.properties (android.useDeprecatedNdk=true)
4. build.gradle code below 

5.Edit line 8 inside copied build.gradle file - change application id to your application id.
(we can use existing build.gradle which is generated on import, but you will need add then manual multiple changes - see end of video)
6.While we have multiDexEnabled true inside build.gradle file, we need to make small change in AndroidManifest.xml file to make app works for API 20 and below. 

7.Go to inside app folder to src/main/AndroidManifest.xmland inside application tag add following line:

8.android:name="android.support.multidex.MultiDexApplication"
```
9.Without this line, you can create android app, even publish it, but it will crash on any device with API 20 or lower. - android 4.4W KitKat or older.
10.Party

//Start

apply plugin: 'com.android.application'

android {
    compileSdkVersion 28
    buildToolsVersion "28.0.3"

    defaultConfig {
        applicationId "com.companyname.gamename"
        minSdkVersion 18
        targetSdkVersion 28
        multiDexEnabled true

        ndk {
            moduleName "player_shared"
        }
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.txt'
        }
    }
    sourceSets.main {
        jni.srcDirs = []// <-- disable automatic ndk-build call
    }
}
dependencies {
    implementation ('com.google.android.gms:play-services:+'){exclude group: 'com.android.support'
        exclude module: 'appcompat-v7'
        exclude module: 'support-v4'}
    implementation files('libs/dagger-1.2.2.jar')
    implementation files('libs/javax.inject-1.jar')
    implementation files('libs/nineoldandroids-2.4.0.jar')
    implementation files('libs/support-v4-19.0.1.jar')
    implementation ('com.android.support:multidex:1.0.1')
}
//END
```
    
