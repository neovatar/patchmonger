### Patchmonger

This tool was developed for patching [GOG](http://gog.com) distributed games on Linux. For those games GOG only offers updated installers, forcing you to download many gigabytes every time a patch is releases (e.g. for [Pillars of Eternity](http://www.gog.com/game/pillars_of_eternity_hero_edition)).

Patchmonger can be used to create a binary diff of the files packages in two different installers, thus giving you a much smaller patchset.

### Prerequisites

You need the [xdelta3](https://github.com/jmacd/xdelta) tool with LZMA support for creating compressed binary diffs. This tool should be available in your Linux distributions repository. On Ubuntu you can install it with apt:

````
sudo apt-get install xdelta3
````
If you are building it yourself, make sure to include LZMA support (e.g. install LZMA lib and headers before running configure).

### Features

Patchmonger can diff two directory structures and will check for the following:

  * New files added
  * Files removed
  * Files changed

For changed files, patchmonger will create a xdelta3 based binary diff and compress it with LZMA. It will include a copy of itself in the patchset, which can be run to apply the patch.

### Examples

#### Creating a patch

Download the two GOG game installer tgz files (e.g. forPillars of Eternity):

````
ls -al ~/Downloads/poe/
insgesamt 20688768
drwxrwxr-x  2 tom tom       4096 Apr 17 21:55 .
drwxrwxr-x 64 tom tom      20480 Apr 18 14:44 ..
-rw-rw-r--  1 tom tom 7056555249 Mär 26 18:55 gog_pillars_of_eternity_1.0.0.1.tar.gz
-rw-rw-r--  1 tom tom 7064318981 Apr  8 09:28 gog_pillars_of_eternity_1.1.0.2.tar.gz
````

Clone patchmonger:

````
git clone https://github.com/neovatar/patchmonger.git
````

Change into patchmonger directory and create a temporary directory:

````
cd patchmonger
mkdir tmp
````

Extract your installers into subdirectories of the temporary directory:

````
mkdir tmp/gog-poe-1.0.0.1
tar xfz ~/Downloads/poe/gog_pillars_of_eternity_1.0.0.1.tar.gz \
    -C ./tmp/gog-poe-1.0.0.1 --strip-components=1
mkdir tmp/gog-poe-1.1.0.2
tar xfz ~/Downloads/poe/gog_pillars_of_eternity_1.1.0.2.tar.gz \
    -C ./tmp/gog-poe-1.1.0.2 --strip-components=1
````

Create a patchset:

````
./patchmonger create-patch ./tmp/gog-poe-1.0.0.1 \
                           ./tmp/gog-poe-1.1.0.2 \
                           ./tmp/gog_poe-patch-1.0.0.1_1.1.0.2
````

Patchmonger will create a patch, this will take a while. When the process is finished, you will find a distributable tgz file in your patch folder:

````
ls -al tmp/gog_poe-patch-1.0.0.1_1.1.0.2/
insgesamt 80336
drwxrwxr-x 2 tom tom     4096 Apr 19 18:22 .
drwxrwxr-x 6 tom tom     4096 Apr 19 18:22 ..
-rw-rw-r-- 1 tom tom 82254392 Apr 19 18:22 gog_poe-patch-1.0.0.1_1.1.0.2.tgz
````

#### Applying a patch

You need to extract the patch tgz first:

````
cd ./tmp/gog_poe-patch-1.0.0.1_1.1.0.2
tar xvfz gog_poe-patch-1.0.0.1_1.1.0.2.tgz
````

The patch contains the patchset data and patchmonger:

````
ls -al gog_poe-patch-1.0.0.1_1.1.0.2
insgesamt 20
drwxrwxr-x 3 tom tom 4096 Apr 19 18:22 .
drwxrwxr-x 3 tom tom 4096 Apr 19 18:27 ..
-rwxrwxr-x 1 tom tom 7770 Apr 19 17:36 patchmonger
drwxrwxr-x 3 tom tom 4096 Apr 19 18:12 pmdata

````

Change into the extracted patch directory and start the patch process:

````
cd gog_poe-patch-1.0.0.1_1.1.0.2
./patchmonger
````

Patchmonger will ask for the Pillars of Eternity installation directory and start the patch process.

You can also specify the installation directory of the game that you want to patch directly:

````
./patchmonger "/home/tom/Data/games/Pillars of Eternity"
````

#### Special command line arguments

**`-f`** Turns some errors to warnings when applying a patch, use with caution.

**`-v`** Gives more verbose output.
