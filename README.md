# bridge
Version-agnostic and package-agnostic interfaces used in Constellar.
Zero strict dependencies, works as a submodule.

Used for cross-compatibility in Constellar, works across versions.
All different client versions have to do is implement these interfaces accordingly.

The aim of this system is to add a layer of agnosticism between versions,
allowing similar (or even exactly the same(!))code to work on different versions,
using only bridges.

Yes, this concept is nearly identical to Lunar's bridge system.

### Examples
Here is an example definition of a bridge:
```java
public interface IExampleBridge extends IBridge {
    // Implement the package.
    @Override
    default String getPackageAssociation() { return "net.minecraft"; }
    
    String getFieldExample();
}
```
This interface defines a bridge that can be implemented.

Here is an example of a mixin-less implementation of a bridge, which is not recommended:
```java
public class MyExampleBridge extends IExampleBridge
{
    // We can use a singleton here if the bridge connects to a static class. Otherwise, use instances.
    public static final MyExampleBridge INSTANCE = new MyExampleBridge();
    
    @Override
    public String getFieldExample() { return Example.INSTANCE.fieldExample; }
}
```

Here is the recommended approach, which uses mixins:
```java
@Mixin(Example.class) // mixin into the example class
public abstract class MixinExampleBridge implements IExampleBridge // pretend Example implements IExample
{
    // Decorate a field with shadow to reference the field in Example's class.
    @Shadow public String fieldExample;
    
    // Then simply implement the interface.
    @Override
    public string getFieldExample() { return fieldExample; }
}
```
Thanks to the magic of mixins, we can safely cast an instance of `Example` to `IExampleBridge`, and use it with a safe conscience.
`((IExampleBridge) Example.INSTANCE).getFIeldExample()`
This is similar in concept to Mixin accessors. The importancs of this system, however, can be seen below:
```java
public interface IMinecraftBridge extends IBridge
{
    // define methods, etc.
}

@Mixin(Minecraft.class)
public class MinecraftBridgeMixin implements IMinecraftBridge
{
    // interface impl, this can vary between versions but the usage outside of the mixin will always be the same!!
}

public final class MyClient
{
    public static IMinecraftBridge Minecraft; // set this value somewhere
    
    void foobar() {
        // This code will always work, no matter the version.
        // This entire class, if designed correctly, can be completely version-independent!
        System.out.println(Minecraft.getSomeField());
    }
}
```
