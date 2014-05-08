---
author: psgs
layout: post
excerpt: >
  When developing moderately complex plugins with the Bukkit API,
  it is often necessary to perform certain tasks on a regular basis,
  such as changing a player's hunger level or starting a new mini-game arena.
  To schedule tasks that recur in minecraft, a number of methods can be used.
  These methods are all quite similar, however will perform different types
  of scheduling based on your needs.
categories:
  - development
tags:
  - java
  - minecraft
  - bukkit
  - scheduling
  - tutorial
title: Scheduling Tasks with Bukkit
---

When developing moderately complex plugins with the [Bukkit API](http://bukkit.org), it is often necessary to perform certain tasks on a regular basis, such as changing a player's hunger level or starting a new mini-game arena.
To schedule tasks that recur in minecraft, a number of methods can be used. These methods are all quite similar, however will perform different types of scheduling based on your needs.

The most common way of scheduling a regular task in bukkit is to create a new class that handles the task and extends the BukkitRunnable[^1] class.

For example, the following class could broadcast a message to the server:

    import org.bukkit.Bukkit;
    import org.bukkit.plugin.java.JavaPlugin;
    import org.bukkit.scheduler.BukkitRunnable;
     
    public class BroadCastTask extends BukkitRunnable {
     
        private final JavaPlugin plugin;
     
        public BroadCastTask(JavaPlugin plugin) {
            this.plugin = plugin;
        }
     
        @Override
        public void run() {
            // What you want to schedule goes here
            plugin.getServer().broadcastMessage("Remember to vote everyday!");
        }
     
    }

The task would then be executed by calling ```BroadCastTask.runTaskTimer(this.plugin, 10, 20);``` from another method, such as the onEnable() method in the main class, where '10' is the delay in ticks before the task is initially run, and '20' is the interval in ticks between each time the task is run.
In minecraft, one tick as equal to about 0.05 seconds, or one 20th of a second. Making 20 ticks equal to a second and 24,000 ticks equal to one in-game day[^2].

Scheduled tasks can be run asynchronously, however. Running tasks asynchronously could be quite useful when querying SQL databases on a regular basis or manipulating data in an asynchronous manner.
To do this, instead of triggering a task using ```BroadCastTask.runTaskTimer(this.plugin, 10, 20);```, we would use ```BroadCastTask.runTaskTimerAsynchronously(this.plugin, 10, 20);```.
We wouldn't be able to use BroadCastTask, however, as asynchronous tasks *must never access the Bukkit API[^3]*!

Creating a new class for each task we want to schedule can be quite clumsy sometimes. When developing a complex mini-game, for example, a MineZ[^4] plugin, we may need to create up to twenty scheduled tasks!!
We could create an anonymous task, like so:

    BukkitScheduler scheduler = Bukkit.getServer().getScheduler();
    scheduler.scheduleSyncRepeatingTask(this, new Runnable() {
        @Override
        public void run() {
            // Do something
        }, 0L, 20L);

However, this wouldn't allow us to cancel the task, as we wouldn't have any way to reference which task we wanted to cancel.
A clever hack to reference our anonymous task would be to store the result of our anonymous task inside an integer or other primitive data type[^5]; as shown by Tips48 in his StackOverflow answer [^6].

    int id = Bukkit.getServer().getScheduler().scheduleSyncRepeatingTask(plugin, new Runnable() {
        public void run() {
            myMethod();
        }, 0, 2);

To cancel that task, we could then use ```Bukkit.getServer().getScheduler().cancelTask(id);```.

A full list of scheduler types and snippets detailing how to self-cancel and trigger tasks can be found on the Bukkit Wiki.

*I hope you enjoyed this tutorial. If you have any questions, feel free to send me a [tweet](http://twitter.com/psgs00) or an [email](http://github.com/psgs).*

[^1]: [BukkitRunnable JavaDoc](http://jd.bukkit.org/dev/doxygen/d4/d0c/classorg_1_1bukkit_1_1scheduler_1_1BukkitRunnable.html)
[^2]: [Gamepedia Tick Page](http://minecraft.gamepedia.com/Tick)
[^3]: [Multithreading using Async Events](https://forums.bukkit.org/threads/tutorial-multi-threading-using-async-events.194732/)
[^4]: [MineZ Wiki](http://minezwiki.net/wiki/Home:Main_Page)
[^5]: [Oracle Primitive Data Types](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html)
[^6]: [Make a function to be called every 2 ticks](http://stackoverflow.com/questions/16126978/bukkit-how-to-make-a-function-be-called-every-2-ticks)
[^7]: [Bukkit Scheduler Programming](http://wiki.bukkit.org/Scheduler_Programming)