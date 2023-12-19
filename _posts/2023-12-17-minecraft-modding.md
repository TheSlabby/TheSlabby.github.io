---
layout: post
title: "Minecraft Modding: Slab Client Mod and AmitySMP Plugin"
date: 2023-12-17
---

I'm excited to share my recent foray into Minecraft modding, where I developed a PvP client mod called **Slab Client** and a server plugin named **AmitySMP**. This journey has been instrumental in enhancing my Java programming skills, particularly in mastering Java's Gradle system for dependencies and building, as well as Maven for the server plugin.

## Slab Client: A PvP Mod for Minecraft

**Slab Client** is a PvP client mod designed to enhance the Minecraft PvP experience. One of the key features I implemented was a dynamic GUI system. Here's a snippet from the `GuiMenu` class, showcasing the implementation:

```java
// GuiMenu.java in Slab Client
// This class manages the GUI menu, including rendering and user interaction handling.

public class GuiMenu extends GuiScreen {
    // Instance variables for GUI elements and state management
    private Module dragging;
    private int dragInitX, dragInitY, mouseInitX, mouseInitY;
    private List<Button> buttons = new ArrayList<Button>();
    private HashMap<Long, Module> clicks = new HashMap<Long, Module>(); // Tracks clicks for double-click detection

    // Method to initialize GUI components
    @Override
    public void initGui() {
        // Adding a button to close the GUI
        buttons.add(new Button("Close", /* button dimensions and properties */));
        // Additional initialization code...
    }

    // Method to render the GUI screen
    @Override
    public void drawScreen(int mouseX, int mouseY, float partialTicks) {
        // Render the background and the Slab Client logo
        Gui.drawRect(0, 0, getWidth(), getHeight(), /* semi-transparent black */);
        // Code to render the logo with scaling...
        
        // Render buttons and handle their interactivity
        for (Button button : buttons) {
            button.render(this, mc, mouseX, mouseY);
        }

        // Render and manage draggable modules
        for (Module module : RenderGuiHandler.modules) {
            // Code to render modules and handle dragging...
        }

        // Handle dragging of modules
        if (dragging != null) {
            // Update module position based on mouse movement
            dragging.x = dragInitX + (mouseX - mouseInitX);
            dragging.y = dragInitY + (mouseY - mouseInitY);
        }
    }

    // Method to handle mouse click events
    @Override
    public void mouseClicked(int x, int y, int buttonID) {
        // Detect clicks on modules and buttons, handle dragging and double-clicks
        // ...
    }

    // Additional methods and event handlers...
}
```

This code demonstrates the GUI rendering and event handling in the Slab Client. The `drawScreen` method is responsible for rendering the GUI elements, while `mouseClicked` handles user interactions.


## AmitySMP: A Bukkit Plugin for Minecraft Servers

![amitySMPPreview](https://i.imgur.com/yXmNTtq.png)

AmitySMP is a simple PaperMC plugin I developed for a small survival world in Minecraft. It includes essential commands like `/sethome`, `/home`, `/broadcast`, and more. I used Maven for building this plugin and learned to manage dependencies effectively. Here's a snippet from the `AmityEvents` class, which handles various player events:

```java
// AmityEvents.java in AmitySMP
// This class listens to and handles various player-related events on the server.

public class AmityEvents implements Listener {

    // Event handler for player joining the server
    @EventHandler
    public void onPlayerJoin(PlayerJoinEvent e) {
        // Custom join message with styling
        e.setJoinMessage(ChatColor.DARK_AQUA + e.getPlayer().getName() + " joined the game");

        // Schedule a task to send the message of the day (MOTD) to the player
        AmitySMP.server.getScheduler().runTaskLater(AmitySMP.plugin, () -> {
            e.getPlayer().sendMessage(ChatColor.BLUE + "Welcome " + e.getPlayer().getName() + "\n" + Data.loadData(Data.path).motd);
        }, 40); // Delay of 2 seconds (40 ticks)

        // Broadcast a special message if the player is joining for the first time
        if (!e.getPlayer().hasPlayedBefore()) {
            AmitySMP.server.broadcastMessage(ChatColor.GOLD + e.getPlayer().getName() + " joined for the first time!");
        }
    }

    // Event handler for player leaving the server
    @EventHandler
    public void onPlayerLeave(PlayerQuitEvent e) {
        // Custom leave message
        e.setQuitMessage(ChatColor.DARK_PURPLE + e.getPlayer().getName() + " left the game");
    }

    // Event handler for player chat
    @EventHandler
    public void onChat(PlayerChatEvent e) {
        // Custom chat formatting
        e.setCancelled(true); // Cancel the default chat event
        AmitySMP.server.broadcastMessage(ChatColor.DARK_GREEN + e.getPlayer().getName() + " ⫸ " + ChatColor.GREEN + e.getMessage());
    }
}
```
This code is part of the event handling system in AmitySMP. It customizes player join/leave messages and chat formatting, enhancing the server experience.

## Discord Integration and Persistent Data

For AmitySMP, I also integrated Discord functionalities and set up a config file for server administrators. This allowed for seamless communication between the Minecraft server and a Discord channel. Additionally, using `BukkitObjectInputStream` and `BukkitObjectOutputStream`, I implemented a simple database system to store player home locations, which was crucial for the /sethome and /home commands.

## Conclusion

Overall, these projects have significantly improved my understanding of Minecraft client and server modifications. They have also deepened my knowledge of Java programming, particularly in using Gradle and Maven for build management. I'm looking forward to applying these skills in future projects and exploring more possibilities in the realm of game modding.

