# Vague Outline
Item Replacer
-> Replaces original items with pickups that can run arbitrary code with the original item as input
-> -> The original item needs to have a unique identifier
-> -> The arbitrary code that is ran should be patchable by other mods
-> Can grant any item to the player
-> -> The only thing granting an item to the player needs is the unique identifer of the item; the technicalities of how each type of item is given should be handled by this mod
-> Must be able to replace items in real time, as some items are created or granted upon defeating a boss or enemy and not just when the room loads

Randomiser Core
-> Associates a "logic" to each item (the manditory prerequisits each item has)
-> -> This must also take into account non-item pre-requists, such as entering act 2, to avoid breaking game triggers
-> Must handle "weird" item pickups, such as needolin, to avoid the player being hardlocked in locations requiring abilities to progress through

Local Randomiser
-> Uses Randomiser Core's logic to create a validly randomised game
-> -> Must be able to validate that a randomised game follows logic
-> Patches Item Replacer to give the player an item based on the original item

*Below requires more research*

Archipelago Randomiser
-> Patches Item Replacer to send that an item was picked up to an archipelago server
-> Listens for responses from an archipelago server
-> -> Patches Item Replacer to give the player an item instructed by archipelago

Archipelago Options and APWorld Generator
-> Somehow provides logic to archipelago



