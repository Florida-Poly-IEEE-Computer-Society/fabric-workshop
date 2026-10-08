# Fabric Minecraft Modding Workshop

In this workshop, we will create a simple Minecraft mod using Fabric, Java, Gradle, and VSCode.

> Note: You are not expected to understand every part of Gradle, Fabric, or Minecraft's source code by the end of this workshop. The goal is to understand the general workflow for making a mod and leave with a working project you can continue experimenting with.

---

# 1. Install Visual Studio Code

Download and install [VSCode](https://code.visualstudio.com/download).

After installing it, open it.

---

# 2. Install JDK 25

Minecraft 26.x mod development requires [JDK 25](https://adoptium.net/temurin/releases?version=25). Download and install for whatever platform you're developing on.

The JDK contains the Java compiler and development tools we need to write Java code.

After installing JDK 25, open a terminal and run:

```bash
java -version
```

You should see Java 25 listed somewhere in the output.

For example:

```text
java version "25"
```

If Java is not recognized, or some older version appears, make sure JDK 25 was installed correctly before continuing.

## Why do we need the JDK?

Minecraft (though not Bedrock Edition) is written in Java, and while the Java Runtime Environment (JRE) is enough to run most apps, we need the Java Development Kit (JDK) to create Java programs.

---

# 3. Install Java Support in VSCode

VSCode does not include full Java development support by default, so we'll install the Extension Pack for Java.

Open the Extensions tab on the left side of VSCode.

Search for:

```text
Extension Pack for Java
```

Install the extension pack published by Microsoft.

This adds features such as:

- Java syntax highlighting
- Autocomplete
- Error checking
- Java project support
- Debugging
- Gradle integration (important!)

Once it finishes installing, restart VSCode.
# 4. Get the Fabric Example Mod

Instead of creating every Fabric configuration file manually, we are going to start from Fabric's example mod.

Open VSCode.

Then open the integrated terminal:

```text
Terminal -> New Terminal
```

In the terminal, navigate to the folder where you want to store the project.
```text
(pwd - prints current directory
cd [folder] - to get to a folder
cd .. - to go up a folder, i.e. /level1/level2 -> /level1)
```

If you have Git installed, clone the Fabric example mod with:

```bash
git clone https://github.com/FabricMC/fabric-example-mod.git
```

Then open the project folder in VSCode:

```text
In VSCode, top left:
File -> Open Folder
```

Select:

```text
fabric-example-mod
```

from wherever you created it.

## If You Don't Have Git

If Git is not installed or does not work on your computer, download the provided ZIP file instead:

[DOWNLOAD ZIP HERE](https://raw.githubusercontent.com/Florida-Poly-IEEE-Computer-Society/fabric-workshop/refs/heads/main/fabric-example-mod.zip)

Extract the ZIP file, then return to VSCode and use:

```text
File -> Open Folder
```

Select the extracted project folder and open it.

Both methods give you the same starter project, so use whichever works for you (but git is important to know!)

# 5. Let Gradle Import the Project

Fabric projects use Gradle as their build system.

When you first open the project, VSCode may take a little while to:

- Detect the Java project
- Download dependencies
- Import the Gradle project
- Index everything

Let this process finish before continuing.

You might see a Gradle elephant icon or Gradle section appear in VSCode.

## What is Gradle?

Gradle handles much of the boring (zzz...) setup for us.

It takes care of things such as:

- Downloading Fabric
- Downloading Minecraft dependencies
- Compiling our Java code
- Running a development version of Minecraft
- Packaging our mod into a `.jar`

You don't have to understand the entire Gradle configuration for this workshop, but figuring out how it works is a worthwhile endeavor. It'll get you familiar with how build systems work, which is important for being a developer in general.

> If Java is not recognized, an older version appears, or Gradle says that it is using an older version of Java, see [Fixing Java 25 When Another Java Version Is Being Used](#fixing-java-25-when-another-java-version-is-being-used).

---

# 6. Look Around the Project

Before changing anything, take a minute to look at the project structure.

Some of the most important locations are:

```text
fabric-example-mod/
├── src/
│   └── main/
│       ├── java/
│       └── resources/
├── build.gradle
├── gradle.properties
├── gradlew
├── gradlew.bat
└── settings.gradle
```

## `src/main/java`

This is where most of our Java source code lives.

For example:

```text
src/main/java/com/example/ExampleMod.java
```

## `src/main/resources`

This contains files used by Fabric and Minecraft.

One particularly important file is:

```text
fabric.mod.json
```

This file describes the mod to Fabric Loader.

## `gradle.properties`

This contains several project settings, including information used when naming and building our mod.

## `build.gradle`

This tells Gradle how the project should be compiled.

We will mostly leave this file alone for this workshop.

---

# 7. Run Minecraft Before Changing Anything

Before modifying the project, we want to make sure the development environment works.

This gives verifies whether we've installed everything correctly, and can save us from a potential headache later.

## Using Gradle

In the VSCode terminal we will run the following:

On Windows:

```bash
./gradlew.bat runClient
```

On macOS or Linux:

```bash
./gradlew runClient
```

You can also find the `runClient` task in VSCode's Gradle panel.

Gradle may need to download additional files the first time you run this command.

Once done, a development version of Minecraft should launch.

## Checkpoint 1

At this point:

> Minecraft launches from our development environment.

If Minecraft does not launch, fix the environment before continuing.

This is much easier than debugging your code and your environment at the same time (speaking from experience).

Once Minecraft launches successfully, close the game.

---

# 8. Customize the Mod

Right now, the project is still named like the Fabric example mod, so let's change that.

## Edit `gradle.properties`

Open:

```text
gradle.properties
```

Look for:

```properties
group=com.example
```

Change it to something appropriate for your project.

For example:

```properties
group=com.lastnamefirstname
```

Your group is normally written like a reversed domain name.

For this project, something simple like this is fine:

```properties
group=com.anthony
```

## Edit `settings.gradle`

Open:

```text
settings.gradle
```

Near the bottom, you should see:

```gradle
rootProject.name = 'modid'
```

Change it to:

```gradle
rootProject.name = 'nocroptrample'
```

This should match the ID we are going to give our mod.

---

# 9. Edit `fabric.mod.json`

Open:

```text
src/main/resources/fabric.mod.json
```

This file contains metadata that Fabric uses to identify your mod.

Look for fields such as:

```json
"id": "modid",
"name": "Example Mod",
"description": "This is an example description!"
```

Which we can change.

For example:

```json
"id": "nocroptrample",
"name": "No Crop Trample",
"description": "Prevents entities from trampling farmland."
```

## The Mod ID

The ID is the internal identifier Fabric uses for your mod, generally simpler is better.

> Mod IDs generally use lowercase letters without spaces.

## Remove the Fabric API Dependency

The example project includes Fabric API by default, but this mod does not actually use it.

Open:

```text
build.gradle
```

and remove the Fabric API dependency line:

```gradle
implementation "net.fabricmc.fabric-api:fabric-api:${project.fabric_api_version}"
```

Then return to:

```text
src/main/resources/fabric.mod.json
```

and remove:

```json
"fabric-api": "*"
```

from the `depends` section.

This leaves us with a Fabric Loader + Mixin mod without an unnecessary Fabric API dependency.

---

# 10. Generate and Browse Minecraft Source

One of the most useful parts of Minecraft modding is being able to inspect Minecraft's own classes.

Run:

### Windows

```bash
./gradlew.bat genSources
```

### macOS/Linux

```bash
./gradlew genSources
```

This allows your development environment to provide Minecraft source information when navigating through classes.

One of the most important skills in Minecraft modding is learning how to answer questions like:

> "What Minecraft class actually controls the behavior I want to change?"

For our mod, that question is:

> "What code causes farmland to turn into dirt when something lands on it?"

---

# 11. The Fabric Mod Entry Point

Open the main Java class.

It will look something like this:

```java
public class ExampleMod implements ModInitializer {
    @Override
    public void onInitialize() {
        // Initialization code goes here
    }
}
```

You may be used to seeing Java programs begin with:

```java
public static void main(String[] args)
```

Fabric mods work differently since Minecraft is already a running program. The Fabric Loader discovers our mod and calls its entry point *while* Minecraft starts up.

For a basic Fabric mod, our class implements:

```java
ModInitializer
```

and Fabric calls:

```java
onInitialize()
```

when our mod is loaded.

---

# 12. Let's Make Sure Our Code Runs

The example project includes a logger.

Inside `onInitialize()`, add or change the log message:

```java
LOGGER.info("No Crop Trample has loaded!");
```

Run Minecraft again:

```bash
./gradlew.bat runClient
```

or:

```bash
./gradlew runClient
```

Watch the terminal in VSCode; you should eventually see that message appear. This means our mod was initialized successfully!

## Checkpoint 2

We now know:

> Minecraft is successfully loading our Java code.

Great! Now close Minecraft again, now that all our setup is done, we're going to write the actual logic for our mod.

---

# 13. The Goal: Stop Farmland Trampling

Now we can make an actual gameplay change.

In normal Minecraft, sufficiently hard falls (>= 1 block) onto farmland can cause farmland to turn into dirt, i.e. trampling.

The usual logic is:

```text
Farmland -> Dirt
```

We want:

```text
Farmland -> Farmland
```

The first question is:

> Where does Minecraft implement this behavior?

Search the generated Minecraft source for the `FarmlandBlock` class.

In VSCode, press:

```text
Ctrl + P
```

Then type:

```text
#FarmlandBlock
```

and select the Minecraft `FarmlandBlock` class from the results.

Once you have `FarmlandBlock` open, look for the method responsible for an entity landing on the block.

For Minecraft 26.3, you should find this method:

```java
fallOn(...)
```

This is where Minecraft handles farmland trampling behavior.

This method contains Minecraft's farmland trampling behavior. If you go on to create other Fabric mods, you can usually just Google which class contains the behavior you're looking to modify.

> If Minecraft classes do not appear in the search results, make sure you ran the `genSources` Gradle task first.

---

# 14. But We Should Not Edit Minecraft's Code

It may be tempting to open `FarmlandBlock.java` and simply delete the code, however, this won't work.

Minecraft's source code is not actually part of our project (due to copyright concerns). Its being provided to us so we can inspect it, and write something that modifies the bytecode at runtime.

Instead, we need a way for our mod to modify Minecraft's behavior while the game is loading; one of the tools Fabric mods use for this is called a Mixin.

---

# 15. What Is a Mixin?

A Mixin allows us to modify the behavior of an existing Minecraft class without directly editing Minecraft's source code.

Conceptually, think of it this way:

```text
When Minecraft reaches FarmlandBlock.fallOn(...),
run our code instead of, before, or after a portion of the original behavior.
```

Mixins are REALLY useful.

For this workshop, though, we only need to understand these three ideas:

```text
@Mixin
```

Specifies the Minecraft class we want to modify.

```text
@Inject
```

Specifies where we want our code inserted.

```text
CallbackInfo
```

Allows our injected method to interact with the original method, including cancelling it when possible.

---

# 16. Create the Farmland Mixin

Inside your Java package, create a package named:

```text
mixin
```

For example:

```text
src/main/java/com/example/mixin/
```

Create:

```text
FarmlandBlockMixin.java
```

The class should target Minecraft's `FarmlandBlock`.

The structure will look approximately like this:

```java
@Mixin(FarmlandBlock.class)
public class FarmlandBlockMixin {

}
```

Import:

```java
net.minecraft.world.level.block.FarmlandBlock
```

and:

```java
org.spongepowered.asm.mixin.Mixin
```

---

# 17. Inject Into the Landing Behavior

We want to intercept the method responsible for an entity landing on farmland.

Our injection will target:

```text
fallOn
```

and run at the beginning of that method.

Conceptually:

```java
@Inject(
    method = "fallOn",
    at = @At("HEAD"),
    cancellable = true
)
```

What does this all mean?

`HEAD` means:

> Run our injected code at the very beginning of the original method.

`cancellable = true` means:

> Allow our injected code to prevent the rest of the original method from running.

Inside the injected method, we can cancel the normal farmland behavior.

In general, exact parameter types of Minecraft methods can change between Minecraft versions. Use the generated source and VSCode autocomplete to match the `fallOn` signature in the version being used for this workshop.

> Though, I'd be surprised if this specific method were to change, unless we maybe get some kind of farming update.

The important idea is:

```java
ci.cancel();
```

This prevents the original `FarmlandBlock.fallOn()` implementation from continuing.

Here is something that will work for this workshop:

```java
package com.example.mixin;

import net.minecraft.core.BlockPos;
import net.minecraft.world.entity.Entity;
import net.minecraft.world.level.Level;
import net.minecraft.world.level.block.FarmlandBlock;
import net.minecraft.world.level.block.state.BlockState;

import org.spongepowered.asm.mixin.Mixin;
import org.spongepowered.asm.mixin.injection.At;
import org.spongepowered.asm.mixin.injection.Inject;
import org.spongepowered.asm.mixin.injection.callback.CallbackInfo;

@Mixin(FarmlandBlock.class)
public class FarmlandBlockMixin {

    @Inject(
        method = "fallOn",
        at = @At("HEAD"),
        cancellable = true
    )
    private void preventTrampling(
        Level level,
        BlockState state,
        BlockPos pos,
        Entity entity,
        double fallDistance,
        CallbackInfo ci
    ) {
        ci.cancel();
    }
}
```

---

# 18. What Did We Just Do?

Without our Mixin, Minecraft does roughly this:

```text
Entity falls
    V
FarmlandBlock.fallOn()
    V
Minecraft checks fall conditions
    V
Farmland may become dirt
```

With our Mixin:

```text
Entity falls
    V
FarmlandBlock.fallOn()
    V
Our Mixin runs first
    V
Cancel the rest of the method
    V
Minecraft's farmland trampling logic never runs
```

In essence, we changed vanilla Minecraft behavior without modifying Minecraft's source files.

This is one of the fundamental ideas behind Minecraft modding.

> Plugins work somewhat differently because platforms like Bukkit and Paper expose a much more detailed event API for plugins to hook into.

---

# 19. Register the Mixin

Now that we've written the Mixin, we need to add it to the Mixin configuration that the example project already uses.

Open:

```text
src/main/resources/modid.mixins.json
```

It should look roughly like:

```json
{
  "required": true,
  "package": "com.example.mixin",
  "compatibilityLevel": "JAVA_25",
  "mixins": [
    "ExampleMixin"
  ],
  "injectors": {
    "defaultRequire": 1
  }
}
```

Replace `ExampleMixin` with our new Mixin:

```json
"mixins": [
    "FarmlandBlockMixin"
]
```

Do not include `.java`, just the class name.

You can also delete the old `ExampleMixin.java` file since we are not using it anymore.

We do not need to add anything new to `fabric.mod.json`, because the example project already registers this Mixin configuration:

```json
"mixins": [
  "modid.mixins.json",
  {
    "config": "modid.client.mixins.json",
    "environment": "client"
  }
]
```

These are two separate entries.

`modid.mixins.json` is the normal Mixin configuration, so this is where `FarmlandBlockMixin` belongs.

`modid.client.mixins.json` is only loaded on the client because it has:

```json
"environment": "client"
```

We are not using the client-only Mixin configuration for this mod.

> The filename `modid.mixins.json` does not have to match the mod ID. You can leave it named `modid.mixins.json` for this workshop. If you rename the file, make sure you also change its filename in the `mixins` section of `fabric.mod.json`.

---

# 20. Run the Mod

Launch Minecraft again:

```bash
./gradlew.bat runClient
```

or:

```bash
./gradlew runClient
```

Create a world (in creative), till some land, and start hopping on it!

Try falling onto it from several block heights >= 1. If it stays farmland, you've now successfully defeated my least favorite Minecraft mechanic!

## Checkpoint 3

Without our mod:

```text
Farmland can be trampled into dirt.
```

With our mod:

```text
Farmland stays farmland.
```

Congratulations!!!!

You have now modified vanilla Minecraft behavior.

---

# 21. What We Have Learned So Far

At this point, you have used several major pieces of the Minecraft modding ecosystem:

### Java

The programming language our mod uses.

### Fabric Loader

Loads our mod into Minecraft.

### Gradle

Builds and launches our project.

### Minecraft Source

Allows us to investigate how vanilla behavior works.

### Mixins

Allow us to modify existing Minecraft code.

This workflow appears constantly in real Minecraft mod development:

```text
Think of a behavior
        V
Find the Minecraft class responsible
        V
Read the Minecraft source
        V
Find a useful method
        V
Use a Fabric API hook or a Mixin
        V
Run Minecraft
        V
Test
```

---

# 22. Build the Mod

So far, we have been running Minecraft through our development environment.

Now we want to create an actual `.jar` file that can be installed like a normal mod, so you can give it to your friends, who are surely green from envy, or to use it in a Fabric multiplayer server.

Run:

### Windows

```bash
./gradlew.bat build
```

### macOS/Linux

```bash
./gradlew build
```

Gradle will compile and package the project.

If everything succeeds, you should eventually see:

```text
BUILD SUCCESSFUL
```

---

# 23. Find the Finished Mod

Open:

```text
build/libs/
```

You will probably see multiple `.jar` files.

For example:

```text
no-crop-trample-1.0.0.jar
no-crop-trample-1.0.0-sources.jar
```

The `sources` JAR contains source code.

The shorter normal JAR is the one we want to install:

```text
no-crop-trample-1.0.0.jar
```

## Checkpoint 4

You have now created a distributable Minecraft mod.

---

# 24. Install the Mod Normally

To test the mod outside the development environment, copy the built `.jar` into your Minecraft Fabric installation's:

```text
mods
```

folder.

On Windows, this is commonly:

```text
%appdata%\.minecraft\mods
```

Make sure the Minecraft instance has:

- Fabric Loader
- The correct Minecraft version
- Your mod

Start Minecraft using the Fabric profile.

Create or enter a world, and jump around on some farmland again.

If the farmland does not turn into dirt, your built `.jar` works.

---

# Stretch Goal: The Rocket Sword

If we have time remaining, we will move from modifying existing Minecraft behavior to adding our own custom item: the rocket sword.

When the player right clicks with the sword, we take the direction the player is currently looking. You can think of this as a vector pointing straight out from the player's perspective.

We then take the negative of that vector, which points directly behind the player.

Behind the player, we construct a plane perpendicular to their viewing direction and spawn five TNT entities arranged in a circle on that plane.

For example, if the player is looking upward:

```text
                         ↑
                         ↑  Player view direction
                         ↑
                       Player

                         |
                         |
                    TNT  TNT  TNT
                      TNT   TNT

            Plane perpendicular to view direction
```

So the TNT positions lie in a plane whose normal vector is the player's view direction.

Each TNT entity is spawned with a fuse time of `0`, causing all five to explode immediately.

The explosions should act like a very dumb rocket booster!

The direction of the boost depends on where the player is looking. Look forward and activate the sword to launch forward. Look upward and activate it to launch upward. Look diagonally and the explosions should launch you in roughly that direction.

For now, we're not really worried about making this balanced or safe. The TNT can damage the player and destroy blocks; the goal is just to make something fun and learn how to create custom behavior.

This introduces several additional concepts:

- Item registration
- Minecraft registries
- Custom item classes
- Player interaction
- Player view vectors
- Basic vector math
- Constructing a plane orthogonal to a vector
- Spawning entities
- TNT fuse times
- Explosion physics

The general process is:

```text
Register the Sword
        V
Detect Right Click
        V
Get Player View Vector
        V
Find the Point Behind the Player
        V
Construct a Plane Perpendicular to the View Vector
        V
Place 5 TNT in a Circle on That Plane
        V
Set Each TNT Fuse to 0
        V
KABOOM
        V
Go Flying
```

If we do not reach this section during the workshop, it makes a good project to continue experimenting with afterward.
---

# Workshop Checkpoints

By the end of the workshop, you should have completed these four checkpoints:

### Checkpoint 1

Minecraft launches from the Fabric development environment.

### Checkpoint 2

Your own Java code runs when Fabric loads the mod.

### Checkpoint 3

Farmland can no longer be trampled.

### Checkpoint 4

Your mod builds into a working `.jar`.

If you reached all four, you have completed the basic workflow for developing a Fabric Minecraft mod.

---

# Where to Go From Here

The no crop trample mod is intentionally small, and only really targets a behavior I particularly dislike.

The same development process can be used to create much larger projects, though.

Some possible next projects:

- Add a custom item
- Add a custom block
- Add a new crafting recipe
- Create a command
- Change mob behavior (sprinter zombies 🤔??)
- Add a new enchantment
- Add custom status effects
- Create custom weapons
- Add new world generation (this one is hard, though)
- Add keybinds
- Create a configuration menu
- Modify other vanilla mechanics with Mixins

# Troubleshooting

## `java` is not recognized

Run:

```bash
java -version
```

If the command fails, Java may not be installed correctly or may not be available on your system's `PATH`.

Make sure you installed JDK 25 specifically.

---

## VSCode Shows Lots of Java Errors

The Gradle project may still be loading.

Wait for Java and Gradle initialization to finish.

If problems continue:

1. Close VSCode.
2. Reopen the entire project folder.
3. Make sure JDK 25 is selected.
4. Allow Gradle to reload.

---

## Minecraft Will Not Launch

Try:

```bash
./gradlew.bat runClient
```

on Windows or:

```bash
./gradlew runClient
```

on macOS/Linux.

Look at the error near the bottom of the terminal output.

The first useful error is usually more important than the billions of lines that might follow.

---

## My Mixin Does Not Work

Check:

- Is the Mixin listed in the Mixin JSON file?
- Is the Mixin configuration listed in `fabric.mod.json`?
- Is the package name correct?
- Does `@Mixin` target the correct class?
- Does the injected method match the current Minecraft method signature?
- Is the method name spelled correctly?
- Did Minecraft print a Mixin error while launching?

Mixin errors are often very specific, so read the first error carefully.

---

## `build` Fails

Make sure the game launches successfully with:

```bash
./gradlew runClient
```

before trying to build.

Then run:

```bash
./gradlew build
```

again and examine the first compilation error.

---

# Useful Gradle Commands

| Command | Purpose |
|---|---|
| `./gradlew runClient` | Launch Minecraft with the mod |
| `./gradlew build` | Compile and package the mod |
| `./gradlew genSources` | Prepare Minecraft sources for development |
| `./gradlew clean` | Delete generated build files |

On Windows, use:

```text
gradlew.bat
```

instead of:

```text
gradlew
```

For example:

```bash
./gradlew.bat build
```

---

# Fixing Java 25 When Another Java Version Is Being Used

If you have multiple versions of Java installed, your computer may not automatically use JDK 25.

When you run:

```bash
java --version
```

your system generally uses the first Java installation it finds in its `PATH`.

VSCode and Gradle can also select their own Java runtime, so it is possible for `java --version` to show one version while Gradle uses another.

If you see an error similar to:

```text
Dependency requires at least JVM runtime version 25.
This build uses a Java 21 JVM.
```

you need to make sure JDK 25 is actually being used.

## Windows

First, check which Java installations Windows can find:

```bash
where.exe java
```

You may see something like:

```text
C:\Program Files\Eclipse Adoptium\jdk-21...\bin\java.exe
C:\Program Files\Eclipse Adoptium\jdk-17...\bin\java.exe
C:\Users\YourName\AppData\Local\Programs\Eclipse Adoptium\jdk-25...\bin\java.exe
```

Windows will normally use the first one in this list.

Check your current Java version with:

```bash
java --version
```

If JDK 25 is not being used, open:

```text
Start -> Edit the system environment variables -> Environment Variables
```

Make sure `JAVA_HOME` points to your JDK 25 installation.

For example:

```text
C:\Users\YourName\AppData\Local\Programs\Eclipse Adoptium\jdk-25...
```

Then make sure your `Path` contains:

```text
%JAVA_HOME%\bin
```

and that it appears before older Java installations.

Close and reopen VSCode after making these changes.

Then check again:

```bash
java --version
```

You should see Java 25.

### If VSCode Gradle Still Uses Java 21

VSCode's Java extensions may use their own Java runtime even if your terminal is correctly using Java 25.

If the Gradle output contains something like:

```text
Java Home: C:\Users\YourName\.vscode\extensions\redhat.java...\jre\21...
```

open the VSCode Command Palette:

```text
Ctrl + Shift + P
```

Then select:

```text
Preferences: Open User Settings (JSON)
```

Add:

```json
"java.import.gradle.java.home": "C:\\path\\to\\your\\jdk-25"
```

For example:

```json
"java.import.gradle.java.home": "C:\\Users\\YourName\\AppData\\Local\\Programs\\Eclipse Adoptium\\jdk-25.0.4.101-hotspot"
```

Save the file.

Then open the Command Palette again and run:

```text
Java: Clean Java Language Server Workspace
```

Allow VSCode to restart.

---

## macOS

First, check which JDKs are installed:

```bash
/usr/libexec/java_home -V
```

You should see JDK 25 somewhere in the list.

To find its location, run:

```bash
/usr/libexec/java_home -v 25
```

To use Java 25 in your current terminal, run:

```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 25)
export PATH="$JAVA_HOME/bin:$PATH"
```

Then check:

```bash
java --version
```

You should see Java 25.

To make this permanent, add the same lines to your shell configuration file.

For the default macOS shell, this will normally be:

```text
~/.zshrc
```

Add:

```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 25)
export PATH="$JAVA_HOME/bin:$PATH"
```

Then restart your terminal or run:

```bash
source ~/.zshrc
```

### If VSCode Gradle Still Uses an Older Java Version

Open the VSCode Command Palette:

```text
Cmd + Shift + P
```

Then select:

```text
Preferences: Open User Settings (JSON)
```

Find the JDK 25 path with:

```bash
/usr/libexec/java_home -v 25
```

Then add that path to your VSCode settings:

```json
"java.import.gradle.java.home": "/path/to/your/jdk-25"
```

For example:

```json
"java.import.gradle.java.home": "/Library/Java/JavaVirtualMachines/temurin-25.jdk/Contents/Home"
```

Save the file.

Then open the Command Palette again and run:

```text
Java: Clean Java Language Server Workspace
```

Allow VSCode to restart.

# Resources

- [Fabric Documentation](https://docs.fabricmc.net/)
- [Fabric Example Mod](https://github.com/FabricMC/fabric-example-mod)
- [Fabric Project Generator](https://fabricmc.net/develop/template/)
- [Mixin Introduction](https://wiki.fabricmc.net/tutorial:mixin_introduction)
- [Git Documentation](https://git-scm.com/doc)

## Open Source Mods

Looking through existing open source mods is one of the best ways to learn how larger Fabric projects are structured.

- [Modrinth](https://modrinth.com/)
- [CurseForge](https://www.curseforge.com/minecraft)

If a mod links to its source code, open the repository and try to find where a feature you recognize is implemented.

Thanks for reading!!
