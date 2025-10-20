- [x] PodFile
- [x] build.gradle
- [x] versions.xyz.toml
- [ ] search path '/Users/vfa-thanhvh/Library/Caches/Google/AndroidStudio2025.1.4/DerivedData/iosApp-cjjwjneksgsndxarkquuuwfucmpa/Build/Products/Debug-iphoneos/XCFrameworkIntermediates/GoogleAppMeasurement/IdentitySupport' not found

### Links issue with Pods and XcodeBuild
+ Delete Pods and recreate it, then gradlew clean build
+ 
+ 
```
@file:OptIn(ExperimentalKotlinGradlePluginApi::class)  
  
import org.jetbrains.compose.ExperimentalComposeLibrary  
import org.jetbrains.kotlin.gradle.ExperimentalKotlinGradlePluginApi  
import org.jetbrains.kotlin.gradle.plugin.KotlinSourceSetTree  
  
plugins {  
    alias(libs.plugins.multiplatform)  
    alias(libs.plugins.compose.compiler)  
    alias(libs.plugins.compose)  
    alias(libs.plugins.android.application)  
    alias(libs.plugins.kotlinx.serialization)  
    alias(libs.plugins.ksp)  
    alias(libs.plugins.room)  
    alias(libs.plugins.kotlinCocoapods)  
    id("com.google.gms.google-services")  
}  
  
kotlin {  
    jvmToolchain(11)  
    androidTarget {  
        instrumentedTestVariant.sourceSetTree.set(KotlinSourceSetTree.test)  
    }  
  
    iosX64()  
    iosArm64()  
    iosSimulatorArm64()  
  
    cocoapods {  
        summary = "Some description for the Shared Module"  
        homepage = "Link to the Shared Module homepage"  
        version = "1.0"  
        ios.deploymentTarget = "15.4"  
        podfile = project.file("../iosApp/Podfile")  
        framework {  
            baseName = "ComposeApp"  
            isStatic = true  
        }  
  
//        pod("GoogleMaps") {  
//            version = libs.versions.pods.google.maps.get()  
//            extraOpts += listOf("-compiler-option", "-fmodules")  
//        }  
//  
//  
//        pod("Google-Maps-iOS-Utils") {  
//            moduleName = "GoogleMapsUtils"  
//            version = libs.versions.pods.google.ios.maps.utils.get()  
//            extraOpts += listOf("-compiler-option", "-fmodules")  
//        }  
  
    }  
  
    sourceSets {  
        commonMain.dependencies {  
            implementation(compose.runtime)  
            implementation(compose.foundation)  
            implementation(compose.material3)  
            implementation(compose.ui)  
            implementation(compose.components.resources)  
            implementation(compose.components.uiToolingPreview)  
            implementation(libs.ktor.client.core)  
            implementation(libs.ktor.client.content.negotiation)  
            implementation(libs.ktor.client.serialization)  
            implementation(libs.ktor.client.logging)  
            implementation(libs.androidx.lifecycle.viewmodel)  
            implementation(libs.androidx.lifecycle.runtime.compose)  
            implementation(libs.androidx.navigation.composee)  
            implementation(libs.kotlinx.serialization.json)  
  
            //supabase  
            implementation(project.dependencies.platform(libs.supabase.bom))  
            implementation(libs.supabase.postgrest)  
            implementation(libs.supabase.auth)  
            implementation(libs.supabase.realtime)  
  
            implementation(libs.androidx.room.runtime)  
            implementation(libs.sqlite.bundled)  
  
            implementation(libs.bundles.ktor)  
            implementation(libs.bundles.coil)  
  
            implementation(libs.koin.compose)  
            implementation(libs.koin.compose.viewmodel)  
            api(libs.koin.core)  
            implementation(compose.material3)  
            implementation(compose.materialIconsExtended)  
  
            implementation(libs.firebase.gitlive.common)  
            implementation(libs.firebase.gitlive.auth)  
        }  
  
        commonTest.dependencies {  
            implementation(kotlin("test"))  
            @OptIn(ExperimentalComposeLibrary::class)  
            implementation(compose.uiTest)  
        }  
  
        androidMain.dependencies {  
            implementation(compose.uiTooling)  
            implementation(libs.androidx.activityCompose)  
            implementation(libs.ktor.client.okhttp)  
            implementation(libs.ktor.client.android)  
            implementation(compose.preview)  
  
            implementation(libs.koin.android)  
            implementation(libs.koin.androidx.compose)  
  
            implementation(libs.play.services.auth)  
        }  
  
        iosMain.dependencies {  
            implementation(libs.ktor.client.darwin)  
            implementation(libs.ktor.client.ios)  
        }  
        room {  
            schemaDirectory("$projectDir/schemas")  
        }  
    }}  
  
android {  
    namespace = "com.thanh0x.supabaseselfhost"  
    compileSdk = 36  
  
    defaultConfig {  
        minSdk = 23  
        targetSdk = 35  
        applicationId = "com.thanh0x.FocusBloom"  
        versionCode = 1  
        versionName = "1.0.0"  
  
        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"  
    }  
    buildTypes {  
        getByName("release") {  
            isMinifyEnabled = false  
        }  
    }    dependencies {  
        ksp(libs.androidx.room.compiler)  
    }  
}  
  
dependencies {  
    androidTestImplementation(libs.androidx.uitest.junit4)  
    debugImplementation(libs.androidx.uitest.testManifest)  
    debugImplementation(compose.uiTooling)  
}


----

plugins {  
    alias(libs.plugins.multiplatform).apply(false)  
    alias(libs.plugins.compose.compiler).apply(false)  
    alias(libs.plugins.compose).apply(false)  
    alias(libs.plugins.android.application).apply(false)  
    alias(libs.plugins.kotlinx.serialization).apply(false)  
    alias(libs.plugins.ksp) apply false  
    alias(libs.plugins.room) apply false  
    alias(libs.plugins.kotlinCocoapods).apply(false)  
    id("com.google.gms.google-services") version "4.4.0" apply false  
}

----

platform :ios, '15.4'
target 'iosApp' do
  use_frameworks!
  platform :ios, '14.1'

  # Include the KMP shared module as a pod
#   pod 'ComposeApp', :path => '../composeApp'

  # Add any other native iOS pods here
  # pod 'GoogleSignIn'

 
  pod 'composeApp', :path => '../composeApp'
  pod 'GoogleMaps', '8.4.0'
  pod 'Google-Maps-iOS-Utils', '5.0.0'
  pod 'GoogleSignIn'
  pod 'Firebase/Auth'
  pod 'Firebase/Core'
  
end


-----


PODS:  
  - AppAuth (2.0.0):  
    - AppAuth/Core (= 2.0.0)  
    - AppAuth/ExternalUserAgent (= 2.0.0)  
  - AppAuth/Core (2.0.0)  
  - AppAuth/ExternalUserAgent (2.0.0):  
    - AppAuth/Core  
  - AppCheckCore (11.2.0):  
    - GoogleUtilities/Environment (~> 8.0)  
    - GoogleUtilities/UserDefaults (~> 8.0)  
    - PromisesObjC (~> 2.4)  
  - composeApp (1.0)  
  - Firebase/Auth (12.4.0):  
    - Firebase/CoreOnly  
    - FirebaseAuth (~> 12.4.0)  
  - Firebase/Core (12.4.0):  
    - Firebase/CoreOnly  
    - FirebaseAnalytics (~> 12.4.0)  
  - Firebase/CoreOnly (12.4.0):  
    - FirebaseCore (~> 12.4.0)  
  - FirebaseAnalytics (12.4.0):  
    - FirebaseAnalytics/Default (= 12.4.0)  
    - FirebaseCore (~> 12.4.0)  
    - FirebaseInstallations (~> 12.4.0)  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/MethodSwizzler (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
    - nanopb (~> 3.30910.0)  
  - FirebaseAnalytics/Default (12.4.0):  
    - FirebaseCore (~> 12.4.0)  
    - FirebaseInstallations (~> 12.4.0)  
    - GoogleAppMeasurement/Default (= 12.4.0)  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/MethodSwizzler (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
    - nanopb (~> 3.30910.0)  
  - FirebaseAppCheckInterop (12.4.0)  
  - FirebaseAuth (12.4.0):  
    - FirebaseAppCheckInterop (~> 12.4.0)  
    - FirebaseAuthInterop (~> 12.4.0)  
    - FirebaseCore (~> 12.4.0)  
    - FirebaseCoreExtension (~> 12.4.0)  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/Environment (~> 8.1)  
    - GTMSessionFetcher/Core (< 6.0, >= 3.4)  
    - RecaptchaInterop (~> 101.0)  
  - FirebaseAuthInterop (12.4.0)  
  - FirebaseCore (12.4.0):  
    - FirebaseCoreInternal (~> 12.4.0)  
    - GoogleUtilities/Environment (~> 8.1)  
    - GoogleUtilities/Logger (~> 8.1)  
  - FirebaseCoreExtension (12.4.0):  
    - FirebaseCore (~> 12.4.0)  
  - FirebaseCoreInternal (12.4.0):  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
  - FirebaseInstallations (12.4.0):  
    - FirebaseCore (~> 12.4.0)  
    - GoogleUtilities/Environment (~> 8.1)  
    - GoogleUtilities/UserDefaults (~> 8.1)  
    - PromisesObjC (~> 2.4)  
  - Google-Maps-iOS-Utils (5.0.0):  
    - GoogleMaps (~> 8.0)  
  - GoogleAdsOnDeviceConversion (3.1.0):  
    - GoogleUtilities/Environment (~> 8.1)  
    - GoogleUtilities/Logger (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - nanopb (~> 3.30910.0)  
  - GoogleAppMeasurement/Core (12.4.0):  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/MethodSwizzler (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
    - nanopb (~> 3.30910.0)  
  - GoogleAppMeasurement/Default (12.4.0):  
    - GoogleAdsOnDeviceConversion (~> 3.1.0)  
    - GoogleAppMeasurement/Core (= 12.4.0)  
    - GoogleAppMeasurement/IdentitySupport (= 12.4.0)  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/MethodSwizzler (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
    - nanopb (~> 3.30910.0)  
  - GoogleAppMeasurement/IdentitySupport (12.4.0):  
    - GoogleAppMeasurement/Core (= 12.4.0)  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/MethodSwizzler (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
    - nanopb (~> 3.30910.0)  
  - GoogleMaps (8.4.0):  
    - GoogleMaps/Maps (= 8.4.0)  
  - GoogleMaps/Base (8.4.0)  
  - GoogleMaps/Maps (8.4.0):  
    - GoogleMaps/Base  
  - GoogleSignIn (9.0.0):  
    - AppAuth (~> 2.0)  
    - AppCheckCore (~> 11.0)  
    - GTMAppAuth (~> 5.0)  
    - GTMSessionFetcher/Core (~> 3.3)  
  - GoogleUtilities/AppDelegateSwizzler (8.1.0):  
    - GoogleUtilities/Environment  
    - GoogleUtilities/Logger  
    - GoogleUtilities/Network  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/Environment (8.1.0):  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/Logger (8.1.0):  
    - GoogleUtilities/Environment  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/MethodSwizzler (8.1.0):  
    - GoogleUtilities/Logger  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/Network (8.1.0):  
    - GoogleUtilities/Logger  
    - "GoogleUtilities/NSData+zlib"  
    - GoogleUtilities/Privacy  
    - GoogleUtilities/Reachability  
  - "GoogleUtilities/NSData+zlib (8.1.0)":  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/Privacy (8.1.0)  
  - GoogleUtilities/Reachability (8.1.0):  
    - GoogleUtilities/Logger  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/UserDefaults (8.1.0):  
    - GoogleUtilities/Logger  
    - GoogleUtilities/Privacy  
  - GTMAppAuth (5.0.0):  
    - AppAuth/Core (~> 2.0)  
    - GTMSessionFetcher/Core (< 4.0, >= 3.3)  
  - GTMSessionFetcher/Core (3.5.0)  
  - nanopb (3.30910.0):  
    - nanopb/decode (= 3.30910.0)  
    - nanopb/encode (= 3.30910.0)  
  - nanopb/decode (3.30910.0)  
  - nanopb/encode (3.30910.0)  
  - PromisesObjC (2.4.0)  
  - RecaptchaInterop (101.0.0)  
  
DEPENDENCIES:  
  - composeApp (from `../composeApp`)  
  - Firebase/Auth  
  - Firebase/Core  
  - Google-Maps-iOS-Utils (= 5.0.0)  
  - GoogleMaps (= 8.4.0)  
  - GoogleSignIn  
  
SPEC REPOS:  
  trunk:  
    - AppAuth  
    - AppCheckCore  
    - Firebase  
    - FirebaseAnalytics  
    - FirebaseAppCheckInterop  
    - FirebaseAuth  
    - FirebaseAuthInterop  
    - FirebaseCore  
    - FirebaseCoreExtension  
    - FirebaseCoreInternal  
    - FirebaseInstallations  
    - Google-Maps-iOS-Utils  
    - GoogleAdsOnDeviceConversion  
    - GoogleAppMeasurement  
    - GoogleMaps  
    - GoogleSignIn  
    - GoogleUtilities  
    - GTMAppAuth  
    - GTMSessionFetcher  
    - nanopb  
    - PromisesObjC  
    - RecaptchaInterop  
  
EXTERNAL SOURCES:  
  composeApp:  
    :path: "../composeApp"  
  
SPEC CHECKSUMS:  
  AppAuth: 1c1a8afa7e12f2ec3a294d9882dfa5ab7d3cb063  
  AppCheckCore: cc8fd0a3a230ddd401f326489c99990b013f0c4f  
  composeApp: 566a3098fb261e357240ba9987ddeeda065a21e3  
  Firebase: f07b15ae5a6ec0f93713e30b923d9970d144af3e  
  FirebaseAnalytics: 0fc2b20091f0ddd21bf73397cf8f0eb5346dc24f  
  FirebaseAppCheckInterop: f734c802f21fe1da0837708f0f9a27218c8a4ed0  
  FirebaseAuth: 4a2aed737c84114a9d9b33d11ae1b147d6b94889  
  FirebaseAuthInterop: 858e6b754966e70740a4370dd1503dfffe6dbb49  
  FirebaseCore: bb595f3114953664e3c1dc032f008a244147cfd3  
  FirebaseCoreExtension: 7e1f7118ee970e001a8013719fb90950ee5e0018  
  FirebaseCoreInternal: d7f5a043c2cd01a08103ab586587c1468047bca6  
  FirebaseInstallations: ae9f4902cb5bf1d0c5eaa31ec1f4e5495a0714e2  
  Google-Maps-iOS-Utils: 66d6de12be1ce6d3742a54661e7a79cb317a9321  
  GoogleAdsOnDeviceConversion: e03a386840803ea7eef3fd22a061930142c039c1  
  GoogleAppMeasurement: 1e718274b7e015cefd846ac1fcf7820c70dc017d  
  GoogleMaps: 8939898920281c649150e0af74aa291c60f2e77d  
  GoogleSignIn: c7f09cfbc85a1abf69187be091997c317cc33b77  
  GoogleUtilities: 00c88b9a86066ef77f0da2fab05f65d7768ed8e1  
  GTMAppAuth: 217a876b249c3c585a54fd6f73e6b58c4f5c4238  
  GTMSessionFetcher: 5aea5ba6bd522a239e236100971f10cb71b96ab6  
  nanopb: fad817b59e0457d11a5dfbde799381cd727c1275  
  PromisesObjC: f5707f49cb48b9636751c5b2e7d227e43fba9f47  
  RecaptchaInterop: 11e0b637842dfb48308d242afc3f448062325aba  
  
PODFILE CHECKSUM: e279ae19668c627d3df392e9c72bff7f62fa7279  
  
COCOAPODS: 1.16.2

----

[versions]  
  
kotlin = "2.2.21-RC2"  
compose = "1.9.1"  
agp = "8.13.0"  
androidx-activityCompose = "1.11.0"  
androidx-uiTest = "1.9.3"  
coil3 = "3.3.0"  
koin = "4.1.1"  
ktor = "3.3.1"  
androidx-lifecycle = "2.9.4"  
androidx-navigation = "2.9.0"  
kotlinx-serialization = "1.9.0"  
ksp = "2.2.20-2.0.4" #Kotlin Symbol Processing. process annotations at compile time(generate code. Such as: room, koin)  
room = "2.8.2" #The room for Jetpack Compose instead of SQLDelight  
sqlite = "2.6.1"  
supabase = "3.2.4"  
playServicesAuth = "21.0.0"  
gitlive-firebase = "1.10.4"  
[libraries]  
  
androidx-activityCompose = { module = "androidx.activity:activity-compose", version.ref = "androidx-activityCompose" }  
androidx-uitest-testManifest = { module = "androidx.compose.ui:ui-test-manifest", version.ref = "androidx-uiTest" }  
androidx-uitest-junit4 = { module = "androidx.compose.ui:ui-test-junit4", version.ref = "androidx-uiTest" }  
supabase-bom = { module = "io.github.jan-tennert.supabase:bom", version.ref = "supabase" }  
supabase-auth = { module = "io.github.jan-tennert.supabase:auth-kt" }  
  
# A chain of dependencies for Koin: core -> android -> androidx-compose | compose-viewmodel  
koin-android = { module = "io.insert-koin:koin-android", version.ref = "koin" }  
koin-androidx-compose = { module = "io.insert-koin:koin-androidx-compose", version.ref = "koin" }  
koin-core = { module = "io.insert-koin:koin-core", version.ref = "koin" }  
koin-compose = { module = "io.insert-koin:koin-compose", version.ref = "koin" }  
koin-compose-viewmodel = { module = "io.insert-koin:koin-compose-viewmodel", version.ref = "koin" }  
  
#ktor  
ktor-client-android = { module = "io.ktor:ktor-client-android", version.ref = "ktor" }  
ktor-client-core = { module = "io.ktor:ktor-client-core", version.ref = "ktor" }  
ktor-client-content-negotiation = { module = "io.ktor:ktor-client-content-negotiation", version.ref = "ktor" }  
ktor-client-ios = { module = "io.ktor:ktor-client-ios", version.ref = "ktor" }  
ktor-client-serialization = { module = "io.ktor:ktor-client-serialization", version.ref = "ktor" }  
ktor-client-logging = { module = "io.ktor:ktor-client-logging", version.ref = "ktor" }  
ktor-client-darwin = { module = "io.ktor:ktor-client-darwin", version.ref = "ktor" }  
ktor-client-okhttp = { module = "io.ktor:ktor-client-okhttp", version.ref = "ktor" }  
ktor-client-js = { module = "io.ktor:ktor-client-js", version.ref = "ktor" }  
ktor-client-curl = { module = "io.ktor:ktor-client-curl", version.ref = "ktor" }  
ktor-client-winhttp = { module = "io.ktor:ktor-client-winhttp", version.ref = "ktor" }  
androidx-lifecycle-viewmodel = { module = "org.jetbrains.androidx.lifecycle:lifecycle-viewmodel", version.ref = "androidx-lifecycle" }  
androidx-lifecycle-runtime-compose = { module = "org.jetbrains.androidx.lifecycle:lifecycle-runtime-compose", version.ref = "androidx-lifecycle" }  
androidx-navigation-composee = { module = "org.jetbrains.androidx.navigation:navigation-compose", version.ref = "androidx-navigation" }  
  
# We have room and doesn't need to defined for each platform  
androidx-room-compiler = { group = "androidx.room", name = "room-compiler", version.ref = "room" }  
androidx-room-runtime = { group = "androidx.room", name = "room-runtime", version.ref = "room" }  
sqlite-bundled = { module = "androidx.sqlite:sqlite-bundled", version.ref = "sqlite" }  
  
# A chain of dependencies for Coil: core -> network-ktor2 | network-ktor3 -> compose-core -> compose  
coil-compose = { module = "io.coil-kt.coil3:coil-compose", version.ref = "coil3" }  
coil-compose-core = { module = "io.coil-kt.coil3:coil-compose-core", version.ref = "coil3" }  
coil-network-ktor2 = { module = "io.coil-kt.coil3:coil-network-ktor2", version.ref = "coil3" }  
coil-network-ktor3 = { module = "io.coil-kt.coil3:coil-network-ktor3", version.ref = "coil3" }  
coil-mp = { module = "io.coil-kt.coil3:coil", version.ref = "coil3" }#specically for multiplatform (Multiplatform)  
  
#Supabase  
kotlinx-serialization-json = { module = "org.jetbrains.kotlinx:kotlinx-serialization-json", version.ref = "kotlinx-serialization" }  
supabase-realtime = { module = "io.github.jan-tennert.supabase:realtime-kt" }  
supabase-postgrest = { module = "io.github.jan-tennert.supabase:postgrest-kt" }  
  
#Firebase  
firebase-gitlive-common = { module = "dev.gitlive:firebase-common", version.ref = "gitlive-firebase" }  
firebase-gitlive-auth = { module = "dev.gitlive:firebase-auth", version.ref = "gitlive-firebase" }  
  
play-services-auth = { module = "com.google.android.gms:play-services-auth", version.ref = "playServicesAuth" }  
  
  
[plugins]  
  
multiplatform = { id = "org.jetbrains.kotlin.multiplatform", version.ref = "kotlin" }  
compose-compiler = { id = "org.jetbrains.kotlin.plugin.compose", version.ref = "kotlin" }  
compose = { id = "org.jetbrains.compose", version.ref = "compose" }  
android-application = { id = "com.android.application", version.ref = "agp" }  
kotlinx-serialization = { id = "org.jetbrains.kotlin.plugin.serialization", version.ref = "kotlin" }  
ksp = { id = "com.google.devtools.ksp", version.ref = "ksp" }  
room = { id = "androidx.room", version.ref = "room" }  
kotlinCocoapods = { id = "org.jetbrains.kotlin.native.cocoapods", version.ref = "kotlin" }  
  
[bundles]  
# A bundle of related dependencies to use the reference in single line  
#Example usage:  
# implementation(libs.bundles.ktor)  
ktor = [  
    "ktor-client-core",  
    "ktor-client-content-negotiation",  
    "ktor-client-logging",  
]  
coil = [  
    "coil-compose",  
    "coil-compose-core",  
    "coil-network-ktor2",  
    "coil-network-ktor3",  
    "coil-mp"  
]
-----

// !$*UTF8*$!  
{  
    archiveVersion = 1;  
    classes = {  
    };  
    objectVersion = 56;  
    objects = {  
  
/* Begin PBXBuildFile section */  
       6CEC3A2849C2E3019442F82F /* Pods_iosApp.framework in Frameworks */ = {isa = PBXBuildFile; fileRef = B0AFA1B6EFED2FE6CF6D790E /* Pods_iosApp.framework */; };  
       A93A953B29CC810C00F8E227 /* iosApp.swift in Sources */ = {isa = PBXBuildFile; fileRef = A93A953A29CC810C00F8E227 /* iosApp.swift */; };  
       A93A953F29CC810D00F8E227 /* Assets.xcassets in Resources */ = {isa = PBXBuildFile; fileRef = A93A953E29CC810D00F8E227 /* Assets.xcassets */; };  
       A93A954229CC810D00F8E227 /* Preview Assets.xcassets in Resources */ = {isa = PBXBuildFile; fileRef = A93A954129CC810D00F8E227 /* Preview Assets.xcassets */; };  
/* End PBXBuildFile section */  
  
/* Begin PBXFileReference section */  
       8C04C9AACE04C90505E2C4B3 /* Pods-iosApp.release.xcconfig */ = {isa = PBXFileReference; includeInIndex = 1; lastKnownFileType = text.xcconfig; name = "Pods-iosApp.release.xcconfig"; path = "Target Support Files/Pods-iosApp/Pods-iosApp.release.xcconfig"; sourceTree = "<group>"; };  
       A93A953729CC810C00F8E227 /* SupabaseSelfHostPlayGround.app */ = {isa = PBXFileReference; explicitFileType = wrapper.application; includeInIndex = 0; path = SupabaseSelfHostPlayGround.app; sourceTree = BUILT_PRODUCTS_DIR; };  
       A93A953A29CC810C00F8E227 /* iosApp.swift */ = {isa = PBXFileReference; lastKnownFileType = sourcecode.swift; path = iosApp.swift; sourceTree = "<group>"; };  
       A93A953E29CC810D00F8E227 /* Assets.xcassets */ = {isa = PBXFileReference; lastKnownFileType = folder.assetcatalog; path = Assets.xcassets; sourceTree = "<group>"; };  
       A93A954129CC810D00F8E227 /* Preview Assets.xcassets */ = {isa = PBXFileReference; lastKnownFileType = folder.assetcatalog; path = "Preview Assets.xcassets"; sourceTree = "<group>"; };  
       B0AFA1B6EFED2FE6CF6D790E /* Pods_iosApp.framework */ = {isa = PBXFileReference; explicitFileType = wrapper.framework; includeInIndex = 0; path = Pods_iosApp.framework; sourceTree = BUILT_PRODUCTS_DIR; };  
       C2B975E0314AB5EC160B7747 /* Pods-iosApp.debug.xcconfig */ = {isa = PBXFileReference; includeInIndex = 1; lastKnownFileType = text.xcconfig; name = "Pods-iosApp.debug.xcconfig"; path = "Target Support Files/Pods-iosApp/Pods-iosApp.debug.xcconfig"; sourceTree = "<group>"; };  
/* End PBXFileReference section */  
  
/* Begin PBXFrameworksBuildPhase section */  
       A93A953429CC810C00F8E227 /* Frameworks */ = {  
          isa = PBXFrameworksBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
             6CEC3A2849C2E3019442F82F /* Pods_iosApp.framework in Frameworks */,  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
       };  
/* End PBXFrameworksBuildPhase section */  
  
/* Begin PBXGroup section */  
       9351F99B501DA74B70C82556 /* Pods */ = {  
          isa = PBXGroup;  
          children = (  
             C2B975E0314AB5EC160B7747 /* Pods-iosApp.debug.xcconfig */,  
             8C04C9AACE04C90505E2C4B3 /* Pods-iosApp.release.xcconfig */,  
          );  
          path = Pods;  
          sourceTree = "<group>";  
       };  
       A93A952E29CC810C00F8E227 = {  
          isa = PBXGroup;  
          children = (  
             A93A953929CC810C00F8E227 /* iosApp */,  
             A93A953829CC810C00F8E227 /* Products */,  
             C4127409AE3703430489E7BC /* Frameworks */,  
             9351F99B501DA74B70C82556 /* Pods */,  
          );  
          sourceTree = "<group>";  
       };  
       A93A953829CC810C00F8E227 /* Products */ = {  
          isa = PBXGroup;  
          children = (  
             A93A953729CC810C00F8E227 /* SupabaseSelfHostPlayGround.app */,  
          );  
          name = Products;  
          sourceTree = "<group>";  
       };  
       A93A953929CC810C00F8E227 /* iosApp */ = {  
          isa = PBXGroup;  
          children = (  
             A93A953A29CC810C00F8E227 /* iosApp.swift */,  
             A93A953E29CC810D00F8E227 /* Assets.xcassets */,  
             A93A954029CC810D00F8E227 /* Preview Content */,  
          );  
          path = iosApp;  
          sourceTree = "<group>";  
       };  
       A93A954029CC810D00F8E227 /* Preview Content */ = {  
          isa = PBXGroup;  
          children = (  
             A93A954129CC810D00F8E227 /* Preview Assets.xcassets */,  
          );  
          path = "Preview Content";  
          sourceTree = "<group>";  
       };  
       C4127409AE3703430489E7BC /* Frameworks */ = {  
          isa = PBXGroup;  
          children = (  
             B0AFA1B6EFED2FE6CF6D790E /* Pods_iosApp.framework */,  
          );  
          name = Frameworks;  
          sourceTree = "<group>";  
       };  
/* End PBXGroup section */  
  
/* Begin PBXNativeTarget section */  
       A93A953629CC810C00F8E227 /* iosApp */ = {  
          isa = PBXNativeTarget;  
          buildConfigurationList = A93A954529CC810D00F8E227 /* Build configuration list for PBXNativeTarget "iosApp" */;  
          buildPhases = (  
             EE9766890392CA68D8BEC09C /* [CP] Check Pods Manifest.lock */,  
             A93A953329CC810C00F8E227 /* Sources */,  
             A93A953429CC810C00F8E227 /* Frameworks */,  
             A93A953529CC810C00F8E227 /* Resources */,  
             CBD59A5413E38BECCE9E1030 /* [CP] Copy Pods Resources */,  
             D4B2CD952EA52C17009109E2 /* [CP] Embed Pods Frameworks */,  
          );  
          buildRules = (  
          );  
          dependencies = (  
          );  
          name = iosApp;  
          productName = iosApp;  
          productReference = A93A953729CC810C00F8E227 /* SupabaseSelfHostPlayGround.app */;  
          productType = "com.apple.product-type.application";  
       };  
/* End PBXNativeTarget section */  
  
/* Begin PBXProject section */  
       A93A952F29CC810C00F8E227 /* Project object */ = {  
          isa = PBXProject;  
          attributes = {  
             LastSwiftUpdateCheck = 1420;  
             LastUpgradeCheck = 1420;  
             TargetAttributes = {  
                A93A953629CC810C00F8E227 = {  
                   CreatedOnToolsVersion = 14.2;  
                };  
             };  
          };  
          buildConfigurationList = A93A953229CC810C00F8E227 /* Build configuration list for PBXProject "iosApp" */;  
          compatibilityVersion = "Xcode 14.0";  
          developmentRegion = en;  
          hasScannedForEncodings = 0;  
          knownRegions = (  
             en,  
             Base,  
          );  
          mainGroup = A93A952E29CC810C00F8E227;  
          productRefGroup = A93A953829CC810C00F8E227 /* Products */;  
          projectDirPath = "";  
          projectRoot = "";  
          targets = (  
             A93A953629CC810C00F8E227 /* iosApp */,  
          );  
       };  
/* End PBXProject section */  
  
/* Begin PBXResourcesBuildPhase section */  
       A93A953529CC810C00F8E227 /* Resources */ = {  
          isa = PBXResourcesBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
             A93A954229CC810D00F8E227 /* Preview Assets.xcassets in Resources */,  
             A93A953F29CC810D00F8E227 /* Assets.xcassets in Resources */,  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
       };  
/* End PBXResourcesBuildPhase section */  
  
/* Begin PBXShellScriptBuildPhase section */  
       CBD59A5413E38BECCE9E1030 /* [CP] Copy Pods Resources */ = {  
          isa = PBXShellScriptBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
          );  
          inputFileListPaths = (  
             "${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-resources-${CONFIGURATION}-input-files.xcfilelist",  
          );  
          inputPaths = (  
          );  
          name = "[CP] Copy Pods Resources";  
          outputFileListPaths = (  
             "${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-resources-${CONFIGURATION}-output-files.xcfilelist",  
          );  
          outputPaths = (  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
          shellPath = /bin/sh;  
          shellScript = "\"${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-resources.sh\"\n";  
          showEnvVarsInLog = 0;  
       };  
       D4B2CD952EA52C17009109E2 /* [CP] Embed Pods Frameworks */ = {  
          isa = PBXShellScriptBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
          );  
          inputFileListPaths = (  
             "${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-frameworks-${CONFIGURATION}-input-files.xcfilelist",  
          );  
          inputPaths = (  
          );  
          name = "[CP] Embed Pods Frameworks";  
          outputFileListPaths = (  
             "${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-frameworks-${CONFIGURATION}-output-files.xcfilelist",  
          );  
          outputPaths = (  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
          shellPath = /bin/sh;  
          shellScript = "\"${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-frameworks.sh\"\n";  
          showEnvVarsInLog = 0;  
       };  
       EE9766890392CA68D8BEC09C /* [CP] Check Pods Manifest.lock */ = {  
          isa = PBXShellScriptBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
          );  
          inputFileListPaths = (  
          );  
          inputPaths = (  
             "${PODS_PODFILE_DIR_PATH}/Podfile.lock",  
             "${PODS_ROOT}/Manifest.lock",  
          );  
          name = "[CP] Check Pods Manifest.lock";  
          outputFileListPaths = (  
          );  
          outputPaths = (  
             "$(DERIVED_FILE_DIR)/Pods-iosApp-checkManifestLockResult.txt",  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
          shellPath = /bin/sh;  
          shellScript = "diff \"${PODS_PODFILE_DIR_PATH}/Podfile.lock\" \"${PODS_ROOT}/Manifest.lock\" > /dev/null\nif [ $? != 0 ] ; then\n    # print error to STDERR\n    echo \"error: The sandbox is not in sync with the Podfile.lock. Run 'pod install' or update your CocoaPods installation.\" >&2\n    exit 1\nfi\n# This output is used by Xcode 'outputs' to avoid re-running this script phase.\necho \"SUCCESS\" > \"${SCRIPT_OUTPUT_FILE_0}\"\n";  
          showEnvVarsInLog = 0;  
       };  
/* End PBXShellScriptBuildPhase section */  
  
/* Begin PBXSourcesBuildPhase section */  
       A93A953329CC810C00F8E227 /* Sources */ = {  
          isa = PBXSourcesBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
             A93A953B29CC810C00F8E227 /* iosApp.swift in Sources */,  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
       };  
/* End PBXSourcesBuildPhase section */  
  
/* Begin XCBuildConfiguration section */  
       A93A954329CC810D00F8E227 /* Debug */ = {  
          isa = XCBuildConfiguration;  
          buildSettings = {  
             ALWAYS_SEARCH_USER_PATHS = NO;  
             CLANG_ANALYZER_NONNULL = YES;  
             CLANG_ANALYZER_NUMBER_OBJECT_CONVERSION = YES_AGGRESSIVE;  
             CLANG_CXX_LANGUAGE_STANDARD = "gnu++20";  
             CLANG_ENABLE_MODULES = YES;  
             CLANG_ENABLE_OBJC_ARC = YES;  
             CLANG_ENABLE_OBJC_WEAK = YES;  
             CLANG_WARN_BLOCK_CAPTURE_AUTORELEASING = YES;  
             CLANG_WARN_BOOL_CONVERSION = YES;  
             CLANG_WARN_COMMA = YES;  
             CLANG_WARN_CONSTANT_CONVERSION = YES;  
             CLANG_WARN_DEPRECATED_OBJC_IMPLEMENTATIONS = YES;  
             CLANG_WARN_DIRECT_OBJC_ISA_USAGE = YES_ERROR;  
             CLANG_WARN_DOCUMENTATION_COMMENTS = YES;  
             CLANG_WARN_EMPTY_BODY = YES;  
             CLANG_WARN_ENUM_CONVERSION = YES;  
             CLANG_WARN_INFINITE_RECURSION = YES;  
             CLANG_WARN_INT_CONVERSION = YES;  
             CLANG_WARN_NON_LITERAL_NULL_CONVERSION = YES;  
             CLANG_WARN_OBJC_IMPLICIT_RETAIN_SELF = YES;  
             CLANG_WARN_OBJC_LITERAL_CONVERSION = YES;  
             CLANG_WARN_OBJC_ROOT_CLASS = YES_ERROR;  
             CLANG_WARN_QUOTED_INCLUDE_IN_FRAMEWORK_HEADER = YES;  
             CLANG_WARN_RANGE_LOOP_ANALYSIS = YES;  
             CLANG_WARN_STRICT_PROTOTYPES = YES;  
             CLANG_WARN_SUSPICIOUS_MOVE = YES;  
             CLANG_WARN_UNGUARDED_AVAILABILITY = YES_AGGRESSIVE;  
             CLANG_WARN_UNREACHABLE_CODE = YES;  
             CLANG_WARN__DUPLICATE_METHOD_MATCH = YES;  
             COPY_PHASE_STRIP = NO;  
             DEBUG_INFORMATION_FORMAT = dwarf;  
             ENABLE_STRICT_OBJC_MSGSEND = YES;  
             ENABLE_TESTABILITY = YES;  
             GCC_C_LANGUAGE_STANDARD = gnu11;  
             GCC_DYNAMIC_NO_PIC = NO;  
             GCC_NO_COMMON_BLOCKS = YES;  
             GCC_OPTIMIZATION_LEVEL = 0;  
             GCC_PREPROCESSOR_DEFINITIONS = (  
                "DEBUG=1",  
                "$(inherited)",  
             );  
             GCC_WARN_64_TO_32_BIT_CONVERSION = YES;  
             GCC_WARN_ABOUT_RETURN_TYPE = YES_ERROR;  
             GCC_WARN_UNDECLARED_SELECTOR = YES;  
             GCC_WARN_UNINITIALIZED_AUTOS = YES_AGGRESSIVE;  
             GCC_WARN_UNUSED_FUNCTION = YES;  
             GCC_WARN_UNUSED_VARIABLE = YES;  
             IPHONEOS_DEPLOYMENT_TARGET = 16.2;  
             MTL_ENABLE_DEBUG_INFO = INCLUDE_SOURCE;  
             MTL_FAST_MATH = YES;  
             ONLY_ACTIVE_ARCH = YES;  
             SDKROOT = iphoneos;  
             SWIFT_ACTIVE_COMPILATION_CONDITIONS = DEBUG;  
             SWIFT_OPTIMIZATION_LEVEL = "-Onone";  
          };  
          name = Debug;  
       };  
       A93A954429CC810D00F8E227 /* Release */ = {  
          isa = XCBuildConfiguration;  
          buildSettings = {  
             ALWAYS_SEARCH_USER_PATHS = NO;  
             CLANG_ANALYZER_NONNULL = YES;  
             CLANG_ANALYZER_NUMBER_OBJECT_CONVERSION = YES_AGGRESSIVE;  
             CLANG_CXX_LANGUAGE_STANDARD = "gnu++20";  
             CLANG_ENABLE_MODULES = YES;  
             CLANG_ENABLE_OBJC_ARC = YES;  
             CLANG_ENABLE_OBJC_WEAK = YES;  
             CLANG_WARN_BLOCK_CAPTURE_AUTORELEASING = YES;  
             CLANG_WARN_BOOL_CONVERSION = YES;  
             CLANG_WARN_COMMA = YES;  
             CLANG_WARN_CONSTANT_CONVERSION = YES;  
             CLANG_WARN_DEPRECATED_OBJC_IMPLEMENTATIONS = YES;  
             CLANG_WARN_DIRECT_OBJC_ISA_USAGE = YES_ERROR;  
             CLANG_WARN_DOCUMENTATION_COMMENTS = YES;  
             CLANG_WARN_EMPTY_BODY = YES;  
             CLANG_WARN_ENUM_CONVERSION = YES;  
             CLANG_WARN_INFINITE_RECURSION = YES;  
             CLANG_WARN_INT_CONVERSION = YES;  
             CLANG_WARN_NON_LITERAL_NULL_CONVERSION = YES;  
             CLANG_WARN_OBJC_IMPLICIT_RETAIN_SELF = YES;  
             CLANG_WARN_OBJC_LITERAL_CONVERSION = YES;  
             CLANG_WARN_OBJC_ROOT_CLASS = YES_ERROR;  
             CLANG_WARN_QUOTED_INCLUDE_IN_FRAMEWORK_HEADER = YES;  
             CLANG_WARN_RANGE_LOOP_ANALYSIS = YES;  
             CLANG_WARN_STRICT_PROTOTYPES = YES;  
             CLANG_WARN_SUSPICIOUS_MOVE = YES;  
             CLANG_WARN_UNGUARDED_AVAILABILITY = YES_AGGRESSIVE;  
             CLANG_WARN_UNREACHABLE_CODE = YES;  
             CLANG_WARN__DUPLICATE_METHOD_MATCH = YES;  
             COPY_PHASE_STRIP = NO;  
             DEBUG_INFORMATION_FORMAT = "dwarf-with-dsym";  
             ENABLE_NS_ASSERTIONS = NO;  
             ENABLE_STRICT_OBJC_MSGSEND = YES;  
             GCC_C_LANGUAGE_STANDARD = gnu11;  
             GCC_NO_COMMON_BLOCKS = YES;  
             GCC_WARN_64_TO_32_BIT_CONVERSION = YES;  
             GCC_WARN_ABOUT_RETURN_TYPE = YES_ERROR;  
             GCC_WARN_UNDECLARED_SELECTOR = YES;  
             GCC_WARN_UNINITIALIZED_AUTOS = YES_AGGRESSIVE;  
             GCC_WARN_UNUSED_FUNCTION = YES;  
             GCC_WARN_UNUSED_VARIABLE = YES;  
             IPHONEOS_DEPLOYMENT_TARGET = 16.2;  
             MTL_ENABLE_DEBUG_INFO = NO;  
             MTL_FAST_MATH = YES;  
             SDKROOT = iphoneos;  
             SWIFT_COMPILATION_MODE = wholemodule;  
             SWIFT_OPTIMIZATION_LEVEL = "-O";  
             VALIDATE_PRODUCT = YES;  
          };  
          name = Release;  
       };  
       A93A954629CC810D00F8E227 /* Debug */ = {  
          isa = XCBuildConfiguration;  
          baseConfigurationReference = C2B975E0314AB5EC160B7747 /* Pods-iosApp.debug.xcconfig */;  
          buildSettings = {  
             ASSETCATALOG_COMPILER_APPICON_NAME = AppIcon;  
             ASSETCATALOG_COMPILER_GLOBAL_ACCENT_COLOR_NAME = AccentColor;  
             CODE_SIGN_STYLE = Automatic;  
             CURRENT_PROJECT_VERSION = 1;  
             DEVELOPMENT_ASSET_PATHS = "\"iosApp/Preview Content\"";  
             DEVELOPMENT_TEAM = 99A9KNU9ZS;  
             ENABLE_PREVIEWS = YES;  
             GENERATE_INFOPLIST_FILE = YES;  
             INFOPLIST_FILE = iosApp/Info.plist;  
             INFOPLIST_KEY_UILaunchScreen_Generation = YES;  
             LD_RUNPATH_SEARCH_PATHS = (  
                "$(inherited)",  
                "@executable_path/Frameworks",  
             );  
             MARKETING_VERSION = 1.0;  
             PRODUCT_BUNDLE_IDENTIFIER = com.thanh0x.supabaseselfhost.iosApp;  
             PRODUCT_NAME = SupabaseSelfHostPlayGround;  
             SWIFT_EMIT_LOC_STRINGS = YES;  
             SWIFT_VERSION = 5.0;  
             TARGETED_DEVICE_FAMILY = "1,2";  
          };  
          name = Debug;  
       };  
       A93A954729CC810D00F8E227 /* Release */ = {  
          isa = XCBuildConfiguration;  
          baseConfigurationReference = 8C04C9AACE04C90505E2C4B3 /* Pods-iosApp.release.xcconfig */;  
          buildSettings = {  
             ASSETCATALOG_COMPILER_APPICON_NAME = AppIcon;  
             ASSETCATALOG_COMPILER_GLOBAL_ACCENT_COLOR_NAME = AccentColor;  
             CODE_SIGN_STYLE = Automatic;  
             CURRENT_PROJECT_VERSION = 1;  
             DEVELOPMENT_ASSET_PATHS = "\"iosApp/Preview Content\"";  
             DEVELOPMENT_TEAM = 99A9KNU9ZS;  
             ENABLE_PREVIEWS = YES;  
             GENERATE_INFOPLIST_FILE = YES;  
             INFOPLIST_FILE = iosApp/Info.plist;  
             INFOPLIST_KEY_UILaunchScreen_Generation = YES;  
             LD_RUNPATH_SEARCH_PATHS = (  
                "$(inherited)",  
                "@executable_path/Frameworks",  
             );  
             MARKETING_VERSION = 1.0;  
             PRODUCT_BUNDLE_IDENTIFIER = com.thanh0x.supabaseselfhost.iosApp;  
             PRODUCT_NAME = SupabaseSelfHostPlayGround;  
             SWIFT_EMIT_LOC_STRINGS = YES;  
             SWIFT_VERSION = 5.0;  
             TARGETED_DEVICE_FAMILY = "1,2";  
          };  
          name = Release;  
       };  
/* End XCBuildConfiguration section */  
  
/* Begin XCConfigurationList section */  
       A93A953229CC810C00F8E227 /* Build configuration list for PBXProject "iosApp" */ = {  
          isa = XCConfigurationList;  
          buildConfigurations = (  
             A93A954329CC810D00F8E227 /* Debug */,  
             A93A954429CC810D00F8E227 /* Release */,  
          );  
          defaultConfigurationIsVisible = 0;  
          defaultConfigurationName = Release;  
       };  
       A93A954529CC810D00F8E227 /* Build configuration list for PBXNativeTarget "iosApp" */ = {  
          isa = XCConfigurationList;  
          buildConfigurations = (  
             A93A954629CC810D00F8E227 /* Debug */,  
             A93A954729CC810D00F8E227 /* Release */,  
          );  
          defaultConfigurationIsVisible = 0;  
          defaultConfigurationName = Release;  
       };  
/* End XCConfigurationList section */  
    };  
    rootObject = A93A952F29CC810C00F8E227 /* Project object */;  
}

------
Pod::Spec.new do |spec|  
    spec.name                     = 'composeApp'  
    spec.version                  = '1.0'  
    spec.homepage                 = 'Link to the Shared Module homepage'  
    spec.source                   = { :http=> ''}  
    spec.authors                  = ''  
    spec.license                  = ''  
    spec.summary                  = 'Some description for the Shared Module'  
    spec.vendored_frameworks      = 'build/cocoapods/framework/ComposeApp.framework'  
    spec.libraries                = 'c++'  
    spec.ios.deployment_target    = '15.4'  
                  
                  
    if !Dir.exist?('build/cocoapods/framework/ComposeApp.framework') || Dir.empty?('build/cocoapods/framework/ComposeApp.framework')  
        raise "  
  
        Kotlin framework 'ComposeApp' doesn't exist yet, so a proper Xcode project can't be generated.        'pod install' should be executed after running ':generateDummyFramework' Gradle task:  
            ./gradlew :composeApp:generateDummyFramework  
        Alternatively, proper pod installation is performed during Gradle sync in the IDE (if Podfile location is set)"  
    end  
                    spec.xcconfig = {  
        'ENABLE_USER_SCRIPT_SANDBOXING' => 'NO',  
    }  
                  
    spec.pod_target_xcconfig = {  
        'KOTLIN_PROJECT_PATH' => ':composeApp',  
        'PRODUCT_MODULE_NAME' => 'ComposeApp',  
    }  
                  
    spec.script_phases = [  
        {  
            :name => 'Build composeApp',  
            :execution_position => :before_compile,  
            :shell_path => '/bin/sh',  
            :script => <<-SCRIPT  
                if [ "YES" = "$OVERRIDE_KOTLIN_BUILD_IDE_SUPPORTED" ]; then                  echo "Skipping Gradle build task invocation due to OVERRIDE_KOTLIN_BUILD_IDE_SUPPORTED environment variable set to \"YES\""  
                  exit 0                fi                set -ev                REPO_ROOT="$PODS_TARGET_SRCROOT"                "$REPO_ROOT/../gradlew" -p "$REPO_ROOT" $KOTLIN_PROJECT_PATH:syncFramework \                    -Pkotlin.native.cocoapods.platform=$PLATFORM_NAME \                    -Pkotlin.native.cocoapods.archs="$ARCHS" \                    -Pkotlin.native.cocoapods.configuration="$CONFIGURATION"            SCRIPT  
        }  
    ]  
    spec.resources = ['build/compose/cocoapods/compose-resources']  
end

----
#Gradle  
org.gradle.jvmargs=-Xmx4G  
org.gradle.caching=true  
org.gradle.configuration-cache=true  
org.gradle.daemon=true  
org.gradle.parallel=true  
  
#Kotlin  
kotlin.code.style=official  
kotlin.daemon.jvmargs=-Xmx4G  
  
#Android  
android.useAndroidX=true  
android.nonTransitiveRClass=true

```

