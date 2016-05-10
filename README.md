# if-i-ran-tf2

Updates and fixes I would make to Team Fortress 2, were I on the TF2 Team.

## Development Best Practices

Changes I'd make to the development pipeline of the TF2 team going forward:

- Run a [Matrix-Driven Failure Mode & Effect Analysis][MD-FMEA] when adding/changing mechanics and features, to catch exploits, bugs, and inconsistencies before they're released in the wild.
- Open up a public TF2 issue tracker, and post open items like art assets that need to be rebuilt, so that the community can provide resources and alleviate the burden on the dev team. (Doing this on GitHub might not be a terrible idea.)

[MD-FMEA]: https://www.kickstarter.com/projects/tiko3d/tiko-the-unibody-3d-printer/posts/1572161

## Loadouts

- Allow users to wear as many non-conflicting cosmetics as possible.
  - Note that this was written before many hats were changed to be non-conflicting: there'd probably be some thought given here to stop people from sticking eight sets of ears and horns out of their helmets simultaneously.
- Replace the 4 complete-loadout sets with a user-fillable list of nameable loadout "layers", defining a set of items to switch to together, while not changing items in other slots.
  - This list is toggleable with a list of currently-equipped items, sorted by slot, which replaces the current slots.
- Add [a mechanism for applying these loadouts automatically](https://github.com/stuartpb/tf2-loadrules).

## Items

- Add descriptions to the stock weapons, mirroring the descriptions of their unlockable variants.
- Replace the "Backpack" with "Cargo" and "Lockers".
  - New items are dropped into "Cargo".
  - Only items in "Lockers" are displayed when switching loadout.
  - The default dismissal button when receiving a new, non-duplicate item is "Put In Lockers".
  - Lockers are sorted as a list, per-class, per-slot.
  - Stock items are unremovable from "Lockers".
- Add stock cosmetic items representing the components of classes' default appearance (ie. Hard Hat for Engineer, Rifle Holster for Sniper, Dud Bandoliers for Demoman, No Badge item for all classes, etc).
  - This is primarily for "unequipping" cosmetics in the overhauled loadout model described above.
  - Name/description tags *can* be applied to items that only clear cosmetic slots, just as they can be applied to the non-stock cosmetic items that remove the default hats.
- Explain that using name/description tags on a stock item will *duplicate* that item, and for weapons, are probably not preferable to naming a strange and/or cosmetic variant of the weapon.

## Class Selection, Loadout Selection, and Respawn

- Display class counts as the number of players as that class "+ You".
- Merge the Class Selection screen and the Loadout Selection screen, with the descriptions of the class's loadout replacing the descriptions on the chalkboard, and special class-specific attributes merged into the On Wearer description.
  - Note that this entails descriptions on stock items as described above.

## Technical improvements

- Overhaul the UI code:
  - While loading, poll the event loop every frame, not just when updating the UI (so clicking the "Cancel" button during map load wouldn't require waiting a minute for the click to register).

## Voice command interface

- Holding a voice menu button brings up a radial menu, with icons, like Left 4 Dead.
- "Ubercharge ready!" is dimmed when playing classes other than Medic.

## Disguising

- Introduce an option (applied by default for new players) for a new disguise menu class order, arranged by how likely Spies are to select the disguise based speed and utility (instead of "Attack", "Defense", and "Support", the order is "Safe", "Tricky", and "Slow"):

  - 1: Pyro
  - 2: Engineer
  - 3: Sniper
  - 4: Spy (this makes double-tapping "4" disguise as a spy on the other team)
  - 5: Medic
  - 6: Scout
  - 7: Demoman
  - 8: Soldier
  - 9: Heavy
  
  This makes twitch re-disguising **way** more practical. Also, this would stack with the 3-button disguise menu (if the player *really* wants to trade harder access to Safe disguises for easier access to Slow disguises), so the key sequence for disguising as a Demoman with that option turned on would be 4 (Disguise), 3 (Slow Classes), 1 (Demoman).

- Change the "Last/Random Disguise" key to only randomly select Pyro, Engineer, Sniper or Spy, so it never penalizes movement speed or disguise effectiveness based on movement speed.
- Allow Spies to change their Last Used Disguise while waiting to respawn.
- (slight balance impact) Allow Spies to reload their current disguised weapon at any time, whether their real active weapon can be reloaded or not. (Pressing "R" begins the reload animation equivalent to a full clip.)

## Overlay while healing

- Display the type of Ubercharge the current medigun has before the percentage of charge (rather than "Ubercharge" and the name of the medigun):
  - Ubercharge
  - Kritzcharge
  - Buffencharge
  - Immunization
- Use the above terms for canisters in Mann Vs. Machine.
- Change Vaccinator charge percentage to go from 0 to 400% rather than 100%.

## Community contributions and Minor Cosmetic Fixes

- Allow an "Other" category in the Workshop for *any* kind of contribution, with a CLA stating that the user provides their contribution for use by Valve with no required compensation (although the user may be given a hat as thanks).
- Integrate the first community-contributed consistent kill-icon set replacement.
- Integrate the first community-contributed replacement for the static class portraits (used on scoreboards, and formerly/optionally used in the corner of the screen) based on their pre-release models/textures.
- Integrate the first community-contributed replacement model for the Demoman's Grenade Launcher having 4 chambers instead of 6.

## Weapon Spread

- Use a [Fermat's Spiral](https://en.wikipedia.org/wiki/Fermat%27s_spiral)-based algorithm for orienting hitscan rays instead of [a 3x3 grid with extra pellets on the right](http://gaming.stackexchange.com/questions/3028/what-is-weapon-spread-as-it-relates-to-team-fortress-2).
  - This assumes that "random" positioning, which is virtually universally unused now that fixed spreads have been made the default, is removed altogether.

## Scoreboard

- Replace the class image in the corner (which is redundant with the one used during gameplay) with the current map's thumbnail.
- Display the next map in the corner of the scoreboard (when set), next to the time remaining before changing to it.
  - Server extensions should be able to override this with "Next Map Pending Vote".

## Auto-balance

- Add a convar / new convar values for team rebalancing, where after the first member of the imbalanced team dies, all other members of that team who dies before the player respawns are taken into consideration, and the member of the team to be rebalanced is selected when the first player to die respawns, based on:
  - Player closest to own/other team's median/mean score (4 different settings)
    - This mean/median should be frozen when the player dies, so as to prevent candidates going from most likely, to not most likely, to most likely again as the scoring composition changes 
  - Lowest-scoring player (for least effect on team composition)
  - Highest-scoring player (for maximum tide-turning)
  - The "You have been switched!" dialog would then be "You may be switched in X seconds" in the time before the switch, displayed in the *current team's* color, to anybody who is made a candidate - if another candidate replaces them, the dialog text is changed to "<player> will be switched in X seconds". (The "You have been switched" dialog, in the *new* team's color, is then only displayed for a period of time *after the switch*.)
