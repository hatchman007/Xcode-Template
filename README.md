# Vokal Xcode Project Templates
> Maintained by [@bryanluby](https://github.com/bryanluby)

This repo contains Objective-C and Swift templates to start up a new Vokal project. These templates are a good start, but don't produce a ready-to-go project. Be sure to follow the steps in *After creating a project* each time you use one of these templates. Also note that creating the Xcode project is just one step in the process of [setting up a new project](https://engineering.vokal.io/iOS/Creating-Projects.md.html); consult that documentation before following instructions here.

For people outside of Vokal: you'll need to make some adjustments after creating your project from these templates. In particular, you'll need to remove our private podspec repo from the `Podfile`, and update the certificate repo in `fastlane/Matchfile`. If you have issues beyond that, feel free to [drop us a note](mailto:ios@vokal.io).

## Maintaining these templates
Apple doesn't have documentation on managing templates like this, but [there is a third-party guide for Xcode 4](https://web.archive.org/web/20180423060655/http://www.learn-cocos2d.com/store/xcode4-template-documentation) that's still mostly correct for Xcode 11.

If you're updating the templates, you'll note that there are Vokal-fied versions of several templates in this project - this is to facilitate making sure that certain stock files which would otherwise be created are not created. These were forked from the Xcode 7 templates, but as of Xcode 11, the base template had not changed significantly from the one included in 7.

Note that you may need to grab updated versions of these templates from the belly of Xcode when a new version of Xcode is released. As of Xcode 11, these templates can be found at the following paths:

* Projects: `/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates/Project Templates/iOS`
* Aggregate target: `/Applications/Xcode.app/Contents/Developer/Library/Xcode/Templates/Project Templates/Base/Other/Aggregate.xctemplate`

You'll also note that we have separate templates for Swift and Objective-C - there were several features for the Swift template which simply did not work at all without ripping it out into a separate template. Common code is within the `Vokal-Cocoa Touch Application Base.xctemplate` folder.

## Installation
To install or update the templates in Xcode:

- Clone or download this repository
- On the command line, `cd` into the `Xcode-Template` directory and run `./install.sh`
- Launch Xcode and create a new project. Confirm you see a Vokal category under iOS, as seen below.

![](screenshots/template_selection.png)

## After Creating A Project

### 1. Remove unnecessary references

- Open up the `.xcodeproj` file. 

- Remove the `Non-iOS Resources` folder from the Xcode project by selecting that folder in the sidebar, right clicking, then selecting delete from the menu, then selecting "Remove References" in the dialog that pops up: 

![](screenshots/remove_non_ios0.png)

![](screenshots/remove_non_ios1.png)

- Using this same method, remove the reference to the `Scripts` folder as well.

- Remove the references to the `.gitkeep` files which were used to create the folder/group hierarchy, but leave the groups themselves: 

![](screenshots/placeholders.png)

- Once those files are removed your groups should look like this: 

![](screenshots/placeholders_removed.png)

### 2. If You Added The Starter Network Utility

If you chose to include the Starter Network Utility when creating the project, you get a bunch of tests for free! The bad news is that templates are dumb, so you have to move them around. 

- Remove the references in "MoveToTestTarget" in the same way you did the Non-iOS resources.

- Go into the folder hierarchy and find the `MoveToTestTarget` folder. Select all these files and move them to the top-level of the test target directory. Then add these files to the test target by dragging them into Xcode:

**NOTE: For the `VOKMockData` folder make sure you've selected "Create folder references" so the mock data is added properly**.

![](screenshots/test_add_to_both_targets.png)

You can then delete the `MoveToTestTarget` folder from the filesystem as the test stuff has been moved over.

### 3. Cocoapod setup

- Close the project window in Xcode.

- On the command line, run `bundle install`. This will ensure that the correct version of CocoaPods is installed, along with the necessary gems for the build and test scripts that run on Travis, so that you can run them locally later on if needed. It will also generate `Gemfile.lock`, which should be committed so that the same versions of those gems will be installed on Travis. If this command fails, check that you have [RubyGems](https://rubygems.org/pages/download) and [Bundler](http://bundler.io/) installed.

- On the command line, run `bundle exec pod install`. Be sure to launch the project from the newly-created `.xcworkspace` file.

### 4. For All Projects

- For Objective-C projects, the Objective-Clean run script is already installed for you and the settings file is already in place. For Swift projects, SwiftLint is set up similarly. Fix any warnings that either utility generates when you build the workspace. If you get an error about Objective-Clean not being found, download and install the app following the [full instructions in our Objective-Clean docs](https://engineering.vokal.io/iOS/ObjClean/README.md.html).

- Create a git repository if one has not already been created: `git init`

- Commit the initial template files: `git add . && git commit -m "Add initial project files from template"`

- Hit ⌘-U to run the tests. The tests should fail since you haven't set anything up yet.

- Double-check the Travis configuration and add secure keys as needed. See our [Travis documentation](https://engineering.vokal.io/iOS/Fastlane-Travis-CI.md.html) for the full details on that. This step can be handled later, since most of the information needed for this probably isn't available yet.

- Find and address all the `TODO:`s in the boilerplate code.

## Notes On Scripts

All scripts added to a given project will be added to the `[project]/Scripts` folder. 

Build or Run scripts (designed to be run in conjunction with every single build or run which takes place within a given workspace) have been added to the appropriate place—either a post-build action or a Run Script Build Phase. 

Scripts tied to code generation are added to Run Script Build Phases in the Code Generation Scripts target. This target must be run manually to generate boilerplate code.

For Objective-C projects, this target runs the scripts for Cat2Cat and Mogenerator. For Swift, it only runs Mogenerator, since R.swift serves the same purpose as Cat2Cat in Swift projects.

Adjust the run scripts in this target as necessary. For example, if your project isn't using Core Data, you can remove the Mogenerator script build phase. You can also add additional build phases if you need to run other scripts.

## Version History

* 1.x: works with Swift 2.2 and 2.3 in Xcode 7 or 8
* 2.x: works with Swift 3 in Xcode 8
* 3.x: works with Swift 4 in Xcode 9
* 4.x: works with Swift 4.2 in Xcode 10
* 5.x: works with Swift 5.0 in Xcode 10.2
* 6.x: works with Swift 5.1 in Xcode 11
