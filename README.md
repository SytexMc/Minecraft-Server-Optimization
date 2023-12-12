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

`Good starting value: true `

You can enable Purpur's alternate keepalive system so players with bad connection don't get timed out as often. Has known incompatibility with TCPShield.
> Enabling this sends a keepalive packet once per second to a player, and only kicks for timeout if none of them were responded to in 30 seconds. Responding to any of them in any order will keep the player connected. AKA, it won't kick your players because 1 packet gets dropped somewhere along the lines.
