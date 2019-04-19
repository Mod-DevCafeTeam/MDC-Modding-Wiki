# Key & Mouse Input

Forge uses a library called GLFW for its input events. For more in-depth information on the GLFW library, please visit their documentation website: https://www.glfw.org/documentation.html  

With forge 1.13 it is super easy to register key and mouse input within your mod. To keep your files nice and organized I highly suggest that you make a seperate class for registering input. Name it something like InputEventHandler.

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

We're going to make a public static method with a return type of void. We have to give this method the annotation: `@SubscribeEvent`. This tells forge that this method wants to subscribe to a certain event.   

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

Now this already works! if you put a `System.out.println` in this method it will print out the message in the console on every mouse click. Of course we want to be able to differentiate between different mouse buttons and whether the mouse button was pressed or released. That's what we're going to look at now.

The event parameter that we passed into our method contains a bunch of features. The two most important ones are `event.getButton()` and `event.getAction()`. With the former you can check for specific mouse buttons like the left, right or middle mouse button. With the latter you can check if the mouse button was pressed or released, basically OnMouseButtonDown and OnMouseButtonUp.

GLFW has a lot of enums set up for you to use in conjunction with the above two methods. The most important ones for mouse input are:

`GLFW_MOUSE_BUTTON_LEFT`  
`GLFW_MOUSE_BUTTON_RIGHT`  
`GLFW_MOUSE_BUTTON_MIDDLE`

In the following code snippet I'll show you how you can use them with the event parameter to check which key was pressed:

```java
    @SubscribeEvent
    public static void onMouseClick(InputEvent.MouseInputEvent event){
        if (event.getButton() == GLFW_MOUSE_BUTTON_LEFT){
            //Do your stuff here
        }
    }
```
It's as simple as that! Whith that if statement we check which mouse button was pressed before we execute our desired code.  

Now if we also want to check whether the mouse button was pressed or released we can add an extra check in the if statement using `event.getAction()`. This function also returns an int and you guessed it, GLFW also has a couple of enums for this:

`GLFW_PRESS`  
`GLFW_RELEASE`  

We can just use them in the same if statement like this:

```java
    @SubscribeEvent
    public static void onMouseClick(InputEvent.MouseInputEvent event){
        if (event.getButton() == GLFW_MOUSE_BUTTON_LEFT && event.getAction() == GLFW_PRESS){
            //Do your stuff here
        }
    }
```
This method checks if the **left** mouse button was **pressed**.

That's it for mouse input! Let's continue to key input, which is very similar.

### Key Input

We already have all the imports we need from the previous section.  

For our key input we're going to make another public static method with return type void and annotate it with `@SubscribeEvent`. Instead of passing in `InputEvent.MouseInputEvent event`, we're now going to pass in `InputEvent.KeyInputEvent event` as our method parameter. Your method should look like this:

```java
    @SubscribeEvent
    public static void onKeyPress(InputEvent.KeyInputEvent event){

    }
```

Now the event parameter for key input uses `event.getKey()` instead of `event.getButton()`. The only thing that changes is the key enums. They are constructed like this: `GLFW_KEY_R`. For all the keys that are available you should take a look inside the GLFW class, they're all there. Below you will find an example of a method that checks if the **R** key was **released**:

```java
    @SubscribeEvent
    public static void onKeyRelease(InputEvent.KeyInputEvent event){
        if (event.getKey() == GLFW_KEY_R && event.getAction() == GLFW_RELEASE){
            //Do your stuff here
        }
    }
```

And that's basically all there's to it! You can make it as fancy as you want with multiple listeners, for example, `onKeyDown(), onKeyUP(), onKey()`. You can also use a switch case to check for more keys or even create a custom keybinding system.

You now know the basics and it's up to you to get creative with it!
