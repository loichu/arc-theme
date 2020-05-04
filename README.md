# Arc Theme xfwm4 HiDPI

This is a fork of [arc-theme](https://github.com/horst3180/arc-theme). I just regenerated the assets I needed by modifying the scripts. [Install from the archive](https://github.com/loichu/arc-theme-xfwm4-hidpi#archive) to have it **in 192 DPI**  or [build it yourself](https://github.com/loichu/arc-theme-xfwm4-hidpi#how-to-reproduce) to set **any DPI value**.

## Arc is available in three variants 

##### Arc

![A screenshot of the Arc theme](http://i.imgur.com/Ph5ObOa.png)

##### Arc-Darker

![A screenshot of the Arc-Darker theme](http://i.imgur.com/NC6dqyl.png)

##### Arc-Dark

![A screenshot of the Arc-Dark theme](http://i.imgur.com/5AGlCnA.png)

## Installation

### Archive

The easiest method to install the hidpi theme in

* [Download](https://sourceforge.net/projects/arc-xfwm4-hidpi/files/arc-theme-xfwm4-hidpi.tar.gz/download) the archive.
* Extract it to `/usr/share/themes`.
* Choose between Light, Darker or Dark in `Settings -> Windows Manager -> Style`.
* Also apply the theme in `Settings -> Appearance -> Style`.

---

### How to reproduce

I did it on Arch Linux and I followed the [manual installation](https://github.com/horst3180/arc-theme#manual-installation).

Requirements on Arch Linux:
* autoconf
* automake
* pkgconf
* gtk3
* git
* inkscape
* [official arc theme](https://github.com/horst3180/arc-theme) (cloned in a local directory)

> If you don't want to rebuild the assets (which allows you to set any DPI value you want), clone this repository and start from [step 5](https://github.com/loichu/arc-theme-xfwm4-hidpi#5-generate-theme-if-you-cloned-my-repo-start-here).

#### 1 Delete assets

We will delete the assets that we want to regenerate.

```
rm -f common/xfwm4/assets{,-dark}/*.png
rm -f common/gtk-2.0/assets{,-dark}/*.png
rm -f common/gtk-3.0/3.22/assets/*.png
```

#### 2 Add DPI option in scripts

The three scripts we want to edit and run:
* `common/gtk-3.0/3.22/render-assets.sh`
* `common/gtk-2.0/render-assets.sh`
* `common/xfwm4/render-assets.sh`

We need to add `--export-dpi=192` on inkscape commands. You can change 192 by any value you want. The scripts for `gtk-2.0` and `xfwm4` are similar: you need to add this option twice per script. In `gtk-3.0` it's a little bit different, they already generate a HiDPI version suffixed by "@2" but I guess that this only works on GNOME. Thus I just added the `--export-dpi=192` on the first one.

#### 3 Change the scripts to work in Inkscape version 1

The scripts have been made to work well with inkscape 0.9, the option `--export-png` has been replaced by `--export-filename`. Here's a command to apply this change in all scripts:

```
find ./ -type f -exec sed -i 's/--export-file/--export-filename/g' {} \;
```

#### 4 Generate the assets using the scripts

Simply run this command:

```
for d in common/{xfwm4,gtk-2.0,gtk-3.0/3.22}; do ( cd "$d" && ./render-assets.sh );done
```

Then your local folder should be the same than this repo.

#### 5 Generate theme (if you cloned my repo start here)

In my case, I only rebuilded the assets for `gtk2`, `gtk3` and `xfwm4` so **I exclude the other desktop engines**. Also, I use GTK 3.24 which is not supported yet by arc-theme so **I force the GTK version to 3.22**.

```
./autogen.sh --prefix=/usr --disable-cinnamon --disable-gnome-shell --disable-metacity --disable-unity --with-gnome=3.22
```

#### 6 Make install

Finally write the theme to the system:
```
sudo make install
```

#### 7 Select the theme in XFCE5
* Choose between Light, Darker or Dark in `Settings -> Windows Manager -> Style`.
* Also apply the theme in `Settings -> Appearance -> Style`.

## Uninstall

Run

    sudo make uninstall

from the cloned git repository, or

    sudo rm -rf /usr/share/themes/{Arc,Arc-Darker,Arc-Dark}

## Extras

### Arc KDE
A port of Arc for the Plasma 5 desktop with a few additions and extras. Available [here](https://github.com/PapirusDevelopmentTeam/arc-kde).

### Arc Firefox theme
A theme for Firefox is available at https://github.com/horst3180/arc-firefox-theme

### Arc icon theme
The Arc icon theme is available at https://github.com/horst3180/arc-icon-theme

### Chrome/Chromium theme
To install the Chrome/Chromium theme go to the `extra/Chrome` folder and drag and drop the arc-theme.crx or arc-dark-theme.crx file into the Chrome/Chromium window. The source of the Chrome themes is located in the source "Chrome/arc-theme" folder.

### Plank theme
To install the Plank theme, copy the `extra/Arc-Plank` folder to `~/.local/share/plank/themes` or to `/usr/share/plank/themes` for system-wide use.
Now open the Plank preferences window by executing `plank --preferences` from a terminal and select `Arc-Plank` as the theme.

### Arc-Dark for Ubuntu Software Center
The Arc Dark theme for the Ubuntu Software Center by [mervick](https://github.com/mervick) can be installed from [here](https://github.com/mervick/arc-dark-software-center). It solves readability issues with Arc Dark and the Ubuntu Software Center.

## Troubleshooting

If you use Ubuntu with a newer GTK/GNOME version than the one included by default (i.e Ubuntu 14.04 with GTK 3.14 or Ubuntu 15.04 with GTK 3.16, etc.) the prebuilt packages won't work properly and the theme has to be installed manually as described above.
This is also true for other distros with a different GTK/GNOME version than the one included by default

--

If you get artifacts like black or invisible backgrounds under Unity, disable overlay scrollbars with

    gsettings set com.canonical.desktop.interface scrollbar-mode normal


## Bugs
If you find a bug, please report it at https://github.com/horst3180/arc-theme/issues

## License
Arc is available under the terms of the GPL-3.0. See `COPYING` for details.

## Full Preview
![A full screenshot of the Arc theme](http://i.imgur.com/tD1OBQ3.png)
<sub>Screenshot Details: Icons: [Arc](https://github.com/horst3180/arc-icon-theme) | Launcher Icons based on [White Pixel Icons](http://darkdawg.deviantart.com/art/White-Pixel-Icons-252310560) | [Wallpaper](https://pixabay.com/photo-869593/) | Font: Futura Bk bt</sub>

[obs-repo]: http://software.opensuse.org/download.html?project=home%3AHorst3180&package=arc-theme
[sk-overlay]: https://c.darenet.org/scriptkitties/overlay
