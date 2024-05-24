# Polymorph Dice

Simulate dice rolls for the [polymorph](https://9thlevel.com/pages/polymorph) RPG platform with a built-in success chart.

[![built with polymorph](src/lib/assets/polymorph.avif)](https://9thlevel.com/pages/polymorph)

See the [polymorph website](https://9thlevel.com/pages/polymorph) and rulebooks for additional details, but for using this app:

Each player gets assigned a specific die (d4, d6, d8, or d10) based on their "role".

When performing an action, click "Roll D?" to roll the die corresponding with a player's "role". If the value on the die is in the number range for the desired type of "Outcome" it is a success and will be highlighted in green. Failed "Outcome"s are greyed out.

 If the player has an "Advantage" or "Disadvantage" for the current circumstances, clicking that button will add a second die roll. For "Advantage", "Outcome"s matching either die are successful. For "Disadvantage", both dice have to match for success.

## Support
This was a quick project to get familiar with *polymorph* and to test out Svelte 5. I probably won't be spending much more time on this, but wanted to go ahead and publish it in case anyone else finds it useful.
