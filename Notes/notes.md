SceneData.instance.PersistentBools provides a list of persistent bools (yes or no pickups)

SceneManager.GetActiveScene() gets the active scene

https://docs.unity3d.com/6000.3/Documentation/ScriptReference/SceneAsset.html


Due to the rather hack-together nature of this project, I have cobbled together a new class called GenericSavedItem which effectively is just CollectableItem but stripped down.

From the looks of it, certain game objects are directly inhereting SavedItem; fleas may work like this, I should have a look

For the sake of easily creating multiple instances of a generic pickup in the same scene, I am bypassing the ObjectPool. This is done by Team Cherry themselves for objects with presumably no spawn limit, so this should not cause any issues.

Electing to not care about prefabcollectable, as fleas do not seem to be them

[Conditional("IsPersistenceHandled", false, true, false)]

Voltvyrm seems to provide 2 drops, one on entry and one on defeat???
-> deal with later


use CollectableItemPickup.IsPersistenceHandled
-> be careful to make sure that some checks aren't weird about this


Persistent bools have some quirks
-> they can find their own finite state machine
-> they can modify finite state machines


CollectableItemPickups that exist within a scene modify SceneData to set persistence
These have SceneName, ID, Value, Mutator

Geo rocks are handled also in scenedata however using SaveMyState and not using SetValue


I should attempt to override persistent bools instead of make new ones
The issue is, somehow, it is correctly identifying that the new one is a clone and marking it as such in persistence tracking
It's almost like it is scanning a room to make the list, and not using the SetValue function

On further research, it isn't tracking both, it just somehow is setting ID to the name of the object and not the itemdata id

Perhaps just yoink the name lol

Okay from further testing, the ID that is saved to the save file is not the ID stored by PersistentBool, but instead the name of the CollectableItemPickup that modifies the bool



New issue: the weaver effigy relic in "Bonetown" is not tracked as a persistent bool
-> By accidentally tracking it, there may be issues caused
-> It is instead tracked by a seperate Relics information
-> -> This is problematic; by spawning the relic based of itself and not the pickup, it means I can't track it independently to the ownership

Updated issue: this seems to also be done with tools
-> This means the mod is hard required to independantly store whether a randomised check has been picked up

I think the best option is to just track every replaced pickup using persistent bools independantly to the base game tracking


Fleas collected target order
"UnlockedExtraBlueSlot": true,
"UnlockedExtraYellowSlot": true,

Progressive tools have IsHidden set to true to hide old ones

Silkshots are called "WebShot ___", so may have a seperate check for being picked up

"ToolPouchUpgrades": 4,
"ToolKitUpgrades": 4,
These imply that which pouches and kits are tracked elsewhere

"UnlockedFastTravel": true,
"FastTravelNPCLocation": 2,
"UnlockedFastTravelTeleport": true,
"UnlockedDocksStation": true,
"UnlockedBoneforestEastStation": true,
"UnlockedGreymoorStation": true,
"UnlockedBelltownStation": true,
"UnlockedCoralTowerStation": true,
"UnlockedCityStation": true,
"UnlockedPeakStation": true,
"UnlockedShellwoodStation": true,
"UnlockedShadowStation": true,
"UnlockedAqueductStation": true,
"bellCentipedeAppeared": true,
"UnlockedSongTube": true,
"UnlockedUnderTube": true,
"UnlockedCityBellwayTube": true,
"UnlockedHangTube": true,
"UnlockedEnclaveTube": true,
"UnlockedArboriumTube": true,
"MushroomQuestFound1": true,
"MushroomQuestFound2": true,
"MushroomQuestFound3": true,
"MushroomQuestFound4": true,
"MushroomQuestFound5": true,
"MushroomQuestFound6": true,
"MushroomQuestFound7": true,

PlayerStory tracks HeartPiece, SpoolPiece, SimpleKey, MemoryLocket

"PurchasedPilgrimsRestToolPouch": true,
"PurchasedPilgrimsRestMemoryLocket": true,
"PilgrimsRestDoorBroken": false,
"pilgrimsRestRosaryThiefCowardLeft": true,
"mortKeptWeightedAnklet": false,

"PurchasedBonebottomFaithToken": true,
"PurchasedBonebottomHeartPiece": true,
"PurchasedBonebottomToolMetal": true,

There are also purchase trackers for the two non-tool non-house items in bellhart, the spool fragment and memory locket

From the looks of it, all items that are not tracked by nature of being tools have specifc flags for purchased status
-> Unclear what this effects, although I imagine this is how a lot of the moveable items work
-> It also seems to be the case for the mottled skarr and pilgrims rest tools
-> -> Unclear for bone bottom

Need to track:
Ant guy items
Pilgrims rest items
Anything thief takes
Ladybug guy drop
Sinners road guys items


Persistent Bools will be used for tracking  
-> Added benefit that it handles deleting checks for me  
-> I will have to make a pickup name standardised  
-> I will have to make a way of moving checks getting handled  
-> Assuming the deleting repeats works the way I hope, I should be able to name shop entry game objects and it work the same way  
-> If I can standardise moving checks too, it will handle that also  
-> With any luck, I should just be able to set some "default" flags to guarantee item moving, and then let persistent bools handle whether they actually stay around  

Standardisation ideas:  
-> SceneName cannot be modified  
-> -> Make get also send a ping to a moving check handler  
-> -> No point attempting to make globally unique IDs and then attempting to scan for repeats in every scene; worst comes to worst, a moving check is split in two  
-> Perhaps make a list of list of "equivalent" checks, which has a default of moving checks which can be modified or toggled on or off  
-> -> I would go a step further and make every grantable item in the game have a unique grant function and associated original checks, but that would take forever and not handle changes to the game well  
-> -> -> This being said, I think having this be automatically generated and hence also being the place the open requests are sent too (and hence be easily independantly modifiable) may make sense  

There should be a front facing layer to this mod  
-> This effectively is a request handler  
-> It deals with granting and collection etc  
-> It will contain a (by default empty) list of checks that handle granting requests, icons, descriptions, locations (specifically what requests map to what checks)  
-> A configuration of what you wish to replace can be provided to generate the list of checks  
-> -> A default configuration should be provided  
For the sake of formality, the other lower layer to this mod 
-> This handles the in game side of things  
-> Removing and replacing all types of pickups
-> Actually granting things
-> Interacts with the front facing layer through sending requests to query the check it actually is

Important to consider: logic for checks is defined by each of the associated pickups; the randomiser will have to handle both

TODO:  
Standardised persistence ID conventions  

Move onto PlayerStory items  

Test that playerstory items and relics/tools don't break replacability by cutting of the original checks before they can be replaced  

Move onto weaver statues  
Move onto fleas  

Move onto shops  
Move onto dialogue  
Move onto boss sequences  

Make popups  
Make the front facing layer  
Make a default configuration  

Map logic to each pickup  

Once a granting system is implemented, move onto archepelago  