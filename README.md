# Minecraft-Server-Optimization

There will never be a specific ready-made configuration that will give you perfect results. Each server has their own needs and limits on how much you can or are willing to sacrifice. Changing around with the options to fine tune them to your server's needs is what it's all about. If you think you found inaccurate information within this guide, you're free to open an issue or set up a pull request to correct it.

## Recommended Server.jar's

Your choice of server software can make a huge difference in performance and API possibilities. There are currently multiple viable popular server .Jars:
  
  - [Pufferfish/Pufferfish+](https://pufferfish.host/downloads) 
   A highly optimized Paper fork designed for large servers requiring both maximum performance & stability.
  - [PurpurMc](https://purpurmc.org/) 
   PurpurMc is a Pufferfish fork, made as a drop-in replacement for Paper servers, designed for performance, configurability and new fun and exciting gameplay features.
  - [PaperMc](https://papermc.io/downloads) 
   The most widely used, high performance Minecraft server that aims to fix gameplay and mechanics inconsistencies.

What you should stay away from:

  - [Bukkit](https://dev.bukkit.org/)/[CraftBukkit](https://getbukkit.org/download/craftbukkit)/[Spigot](https://www.spigotmc.org/) - Extremely outdated in terms of performance & exploit fixes compared to other server software these days.
  - Many forks further downstream from Pufferfish or Purpur will encounter instability and other issues. If you're seeking more performance gains, optimize your server or invest in a personal private fork.
  - Any plugin/software that enables/disables/reloads plugins on runtime. See [here](https://madelinemiller.dev/blog/problem-with-reload/) to understand why.
    
## Recommended Plugin's

  - [Spark](https://www.spigotmc.org/resources/spark.57242/) is a performance profiler for Minecraft clients, servers and proxies. (You do not need this if you are using Purpur, due to it coming with spark build in already)
  - [Chunky](https://www.spigotmc.org/resources/chunky.81534/) pre-generates your world to avoid generating now chunks when playing. Keep in mind that with PaperMc and above your tps will not be affected by chunk loading, but the speed of loading chunks can significantly slow down when your server's cpu is overloaded.

## Sources & Guides
  - [YouHaveTrouble's Optimization Guide](https://github.com/YouHaveTrouble/minecraft-optimization)
  - [YouHaveTrouble's Exploit fixing Guide](https://github.com/YouHaveTrouble/minecraft-exploits-and-how-to-fix-them)
  - [Paper Chan's Optimization Guide](https://paper-chan.moe/paper-optimization/)
  - [Paper Docs](https://docs.papermc.io/paper)
  - [Purpur Docs](https://purpurmc.org/docs/)
  - [Pufferfish Docs](https://docs.pufferfish.host/)

# Configurations

## Networking

### [server.properties](https://minecraft.wiki/w/Server.properties)

**network-compression-threshold**

`Good starting value: 256 (default)`

This allows you to set the cap for the size of a packet before the server attempts to compress it. Setting it higher can save some CPU resources at the cost of bandwidth, and setting it to -1 disables it. Setting this higher may also hurt clients with slower network connections. If your server is in a network with a proxy or on the same machine (with less than 2 ms ping), disabling this (-1) will be beneficial, since internal network speeds can usually handle the additional uncompressed traffic.

### [pupur.yml](https://purpurmc.org/docs/Configuration/)

**network-compression-threshold**

`Good starting value: true`

You can enable Purpur's alternate keepalive system so players with bad connection don't get timed out as often. Has known incompatibility with TCPShield.
> Enabling this sends a keepalive packet once per second to a player, and only kicks for timeout if none of them were responded to in 30 seconds. Responding to any of them in any order will keep the player connected. AKA, it won't kick your players because 1 packet gets dropped somewhere along the lines.

---

## Chunks

### [server.properties](https://minecraft.wiki/w/Server.properties)

**simulation-distance**

`Good starting value: 4 (default: 10)`

Simulation distance is distance in chunks around the player that the server will tick. Essentially the distance from the player that things will happen. This includes furnaces smelting, crops and saplings growing, etc. This is an option you want to purposefully set low, somewhere around `3` or `4`, because of the existence of `view-distance`. This allows to load more chunks without ticking them. This effectively allows players to see further without the same performance impact.

### view-distance

`Good starting value: 8 (default: 10)`

This is the distance in chunks that will be sent to players, similar to no-tick-view-distance from paper. The total view distance will be equal to the greatest value between `simulation-distance` and `view-distance`. For example, if the simulation distance is set to 4, and the view distance is 12, the total distance sent to the client will be 12 chunks.

### [spigot.yml](https://www.spigotmc.org/wiki/spigot-configuration/)

### view-distance

`Good starting value: default`

This value overwrites server.properties one if not set to default. You should keep it `default` to have both simulation and view distance in one place for easier management.

### [paper-world configuration](https://docs.papermc.io/paper/reference/world-configuration)

### delay-chunk-unloads-by 

`Good starting value: 10s (default)`

This option allows you to configure how long chunks will stay loaded after a player leaves. This helps to not constantly load and unload the same chunks when a player moves back and forth. Too high values can result in way too many chunks being loaded at once. In areas that are frequently teleported to and loaded, consider keeping the area permanently loaded. This will be lighter for your server than constantly loading and unloading chunks.

### max-auto-save-chunks-per-tick 

`Good starting value: 24 (default)`

Lets you slow down incremental world saving by spreading the task over time even more for better average performance. You might want to set this higher than 8 with more than 20-30 players. If incremental save can't finish in time then bukkit will automatically save leftover chunks at once and begin the process again.

### prevent-moving-into-unloaded-chunks

`Good starting value: true`

When enabled, prevents players from moving into unloaded chunks and causing sync loads that bog down the main thread causing lag. The probability of a player stumbling into an unloaded chunk is higher the lower your view-distance is.

### entity-per-chunk-save-limit

```
Good starting values:

    area_effect_cloud: 8
    arrow: 16
    dragon_fireball: 3
    egg: 8
    ender_pearl: 8
    experience_bottle: 3
    experience_orb: 16
    eye_of_ender: 8
    fireball: 8
    firework_rocket: 8
    llama_spit: 3
    potion: 8
    shulker_bullet: 8
    small_fireball: 8
    snowball: 8
    spectral_arrow: 16
    trident: 16
    wither_skull: 4
```

With the help of this entry you can set limits to how many entities of specified type can be saved. You should provide a limit for each projectile at least to avoid issues with massive amounts of projectiles being saved and your server crashing on loading that. You can put any entity id here, see the minecraft wiki to find IDs of entities. Please adjust the limit to your liking. Suggested value for all projectiles is around `10`. You can also add other entities by their type names to that list. This config option is not designed to prevent players from making large mob farms.

### [pufferfish.yml](https://docs.pufferfish.host/setup/pufferfish-fork-configuration/)

#### max-loads-per-projectile

`Good starting value: 8`

Specifies the maximum amount of chunks a projectile can load in its lifetime. Decreasing will reduce chunk loads caused by entity projectiles, but could cause issues with tridents, enderpearls, etc.
