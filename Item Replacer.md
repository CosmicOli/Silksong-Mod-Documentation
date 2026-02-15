All interactable items in silksong need removing and replacing:
*Non-standard items in their categories have been specified*

-> Fleas
-> -> Kratt (Greymoor)
-> -> Vog (Putrified Ducts)
-> -> Huge Flea (Memorium)

-> Tools
-> -> Progressive Tools
-> -> -> Claw Mirror / Claw Mirrors
-> -> -> Curveclaw / CurveSickle
-> -> -> Druid's Eye / Druid's Eyes
-> -> Alternative Tools
-> -> -> Silkshot
-> -> -> Dead Bugs Purse / Shell Satchel

-> Mask Shards

-> Spool Fragments

-> Needle Upgrades

-> Needolin Upgrades

-> Abilities and Spells

-> Crests
-> -> eva upgrades


Depending on how the pickup is done, there are certain functions that are called:

-> CollectableItemPickup.DoPickup()
-> -> Some tools, rosary items, relics, misc items
*Presumably SavedItem.isUnique only; this may break rosary item pickups though so needs testing*

-> *Some quest reward function*
-> -> Quests are fulfilled by QuestBoardInteractable, which tells the quest to reward it's item(s)
-> -> FullQuestBase's have reward icons, counts, etc, which should be replaced when the quest is created

-> *Some npc interaction/give function*
-> -> On ending dialouge this.EndedDialogue?.Invoke(); is called
-> -> *find out what is being called in this situation to give an item, and see if it is replaceable*

-> *Crest pickup function*
-> -> Presumably one of AutoEquipCrest 1 through 4 is called when a crest is picked up
-> -> Every crest (bar eva upgrades) is gotten through an interaction, so these can just be replaced with item pickups
-> -> -> Eva upgrades are likely handled like every other npc

-> *Boss fight reward function & Main ability statue interaction function*
-> The abilities provided bring up a big MsgBox
-> I actually think it is easiest to potentially alter message boxes than to bypass them
-> As these items are probably granted before the message boxes appear, the granting of these abilities should be verified before gifted 

-> *Shop purchase function*
-> -> ShopMenuStock seems to handle what is available in shops

-> Anything else
-> -> There is almost certainly something that changes the perminant player data to "give" an item that isn't covered by these cases
-> -> Due to it being particularly unclear how some items are granted to the player, it is hard to generically monitor changes to the player
-> -> It is possible to make it so when the player attempts to interact with something they have been granted but not supposed to have been that they then recieve the "correct" item and the wrong one is removed, but I have had issues with caching this kind of data causing conflicts with other mods that I intend to avoid breaking
-> -> -> E.g. say a mod changed my crest randomly and I tried to use it; this system would give me an item I am not supposed to have and disallow that crest from being used
->
-> I would rather handle changes to the player's data as they happen and miss some than make a catch all solution that breaks compatibility



# Design Principles
This mod should intend to modify or replace pickups or rewards instead preemptively
-> This mod should not *generally* handle the replacement as it happens or in post
-> There may be exceptions to this rule due to it being necessary, but if alternatives are found they should be prioritised

This mod should not cache what items have been picked up
-> The game tracks what events have happened in game, and these trackers should not be hindered by replacing the pickups
-> Duplicate items will not spawn by proxy of these trackers determining what pickups are available
-> -> If there is an event which gives an item that is only blocked by whether the intended item has already been picked up (independantly to any event trackers), then an exception can be made to the no chacheing rule

Functionality and stability is more important than in game clarity or immersion
-> The most important thing is that items are either replaced successfully 1 to 1 or not replaced at all
-> -> No item should ever accidentally have multiple effects (e.g. giving the original and new effect)
-> If it is difficult to communicate an item has been replaced, it should still be replaced
-> -> For example, if it isn't possible to remove or modify the needolin msg text when beating widow, the pickup for needolin should still be replaced even if unclear

Association should not be defined or stored by this mod
-> All this mod should do is turn every item pickup into an open ended request based on a label
-> -> Inherently this makes the mod handle half of the association, but it should not generate responses
-> There should be room to allow dependant mods to replace the generic pickup prefab with a specifically associated prefab
-> -> Specifically, when replacing the items the unique identifier should be used to get a replacement prefab, in the same way that the unique identifier is used to get an associated item

There should be simple and abstracted methods for granting items
-> Dependant mods using these methods should not need to interact with code written by Team Cherry to achieve these method's intended functions 

All replaced pickups should have unique identifiers
-> These identifiers should be systematically generated using labeling provided by Team Cherry
-> -> Label formats should be standardised and include identifying information about location and original item

Logging is manditory
-> Replacing an item should be logged
-> Picking up an item should be logged
-> Being granted an item should be logged