# New Project

Setting up a new modding environment is easy to do. You can find the
[Forge documentation here](https://mcforge.readthedocs.io/en/latest/), and specifically the page for
[Getting Started with Forge here](https://mcforge.readthedocs.io/en/latest/gettingstarted/)
(which you'll find is rather similar to this page).

### Getting the MDK

First of all, you'll need to get yourself a Forge MDK from the official Forge site:

http://files.minecraftforge.net/

Ideally, you'll want to get the latest stable version for the Minecraft version you want to create a mod in. These
tutorials will all be using Minecraft 1.12.2 and the recommended version at time of writing is 1.12.2 - 14.23.5.2768.

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

The `group` is your base package name. Normally this will be basically the package structure of your mod between the
"java" package and your main location where your classes are located. Taking the example mod as an example here, the
structure is `/src/main/java/com/example/examplemod`. In this case, I'd use `com.example.examplemod` as the `group`.
However when you rename the example mod, you'd use a name for the author (probably your own alias or team name) and the
mod ID. For example, for the mod [Sparks Hammers](https://github.com/thebrightspark/SparksHammers) you will find the
structure is `/src/main/java/brightspark/sparkshammers` so the `group` is set to `brightspark.sparkshammers`.

The `archivesBaseName` will be used for the built JAR file name (along with the version), and ideally would be your mod
ID.

Further down the file we can find the following section:
```groovy
minecraft {
    version = "1.12.2-14.23.5.2768"
    runDir = "run"
    
    // the mappings can be changed at any time, and must be in the following format.
    // snapshot_YYYYMMDD   snapshot are built nightly.
    // stable_#            stables are built at the discretion of the MCP team.
    // Use non-default mappings at your own risk. they may not always work.
    // simply re-run your setup task after changing the mappings to update your workspace.
    mappings = "snapshot_20171003"
    // makeObfSourceJar = false // an Srg named sources jar is made by default. uncomment this to disable.
}
```
This part defines a few fields used when building your development environment (which we'll get to soon). The first one
you can see there is the `version`. This is the Forge version we're using. If you ever want to update the version of
Forge within your mod, simply update this value.

The next is the `runDir`, which is just the directory where Minecraft will be running from when you click "Run" in your
IDE.

The `mappings` are an important one - these are the MCP mappings version. The mappings are used to convert the hard to
read decompiled class, method and field names in code into much more readable names. Whenever you create a new project
(like this right now!) or update Forge, you should really make sure this is updated to the latest mappings version.
You can get the latest version over at the [MCP Bot exports site](http://export.mcpbot.bspk.rs/). You should always use
the latest stable release for your Minecraft version where possible, otherwise use the latest snapshot. To update you
simply just need to enter the version number into the value as explained in the comments above it.

The last field here which is commented out is `makeObfSourceJar`. This isn't really used anymore, but would not be
commented out in previous Minecraft versions. If this is set to true, then build your mod would also generate a sources
JAR. This isn't needed much anymore as Forge will automatically decompile mod dependencies.

### Using the gradle.properties

To make life easier, you don't have to keep searching through the build.gradle for a few values to change any time you
want to update a version. You can make use of your `gradle.properties` file to collect variables in. You can check out
an example of that in use here in Sparks Hammers:
* [build.gradle](https://github.com/thebrightspark/SparksHammers/blob/1.12/build.gradle)
* [gradle.properties](https://github.com/thebrightspark/SparksHammers/blob/1.12/gradle.properties)

### Setting up your workspace

Once you have everything extracted out of the MDK and configured for your project, you can now build your workspace.
The Gradle that comes in the MDK that you extracted is actually a custom modified version called ForgeGradle. It has
some custom commands so you can setup and handle your workspace very easily. To setup your workspace, you simple need
to run two commands. So open the Command Prompt in your project directory and run `gradlew setupDecompWorkspace`.
That will setup the main workspace and will be ready for development. However it isn't quite ready to be used directly
in an IDE. For that, you need to run one of two commands: if you use IntelliJ Idea, run `gradlew idea`; if you use
Eclipse, run `gradlew eclipse`. You can then either open up the project from within the IDE or open the project file
generated directly.

To summarise, you can run either of these commands to setup your workspace depending on your IDE:
* `gradlew setupDecompWorkspace idea`
* `gradlew setupDecompWorkspace eclipse`

### Building your mod

Once you're ready to build your mod, you can simply run `gradlew build`.

This will generate your mod JAR in the `/build/libs` directory/
