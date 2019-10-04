# Basic Items

At this point you should be able start with creating some basic items.

Let's start with a `ModItems` class.

```java
package com.example.examplemod.items;

import com.example.examplemod.Reference;
import net.minecraft.item.Item;
import net.minecraft.item.Item.Properties;
import net.minecraftforge.event.RegistryEvent;
import net.minecraftforge.eventbus.api.SubscribeEvent;
import net.minecraftforge.fml.common.Mod;
import net.minecraftforge.registries.IForgeRegistry;
import net.minecraftforge.registries.ObjectHolder;

@Mod.EventBusSubscriber(bus=Mod.EventBusSubscriber.Bus.MOD)
@ObjectHolder(Reference.MODID)
public class ModItems {

    @ObjectHolder("gem")
    public static final Item GEM = null;

    @ObjectHolder("example")
    public static final Item EXAMPLE = null;

    @SubscribeEvent
    public static void onRegisterItems(RegistryEvent.Register<Item> e) {
        IForgeRegistry<Item> reg = e.getRegistry();

        reg.registerAll(
                new Item(new Properties()).setRegistryName("gem"),
                new Item(new Properties()).setRegistryName("example")
        );
    }

}
```

Lets go over this class now.

## `@OjectHolder`, `@Mod.EventBusSubscriber` and `@SubscribeEvent`

The object holder annotation tells forge to inject an instance
of your item into the field it is placed on.
If used on a class it tells forge that any later
`@ObjectHolder`s are from this mod.

The `@Mod.EventBusSubscriber` annotation tells forge that this class
wants to handle events. Because `RegistryEvent`s are fired on the `Bus.MOD` Bus and the annotation by default subscribes to the `Bus.FORGE` bus we have to specify it.

`@SubscribeEvent` points forge at the correct functions to call
for every event type specified by the function's arguments.

## onRegisterItems
In this function we instantiate and register our items.
Not in every case, but pretty often you will want
a custom class for your item. In this case
i just used the vanilla `Item` class

The `Properties` obejct contains some information about
the item like which `ItemGroup` (Crative Tab) it belongs to or
how many of them can be in a stack. Those values
can be set using the methods of the `Properties` object
(e.g. `new Properties().maxStackSize(16)`)

the `setRegistryName` method specifes the internal name (id) your items
will use.

### Testing in-game

If you launch your game now (using the `runClient` task in your IDE), 
you should be able to give yourself your items using 
the `/give` command (`/give @p modid:registry_name`).
When you do that you will surely notice that your item is a purple
blocky mess. To fix this you have to add a model and a texture.

## Basic Item Model
The most basic item model you can have is this:

```json
{
    "parent":"item/generated",
    "textures": {
        "layer0":"modid:item/item_texture_name_without_extension"
    }
}
```

All this does is "extend" the default item generated model
using the `parent` option and specify a texture.

You will want to place a JSON file named the same way
as your item (e.g. if your registry name is `gem` then your file will be `gem.json`)
in the `src/main/resources/assets/modid/models/item/` directory.
Then you will want to place a texture file in any directory, with any name
under the `src/main/resources/assets/modid/textures` directory.
You just have to make sure the path in your model file matches.
(e.g. if you place your file in `textures/whatever/xd.png`, then your model
must point to `modid:whatever/xd`, but in theese tutorials i will use the
typical convention of placing item textures under `textures/item`).
Now you can go back to the game and press `F3 + T` to reload assets
without having to close it.

#### !! NOTE !!
In Intellij idea you might need to use the `Run -> Reload Change Classes` option before `F3 + T` succesfuly works, which may only be possible in Debug Mode

## Advanced models
TODO
