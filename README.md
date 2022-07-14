# Xcode defaults

![Check Markdown links](https://github.com/ctreffs/xcode-defaults/workflows/Check%20Markdown%20links/badge.svg) [![License: CC BY-ND 4.0](https://img.shields.io/badge/License-CC%20BY--ND%204.0-green.svg)](https://creativecommons.org/licenses/by-nd/4.0/)

#### Backup Xcode defaults

```sh
defaults read com.apple.dt.Xcode > ~/Desktop/XcodeDefaults.plist
```

#### Restore Xcode vanilla defaults 

If you delete/move the current plist, Xcode will write a fresh one next time you run it.

```sh
killall Xcode
mv ~/Library/Preferences/com.apple.dt.Xcode.plist ~/Desktop/XcodeDefaults.plist
open -b com.apple.dt.Xcode
```

### New Swift build system mode

With [Xcode 13.3](https://developer.apple.com/documentation/xcode-release-notes/xcode-13_3-release-notes) the build system and Swift compiler have a new mode that better utilizes available cores, resulting in faster builds for Swift projects. The mode is opt-in, and you can enable globally with the following user default:

```sh
defaults write com.apple.dt.XCBuild EnableSwiftBuildSystemIntegration 1
```

Source [@BenchR](https://twitter.com/BenchR/status/1460699068846456832)

### Enable project build time

```sh
defaults write com.apple.dt.Xcode ShowBuildOperationDuration -bool YES
```


### Enable parallel builds for Swift

```sh
defaults write com.apple.dt.Xcode BuildSystemScheduleInherentlyParallelCommandsExclusively -bool YES
```

### Disable parallel builds for Swift 

Xcode 9.3 now runs more Swift build tasks in parallel with other commands. This may improve build times for Swift projects, but may also increase memory use during the build. This feature can be disabled from Terminal by setting a user default with 

```sh
defaults write com.apple.dt.Xcode BuildSystemScheduleInherentlyParallelCommandsSerially -bool YES
```

### Enable multi-cursor editing

```sh
defaults write com.apple.dt.Xcode PegasusMultipleCursorsEnabled -bool YES
```

### Set maximum number of concurrent compile tasks

```sh
defaults write com.apple.dt.Xcode IDEBuildOperationMaxNumberOfConcurrentCompileTasks `sysctl -n hw.ncpu`
```

Where `sysctl -n hw.ncpu` gives you the number of CPU threads.

### Set maximum number of parallel build subtasks

```sh
defaults write com.apple.dt.Xcode PBXNumberOfParallelBuildSubtasks `sysctl -n hw.ncpu`
```

Where `sysctl -n hw.ncpu` gives you the number of CPU threads.

### Enable/Disable indexing

Keep in mind that disabling indexing will disable `refactor` code action so you probably won't be able to generate protocol conformances or memberwise initializers

##### Enable indexing

```sh
defaults delete com.apple.dt.Xcode IDEIndexDisable
defaults write com.apple.dt.Xcode IDEIndexEnable -bool YES
```

##### Disable indexing

```sh
defaults delete com.apple.dt.Xcode IDEIndexEnable
defaults write com.apple.dt.Xcode IDEIndexDisable -bool YES
```

### Show Indexing numeric progress

```sh
defaults write com.apple.dt.Xcode IDEIndexerActivityShowNumericProgress -bool YES
```

### Show Indexing logging

This will show you why a particular file is having trouble being compiled for indexing.

```sh
defaults write com.apple.dt.Xcode IDEIndexShowLog -bool YES
```

### Show prebuild step logs

```sh
defaults write com.apple.dt.Xcode IDEShowPrebuildLogs -bool YES
```

### Set SourceKit log level

If set to SourceKit will write a log to /tmp with all the details of what it is doing while indexing. 
A lot of people find they have header hygiene problems or module problems that happen to work while building in certain configurations but aren't actually correct, resulting in missing modules or broken headers from the indexer's point of view.
May not work anymore.

```sh
defaults write com.apple.dt.Xcode IDESourceKitServiceLogLevel -int 3 
```

Also you can do this temporarily by pre-pending this environment variable when starting Xcode:

```sh
SOURCEKIT_LOGGING=3 /Applications/Xcode.app/Contents/MacOS/Xcode
```
Source [AppCode under the hood - Aydar Mukhametzyanov - NSSpain 2021](https://vimeo.com/479319821#t=1070s)

### Disable Main Thread Checker

Deactivates the Main Thread Checker. Found in [Xcode 12 release notes](https://developer.apple.com/documentation/xcode-release-notes/)

```sh
defaults write com.apple.dt.Xcode DVTDisableMainThreadChecker 1
```

### Enable internal Xcode (debug) menu

```sh
defaults write com.apple.dt.Xcode ShowDVTDebugMenu -bool YES
```

### Disable automatic reopening of last project

Prevents Xcode from automatically restoring the last open project. 
This enables running multiple Xcode versions for different projects.

```sh
defaults write com.apple.dt.Xcode ApplePersistenceIgnoreState -bool YES
```

### Some minimal additional logging 

```sh
defaults write com.apple.dt.XCBuild EnableDebugActivityLogs -bool YES
```

### Enable build debugging mode

This slows down the build system & litters DerivedData/<project>/Build/Intermediates.noindex), generally should only be enabled when trying to capture a trace for incremental build debugging purposes.
  
```sh
defaults write com.apple.dt.XCBuild EnableBuildDebugging -bool YES
```

### Make Assistant aware of more companion files

Make Xcode's Assistant aware of your ViewModels, Views, etc.
Found by [@peterfriese](https://twitter.com/peterfriese/status/1364544309878534144)

```sh
defaults write com.apple.dt.Xcode IDEAdditionalCounterpartSuffixes -array-add "ViewModel" "View" "Screen"
```

### Disable move files on restructure

Do not move files when you restructure things in an Xcode project. Found by [@steinpete](https://twitter.com/steipete/status/1287057462454038528)

```sh
defaults write com.apple.dt.Xcode IDEDisableStructureEditingCoordinator -bool YES 
```


### Disable state restoration

Stop Xcode from reopening files on launch. Found by [@SmileyKeith](https://twitter.com/SmileyKeith/status/759850753993375745)

```sh
defaults write com.apple.dt.Xcode IDEDisableStateRestoration -bool YES
```

### Homebrew prefix path

[Homebrew's](https://brew.sh) default [prefix/install path](https://docs.brew.sh/Manpage#--prefix---unbrewed---installed-formula-) can point to different locations depending on your system.   
Xcode by default only looks for `/opt/brew` and `/usr/local`.   
You can adjust that location by writing the following default.  
To get your Homebrew prefix call `brew --prefix`.
Source [@NeoNacho](https://twitter.com/NeoNacho/status/1412514541343166467?s=20)

```sh
defaults write com.apple.dt.Xcode IDEHomebrewPrefixPath -string <BREW_PREFIX>
```
  
### Swift package remote source path

Specify the directory to which remote source packages are fetch or expected to be found.
`xcodebuild` has the same option as `clonedSourcePackagesDirPath`.
Source [@bguidolim](https://twitter.com/bguidolim/status/1249418542111432711)

```sh
defaults write com.apple.dt.Xcode IDEClonedSourcePackagesDirPathOverride -string <PATH>
```

## 📱 Simulator

### Enable Simulator fullscreen mode

```sh
defaults write com.apple.iphonesimulator AllowFullscreenMode -bool YES
```

## 🏗️ XCBuild

### Enable Build Debugging 

Creates an XCBuildData folder in `~/Library/Developer/Xcode/DerivedData/<your target>/Build/Intermediates.noindex/` which contains debugging info for xcodebuild.

```sh
defaults write com.apple.dt.XCBuild -bool YES
```


## 🗑️ Xcode Cleanups


### Remove all Xcode DerivedData

... provided Xcode is set to default folder locations.
This is a quick win and helps to get back gigabytes of storage space.
Do this regularly.

```sh
rm -rdf ~/Library/Developer/Xcode/DerivedData/*
```


### Remove all Xcode DeveloperTools cache files

... provided Xcode is set to default folder locations. 


```sh
CACHE=$(getconf DARWIN_USER_CACHE_DIR)
rm -rdf ${CACHE}com.apple.DeveloperTools
rm -rdf ${CACHE}org.llvm.clang.$(whoami)/ModuleCache
rm -rdf ${CACHE}org.llvm.clang/ModuleCache
rm -rdf ~/Library/Caches/com.apple.dt.*/*
```

#### Remove all Xcode / Swift temporary files. 

```sh
TMP=$(getconf DARWIN_USER_TEMP_DIR)
rm -rdf ${TMP}*.swift
rm -rdf ${TMP}ibtool*
rm -rdf ${TMP}*IBTOOLD*
rm -rdf ${TMP}supplementaryOutputs-*
rm -rdf ${TMP}xcrun_db
rm -rdf ${TMP}sources-*
rm -rdf ${TMP}com.apple.dt.*
rm -rdf ${TMP}com.apple.test.*
```

## 🧰 Tools
  
- [DevCleaner](https://github.com/vashpan/xcode-dev-cleaner) If you want to reclaim tens of gigabytes of your storage used for various Xcode caches - this tool is for you!
- [Kintsugi](https://github.com/Lightricks/Kintsugi) A tool to automatically resolve Git conflicts that occur in Xcode project files
- [BuildTimeAnalyzer](https://github.com/RobertGummesson/BuildTimeAnalyzer-for-Xcode) A build time analyzer for Xcode
- [XCLogParser](https://github.com/MobileNativeFoundation/XCLogParser) A tool to parse Xcode and xcodebuild logs stored in the xcactivitylog format
- [XCMetrics](https://github.com/spotify/XCMetrics) A tool to collect Xcode build metrics and improve developer productivity
