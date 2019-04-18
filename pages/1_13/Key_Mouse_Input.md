# Key & Mouse Input

Forge uses a library called GLFW for its input events. For more in-depth information on the GLFW library, please visit their documentation website: https://www.glfw.org/documentation.html  

With update .146 that forge released two days ago, it is super easy to register key and mouse input within your mod. To keep your files nice and organized I highly suggest that you make a seperate class for registering input. Name it something like InputEventHandler.

### Class Setup

Now that we've created our input class we have to annotate it with `@Mod.EventBusSubscriber`. This tells forge that this class wants to subscribe to the MinecraftForge EVENT_BUS when it's constructed. In order to use said annotation we have to import the following interface: `net.minecraftforge.fml.common.Mod`. Your class should now look a little something like this.

```java
package exampleinput

import net.minecraftforge.fml.common.Mod

@Mod.EventBusSubscriber
public class InputEventHandler {

}
```

### Mouse Input

For our mouse input we need to import the following three classes:

`import net.minecraftforge.client.event.InputEvent`  
`import net.minecraftforge.eventbus.api.SubscribeEvent`  
`import static org.lwjgl.glfw.GLFW.*`

We're going to make a public static method with a return type of void. Just as our class itself, we have to give this method an annotation: `@SubscribeEvent`. This tells forge that this method wants to subscribe to a certain event.   

In order to let forge know which event we want to subscribe to, we have to give the method a parameter. For mouse input we use the following parameter: `InputEvent.MouseInputEvent event`. Your class should now look a little something like this:

```java
package exampleinput;

import net.minecraftforge.client.event.InputEvent;
import net.minecraftforge.eventbus.api.SubscribeEvent;
import net.minecraftforge.fml.common.Mod;

import static org.lwjgl.glfw.GLFW.*;

@Mod.EventBusSubscriber
public class InputEventHandler {

    @SubscribeEvent
    public static void onMouseClick(InputEvent.MouseInputEvent event){
        
    }
}
```

Now this already works! if you put a System.out.println in this method it will print out the message in the console on every mouse click. Ofcourse we want to be able to differentiate between different mouse buttons and whether the mouse was pressed or released. That's what we're going to look at now.

Since the forge update, the event parameter that we passed into our method now contains a bunch of extra features. The two most important ones are `event.getButton()` and `event.getAction()`.
