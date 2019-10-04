# Basic Code Structure

In this article I will be explaining the basic structure
of your mod's code.


## Main Class

This is probably the most important class in your mod.
While it often doesn't contain much code it is the main
entry point of your mod.

```java
package com.example.examplemod;

import net.minecraftforge.eventbus.api.IEventBus;
import net.minecraftforge.fml.common.Mod;
import net.minecraftforge.fml.event.lifecycle.FMLClientSetupEvent;
import net.minecraftforge.fml.event.lifecycle.FMLCommonSetupEvent;
import net.minecraftforge.fml.javafmlmod.FMLJavaModLoadingContext;
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

import static com.example.examplemod.ExampleMod.MODID;

@Mod(MODID)
public class ExampleMod {

    public static final Logger LOGGER = LogManager.getLogger();

    public static final String MODID = "examplemod";

    public ExampleMod() {
        IEventBus bus = FMLJavaModLoadingContext.get().getModEventBus();

        bus.addListener(this::setup);
        bus.addListener(this::clientSetup);
    }

    private void setup(final FMLCommonSetupEvent e) {
    }

    private void clientSetup(final FMLClientSetupEvent e) {
    }

}
```

As you can see for the basic set up you don't need much.

### @Mod
Let's start with the `@Mod` annotation. All you need
in here is your modid.

It lets forge know that this class is the entry point of your mod.

### LOGGER
The logger is used to log information useful for debugging, resolving conflicts with other mods or just additional info for when a user
reports an issue.

### Constructor
This is what forge uses to get an instance of your main class.
In here you can register listeners for various lifecycle events
of your mod like `FMLCommonSetupEvent`, `FMLClientSetupEvent`
and so on.

### FMLCommonSetupEvent
This event (and thus the `#setup` function) can be used to perform any
setup required on both sides. Most of the time it will be empty
because you can just use other more specific events for most things.

### FMLClientSetupEvent
Same as `FMLCommonSetupEvent` but fires only on the client side.
