# Electron Cash Android app

To start developing the app, just open this directory in Android Studio.


## Requirements

You'll need to set up the following things before building the app:

* Python 3.8 must be on the PATH under the name `python3.8` or `python3` on Linux/Mac, or `py`
  on Windows, and it must have the packages listed in `build-requirements.txt`.
* The commands `xgettext` and `msgfmt` must be on the PATH. On Windows, the easiest way to
  get these is to install MSYS2.


## Strings

Most user interface strings are reused from the desktop and iOS apps. Android-specific strings
should be added to `app/src/main/python/electroncash_gui/android/strings.py`.

The Gradle task `generateStrings` takes all localized strings in the repository, and their
translations from Crowdin, and converts them into `strings.xml` format so they can be accessed
through the Android resource API. The string IDs are generated from the first 2 words of each
string, plus as many more words as necessary to make them unique. So if any of the source
strings change, you may need to update ID references in the code.

The `generateStrings` task is run automatically the first time you build the app, and whenever
you edit the `strings.py` file mentioned above. If you need to pick up new strings from
anywhere else in the repository, run the task `regenerateStrings`.

Changes on Crowdin won't be picked up until a Crowdin project manager has run the "build"
command (green button on the project home page). If you're not a project manager but you want
to test a new translation, or fix an invalid translation which is blocking the build, you can
do this:

* To prevent your changes being overwritten, temporarily edit the `generateStrings` block in
  app/build.gradle to add the line `args "--no-download"`. Do not commit this change!
* Edit the string in the .po file under electroncash/locale.
* Run the task `regenerateStrings`, then build and test the app.
* Once you're happy with the result, submit the string on Crowdin.
* Ask a project manager to approve the string and run the "build" command.


## Release

For public releases, the following reproducible build process should be run on Linux x86-64:

If necessary, install Docker using the [instructions on its
website](https://docs.docker.com/install/#supported-platforms).

Copy your release key to `keystore.jks` in this directory. It must contain a key with the
following configuration:

    keyAlias "key0"
    keyPassword "android"
    storePassword "android"

Run `build.sh`. The APK will be generated in `release` in this directory.

Between builds it may be helpful to free up disk space with the command `docker system prune`.