```
plugins {  
    // this is necessary to avoid the plugins to be loaded multiple times  
    // in each subproject's classloader    alias(libs.plugins.androidApplication) apply false  
    alias(libs.plugins.androidLibrary) apply false  
    alias(libs.plugins.jetbrainsCompose) apply false  
    alias(libs.plugins.compose.compiler) apply false  
    alias(libs.plugins.kotlinMultiplatform) apply false  
    alias(libs.plugins.kotlinCocoapods).apply(false)  
    id("com.google.gms.google-services") version "4.4.0" apply false  
}
------
import org.jetbrains.compose.desktop.application.dsl.TargetFormat  
import org.jetbrains.kotlin.gradle.ExperimentalKotlinGradlePluginApi  
import org.jetbrains.kotlin.gradle.dsl.JvmTarget  
  
plugins {  
    alias(libs.plugins.kotlinMultiplatform)  
    alias(libs.plugins.androidApplication)  
    alias(libs.plugins.jetbrainsCompose)  
    alias(libs.plugins.compose.compiler)  
    alias(libs.plugins.kotlinCocoapods)  
    id("com.google.gms.google-services")  
}  
  
kotlin {  
    androidTarget {  
        @OptIn(ExperimentalKotlinGradlePluginApi::class)  
        compilerOptions {  
            jvmTarget.set(JvmTarget.JVM_11)  
        }  
    }  
    iosX64()  
    iosArm64()  
    iosSimulatorArm64()  
  
    cocoapods {  
        summary = "Some description for the Shared Module"  
        homepage = "Link to the Shared Module homepage"  
        version = "1.0"  
        ios.deploymentTarget = "15.4"  
        podfile = project.file("../iosApp/Podfile")  
        framework {  
            baseName = "ComposeApp"  
            isStatic = true  
        }  
  
        pod("GoogleMaps") {  
            version = libs.versions.pods.google.maps.get()  
            extraOpts += listOf("-compiler-option", "-fmodules")  
        }  
  
  
        pod("Google-Maps-iOS-Utils") {  
            moduleName = "GoogleMapsUtils"  
            version = libs.versions.pods.google.ios.maps.utils.get()  
            extraOpts += listOf("-compiler-option", "-fmodules")  
        }  
  
    }  
    sourceSets {  
        androidMain.dependencies {  
            implementation(compose.preview)  
            implementation(libs.androidx.activity.compose)  
  
            implementation(libs.play.services.auth)  
        }  
        commonMain.dependencies {  
            implementation(compose.runtime)  
            implementation(compose.foundation)  
            implementation(compose.material)  
            implementation(compose.ui)  
            implementation(compose.components.resources)  
            implementation(compose.components.uiToolingPreview)  
            implementation(libs.androidx.lifecycle.viewmodel)  
            implementation(libs.androidx.lifecycle.runtime.compose)  
  
            implementation(libs.firebase.gitlive.common)  
            implementation(libs.firebase.gitlive.auth)  
              
            implementation("org.jetbrains.kotlinx:kotlinx-datetime:0.5.0")  
        }  
    }}  
  
android {  
    namespace = "com.furkan.kmpmaps"  
    compileSdk = libs.versions.android.compileSdk.get().toInt()  
  
    sourceSets["main"].manifest.srcFile("src/androidMain/AndroidManifest.xml")  
    sourceSets["main"].res.srcDirs("src/androidMain/res")  
    sourceSets["main"].resources.srcDirs("src/commonMain/resources")  
  
    defaultConfig {  
        applicationId = "com.thanh0x.FocusBloom"  
        minSdk = libs.versions.android.minSdk.get().toInt()  
        targetSdk = libs.versions.android.targetSdk.get().toInt()  
        versionCode = 1  
        versionName = "1.0"  
    }  
    packaging {  
        resources {  
            excludes += "/META-INF/{AL2.0,LGPL2.1}"  
        }  
    }    buildTypes {  
        getByName("release") {  
            isMinifyEnabled = false  
        }  
    }    compileOptions {  
        sourceCompatibility = JavaVersion.VERSION_11  
        targetCompatibility = JavaVersion.VERSION_11  
    }  
    buildFeatures {  
        compose = true  
    }  
    dependencies {  
        debugImplementation(compose.uiTooling)  
    }  
}


-----
platform :ios, '15.4'  
target 'iosApp' do  
  use_frameworks!  
   
  pod 'composeApp', :path => '../composeApp'  
  pod 'GoogleMaps', '8.4.0'  
  pod 'Google-Maps-iOS-Utils', '5.0.0'  
  pod 'GoogleSignIn'  
  pod 'Firebase/Auth'  
  pod 'Firebase/Core'  
    
end

----

PODS:  
  - AppAuth (2.0.0):  
    - AppAuth/Core (= 2.0.0)  
    - AppAuth/ExternalUserAgent (= 2.0.0)  
  - AppAuth/Core (2.0.0)  
  - AppAuth/ExternalUserAgent (2.0.0):  
    - AppAuth/Core  
  - AppCheckCore (11.2.0):  
    - GoogleUtilities/Environment (~> 8.0)  
    - GoogleUtilities/UserDefaults (~> 8.0)  
    - PromisesObjC (~> 2.4)  
  - composeApp (1.0):  
    - Google-Maps-iOS-Utils (= 5.0.0)  
    - GoogleMaps (= 8.4.0)  
  - Firebase/Auth (12.4.0):  
    - Firebase/CoreOnly  
    - FirebaseAuth (~> 12.4.0)  
  - Firebase/Core (12.4.0):  
    - Firebase/CoreOnly  
    - FirebaseAnalytics (~> 12.4.0)  
  - Firebase/CoreOnly (12.4.0):  
    - FirebaseCore (~> 12.4.0)  
  - FirebaseAnalytics (12.4.0):  
    - FirebaseAnalytics/Default (= 12.4.0)  
    - FirebaseCore (~> 12.4.0)  
    - FirebaseInstallations (~> 12.4.0)  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/MethodSwizzler (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
    - nanopb (~> 3.30910.0)  
  - FirebaseAnalytics/Default (12.4.0):  
    - FirebaseCore (~> 12.4.0)  
    - FirebaseInstallations (~> 12.4.0)  
    - GoogleAppMeasurement/Default (= 12.4.0)  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/MethodSwizzler (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
    - nanopb (~> 3.30910.0)  
  - FirebaseAppCheckInterop (12.4.0)  
  - FirebaseAuth (12.4.0):  
    - FirebaseAppCheckInterop (~> 12.4.0)  
    - FirebaseAuthInterop (~> 12.4.0)  
    - FirebaseCore (~> 12.4.0)  
    - FirebaseCoreExtension (~> 12.4.0)  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/Environment (~> 8.1)  
    - GTMSessionFetcher/Core (< 6.0, >= 3.4)  
    - RecaptchaInterop (~> 101.0)  
  - FirebaseAuthInterop (12.4.0)  
  - FirebaseCore (12.4.0):  
    - FirebaseCoreInternal (~> 12.4.0)  
    - GoogleUtilities/Environment (~> 8.1)  
    - GoogleUtilities/Logger (~> 8.1)  
  - FirebaseCoreExtension (12.4.0):  
    - FirebaseCore (~> 12.4.0)  
  - FirebaseCoreInternal (12.4.0):  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
  - FirebaseInstallations (12.4.0):  
    - FirebaseCore (~> 12.4.0)  
    - GoogleUtilities/Environment (~> 8.1)  
    - GoogleUtilities/UserDefaults (~> 8.1)  
    - PromisesObjC (~> 2.4)  
  - Google-Maps-iOS-Utils (5.0.0):  
    - GoogleMaps (~> 8.0)  
  - GoogleAdsOnDeviceConversion (3.1.0):  
    - GoogleUtilities/Environment (~> 8.1)  
    - GoogleUtilities/Logger (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - nanopb (~> 3.30910.0)  
  - GoogleAppMeasurement/Core (12.4.0):  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/MethodSwizzler (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
    - nanopb (~> 3.30910.0)  
  - GoogleAppMeasurement/Default (12.4.0):  
    - GoogleAdsOnDeviceConversion (~> 3.1.0)  
    - GoogleAppMeasurement/Core (= 12.4.0)  
    - GoogleAppMeasurement/IdentitySupport (= 12.4.0)  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/MethodSwizzler (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
    - nanopb (~> 3.30910.0)  
  - GoogleAppMeasurement/IdentitySupport (12.4.0):  
    - GoogleAppMeasurement/Core (= 12.4.0)  
    - GoogleUtilities/AppDelegateSwizzler (~> 8.1)  
    - GoogleUtilities/MethodSwizzler (~> 8.1)  
    - GoogleUtilities/Network (~> 8.1)  
    - "GoogleUtilities/NSData+zlib (~> 8.1)"  
    - nanopb (~> 3.30910.0)  
  - GoogleMaps (8.4.0):  
    - GoogleMaps/Maps (= 8.4.0)  
  - GoogleMaps/Base (8.4.0)  
  - GoogleMaps/Maps (8.4.0):  
    - GoogleMaps/Base  
  - GoogleSignIn (9.0.0):  
    - AppAuth (~> 2.0)  
    - AppCheckCore (~> 11.0)  
    - GTMAppAuth (~> 5.0)  
    - GTMSessionFetcher/Core (~> 3.3)  
  - GoogleUtilities/AppDelegateSwizzler (8.1.0):  
    - GoogleUtilities/Environment  
    - GoogleUtilities/Logger  
    - GoogleUtilities/Network  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/Environment (8.1.0):  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/Logger (8.1.0):  
    - GoogleUtilities/Environment  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/MethodSwizzler (8.1.0):  
    - GoogleUtilities/Logger  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/Network (8.1.0):  
    - GoogleUtilities/Logger  
    - "GoogleUtilities/NSData+zlib"  
    - GoogleUtilities/Privacy  
    - GoogleUtilities/Reachability  
  - "GoogleUtilities/NSData+zlib (8.1.0)":  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/Privacy (8.1.0)  
  - GoogleUtilities/Reachability (8.1.0):  
    - GoogleUtilities/Logger  
    - GoogleUtilities/Privacy  
  - GoogleUtilities/UserDefaults (8.1.0):  
    - GoogleUtilities/Logger  
    - GoogleUtilities/Privacy  
  - GTMAppAuth (5.0.0):  
    - AppAuth/Core (~> 2.0)  
    - GTMSessionFetcher/Core (< 4.0, >= 3.3)  
  - GTMSessionFetcher/Core (3.5.0)  
  - nanopb (3.30910.0):  
    - nanopb/decode (= 3.30910.0)  
    - nanopb/encode (= 3.30910.0)  
  - nanopb/decode (3.30910.0)  
  - nanopb/encode (3.30910.0)  
  - PromisesObjC (2.4.0)  
  - RecaptchaInterop (101.0.0)  
  
DEPENDENCIES:  
  - composeApp (from `../composeApp`)  
  - Firebase/Auth  
  - Firebase/Core  
  - Google-Maps-iOS-Utils (= 5.0.0)  
  - GoogleMaps (= 8.4.0)  
  - GoogleSignIn  
  
SPEC REPOS:  
  trunk:  
    - AppAuth  
    - AppCheckCore  
    - Firebase  
    - FirebaseAnalytics  
    - FirebaseAppCheckInterop  
    - FirebaseAuth  
    - FirebaseAuthInterop  
    - FirebaseCore  
    - FirebaseCoreExtension  
    - FirebaseCoreInternal  
    - FirebaseInstallations  
    - Google-Maps-iOS-Utils  
    - GoogleAdsOnDeviceConversion  
    - GoogleAppMeasurement  
    - GoogleMaps  
    - GoogleSignIn  
    - GoogleUtilities  
    - GTMAppAuth  
    - GTMSessionFetcher  
    - nanopb  
    - PromisesObjC  
    - RecaptchaInterop  
  
EXTERNAL SOURCES:  
  composeApp:  
    :path: "../composeApp"  
  
SPEC CHECKSUMS:  
  AppAuth: 1c1a8afa7e12f2ec3a294d9882dfa5ab7d3cb063  
  AppCheckCore: cc8fd0a3a230ddd401f326489c99990b013f0c4f  
  composeApp: 21a96a25d98ac36797c5610c327b8884c9efcadd  
  Firebase: f07b15ae5a6ec0f93713e30b923d9970d144af3e  
  FirebaseAnalytics: 0fc2b20091f0ddd21bf73397cf8f0eb5346dc24f  
  FirebaseAppCheckInterop: f734c802f21fe1da0837708f0f9a27218c8a4ed0  
  FirebaseAuth: 4a2aed737c84114a9d9b33d11ae1b147d6b94889  
  FirebaseAuthInterop: 858e6b754966e70740a4370dd1503dfffe6dbb49  
  FirebaseCore: bb595f3114953664e3c1dc032f008a244147cfd3  
  FirebaseCoreExtension: 7e1f7118ee970e001a8013719fb90950ee5e0018  
  FirebaseCoreInternal: d7f5a043c2cd01a08103ab586587c1468047bca6  
  FirebaseInstallations: ae9f4902cb5bf1d0c5eaa31ec1f4e5495a0714e2  
  Google-Maps-iOS-Utils: 66d6de12be1ce6d3742a54661e7a79cb317a9321  
  GoogleAdsOnDeviceConversion: e03a386840803ea7eef3fd22a061930142c039c1  
  GoogleAppMeasurement: 1e718274b7e015cefd846ac1fcf7820c70dc017d  
  GoogleMaps: 8939898920281c649150e0af74aa291c60f2e77d  
  GoogleSignIn: c7f09cfbc85a1abf69187be091997c317cc33b77  
  GoogleUtilities: 00c88b9a86066ef77f0da2fab05f65d7768ed8e1  
  GTMAppAuth: 217a876b249c3c585a54fd6f73e6b58c4f5c4238  
  GTMSessionFetcher: 5aea5ba6bd522a239e236100971f10cb71b96ab6  
  nanopb: fad817b59e0457d11a5dfbde799381cd727c1275  
  PromisesObjC: f5707f49cb48b9636751c5b2e7d227e43fba9f47  
  RecaptchaInterop: 11e0b637842dfb48308d242afc3f448062325aba  
  
PODFILE CHECKSUM: e279ae19668c627d3df392e9c72bff7f62fa7279  
  
COCOAPODS: 1.16.2

----
// !$*UTF8*$!  
{  
    archiveVersion = 1;  
    classes = {  
    };  
    objectVersion = 56;  
    objects = {  
  
/* Begin PBXBuildFile section */  
       058557BB273AAA24004C7B11 /* Assets.xcassets in Resources */ = {isa = PBXBuildFile; fileRef = 058557BA273AAA24004C7B11 /* Assets.xcassets */; };  
       058557D9273AAEEB004C7B11 /* Preview Assets.xcassets in Resources */ = {isa = PBXBuildFile; fileRef = 058557D8273AAEEB004C7B11 /* Preview Assets.xcassets */; };  
       2152FB042600AC8F00CF470E /* iOSApp.swift in Sources */ = {isa = PBXBuildFile; fileRef = 2152FB032600AC8F00CF470E /* iOSApp.swift */; };  
       4A737C85902BA264660D8785 /* Pods_iosApp.framework in Frameworks */ = {isa = PBXBuildFile; fileRef = 707176046664E0416C254925 /* Pods_iosApp.framework */; };  
       7555FF83242A565900829871 /* ContentView.swift in Sources */ = {isa = PBXBuildFile; fileRef = 7555FF82242A565900829871 /* ContentView.swift */; };  
       D4D035582EA52315009A1C89 /* GoogleService-Info.plist in Resources */ = {isa = PBXBuildFile; fileRef = D4D035572EA52314009A1C89 /* GoogleService-Info.plist */; };  
/* End PBXBuildFile section */  
  
/* Begin PBXFileReference section */  
       058557BA273AAA24004C7B11 /* Assets.xcassets */ = {isa = PBXFileReference; lastKnownFileType = folder.assetcatalog; path = Assets.xcassets; sourceTree = "<group>"; };  
       058557D8273AAEEB004C7B11 /* Preview Assets.xcassets */ = {isa = PBXFileReference; lastKnownFileType = folder.assetcatalog; path = "Preview Assets.xcassets"; sourceTree = "<group>"; };  
       2152FB032600AC8F00CF470E /* iOSApp.swift */ = {isa = PBXFileReference; lastKnownFileType = sourcecode.swift; path = iOSApp.swift; sourceTree = "<group>"; };  
       358B9345432DCF4A31F835FD /* Pods-iosApp.debug.xcconfig */ = {isa = PBXFileReference; includeInIndex = 1; lastKnownFileType = text.xcconfig; name = "Pods-iosApp.debug.xcconfig"; path = "Target Support Files/Pods-iosApp/Pods-iosApp.debug.xcconfig"; sourceTree = "<group>"; };  
       707176046664E0416C254925 /* Pods_iosApp.framework */ = {isa = PBXFileReference; explicitFileType = wrapper.framework; includeInIndex = 0; path = Pods_iosApp.framework; sourceTree = BUILT_PRODUCTS_DIR; };  
       7555FF7B242A565900829871 /* KMPMaps.app */ = {isa = PBXFileReference; explicitFileType = wrapper.application; includeInIndex = 0; path = KMPMaps.app; sourceTree = BUILT_PRODUCTS_DIR; };  
       7555FF82242A565900829871 /* ContentView.swift */ = {isa = PBXFileReference; lastKnownFileType = sourcecode.swift; path = ContentView.swift; sourceTree = "<group>"; };  
       7555FF8C242A565B00829871 /* Info.plist */ = {isa = PBXFileReference; lastKnownFileType = text.plist.xml; path = Info.plist; sourceTree = "<group>"; };  
       A4EF8736AEAE9062FF086E8A /* Pods-iosApp.release.xcconfig */ = {isa = PBXFileReference; includeInIndex = 1; lastKnownFileType = text.xcconfig; name = "Pods-iosApp.release.xcconfig"; path = "Target Support Files/Pods-iosApp/Pods-iosApp.release.xcconfig"; sourceTree = "<group>"; };  
       AB3632DC29227652001CCB65 /* Config.xcconfig */ = {isa = PBXFileReference; fileEncoding = 4; lastKnownFileType = text.xcconfig; path = Config.xcconfig; sourceTree = "<group>"; };  
       D4D035572EA52314009A1C89 /* GoogleService-Info.plist */ = {isa = PBXFileReference; lastKnownFileType = text.plist.xml; path = "GoogleService-Info.plist"; sourceTree = "<group>"; };  
/* End PBXFileReference section */  
  
/* Begin PBXFrameworksBuildPhase section */  
       B92378962B6B1156000C7307 /* Frameworks */ = {  
          isa = PBXFrameworksBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
             4A737C85902BA264660D8785 /* Pods_iosApp.framework in Frameworks */,  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
       };  
/* End PBXFrameworksBuildPhase section */  
  
/* Begin PBXGroup section */  
       058557D7273AAEEB004C7B11 /* Preview Content */ = {  
          isa = PBXGroup;  
          children = (  
             058557D8273AAEEB004C7B11 /* Preview Assets.xcassets */,  
          );  
          path = "Preview Content";  
          sourceTree = "<group>";  
       };  
       1A728046106AEC80E0690C45 /* Pods */ = {  
          isa = PBXGroup;  
          children = (  
             358B9345432DCF4A31F835FD /* Pods-iosApp.debug.xcconfig */,  
             A4EF8736AEAE9062FF086E8A /* Pods-iosApp.release.xcconfig */,  
          );  
          path = Pods;  
          sourceTree = "<group>";  
       };  
       42799AB246E5F90AF97AA0EF /* Frameworks */ = {  
          isa = PBXGroup;  
          children = (  
             707176046664E0416C254925 /* Pods_iosApp.framework */,  
          );  
          name = Frameworks;  
          sourceTree = "<group>";  
       };  
       7555FF72242A565900829871 = {  
          isa = PBXGroup;  
          children = (  
             AB1DB47929225F7C00F7AF9C /* Configuration */,  
             7555FF7D242A565900829871 /* iosApp */,  
             7555FF7C242A565900829871 /* Products */,  
             42799AB246E5F90AF97AA0EF /* Frameworks */,  
             1A728046106AEC80E0690C45 /* Pods */,  
          );  
          sourceTree = "<group>";  
       };  
       7555FF7C242A565900829871 /* Products */ = {  
          isa = PBXGroup;  
          children = (  
             7555FF7B242A565900829871 /* KMPMaps.app */,  
          );  
          name = Products;  
          sourceTree = "<group>";  
       };  
       7555FF7D242A565900829871 /* iosApp */ = {  
          isa = PBXGroup;  
          children = (  
             058557BA273AAA24004C7B11 /* Assets.xcassets */,  
             7555FF82242A565900829871 /* ContentView.swift */,  
             7555FF8C242A565B00829871 /* Info.plist */,  
             2152FB032600AC8F00CF470E /* iOSApp.swift */,  
             058557D7273AAEEB004C7B11 /* Preview Content */,  
             D4D035572EA52314009A1C89 /* GoogleService-Info.plist */,  
          );  
          path = iosApp;  
          sourceTree = "<group>";  
       };  
       AB1DB47929225F7C00F7AF9C /* Configuration */ = {  
          isa = PBXGroup;  
          children = (  
             AB3632DC29227652001CCB65 /* Config.xcconfig */,  
          );  
          path = Configuration;  
          sourceTree = "<group>";  
       };  
/* End PBXGroup section */  
  
/* Begin PBXNativeTarget section */  
       7555FF7A242A565900829871 /* iosApp */ = {  
          isa = PBXNativeTarget;  
          buildConfigurationList = 7555FFA5242A565B00829871 /* Build configuration list for PBXNativeTarget "iosApp" */;  
          buildPhases = (  
             9DEDF78639FDE8E7DD6D9E1C /* [CP] Check Pods Manifest.lock */,  
             7555FF77242A565900829871 /* Sources */,  
             B92378962B6B1156000C7307 /* Frameworks */,  
             7555FF79242A565900829871 /* Resources */,  
             351EBB13AE560536E4EB2EEC /* [CP] Copy Pods Resources */,  
             135CBE2D458376C8E8BA6E23 /* [CP] Embed Pods Frameworks */,  
          );  
          buildRules = (  
          );  
          dependencies = (  
          );  
          name = iosApp;  
          productName = iosApp;  
          productReference = 7555FF7B242A565900829871 /* KMPMaps.app */;  
          productType = "com.apple.product-type.application";  
       };  
/* End PBXNativeTarget section */  
  
/* Begin PBXProject section */  
       7555FF73242A565900829871 /* Project object */ = {  
          isa = PBXProject;  
          attributes = {  
             BuildIndependentTargetsInParallel = YES;  
             LastSwiftUpdateCheck = 1130;  
             LastUpgradeCheck = 1540;  
             ORGANIZATIONNAME = orgName;  
             TargetAttributes = {  
                7555FF7A242A565900829871 = {  
                   CreatedOnToolsVersion = 11.3.1;  
                };  
             };  
          };  
          buildConfigurationList = 7555FF76242A565900829871 /* Build configuration list for PBXProject "iosApp" */;  
          compatibilityVersion = "Xcode 14.0";  
          developmentRegion = en;  
          hasScannedForEncodings = 0;  
          knownRegions = (  
             en,  
             Base,  
          );  
          mainGroup = 7555FF72242A565900829871;  
          productRefGroup = 7555FF7C242A565900829871 /* Products */;  
          projectDirPath = "";  
          projectRoot = "";  
          targets = (  
             7555FF7A242A565900829871 /* iosApp */,  
          );  
       };  
/* End PBXProject section */  
  
/* Begin PBXResourcesBuildPhase section */  
       7555FF79242A565900829871 /* Resources */ = {  
          isa = PBXResourcesBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
             058557D9273AAEEB004C7B11 /* Preview Assets.xcassets in Resources */,  
             D4D035582EA52315009A1C89 /* GoogleService-Info.plist in Resources */,  
             058557BB273AAA24004C7B11 /* Assets.xcassets in Resources */,  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
       };  
/* End PBXResourcesBuildPhase section */  
  
/* Begin PBXShellScriptBuildPhase section */  
       135CBE2D458376C8E8BA6E23 /* [CP] Embed Pods Frameworks */ = {  
          isa = PBXShellScriptBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
          );  
          inputFileListPaths = (  
             "${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-frameworks-${CONFIGURATION}-input-files.xcfilelist",  
          );  
          name = "[CP] Embed Pods Frameworks";  
          outputFileListPaths = (  
             "${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-frameworks-${CONFIGURATION}-output-files.xcfilelist",  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
          shellPath = /bin/sh;  
          shellScript = "\"${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-frameworks.sh\"\n";  
          showEnvVarsInLog = 0;  
       };  
       351EBB13AE560536E4EB2EEC /* [CP] Copy Pods Resources */ = {  
          isa = PBXShellScriptBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
          );  
          inputFileListPaths = (  
             "${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-resources-${CONFIGURATION}-input-files.xcfilelist",  
          );  
          name = "[CP] Copy Pods Resources";  
          outputFileListPaths = (  
             "${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-resources-${CONFIGURATION}-output-files.xcfilelist",  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
          shellPath = /bin/sh;  
          shellScript = "\"${PODS_ROOT}/Target Support Files/Pods-iosApp/Pods-iosApp-resources.sh\"\n";  
          showEnvVarsInLog = 0;  
       };  
       9DEDF78639FDE8E7DD6D9E1C /* [CP] Check Pods Manifest.lock */ = {  
          isa = PBXShellScriptBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
          );  
          inputFileListPaths = (  
          );  
          inputPaths = (  
             "${PODS_PODFILE_DIR_PATH}/Podfile.lock",  
             "${PODS_ROOT}/Manifest.lock",  
          );  
          name = "[CP] Check Pods Manifest.lock";  
          outputFileListPaths = (  
          );  
          outputPaths = (  
             "$(DERIVED_FILE_DIR)/Pods-iosApp-checkManifestLockResult.txt",  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
          shellPath = /bin/sh;  
          shellScript = "diff \"${PODS_PODFILE_DIR_PATH}/Podfile.lock\" \"${PODS_ROOT}/Manifest.lock\" > /dev/null\nif [ $? != 0 ] ; then\n    # print error to STDERR\n    echo \"error: The sandbox is not in sync with the Podfile.lock. Run 'pod install' or update your CocoaPods installation.\" >&2\n    exit 1\nfi\n# This output is used by Xcode 'outputs' to avoid re-running this script phase.\necho \"SUCCESS\" > \"${SCRIPT_OUTPUT_FILE_0}\"\n";  
          showEnvVarsInLog = 0;  
       };  
/* End PBXShellScriptBuildPhase section */  
  
/* Begin PBXSourcesBuildPhase section */  
       7555FF77242A565900829871 /* Sources */ = {  
          isa = PBXSourcesBuildPhase;  
          buildActionMask = 2147483647;  
          files = (  
             2152FB042600AC8F00CF470E /* iOSApp.swift in Sources */,  
             7555FF83242A565900829871 /* ContentView.swift in Sources */,  
          );  
          runOnlyForDeploymentPostprocessing = 0;  
       };  
/* End PBXSourcesBuildPhase section */  
  
/* Begin XCBuildConfiguration section */  
       7555FFA3242A565B00829871 /* Debug */ = {  
          isa = XCBuildConfiguration;  
          baseConfigurationReference = AB3632DC29227652001CCB65 /* Config.xcconfig */;  
          buildSettings = {  
             ALWAYS_SEARCH_USER_PATHS = NO;  
             CLANG_ANALYZER_NONNULL = YES;  
             CLANG_ANALYZER_NUMBER_OBJECT_CONVERSION = YES_AGGRESSIVE;  
             CLANG_CXX_LANGUAGE_STANDARD = "gnu++14";  
             CLANG_CXX_LIBRARY = "libc++";  
             CLANG_ENABLE_MODULES = YES;  
             CLANG_ENABLE_OBJC_ARC = YES;  
             CLANG_ENABLE_OBJC_WEAK = YES;  
             CLANG_WARN_BLOCK_CAPTURE_AUTORELEASING = YES;  
             CLANG_WARN_BOOL_CONVERSION = YES;  
             CLANG_WARN_COMMA = YES;  
             CLANG_WARN_CONSTANT_CONVERSION = YES;  
             CLANG_WARN_DEPRECATED_OBJC_IMPLEMENTATIONS = YES;  
             CLANG_WARN_DIRECT_OBJC_ISA_USAGE = YES_ERROR;  
             CLANG_WARN_DOCUMENTATION_COMMENTS = YES;  
             CLANG_WARN_EMPTY_BODY = YES;  
             CLANG_WARN_ENUM_CONVERSION = YES;  
             CLANG_WARN_INFINITE_RECURSION = YES;  
             CLANG_WARN_INT_CONVERSION = YES;  
             CLANG_WARN_NON_LITERAL_NULL_CONVERSION = YES;  
             CLANG_WARN_OBJC_IMPLICIT_RETAIN_SELF = YES;  
             CLANG_WARN_OBJC_LITERAL_CONVERSION = YES;  
             CLANG_WARN_OBJC_ROOT_CLASS = YES_ERROR;  
             CLANG_WARN_QUOTED_INCLUDE_IN_FRAMEWORK_HEADER = YES;  
             CLANG_WARN_RANGE_LOOP_ANALYSIS = YES;  
             CLANG_WARN_STRICT_PROTOTYPES = YES;  
             CLANG_WARN_SUSPICIOUS_MOVE = YES;  
             CLANG_WARN_UNGUARDED_AVAILABILITY = YES_AGGRESSIVE;  
             CLANG_WARN_UNREACHABLE_CODE = YES;  
             CLANG_WARN__DUPLICATE_METHOD_MATCH = YES;  
             COPY_PHASE_STRIP = NO;  
             DEBUG_INFORMATION_FORMAT = "dwarf-with-dsym";  
             ENABLE_STRICT_OBJC_MSGSEND = YES;  
             ENABLE_TESTABILITY = YES;  
             ENABLE_USER_SCRIPT_SANDBOXING = NO;  
             GCC_C_LANGUAGE_STANDARD = gnu11;  
             GCC_DYNAMIC_NO_PIC = NO;  
             GCC_NO_COMMON_BLOCKS = YES;  
             GCC_OPTIMIZATION_LEVEL = 0;  
             GCC_PREPROCESSOR_DEFINITIONS = (  
                "DEBUG=1",  
                "$(inherited)",  
             );  
             GCC_WARN_64_TO_32_BIT_CONVERSION = YES;  
             GCC_WARN_ABOUT_RETURN_TYPE = YES_ERROR;  
             GCC_WARN_UNDECLARED_SELECTOR = YES;  
             GCC_WARN_UNINITIALIZED_AUTOS = YES_AGGRESSIVE;  
             GCC_WARN_UNUSED_FUNCTION = YES;  
             GCC_WARN_UNUSED_VARIABLE = YES;  
             IPHONEOS_DEPLOYMENT_TARGET = 15.4;  
             MTL_ENABLE_DEBUG_INFO = INCLUDE_SOURCE;  
             MTL_FAST_MATH = YES;  
             ONLY_ACTIVE_ARCH = YES;  
             SDKROOT = iphoneos;  
             SWIFT_ACTIVE_COMPILATION_CONDITIONS = DEBUG;  
             SWIFT_OPTIMIZATION_LEVEL = "-Onone";  
          };  
          name = Debug;  
       };  
       7555FFA4242A565B00829871 /* Release */ = {  
          isa = XCBuildConfiguration;  
          baseConfigurationReference = AB3632DC29227652001CCB65 /* Config.xcconfig */;  
          buildSettings = {  
             ALWAYS_SEARCH_USER_PATHS = NO;  
             CLANG_ANALYZER_NONNULL = YES;  
             CLANG_ANALYZER_NUMBER_OBJECT_CONVERSION = YES_AGGRESSIVE;  
             CLANG_CXX_LANGUAGE_STANDARD = "gnu++14";  
             CLANG_CXX_LIBRARY = "libc++";  
             CLANG_ENABLE_MODULES = YES;  
             CLANG_ENABLE_OBJC_ARC = YES;  
             CLANG_ENABLE_OBJC_WEAK = YES;  
             CLANG_WARN_BLOCK_CAPTURE_AUTORELEASING = YES;  
             CLANG_WARN_BOOL_CONVERSION = YES;  
             CLANG_WARN_COMMA = YES;  
             CLANG_WARN_CONSTANT_CONVERSION = YES;  
             CLANG_WARN_DEPRECATED_OBJC_IMPLEMENTATIONS = YES;  
             CLANG_WARN_DIRECT_OBJC_ISA_USAGE = YES_ERROR;  
             CLANG_WARN_DOCUMENTATION_COMMENTS = YES;  
             CLANG_WARN_EMPTY_BODY = YES;  
             CLANG_WARN_ENUM_CONVERSION = YES;  
             CLANG_WARN_INFINITE_RECURSION = YES;  
             CLANG_WARN_INT_CONVERSION = YES;  
             CLANG_WARN_NON_LITERAL_NULL_CONVERSION = YES;  
             CLANG_WARN_OBJC_IMPLICIT_RETAIN_SELF = YES;  
             CLANG_WARN_OBJC_LITERAL_CONVERSION = YES;  
             CLANG_WARN_OBJC_ROOT_CLASS = YES_ERROR;  
             CLANG_WARN_QUOTED_INCLUDE_IN_FRAMEWORK_HEADER = YES;  
             CLANG_WARN_RANGE_LOOP_ANALYSIS = YES;  
             CLANG_WARN_STRICT_PROTOTYPES = YES;  
             CLANG_WARN_SUSPICIOUS_MOVE = YES;  
             CLANG_WARN_UNGUARDED_AVAILABILITY = YES_AGGRESSIVE;  
             CLANG_WARN_UNREACHABLE_CODE = YES;  
             CLANG_WARN__DUPLICATE_METHOD_MATCH = YES;  
             COPY_PHASE_STRIP = NO;  
             DEBUG_INFORMATION_FORMAT = "dwarf-with-dsym";  
             ENABLE_NS_ASSERTIONS = NO;  
             ENABLE_STRICT_OBJC_MSGSEND = YES;  
             ENABLE_USER_SCRIPT_SANDBOXING = NO;  
             GCC_C_LANGUAGE_STANDARD = gnu11;  
             GCC_NO_COMMON_BLOCKS = YES;  
             GCC_WARN_64_TO_32_BIT_CONVERSION = YES;  
             GCC_WARN_ABOUT_RETURN_TYPE = YES_ERROR;  
             GCC_WARN_UNDECLARED_SELECTOR = YES;  
             GCC_WARN_UNINITIALIZED_AUTOS = YES_AGGRESSIVE;  
             GCC_WARN_UNUSED_FUNCTION = YES;  
             GCC_WARN_UNUSED_VARIABLE = YES;  
             IPHONEOS_DEPLOYMENT_TARGET = 15.4;  
             MTL_ENABLE_DEBUG_INFO = NO;  
             MTL_FAST_MATH = YES;  
             SDKROOT = iphoneos;  
             SWIFT_COMPILATION_MODE = wholemodule;  
             SWIFT_OPTIMIZATION_LEVEL = "-O";  
             VALIDATE_PRODUCT = YES;  
          };  
          name = Release;  
       };  
       7555FFA6242A565B00829871 /* Debug */ = {  
          isa = XCBuildConfiguration;  
          baseConfigurationReference = 358B9345432DCF4A31F835FD /* Pods-iosApp.debug.xcconfig */;  
          buildSettings = {  
             ASSETCATALOG_COMPILER_APPICON_NAME = AppIcon;  
             CODE_SIGN_IDENTITY = "Apple Development";  
             CODE_SIGN_STYLE = Automatic;  
             DEVELOPMENT_ASSET_PATHS = "\"iosApp/Preview Content\"";  
             DEVELOPMENT_TEAM = 99A9KNU9ZS;  
             ENABLE_PREVIEWS = YES;  
             FRAMEWORK_SEARCH_PATHS = (  
                "$(inherited)",  
                "$(SRCROOT)/../shared/build/xcode-frameworks/$(CONFIGURATION)/$(SDK_NAME)\n$(SRCROOT)/../composeApp/build/xcode-frameworks/$(CONFIGURATION)/$(SDK_NAME)",  
             );  
             INFOPLIST_FILE = iosApp/Info.plist;  
             IPHONEOS_DEPLOYMENT_TARGET = 15.4;  
             LD_RUNPATH_SEARCH_PATHS = (  
                "$(inherited)",  
                "@executable_path/Frameworks",  
             );  
             PRODUCT_BUNDLE_IDENTIFIER = com.thanh0x.FocusBloom;  
             PRODUCT_NAME = "${APP_NAME}";  
             PROVISIONING_PROFILE_SPECIFIER = "";  
             SWIFT_VERSION = 5.0;  
             TARGETED_DEVICE_FAMILY = "1,2";  
          };  
          name = Debug;  
       };  
       7555FFA7242A565B00829871 /* Release */ = {  
          isa = XCBuildConfiguration;  
          baseConfigurationReference = A4EF8736AEAE9062FF086E8A /* Pods-iosApp.release.xcconfig */;  
          buildSettings = {  
             ASSETCATALOG_COMPILER_APPICON_NAME = AppIcon;  
             CODE_SIGN_IDENTITY = "Apple Development";  
             CODE_SIGN_STYLE = Automatic;  
             DEVELOPMENT_ASSET_PATHS = "\"iosApp/Preview Content\"";  
             DEVELOPMENT_TEAM = 99A9KNU9ZS;  
             ENABLE_PREVIEWS = YES;  
             FRAMEWORK_SEARCH_PATHS = (  
                "$(inherited)",  
                "$(SRCROOT)/../shared/build/xcode-frameworks/$(CONFIGURATION)/$(SDK_NAME)\n$(SRCROOT)/../composeApp/build/xcode-frameworks/$(CONFIGURATION)/$(SDK_NAME)",  
             );  
             INFOPLIST_FILE = iosApp/Info.plist;  
             IPHONEOS_DEPLOYMENT_TARGET = 15.4;  
             LD_RUNPATH_SEARCH_PATHS = (  
                "$(inherited)",  
                "@executable_path/Frameworks",  
             );  
             PRODUCT_BUNDLE_IDENTIFIER = com.thanh0x.FocusBloom;  
             PRODUCT_NAME = "${APP_NAME}";  
             PROVISIONING_PROFILE_SPECIFIER = "";  
             SWIFT_VERSION = 5.0;  
             TARGETED_DEVICE_FAMILY = "1,2";  
          };  
          name = Release;  
       };  
/* End XCBuildConfiguration section */  
  
/* Begin XCConfigurationList section */  
       7555FF76242A565900829871 /* Build configuration list for PBXProject "iosApp" */ = {  
          isa = XCConfigurationList;  
          buildConfigurations = (  
             7555FFA3242A565B00829871 /* Debug */,  
             7555FFA4242A565B00829871 /* Release */,  
          );  
          defaultConfigurationIsVisible = 0;  
          defaultConfigurationName = Release;  
       };  
       7555FFA5242A565B00829871 /* Build configuration list for PBXNativeTarget "iosApp" */ = {  
          isa = XCConfigurationList;  
          buildConfigurations = (  
             7555FFA6242A565B00829871 /* Debug */,  
             7555FFA7242A565B00829871 /* Release */,  
          );  
          defaultConfigurationIsVisible = 0;  
          defaultConfigurationName = Release;  
       };  
/* End XCConfigurationList section */  
    };  
    rootObject = 7555FF73242A565900829871 /* Project object */;  
}

-----

[versions]  
agp = "8.13.0"  
android-compileSdk = "36"  
android-minSdk = "24"  
android-targetSdk = "34"  
androidx-activityCompose = "1.11.0"  
androidx-appcompat = "1.7.1"  
androidx-constraintlayout = "2.2.1"  
androidx-core-ktx = "1.17.0"  
androidx-espresso-core = "3.7.0"  
androidx-lifecycle = "2.9.5"  
androidx-material = "1.13.0"  
androidx-test-junit = "1.3.0"  
compose-plugin = "1.9.1"  
junit = "4.13.2"  
kotlin = "2.2.21-RC2"  
  
playServicesAuth = "21.0.0"  
gitlive-firebase = "1.10.4"  
  
pods-google-maps = "8.4.0"  
pods-google-ios-maps-utils = "5.0.0"  
  
[libraries]  
kotlin-test = { module = "org.jetbrains.kotlin:kotlin-test", version.ref = "kotlin" }  
kotlin-test-junit = { module = "org.jetbrains.kotlin:kotlin-test-junit", version.ref = "kotlin" }  
junit = { group = "junit", name = "junit", version.ref = "junit" }  
androidx-core-ktx = { group = "androidx.core", name = "core-ktx", version.ref = "androidx-core-ktx" }  
androidx-test-junit = { group = "androidx.test.ext", name = "junit", version.ref = "androidx-test-junit" }  
androidx-espresso-core = { group = "androidx.test.espresso", name = "espresso-core", version.ref = "androidx-espresso-core" }  
androidx-appcompat = { group = "androidx.appcompat", name = "appcompat", version.ref = "androidx-appcompat" }  
androidx-material = { group = "com.google.android.material", name = "material", version.ref = "androidx-material" }  
androidx-constraintlayout = { group = "androidx.constraintlayout", name = "constraintlayout", version.ref = "androidx-constraintlayout" }  
androidx-activity-compose = { module = "androidx.activity:activity-compose", version.ref = "androidx-activityCompose" }  
androidx-lifecycle-viewmodel = { group = "org.jetbrains.androidx.lifecycle", name = "lifecycle-viewmodel", version.ref = "androidx-lifecycle" }  
androidx-lifecycle-runtime-compose = { group = "org.jetbrains.androidx.lifecycle", name = "lifecycle-runtime-compose", version.ref = "androidx-lifecycle" }  
  
firebase-gitlive-common = { module = "dev.gitlive:firebase-common", version.ref = "gitlive-firebase" }  
firebase-gitlive-auth = { module = "dev.gitlive:firebase-auth", version.ref = "gitlive-firebase" }  
  
play-services-auth = { module = "com.google.android.gms:play-services-auth", version.ref = "playServicesAuth" }  
  
  
[plugins]  
androidApplication = { id = "com.android.application", version.ref = "agp" }  
androidLibrary = { id = "com.android.library", version.ref = "agp" }  
jetbrainsCompose = { id = "org.jetbrains.compose", version.ref = "compose-plugin" }  
compose-compiler = { id = "org.jetbrains.kotlin.plugin.compose", version.ref = "kotlin" }  
kotlinMultiplatform = { id = "org.jetbrains.kotlin.multiplatform", version.ref = "kotlin" }  
kotlinCocoapods = { id = "org.jetbrains.kotlin.native.cocoapods", version.ref = "kotlin" }
-----
kotlin.code.style=official  
  
#Gradle  
org.gradle.jvmargs=-Xmx2048M -Dfile.encoding=UTF-8 -Dkotlin.daemon.jvm.options\="-Xmx2048M"  
  
#Android  
android.nonTransitiveRClass=true  
android.useAndroidX=true  
  
#Kotlin Multiplatform  
kotlin.mpp.enableCInteropCommonization=true  
kotlin.apple.xcodeCompatibility.nowarn=true
```
