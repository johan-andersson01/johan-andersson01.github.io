---
layout: post
title:  'Using a custom keyboard layout in Ubuntu'
---

I haven't found any ONE single resource that describes how to do this, so here I am trying to contribute just that. It took me a ridiculously long time to get this working. For reference, this was all done in Ubuntu 20.04 with GNOME.

The focus of this post is mostly showing HOW to get this working. For more details, you can read [this](https://help.ubuntu.com/community/Custom%20keyboard%20layout%20definitions).

## Adding a layout

Layouts are stored in `/usr/share/X11/xkb/symbols`. Personally, I like the US layout. But I also want to be able to type the Swedish characters å,ä, and ö. By adding the following snippet to the bottom of `/usr/shareX11/xkb/symbols/us`, I get just that.

```
default partial alphanumeric_keys
xkb_symbols "se" {
    name[Group1]="English (US, with åäö)";

    include "us(basic)"

    //           default               shift            altgr            shift altgr
    key <AD11> {[bracketleft,          braceleft,       aring,           Aring ]};
    key <AC10> {[semicolon,            colon,           odiaeresis,      Odiaeresis ]};
    key <AC11> {[apostrophe,           quotedbl,        adiaeresis,      Adiaeresis ]};

    include "level3(ralt_switch)"
};
```

This configuration uses the default US layout and then assigns `å ä ö Å Ä Ö` to the AltGr layer of `[ ' ; { " :`.

It's important to note that this snippet is added to `/usr/share/X11/xkb/symbols/us` and that the name of th field is `se`. This is how keyboard layouts are looked up: `language+layout` (in this case `us+se`)

## Using the layout

Keyboard layouts are stored in `/usr/share/X11/xkb/rules/evdex.xml`.

```
<layout>
  <configItem>
    <name>us</name>
    <!-- Keyboard indicator for English layouts -->
    <shortDescription>en</shortDescription>
    <description>English (US)</description>
    <languageList>
      <iso639Id>eng</iso639Id>
    </languageList>
  </configItem>
  <variantList>
    <variant>
      <configItem>
        <name>se</name>
        <description>Swedish (US keyboard with Swedish letters)</description>
      </configItem>
    </variant>
    <variant>
      ...
    </variant>
  </variantList>
</layout>
```


So, for each language we have a `configItem` entry, and for each keyboard layout of that language we have a child `variant` entry. The `description tag is what you'll see in the GUI settings.

### Applying the change

#### Terminal

```
gsettings set org.gnome.desktop.input-sources source [('xkb'), 'us+se')]"
```

#### GUI

1. Delete the cache with `sudo dpkg-reconfigure xkb-data`
2. Go to Settings > Region & Language > Add
3. Find your new entry (you'll see it has the same description as the one you set in `evdev.xml`)

## Final notes

If you mess up the layout configuration with syntax errors, you're going to have a bad time trying to type stuff. Make sure to have a tty open with tmux so you easily can revert any changes (these changes don't apply there).
