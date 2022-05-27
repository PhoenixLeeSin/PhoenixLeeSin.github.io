# RN 生成的安卓工程配置文件以及问题处理(RN版本0.64.2)

## 1. RN根目录/android/build.gradle

```
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    ext {
        buildToolsVersion = "25.0.2"
        minSdkVersion = 21
        compileSdkVersion = 30
        targetSdkVersion = 30
        ndkVersion = "20.1.5948944"
        supportLibVersion           = "27.0.0"
    }
    repositories {
        maven{url 'http://maven.aliyun.com/nexus/content/groups/public/'}
        google()
        jcenter()
    }
    dependencies {
        classpath("com.android.tools.build:gradle:4.1.0")
        classpath 'com.google.gms:google-services:4.3.3'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        maven{url 'http://maven.aliyun.com/nexus/content/groups/public/'}
        mavenLocal()
        maven {
            // All of React Native (JS, Obj-C sources, Android binaries) is installed from npm
            url("$rootDir/../node_modules/react-native/android")
        }
        maven {
            // Android JSC is installed from npm
            url("$rootDir/../node_modules/jsc-android/dist")
        }

        google()
        jcenter()
        maven { url "https://www.jitpack.io" }
        maven { url "https://maven.google.com" }
    }
}
```

## 2. RN根目录/android/gradle.properties

```
# Project-wide Gradle settings.

# IDE (e.g. Android Studio) users:
# Gradle settings configured through the IDE *will override*
# any settings specified in this file.

# For more details on how to configure your build environment visit
# http://www.gradle.org/docs/current/userguide/build_environment.html

# Specifies the JVM arguments used for the daemon process.
# The setting is particularly useful for tweaking memory settings.
# Default value: -Xmx10248m -XX:MaxPermSize=256m
# org.gradle.jvmargs=-Xmx2048m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8

# When configured, Gradle will run in incubating parallel mode.
# This option should only be used with decoupled projects. More details, visit
# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:decoupled_projects
# org.gradle.parallel=true

# AndroidX package structure to make it clearer which packages are bundled with the
# Android operating system, and which are packaged with your app's APK
# https://developer.android.com/topic/libraries/support-library/androidx-rn
android.useAndroidX=true
# Automatically convert third-party libraries to use AndroidX
android.enableJetifier=true

# Version of flipper SDK to use with React Native
FLIPPER_VERSION=0.75.1
org.gradle.jvmargs=-Xmx4g
org.gradle.parallel=false
org.gradle.caching=false
org.gradle.configureondemand=false
```

## 3.RN根目录/android/settings.gradle

```
rootProject.name = 'rngxtt'
apply from: file("../node_modules/@react-native-community/cli-platform-android/native_modules.gradle"); applyNativeModulesSettingsGradle(settings)
include ':app', ':react-native-code-push'
project(':react-native-code-push').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-code-push/android/app')



// include ':app', ':react-native-code-push', ':react-native-splash-screen'
// project(':react-native-code-push').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-code-push/android/app')
// project(':react-native-splash-screen').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-splash-screen/android')
```



## 4. RN根目录/android/app/build.gradle

