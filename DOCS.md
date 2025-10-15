## How things work

- Created an instance of the new Root Template PTM_TV_TavernGuard.
- The tavern guard instance has the new PTM_TavernKey in their inventory
- The tavern guard has a dialog that allows getting the key via a deception check
  - 2 flags can be set from this dialog
  - PTM_TV_TavernGuard_HasMet - set at the start of the dialog
  - PTM_TV_TavernGuard_GaveKey - set if a deception check is passed
- PTM_TV_TavernGuard_GaveKey triggers a script that creates a PTM_TavernKey in the Player's inventory
- There is also a PTM_TV_BackdoorButton instance on the side of the building
- This button is hidden behind a DC 10 perception check
- If (found and) click it will open the side door of the tavern

## Nits and things I would improve

- The dialog cinematic is the base one, I would want to play around with the timeline editor to make it a bit better
- The get key script (PTM_TV_TavernGuard_GetKeyDialog) script should maybe give the key from the guard instead of creating a new one
  - Debatable since you may want the guard to be able to get in later
  - Did end up with a few extra copies of the keys from testing
- Probably can do better on following naming conventions. I went with PTM \_TV\_ where TV is the tavern level but didn't do it consistently
  - Also little things like referring to the tavern side door as "back door" everywhere

## Dev Log

This is a rough break down of the commits (commits in order chronological order):

1. First commit, familiarizing myself with the tools. Created a new Root Template for the Tavern guard character
   - Cloned from BASE_Dwarves_Male_Hill_Guard
   - Created a new stats entry and gave him custom stats
   - Placed the character in the level
2. Mostly visual changes, the base dwarf model is naked. Added some clothes to the character
3. Missed a clothes change because I didn't save
4. Created a new Root Template, cloned from another button.
   - This will be the hidden button that can be found using Perception
   - It will open the back (technically side) door of the tavern
5. Add a small scrip that links up the button and the back door
   - Script: PTM_Open_Back_Door
   - Needed to set door default state to "closed"
   - Script just checks door state and procs lock or unlock on it
   - Door key id set to "NOKEY" to make it work
6. Added a perception check script (DOS2_PUZZLE_HiddenPerception) to the button
   - This just mostly worked as intended
   - Set DC to 10
7. Started working on the guard dialog
   - Created a basic dialog tree to get it set up
   - Added the dialog to the tavern guard character
   - Generated timeline for the dialog using basic template
8. Expanded on the dialog tree
   - Added branching paths
   - Added a deception check in the dialog to see if you can get the key this way
   - No flags implemented at this time
9. Cloned a basic key to create a new Root Template for the tavern key
   - Placed the key item in the guard's inventory
10. Updated some of the dialog
    - Changed roll difficulty
11. Added 2 flags for the tavern guard dialog
    - PTM_TV_TavernGuard_HasMet - Set when starting the dialog with the guard
    - PTM_TV_TavernGuard_GaveKey - Set if you've successfully passed the deception check
    - Dialog tree updated to not be repeatable, if either flags are set a different dialog will be triggered (depending on which flags are set)
12. Missed author comment in previous script
13. Added script that gives the player the tavern key on successful deception check
    - Script: PTM_TV_TavernGuard_GetKeyDialog
    - checks for the flag set event for `PTM_TV_TavernGuard_GaveKey`
    - Use `TemplateAddTo` to create a new instance of the key. Guard retains his copy if you want to pickpocket it
14. Add this doc
15. Fix front tavern door not properly playing the opening animation
