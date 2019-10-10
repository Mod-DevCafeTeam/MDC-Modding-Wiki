# Basic Asset Structure

In this article I will be explaining the basic structure
of your mod's assets and resources.

## Directory structure

The basic directory structure of your `src/main/resources` folder should
look as follows:

```
- META-INF
    - mods.toml
- pack.mcmeta
- assets
    - examplemod <-- this should be your modid
        - lang
            - en_us.json
        - models
            - item
            - block
        - textures
            - item
            - block
        - blockstates
- data
    - examplemod <-- this should be your modid
        - recipes
```

## Assets
The assets directory is where all client resources are stored
things like: textures, models, translations, etc.

### Lang
This directory contains translation files. You need
a default `en_us.json` file because that's what the 
game will fall back to if it doesn't detect translations for other
languages. (e.g if your mod doesn't have Polish translations, then a player playing in that language will see English (US) names for items from your mod).

There will be a separate article explaining translation in detail.

### Models
This directory contains json models (shapes) for your blocks and items.
A detailed explanation will be included in the respective articles.

### Textures
This directory contains textures for your blocks, items, entities, and
other things.
A detailed explanation will be included in the respective articles.

### Blockstates
This directory contains blockstate files. These allow your blocks
to change texture, shape, rotation and so on based on in-game conditions.

A more detailed explanation will be included in the article about blocks.

## Data
This directory contains resources required on both the server and client
like recipes.

### Recipes
This directory contains json recipes.
A detailed explanation will be included in the article about them.

## mods.toml
`mods.toml` is an important file in your resources directory.
It tells forge that this project contains mods to be loaded.

You can find some info about the TOML format here: https://github.com/toml-lang/toml

Let's go over it's contents now.

```toml
modLoader="javafml" #mandatory
loaderVersion="[28,)" #mandatory (28 is current forge version)
#issueTrackerURL="http://my.issue.tracker/" #optional

[[mods]] #mandatory
modId="examplemod" #mandatory
version="${file.jarVersion}" #mandatory
displayName="Example Mod" #mandatory

#updateJSONURL="http://myurl.me/" #optional
#displayURL="http://example.com/" #optional
#logoFile="examplemod.png" #optional

#credits="Thanks for this example mod goes to Java" #optional
authors="Love, Cheese and small house plants" #optional
description='''
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed mollis lacinia magna. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Sed sagittis luctus odio eu tempus. Interdum et malesuada fames ac ante ipsum primis in faucibus. Pellentesque volutpat ligula eget lacus auctor sagittis. In hac habitasse platea dictumst. Nunc gravida elit vitae sem vehicula efficitur. Donec mattis ipsum et arcu lobortis, eleifend sagittis sem rutrum. Cras pharetra quam eget posuere fermentum. Sed id tincidunt justo. Lorem ipsum dolor sit amet, consectetur adipiscing elit.
'''

# A dependency - use the . to indicate dependency for a specific modid. Dependencies are optional.
[[dependencies.examplemod]] #optional
    modId="forge" #mandatory
    mandatory=true #mandatory
    versionRange="[28,)" #mandatory
    ordering="NONE" #mandatory if the propery mandatory is false. Can be NONE, BEFORE or AFTER
    side="BOTH"

[[dependencies.examplemod]]
    modId="minecraft"
    mandatory=true
    versionRange="[1.14.4]"
    ordering="NONE"
    side="BOTH"

```

### Header
The first few lines of the file speciy general project info.

`modLoader`: The name of the mod loader type to load - for regular FML @Mod mods it should be javafml

`loaderVersion`: A version range to match for said mod loader - for regular FML @Mod it will be the forge version

`issueTrackerUrl`: A URL to refer people to when problems occur with this mod

### Mod
In here you specify things related to your own mod

`modId`: The modid of your mod. generally this will be a shortened version of your name.

`version`: The version of your mod. You can find info on the scheme [here](https://semver.org/)

`displayName`: The long name of your mod.

`credits`: Credits

`authors`: Authors, e.g. your name

`description`: Long description of the mod.

### Dependencies
In here you can specify mods your mod depends on.
E.g. if you wanted to add a new Babule
you would have to depend on Baubles.

```toml
[[dependencies.examplemod]] #optional
    modId="forge" #mandatory
    mandatory=true #mandatory
    versionRange="[28,)" #mandatory
    ordering="NONE" #mandatory if the propery mandatory is false. Can be NONE, BEFORE or AFTER
    side="BOTH"
```

This is a single dependency entry

The first line (`[[dependencies.yourmodidhere]]`) specifies
that this a dependency of your mod.

`modId` is the other mods modid

`mandatory` specifies wheter this dependency is required
for your mod to function.

`versionRange` the version range of that dependency. 
`[X,)` indicates that you need at least this version, but it can be higher,
`[X,Y]` indicates that only versions X and Y of the dependency will work.

You can specify an exact version by setting it to `X`

That's it :)