```
apply plugin: "com.android.application"

import com.android.build.OutputFile

/**
 * The react.gradle file registers a task for each build variant (e.g. bundleDebugJsAndAssets
 * and bundleReleaseJsAndAssets).
 * These basically call `react-native bundle` with the correct arguments during the Android build
 * cycle. By default, bundleDebugJsAndAssets is skipped, as in debug/dev mode we prefer to load the
 * bundle directly from the development server. Below you can see all the possible configurations
 * and their defaults. If you decide to add a configuration block, make sure to add it before the
 * `apply from: "../../node_modules/react-native/react.gradle"` line.
 *
 * project.ext.react = [
 *   // the name of the generated asset file containing your JS bundle
 *   bundleAssetName: "index.android.bundle",
 *
 *   // the entry file for bundle generation. If none specified and
 *   // "index.android.js" exists, it will be used. Otherwise "index.js" is
 *   // default. Can be overridden with ENTRY_FILE environment variable.
 *   entryFile: "index.android.js",
 *
 *   // https://reactnative.dev/docs/performance#enable-the-ram-format
 *   bundleCommand: "ram-bundle",
 *
 *   // whether to bundle JS and assets in debug mode
 *   bundleInDebug: false,
 *
 *   // whether to bundle JS and assets in release mode
 *   bundleInRelease: true,
 *
 *   // whether to bundle JS and assets in another build variant (if configured).
 *   // See http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Build-Variants
 *   // The configuration property can be in the following formats
 *   //         'bundleIn${productFlavor}${buildType}'
 *   //         'bundleIn${buildType}'
 *   // bundleInFreeDebug: true,
 *   // bundleInPaidRelease: true,
 *   // bundleInBeta: true,
 *
 *   // whether to disable dev mode in custom build variants (by default only disabled in release)
 *   // for example: to disable dev mode in the staging build type (if configured)
 *   devDisabledInStaging: true,
 *   // The configuration property can be in the following formats
 *   //         'devDisabledIn${productFlavor}${buildType}'
 *   //         'devDisabledIn${buildType}'
 *
 *   // the root of your project, i.e. where "package.json" lives
 *   root: "../../",
 *
 *   // where to put the JS bundle asset in debug mode
 *   jsBundleDirDebug: "$buildDir/intermediates/assets/debug",
 *
 *   // where to put the JS bundle asset in release mode
 *   jsBundleDirRelease: "$buildDir/intermediates/assets/release",
 *
 *   // where to put drawable resources / React Native assets, e.g. the ones you use via
 *   // require('./image.png')), in debug mode
 *   resourcesDirDebug: "$buildDir/intermediates/res/merged/debug",
 *
 *   // where to put drawable resources / React Native assets, e.g. the ones you use via
 *   // require('./image.png')), in release mode
 *   resourcesDirRelease: "$buildDir/intermediates/res/merged/release",
 *
 *   // by default the gradle tasks are skipped if none of the JS files or assets change; this means
 *   // that we don't look at files in android/ or ios/ to determine whether the tasks are up to
 *   // date; if you have any other folders that you want to ignore for performance reasons (gradle
 *   // indexes the entire tree), add them here. Alternatively, if you have JS files in android/
 *   // for example, you might want to remove it from here.
 *   inputExcludes: ["android/**", "ios/**"],
 *
 *   // override which node gets called and with what additional arguments
 *   nodeExecutableAndArgs: ["node"],
 *
 *   // supply additional arguments to the packager
 *   extraPackagerArgs: []
 * ]
 */

project.ext.react = [
    enableHermes: false,  // clean and rebuild if changing
]

apply from: "../../node_modules/react-native/react.gradle"
apply from: "../../node_modules/react-native-code-push/android/codepush.gradle"

/**
 * Set this to true to create two separate APKs instead of one:
 *   - An APK that only works on ARM devices
 *   - An APK that only works on x86 devices
 * The advantage is the size of the APK is reduced by about 4MB.
 * Upload all the APKs to the Play Store and people will download
 * the correct one based on the CPU architecture of their device.
 */
def enableSeparateBuildPerCPUArchitecture = false

/**
 * Run Proguard to shrink the Java bytecode in release builds.
 */
def enableProguardInReleaseBuilds = false

/**
 * The preferred build flavor of JavaScriptCore.
 *
 * For example, to use the international variant, you can use:
 * `def jscFlavor = 'org.webkit:android-jsc-intl:+'`
 *
 * The international variant includes ICU i18n library and necessary data
 * allowing to use e.g. `Date.toLocaleString` and `String.localeCompare` that
 * give correct results when using with locales other than en-US.  Note that
 * this variant is about 6MiB larger per architecture than default.
 */
def jscFlavor = 'org.webkit:android-jsc:+'

/**
 * Whether to enable the Hermes VM.
 *
 * This should be set on project.ext.react and mirrored here.  If it is not set
 * on project.ext.react, JavaScript will not be compiled to Hermes Bytecode
 * and the benefits of using Hermes will therefore be sharply reduced.
 */
def enableHermes = project.ext.react.get("enableHermes", false);

android {
    ndkVersion rootProject.ext.ndkVersion

    compileSdkVersion rootProject.ext.compileSdkVersion

    aaptOptions.cruncherEnabled = false
    aaptOptions.useNewCruncher = false

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }

    defaultConfig {
        applicationId "com.rngxtt"
        minSdkVersion rootProject.ext.minSdkVersion
        targetSdkVersion rootProject.ext.targetSdkVersion
        versionCode 1
        versionName "1.0.0"
        missingDimensionStrategy 'react-native-camera', 'general'
        flavorDimensions "versionCode"
    }
//     splits {
//         abi {
//             reset()
//             enable enableSeparateBuildPerCPUArchitecture
//             universalApk false  // If true, also generate a universal APK
//             include "armeabi-v7a", "x86", "arm64-v8a", "x86_64"
//         }
//     }
    signingConfigs {
        proSign {
            storeFile file('GenerateAPK.jks')
            storePassword 'guoxin@123'
            keyPassword 'guoxin@123'
            keyAlias = 'guoxin@123'
        }
        devSigin {
            storeFile file('GenerateAPK.jks')
            storePassword 'guoxin@123'
            keyPassword 'guoxin@123'
            keyAlias = 'guoxin@123'
        }

    }
    productFlavors {
        dev {
            buildConfigField "String", "CODEPUSH_KEY", '"ltiiLlrqUA0XkB6uE7dqpBv2HC6A4ksvOXqog"'
            buildConfigField "String", "CODEPUSH_URL", '"http://119.167.151.36:3000/"'
            signingConfig signingConfigs.proSign
        }
        pro {
            buildConfigField "String", "CODEPUSH_KEY", '"JWSBHWq8NaSsI5aT2iqsYqICPl9w4ksvOXqog"'
            buildConfigField "String", "CODEPUSH_URL", '"http://119.167.151.36:3000/"'
            signingConfig signingConfigs.proSign
        }
    }
    buildTypes {
        debug {
//            signingConfig signingConfigs.debug
        }
        release {
            // Caution! In production, you need to generate your own keystore file.
            // see https://reactnative.dev/docs/signed-apk-android.
//            signingConfig signingConfigs.debug
            minifyEnabled enableProguardInReleaseBuilds
            proguardFiles getDefaultProguardFile("proguard-android.txt"), "proguard-rules.pro"
        }
    }

    // applicationVariants are e.g. debug, release
    applicationVariants.all { variant ->
        variant.outputs.each { output ->
            // For each separate APK per architecture, set a unique version code as described here:
            // https://developer.android.com/studio/build/configure-apk-splits.html
            // Example: versionCode 1 will generate 1001 for armeabi-v7a, 1002 for x86, etc.
            def versionCodes = ["armeabi-v7a": 1, "x86": 2, "arm64-v8a": 3, "x86_64": 4]
            def abi = output.getFilter(OutputFile.ABI)
            if (abi != null) {  // null for the universal-debug, universal-release variants
                output.versionCodeOverride =
                        defaultConfig.versionCode * 1000 + versionCodes.get(abi)
            }

        }
    }
}

dependencies {
    implementation fileTree(dir: "libs", include: ["*.jar"])
    //noinspection GradleDynamicVersion
    implementation "com.facebook.react:react-native:+"  // From node_modules
    implementation project(path: ':react-native-code-push')  // From node_modules

    implementation "androidx.swiperefreshlayout:swiperefreshlayout:1.0.0"

    debugImplementation("com.facebook.flipper:flipper:${FLIPPER_VERSION}") {
      exclude group:'com.facebook.fbjni'
    }

    debugImplementation("com.facebook.flipper:flipper-network-plugin:${FLIPPER_VERSION}") {
        exclude group:'com.facebook.flipper'
        exclude group:'com.squareup.okhttp3', module:'okhttp'
    }

    debugImplementation("com.facebook.flipper:flipper-fresco-plugin:${FLIPPER_VERSION}") {
        exclude group:'com.facebook.flipper'
    }

    if (enableHermes) {
        def hermesPath = "../../node_modules/hermes-engine/android/";
        debugImplementation files(hermesPath + "hermes-debug.aar")
        releaseImplementation files(hermesPath + "hermes-release.aar")
    } else {
        implementation jscFlavor
    }
}

// Run this once to be able to run the application with BUCK
// puts all compile dependencies into folder libs for BUCK to use
task copyDownloadableDepsToLibs(type: Copy) {
    from configurations.compile
    into 'libs'
}

apply from: file("../../node_modules/@react-native-community/cli-platform-android/native_modules.gradle"); applyNativeModulesAppBuildGradle(project)
```



