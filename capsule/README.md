# Capsule

Crawling Apps UIs (and Learning Everything)

## Synopsis

Capsule is a program designed to crawl all of the UIs of an Android app and
store the view hierarchies, screenshots, and relationships between views. It
attempts to click on all clickable components and reach as many unique views as
possible.

Capsule requires an Android emulator or phone using a debug version of Android
and uses the
[AndroidViewClient](https://github.com/dtmilano/AndroidViewClient) library and
[Android Debug Bridge](https://developer.android.com/studio/command-line/adb.html)
commands to communicate with the device.

The application considers views to be distinct if they have a different activity
name, fragment composition, or view hierarchy. It stores all of the data in
a /capsule/data/ directory.


## Code Example

If Capsule is run with no command line arguments with the command, it crawls the
current app (assuming that the current view is the starting view of the app.)

Although the device name is optional with no command line arguments, you must
specify the device name if you pass in any arguments.

```$ python capsule.py emulator-5554```

If a text file is passed in as a command line argument (with -f or --file),
Capsule attempts to crawl each app listed in the file (one app per line),
assuming that all of the packages are already installed on the device.

```$ python capsule.py emulator-5554 -f /[PATH TO FILE]/list.txt```

If a directory is passed in as an argument (with -d or --dir), Capsule installs,
crawls, and uninstalls each of the apps in the directory in alphabetical order.

```$ python capsule.py -d /[PATH TO APKS]/```

### Additional Flags
-r or --recrawl: Recrawl an already crawled app. If this is not specified and a
directory already exists at /data/[APP NAME], it will skip crawling the app.

## Motivation

Capsule serves many purposes, included automated testing and acquiring large
samples of Android UIs with very little effort. Developers can use it to test
the behavior of all of their clickable components and can find bugs, crashes,
and possible user traces without having to rely on a testing framework or change
the tests as the app’s UIs change. Although exhaustively searching each app will
not replicate typical user behavior, it can help gain a large corpus of data and
find bugs or unintended app behaviors.

## Installation

### Requirements

* OS
 - Linux
 - Mac OS X
* Tools
 - [ADB](http://developer.android.com/tools/help/adb.html): must be
installed and accessible from `PATH`. ADB comes with the Android SDK.

To use Capsule, you must have AndroidViewClient installed.

### AndroidViewClient
If you are just cloning this repo, you can either directly install
AndroidViewClient by reading the instructions on
[dtmilano’s wiki](https://github.com/dtmilano/AndroidViewClient/wiki#using-easy_install)
or by:

``$ sudo apt-get install python-setuptools # not needed on Ubuntu``

``$ sudo easy_install --upgrade androidviewclient``

However, if you have
[installed Vanadium](https://vanadium.github.io/installation/), you can import
the luma and luma.third\_party repositories by adding the luma manifest to your
.jiri_manifest file or adding it to your manifest from the command line:

``$ jiri import -name=manifest luma https://vanadium.googlesource.com/manifest
&& jiri update``

After this, you will need to add the AndroidViewClient to your PYTHONPATH:

``$ export PYTHONPATH=$PYTHONPATH:$JIRI_ROOT/release/projects/luma_third_party/AndroidViewClient/``

### Facebook/Google Login

Capsule automates Facebook and Google logins to allow the crawler to get through
login procedures. Capsule prioritizes clicking on Facebook or Google login
buttons, so installing/logging into Facebook and logging into a Google account
allows the program to get beyond login screens.

### Config and Typing in Text Fields

Capsule can automatically fill in text fields according to their Android
resource ids. Prepopulated fields with logical rules (such as differentiating
between first and last name and using the zipcode config for views named
'id/zip' are already located in the [config file](config.ini), but it will also
check for any fields that the user adds to the config in the extra info section.

If it does not match any of the fields in the config, it will type the text in
the default field, so delete that field if you do not want text entry.

We recommend personalizing the config, especially since the default email
address (foo@bar.com) has already been used (or blocked) to create accounts for
most applications.

To modify the file without Git tracking it, type

``$ git update-index --assume-unchanged config.ini``

## Contributors

We are happy to accept contributions. However, Vanadium does not accept pull
requests, so you must follow the
[Vanadium contributing](https://vanadium.github.io/community/contributing.html)
instructions.
In addition, feel free to file issues on the
[luma issue tracker](https://github.com/vanadium/luma/issues) or contact the
Capsule engineering lead, [Dan Afergan](afergan@google.com).

## License
This is not an official Google product.

Capsule is governed by BSD-style license found in the
[luma LICENSE file](https://github.com/vanadium/luma/blob/master/LICENSE).


Capsule lives in the Vanadium codebase, but is no longer affiliated with
Vanadium.
