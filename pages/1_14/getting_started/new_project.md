# Getting Started

Setting up a new modding environment is easy to do. You can find the
[Forge documentation here](https://mcforge.readthedocs.io/en/latest/), and specifically the page for
[Getting Started with Forge here](https://mcforge.readthedocs.io/en/latest/gettingstarted/)
(which you'll find is rather similar to this page).

### Getting the MDK

First of all, you'll need to get yourself a Forge MDK from the official Forge site:

http://files.minecraftforge.net/

Ideally, you'll want to get the latest stable version for the Minecraft version you want to create a mod in. These
tutorials will all be using Minecraft 1.14.4 and the recommended version at time of writing is 1.14.4 - 28.1.0.

Now is the easiest time to prepare any source control for your project. If you would like to, create your repository
and clone it to wherever you'd like your project to live locally. If you don't want to use source control, then just
create a directory for your mod somewhere.

### Extracting the MDK

Next we need to extract out the necessary files and folders from the MDK. You **do not need all of the files** in the
MDK... the only ones you need are the following:
* `build.gradle`
* `gradle.properties`
* `gradlew.bat`
* `gradlew`
* the `gradle` folder
* `.gitignore` (this is only needed if you will use Git for source control)
The `src` folder contains an example mod with the correct directory structure and most necessary files. It's recommended
that you extract this folder too and just refactor the necessary files and folders to your own names once you know the
mod loads fine.

### Editing the build.gradle

Now let's just make sure we configure the `build.gradle` to your mod and get the latest mappings. In that file, you'll
see the following:
```groovy
version = "1.0"
group = "com.yourname.modid" // http://maven.apache.org/guides/mini/guide-naming-conventions.html
archivesBaseName = "modid"
```
These fields are used when you build your mod. The `version` is your mod's version number, so be sure to change this
every time you want to release a new version of your mod.

The `group` is your base package name. Typically you would use the domain name of some sort that you own, reversed
If you host your mod's source code on GitHub you can use `io.github.[your name].[name of your mod/mod's repository]`.
If you don't have either of those you can use `[your name].[your mod's name]`.

Later when you will be renaming the example mod make sure the directory structure is the same.
E.g. if your group is `io.github.author.coolmod` your directory structure will be
`src/main/java/io/github/author/coolmod` and it will be the place where you will put all your mod's code

The `archivesBaseName` will be used for the built JAR file name (along with the version), and ideally would be your mod
ID.

Further down the file we can find the following section:
```groovy
minecraft {
    // The mappings can be changed at any time, and must be in the following format.
    // snapshot_YYYYMMDD   Snapshot are built nightly.
    // stable_#            Stables are built at the discretion of the MCP team.
    // Use non-default mappings at your own risk. they may not always work.
    // Simply re-run your setup task after changing the mappings to update your workspace.
    mappings channel: 'snapshot', version: '20190719-1.14.3'
```
This part defines a few fields used when building your development environment (which we'll get to soon). 

The `mappings` are an important one - these are the MCP mappings version. The mappings are used to convert the hard to
read decompiled class, method and field names in code into much more readable names. Whenever you create a new project
(like this right now!) or update Forge, you should really make sure this is updated to the latest mappings version.
You can get the latest version over at the [MCP Bot exports site](http://export.mcpbot.bspk.rs/). You should always use
the latest stable release for your Minecraft version where possible, otherwise use the latest snapshot. To update you
simply just need to enter the version number into the value as explained in the comments above it.

The rest of the `minecraft` section contains information about the run tasks gradle will generate for your IDE. You 
generally don't need to mess with those.

### Dependencies

```groovy
dependencies {
    minecraft 'net.minecraftforge:forge:1.14.4-28.1.26'

    // Other stuff
}
```

In this section you will add any dependencies your project may have.
At the beggining the only thing that is added here is the forge version.

If you ever want to update the version of
Forge within your mod, simply update this value.

You may put jars on which you depend on in ./libs or you may define them like so:
```groovy
compile "some.group:artifact:version:classifier"
compile "some.group:artifact:version"
```

The 'provided' configuration is for optional dependencies that exist at compile-time but might not at runtime.
```groovy
provided 'com.mod-buildcraft:buildcraft:6.0.8:dev'
```

For more info:
http://www.gradle.org/docs/current/userguide/artifact_dependencies_tutorial.html

http://www.gradle.org/docs/current/userguide/dependency_management.html

### Jar manifest
This last section of the build.gradle allows you to add
values to your output jar's manifest. The default
`build.gradle` adds some values about the author and mod here.

```groovy
jar {
    manifest {
        attributes([
            "Specification-Title": project.name, // This may be different by default but you can change it to project.name
            "Specification-Vendor": "author",
            "Specification-Version": "1", // We are version 1 of ourselves
            "Implementation-Title": project.name,
            "Implementation-Version": "${version}",
            "Implementation-Vendor" :"author",
            "Implementation-Timestamp": new Date().format("yyyy-MM-dd'T'HH:mm:ssZ")
        ])
    }
}
```

As you can see the date, name, and version have been taken care of.
All you have to do is enter your name in the places of `author`

### Using the gradle.properties

To make life easier, you don't have to keep searching through the build.gradle for a few values to change any time you
want to update a version. You can make use of your `gradle.properties` file to collect variables in. You can check out
an example of that in use here in Sparks Hammers:
* [build.gradle](https://github.com/thebrightspark/SparksHammers/blob/1.12/build.gradle)
* [gradle.properties](https://github.com/thebrightspark/SparksHammers/blob/1.12/gradle.properties)

### Setting up your workspace

Once you have everything extracted out of the MDK and configured for your project, you can now build your workspace.

All you have to do is import the project into your IDE of choice
and generate the launch tasks for your IDE.

* In Intellij Idea you can do this using the `File -> Open...` menu option and selecting your project directory. To generate to tasks you
have to run `gradlew genIntellijRuns`

!! NOTE !!

You might need to manually set the module to `MODNAME.main` in your
launch configurations

* For Eclipse you have to run `gradlew genEclipseRuns`

### Building your mod

Once you're ready to build your mod, you can simply run `gradlew build`.

This will generate your mod JAR in the `/build/libs` directory.
