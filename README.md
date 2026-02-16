# Silksong-Mod-Documentation
A repository for planning multiple mods and how they would interact.

# Structure
To begin with, I will keep my planning and progress within this readme, but I intend to section this off more formally later.

# Log 1 15/02/2026
# Quick and Dirty Randomiser Project
As of current, 15/02/2026, the team behind Hollow Knight's [RandomizerMod](https://github.com/homothetyhk/RandomizerMod) have not yet ported their work to Hollow Knight's sequel, Hollow Knight Silksong.

There does exist an [item randomiser](https://thunderstore.io/c/hollow-knight-silksong/p/Someone_Else/HKS_Item_Randomizer/) already, however instead of using a 'logic' system to prevent being hardlocked from game progression, it uses a pity system. In my personal experience with the mod, I also found it did not randomise certain major item pickups, although I am unsure if that was instead caused by mod conflicts.

I wish to not just play silksong with items randomised, but also have a randomiser that is compatible with [Archipelago](https://archipelago.gg). I hence intend to implement an arbitrary item replacer along with a 'logic' system that is compatible with archipelago, all before next Sunday, 22/02/2026, where me and some friends are intending to do an archipelago run together.

Considering that I am in full time education with 2 assignments due this week, I do not have full confidence in my ability to produce this within a week. This is my first time modding a game, although thankfully I do have experience with C# and Unity from prior education and projects. Either way, I'm giving it a shot.

# Log 2 15/02/2026
I have written a [vague initual outline](/Notes/Vague%20Initial%20Outline.md) of the mod interaction structure. It's not good or well researched, but it's a start.

More importantly, I have written a [more fleshed out document](/Mod%20Information/Item%20Replacer.md) with useful information and design principles regarding the Item Replacer. I hesitate to call it a design document due to a lack of specificity and consistency in what I wrote, but it does mostly outline the point of the mod.

I won't lie, I feel like definitely took longer than it needed too, but at the very least I feel somewhat more resolute in how specifically I plan on implementing an item replacer. If I was not so short on time, I would do the same for the rest of the planned mods and then write a comprehensive and specific list of features and functions and how they should interact. I may end up doing so later, but right now I must tidy my room lmao  

# Log 3 16/02/2026
This is less of a progress log (given I haven't done anything as of yet today) and more of a general note taking. In watching [Skurry's enemy randomiser video](https://www.youtube.com/watch?v=jypXjzCEpvs) last night I have realised that the notable weird item pickups given by special sequences at the end of boss fights seem to trigger on the boss' death. It is worth looking into whether there is a generic boss defeat function that can be interrupted in these specific cases.  

In other news, I have made good assignment progress and intend to have both finished by tomorrow evening or Wednesday midday, which will leave the rest of the week open for development. Well somewhat at least; I have received some emails today notifying me of new deadlines this week for society committee obligations, but hopefully those will not be as monolithic and lengthy as the coursework.

I will probably attempt to lay out the foundational structure of the item replacer this evening after some more progress with various important responsibilities. If time permits, I will also attempt to begin reading and modifying scene data on scene loading, as this will serve to handle the vaste majority of item replacement.