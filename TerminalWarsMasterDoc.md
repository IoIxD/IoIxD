# TerminalWars MasterDoc

### Summary
TerminalWars is an attempt to create a multiplayer D&D-like experience, that can be used for less serious, comedic campaigns, as well as serious, fleshed out ones. While the experience of a text adventure is retained, players can also have images next to their name. They're also able to create minigames, and make people play them as part of their attack. While you have to use your imagination, you can also express your character visually too.

### Gameplay
Battles work in a turn based system. When a player's turn arrives, they can do any of the following:
- Choose one of their "actives" from a dropdown (explained below).
- Do a very basic attack (unless their bio says they don't have one). To do their basic attack, a player types the command
`/basic <target> <flair>`, i.e. `/basic ioi "throws a rock at {target} and makes him trip"`. `{target}` is replaced with the target name, which is useful because the target can be `{random}`. 
- They can also do an "inspect" active on a player to see their bio and health. 
- Open their bag. They can double click an item in that bag to use it, or they can do `/give <target> <item>`, and they can drag an item from their bag into the window to autofill the name in the input dialog.
- Switch to a different character; a player can own multiple.
- Send messages to anyone on the battlefield. This can be done outside of battle too. People should be able to type in italics by surrounding their text with *.

Actives are custom attacks that are created by the player's owner. The dungeon master can enable a points system for attacks, requiring players to balance their attacks by spending points on certain commands. This is optional, but it's game wide; either all players use points, or none of them. Grouped actives also exist; these are 

Characters also have passives, which work exactly like actives, but are generally not selectable, and are automatically activated at the start of a battle. This is useful for abilities, and also status effects - people have ~~apparently~~ made bios entirely around passives. Changes made by passives are usually always active, but a player can remove their own passive.

Finally, a character may have "specials", which are actives that are reusable by certain attacks. 

A battle may have two modes: a "free for all" in which anyone can attack with max accuracy, or a "dice" mode in which both players have a random chance of missing their attacks. This is game-specific, a player with max accuracy can't go against a dice player. Max accuracy players also have set defense, and that defense is subtracted from each attack.

Turn order should be chosen by a dice roll between all players or teams (see next paragraph), but if it's a one on one battle, we specifically due a coin flip. It can also be determined by a minigame, if a player has a passive that calls for it.

Players can be grouped into a team. When a team gets into a battle, they all share the same turn, and the turn is elapsed when everyone has an attack.

### UI

The UI is based on Windows 98 because that's the next best thing to making this a full on terminal.
This section is here for completeness but honestly not much else needs to be said, especially because 70% of the action will happen around the terminal window anyways.

### System Requirements
I could just as easily learn another game engine and make this an executable, but I want this to be a browser game because it is the easiest way to make sure most, if not all, people can play this regardless of their operating system, as long as they can get a recent version of Chromium or Firefox (you can get a patched Chromium 69 working on Windows XP). On that note, this should be accessible to people regardless of whether they have a good computer or not, and as such, making this as optimized as possible is prefarable.
Thus, the minimum system that should be needed to run this is my Thinkpad T61 from 2008. That has a Core 2 Duo CPU with 4GB Ram, and it runs the OpenRC variant of ArtiX Linux.

### Server Requirements
The server, at it's simplest, should take JSON blocks and store them into a series of JSONs known as a room. A room is the internal name for somebody's campaign, and it is initialized by a base save game comprised of the players participating, and the subrooms; areas in this campaign that the players will travel to and interact with. Subrooms are filled with objects, and dialogue that pops up if a player tries to interact with them via basic commands (said commands will be chosen by the dungeon master). Subrooms will also have enemies. 

Commands are specialized. You shouldn't be able to just put any JSON block anywhere. However, the initial demo should likely have the following ready:

The commands for putting values should be as follows:

- `init` - initalizes a Room object (undefined, currently) on the server, with a random password *(I will actually look into setting up my own implementation of [LavaRand](https://en.wikipedia.org/wiki/Lavarand) that sounds badass tbh)*.
  -  When the server is started, a test room will be created. This room will be disabled in production.
- `player` - adds a User object (defined in `player.js`) to a room. The user will use `testPlayer.json` by default.

These commands should return 0 or 1, and if 1 then it should also return the error.

The following commands can edit values:
- `active` - Pass the player, one of their active/passive/specials, and the target. Calculate the given active/passive/special. If it succeeds, return the amount of damage done. Otherwise, return 1.

The client should receive and do things with the following commands:

- `broadcast` - Messages to be broadcast to the terminal. This should be a command executable by the client too, because it's mostly used for chat messages.
- `dice` - Spawn a dice object. It should have the number it will land on precalculated; the animation will be pre-calculated.
