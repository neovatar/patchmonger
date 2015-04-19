### Patchmonger

This tool was developed for patching [GOG](http://gog.com) distributed games on Linux. For those games GOG only offers updated installers, forcing you to download many gigabytes every time a patch is releases (e.g. for [Pillars of Eternity](http://www.gog.com/game/pillars_of_eternity_hero_edition)).

Patchmonger can be used to create a binary diff of the files packages in two different installers, thus giving you a much smaller patchset.

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
gog_pillars_of_eternity_1.0.0.1.tar.gz
gog_pillars_of_eternity_1.1.0.2.tar.gz
gog_pillars_of_eternity_1.2.0.3.tar.gz
````