## 5. RN工程根目录/node_modules/react-native/react.gradle

```
/*
 * Copyright (c) Facebook, Inc. and its affiliates.
 *
 * This source code is licensed under the MIT license found in the
 * LICENSE file in the root directory of this source tree.
 */

import org.apache.tools.ant.taskdefs.condition.Os

def config = project.hasProperty("react") ? project.react : [:];

def detectEntryFile(config) {
    if (System.getenv('ENTRY_FILE')) {
        return System.getenv('ENTRY_FILE')
    } else if (config.entryFile) {
        return config.entryFile
    } else if ((new File("${projectDir}/../../index.android.js")).exists()) {
        return "index.android.js"
    }

    return "index.js";
}

/**
 * Detects CLI location in a similar fashion to the React Native CLI
 */
def detectCliPath(config) {
    if (config.cliPath) {
        return config.cliPath
    }
    if (new File("${projectDir}/../../node_modules/react-native/cli.js").exists()) {
        return "${projectDir}/../../node_modules/react-native/cli.js"
    }
    throw new Exception("Couldn't determine CLI location. " +
             "Please set `project.ext.react.cliPath` to the path of the react-native cli.js");
}

def composeSourceMapsPath = config.composeSourceMapsPath ?: "node_modules/react-native/scripts/compose-source-maps.js"
def bundleAssetName = config.bundleAssetName ?: "index.android.bundle"
def entryFile = detectEntryFile(config)
def bundleCommand = config.bundleCommand ?: "bundle"
def reactRoot = file(config.root ?: "../../")
def inputExcludes = config.inputExcludes ?: ["android/**", "ios/**"]
def bundleConfig = config.bundleConfig ? "${reactRoot}/${config.bundleConfig}" : null ;
def enableVmCleanup = config.enableVmCleanup == null ? true : config.enableVmCleanup
def hermesCommand = config.hermesCommand ?: "../../node_modules/hermes-engine/%OS-BIN%/hermesc"

def reactNativeDevServerPort() {
    def value = project.getProperties().get("reactNativeDevServerPort")
    return value != null ? value : "8081"
}

def reactNativeInspectorProxyPort() {
    def value = project.getProperties().get("reactNativeInspectorProxyPort")
    return value != null ? value : reactNativeDevServerPort()
}

def getHermesOSBin() {
    if (Os.isFamily(Os.FAMILY_WINDOWS)) return "win64-bin";
    if (Os.isFamily(Os.FAMILY_MAC)) return "osx-bin";
    if (Os.isOs(null, "linux", "amd64", null)) return "linux64-bin";
    throw new Exception("OS not recognized. Please set project.ext.react.hermesCommand " +
                        "to the path of a working Hermes compiler.");
}

// Make sure not to inspect the Hermes config unless we need it,
// to avoid breaking any JSC-only setups.
def getHermesCommand = {
    // If the project specifies a Hermes command, don't second guess it.
    if (!hermesCommand.contains("%OS-BIN%")) {
        return hermesCommand
    }

    // Execution on Windows fails with / as separator
    return hermesCommand
            .replaceAll("%OS-BIN%", getHermesOSBin())
            .replace('/' as char, File.separatorChar);
}

// Set enableHermesForVariant to a function to configure per variant,
// or set `enableHermes` to True/False to set all of them
def enableHermesForVariant = config.enableHermesForVariant ?: {
    def variant -> config.enableHermes ?: false
}

android {
    buildTypes.all {
        resValue "integer", "react_native_dev_server_port", reactNativeDevServerPort()
        resValue "integer", "react_native_inspector_proxy_port", reactNativeInspectorProxyPort()
    }
}

afterEvaluate {
    def isAndroidLibrary = plugins.hasPlugin("com.android.library")
    def variants = isAndroidLibrary ? android.libraryVariants : android.applicationVariants
    variants.all { def variant ->
        // Create variant and target names
        def targetName = variant.name.capitalize()
        def targetPath = variant.dirName

        // React js bundle directories
        def jsBundleDir = file("$buildDir/generated/assets/react/${targetPath}")
        def resourcesDir = file("$buildDir/generated/res/react/${targetPath}")

        def jsBundleFile = file("$jsBundleDir/$bundleAssetName")
        def jsSourceMapsDir = file("$buildDir/generated/sourcemaps/react/${targetPath}")
        def jsIntermediateSourceMapsDir = file("$buildDir/intermediates/sourcemaps/react/${targetPath}")
        def jsPackagerSourceMapFile = file("$jsIntermediateSourceMapsDir/${bundleAssetName}.packager.map")
        def jsCompilerSourceMapFile = file("$jsIntermediateSourceMapsDir/${bundleAssetName}.compiler.map")
        def jsOutputSourceMapFile = file("$jsSourceMapsDir/${bundleAssetName}.map")

        // Additional node and packager commandline arguments
        def nodeExecutableAndArgs = config.nodeExecutableAndArgs ?: ["node"]
        def cliPath = detectCliPath(config)

        def execCommand = []

        if (Os.isFamily(Os.FAMILY_WINDOWS)) {
            execCommand.addAll(["cmd", "/c", *nodeExecutableAndArgs, cliPath])
        } else {
            execCommand.addAll([*nodeExecutableAndArgs, cliPath])
        }

        def enableHermes = enableHermesForVariant(variant)

        def currentBundleTask = tasks.create(
            name: "bundle${targetName}JsAndAssets",
            type: Exec) {
            group = "react"
            description = "bundle JS and assets for ${targetName}."

            // Create dirs if they are not there (e.g. the "clean" task just ran)
            doFirst {
                jsBundleDir.deleteDir()
                jsBundleDir.mkdirs()
                resourcesDir.deleteDir()
                resourcesDir.mkdirs()
                jsIntermediateSourceMapsDir.deleteDir()
                jsIntermediateSourceMapsDir.mkdirs()
                jsSourceMapsDir.deleteDir()
                jsSourceMapsDir.mkdirs()
            }
            doLast {
                def moveFunc = { resSuffix ->
                    File originalDir = file("$buildDir/generated/res/react/dev/release/drawable-${resSuffix}");
                    if (originalDir.exists()) {
                        File destDir = file("$buildDir/../src/main/res/drawable-${resSuffix}");
                        ant.move(file: originalDir, tofile: destDir);
                    }
                }
                moveFunc.curry("ldpi").call()
                moveFunc.curry("mdpi").call()
                moveFunc.curry("hdpi").call()
                moveFunc.curry("xhdpi").call()
                moveFunc.curry("xxhdpi").call()
                moveFunc.curry("xxxhdpi").call()
            }

            // Set up inputs and outputs so gradle can cache the result
            inputs.files fileTree(dir: reactRoot, excludes: inputExcludes)
            outputs.dir(jsBundleDir)
            outputs.dir(resourcesDir)

            // Set up the call to the react-native cli
            workingDir(reactRoot)

            // Set up dev mode
            def devEnabled = !(config."devDisabledIn${targetName}"
                || targetName.toLowerCase().contains("release"))

            def extraArgs = config.extraPackagerArgs ?: [];

            if (bundleConfig) {
                extraArgs = extraArgs.clone()
                extraArgs.add("--config");
                extraArgs.add(bundleConfig);
            }

            commandLine(*execCommand, bundleCommand, "--platform", "android", "--dev", "${devEnabled}",
                "--reset-cache", "--entry-file", entryFile, "--bundle-output", jsBundleFile, "--assets-dest", resourcesDir,
                "--sourcemap-output", enableHermes ? jsPackagerSourceMapFile : jsOutputSourceMapFile, *extraArgs)


            if (enableHermes) {
                doLast {
                    def hermesFlags;
                    def hbcTempFile = file("${jsBundleFile}.hbc")
                    exec {
                        if (targetName.toLowerCase().contains("release")) {
                            // Can't use ?: since that will also substitute valid empty lists
                            hermesFlags = config.hermesFlagsRelease
                            if (hermesFlags == null) hermesFlags = ["-O", "-output-source-map"]
                        } else {
                            hermesFlags = config.hermesFlagsDebug
                            if (hermesFlags == null) hermesFlags = []
                        }

                        if (Os.isFamily(Os.FAMILY_WINDOWS)) {
                            commandLine("cmd", "/c", getHermesCommand(), "-emit-binary", "-out", hbcTempFile, jsBundleFile, *hermesFlags)
                        } else {
                            commandLine(getHermesCommand(), "-emit-binary", "-out", hbcTempFile, jsBundleFile, *hermesFlags)
                        }
                    }
                    ant.move(
                        file: hbcTempFile,
                        toFile: jsBundleFile
                    );
                    if (hermesFlags.contains("-output-source-map")) {
                        ant.move(
                            // Hermes will generate a source map with this exact name
                            file: "${jsBundleFile}.hbc.map",
                            tofile: jsCompilerSourceMapFile
                        );
                        exec {
                            // TODO: set task dependencies for caching

                            // Set up the call to the compose-source-maps script
                            workingDir(reactRoot)
                            if (Os.isFamily(Os.FAMILY_WINDOWS)) {
                                commandLine("cmd", "/c", *nodeExecutableAndArgs, composeSourceMapsPath, jsPackagerSourceMapFile, jsCompilerSourceMapFile, "-o", jsOutputSourceMapFile)
                            } else {
                                commandLine(*nodeExecutableAndArgs, composeSourceMapsPath, jsPackagerSourceMapFile, jsCompilerSourceMapFile, "-o", jsOutputSourceMapFile)
                            }
                        }
                    }
                }
            }

            enabled config."bundleIn${targetName}" != null
                ? config."bundleIn${targetName}"
                : config."bundleIn${variant.buildType.name.capitalize()}" != null
                    ? config."bundleIn${variant.buildType.name.capitalize()}"
                    : targetName.toLowerCase().contains("release")

                        if (isAndroidLibrary) {
                            doLast {
                                def moveFunc = { resSuffix ->
                                    File originalDir = file("${resourcesDir}/drawable-${resSuffix}")
                                    if (originalDir.exists()) {
                                        File destDir = file("${resourcesDir}/drawable-${resSuffix}-v4")
                                        ant.move(file: originalDir, tofile: destDir)
                                    }
                                }
                                moveFunc.curry("ldpi").call()
                                moveFunc.curry("mdpi").call()
                                moveFunc.curry("hdpi").call()
                                moveFunc.curry("xhdpi").call()
                                moveFunc.curry("xxhdpi").call()
                                moveFunc.curry("xxxhdpi").call()
                            }
                        }
        }

        // Expose a minimal interface on the application variant and the task itself:
        variant.ext.bundleJsAndAssets = currentBundleTask
        currentBundleTask.ext.generatedResFolders = files(resourcesDir).builtBy(currentBundleTask)
        currentBundleTask.ext.generatedAssetsFolders = files(jsBundleDir).builtBy(currentBundleTask)

        // registerGeneratedResFolders for Android plugin 3.x
        if (variant.respondsTo("registerGeneratedResFolders")) {
            variant.registerGeneratedResFolders(currentBundleTask.generatedResFolders)
        } else {
            variant.registerResGeneratingTask(currentBundleTask)
        }
        variant.mergeResourcesProvider.get().dependsOn(currentBundleTask)

        // packageApplication for Android plugin 3.x
        def packageTask = variant.hasProperty("packageApplication")
            ? variant.packageApplicationProvider.get()
            : tasks.findByName("package${targetName}")
        if (variant.hasProperty("packageLibrary")) {
            packageTask = variant.packageLibrary
        }

        // pre bundle build task for Android plugin 3.2+
        def buildPreBundleTask = tasks.findByName("build${targetName}PreBundle")

        def resourcesDirConfigValue = config."resourcesDir${targetName}"
        if (resourcesDirConfigValue) {
            def currentCopyResTask = tasks.create(
                name: "copy${targetName}BundledResources",
                type: Copy) {
                group = "react"
                description = "copy bundled resources into custom location for ${targetName}."

                from(resourcesDir)
                into(file(resourcesDirConfigValue))

                dependsOn(currentBundleTask)

                enabled(currentBundleTask.enabled)
            }

            packageTask.dependsOn(currentCopyResTask)
            if (buildPreBundleTask != null) {
                buildPreBundleTask.dependsOn(currentCopyResTask)
            }
        }

        def currentAssetsCopyTask = tasks.create(
            name: "copy${targetName}BundledJs",
            type: Copy) {
            group = "react"
            description = "copy bundled JS into ${targetName}."

            if (config."jsBundleDir${targetName}") {
                from(jsBundleDir)
                into(file(config."jsBundleDir${targetName}"))
            } else {
                into ("$buildDir/intermediates")
                into ("assets/${targetPath}") {
                    from(jsBundleDir)
                }

                // Workaround for Android Gradle Plugin 3.2+ new asset directory
                into ("merged_assets/${variant.name}/merge${targetName}Assets/out") {
                    from(jsBundleDir)
                }

                // Workaround for Android Gradle Plugin 3.4+ new asset directory
                into ("merged_assets/${variant.name}/out") {
                    from(jsBundleDir)
                }
            }

            // mergeAssets must run first, as it clears the intermediates directory
            dependsOn(variant.mergeAssetsProvider.get())

            enabled(currentBundleTask.enabled)
        }

        // mergeResources task runs before the bundle file is copied to the intermediate asset directory from Android plugin 4.1+.
        // This ensures to copy the bundle file before mergeResources task starts
        def mergeResourcesTask = tasks.findByName("merge${targetName}Resources")
        mergeResourcesTask.dependsOn(currentAssetsCopyTask)

        packageTask.dependsOn(currentAssetsCopyTask)
        if (buildPreBundleTask != null) {
            buildPreBundleTask.dependsOn(currentAssetsCopyTask)
        }

        // Delete the VM related libraries that this build doesn't need.
        // The application can manage this manually by setting 'enableVmCleanup: false'
        //
        // This should really be done by packaging all Hermes releated libs into
        // two separate HermesDebug and HermesRelease AARs, but until then we'll
        // kludge it by deleting the .so files out of the /transforms/ directory.
        def isRelease = targetName.toLowerCase().contains("release")
        def libDir = "$buildDir/intermediates/transforms/"
        def vmSelectionAction = {
            fileTree(libDir).matching {
                if (enableHermes) {
                    // For Hermes, delete all the libjsc* files
                    include "**/libjsc*.so"

                    if (isRelease) {
                        // Reduce size by deleting the debugger/inspector
                        include '**/libhermes-inspector.so'
                        include '**/libhermes-executor-debug.so'
                    } else {
                        // Release libs take precedence and must be removed
                        // to allow debugging
                        include '**/libhermes-executor-release.so'
                    }
                } else {
                    // For JSC, delete all the libhermes* files
                    include "**/libhermes*.so"
                }
            }.visit { details ->
                def targetVariant = ".*/transforms/[^/]*/${targetPath}/.*"
                def path = details.file.getAbsolutePath().replace(File.separatorChar, '/' as char)
                if (path.matches(targetVariant) && details.file.isFile()) {
                    details.file.delete()
                }
            }
        }

        if (enableVmCleanup) {
            def task = tasks.findByName("package${targetName}")
            task.doFirst(vmSelectionAction)
        }
    }
}
```



### 打包错误问题（安卓）

#### 1. A failure occurred while executing com.android.build.gradle.tasks.PackageAndroidArtifact$IncrementalSplitterRunnable

解决： https://stackoverflow.com/questions/65157720/a-failure-occurred-while-executing-com-android-build-gradle-tasks-packageandroid



#### 2. [Android] Error: Duplicate resources

https://github.com/facebook/react-native/issues/22234  先删除生成的apk。