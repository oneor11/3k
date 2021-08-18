# 3k

The files, but hopefully they can serve as some inspiration. Provide feedback. Note I modified bot_main (not included here), to call stepCharPostKillChecks in guru3k.tin. I probably made other edits to bot_main too.


## File List
- **botmain**:  Some minor edits have been made to botmain.  One is to call the kill trigger in the char file, which in my case is stopPostKillCharChecks or something like that. There's also some usage of the mage spell slow. Ideally the kill stuff could be replaced with a "initiatecombat" alias or something that any char file can call and process the logic there. 
- **guru3k/guru3k.tin**: This is the main character file for Guru's mage gobj.  It basically captures the first and second lines of the hpbar and uses its values to make decisions about healing in combat and post-kill decisions when using a stepper.  Note that the hpbar regex does not match a standard mage hpbar. I configured it using the mage command confhp(see hpbar section below).  The *stepCharPostKillChecks* alias contains the post-kill decision-making and is called from bot_main.tin. I believe the official Inix build has it as "corpsetrig".  Some variables in this file are set in other .tin files that are read-in.  **NOTE**: stepCharPostKillChecks will only get called from bot_main, which means it will only get called when a stepper is running. So stuff in there, like displaying damage tracker and resting and getting gems and all that won't happen when manually fighting.   
- **guru3k/mage/defense.tin**: Contains alias to set my contingencies and defenses at boot. Contains action to sound a recurring visible and audible alarm when secure shelter is initiated.
- **guru3k/mage/utility.tin**: Catch-all file for triggers for random things. Toggle spell tap when mystic immersion starts/stops, eat section z capsules, deeppocket management, reforging xp.  It's like the junk drawer in the kitchen.  One day I'll split them out into logical files. 
- **guru3k/comms.tin**: Contains triggers that half-ass capture tells and sounds a recurring visible and audible alarm. Also sends an email containing the person who told me something and part of their message to my mobile phone number which gets translated to a text message.  This helps provide extra visibility to tells when I'm mudding from my phone.  There's also a trigger to respond when my familiar is not present. Obviously that doesn't belong in this file and should go in the /mage/utility.tin file.
- **guru3k/alarm.tin**: Contains aliases to sound a recurring visible and audible alarm with configurable alert messages and frequency. You can use this as part of other triggers for events you want extra visibility to.  Dope af imo.
- **guru3k/eternal.tin**: Contains a trigger for (heal which then sets a ticker for 12 minutes which manages the state of the *hasHeal* variable used in the char file.  This file is where other eternal triggers would go like some cool ewell notification or something that I haven't gotten to yet.
- **guru3k/damagecounter.tin**: Contains aliases and triggers that calculate and display rounds fought, cumulative damage, min damage, max damage, and average damage for a single battle. Requires the numbers VAF.  If you thought alarm.tin was dope af, this is (dope af)^2.
- **guru3k/mage/sptracker.tin**: It's notoriously difficult for a mage to find their base sp regen rate, which makes it more difficult to find the regen impact of a particular piece of gear. This file helps with that. It triggers off of offensive and defensive spells to calculate the total sp cost per round and compare it with the previous round to show regen per round.  If you want to make use of this, you will need to add the offensive and defensive spells you use. I didn't include them all. You'll see the pattern when you look at the file. It basically covers mind warp, armor, mag shield, stoneskin, mantle, prismatic sphere, and major globe.  If you use other spells, then you will need to add the triggers for them to this file. 
- **guru3k/death.tin**: Captures deaths in the chat monitor, but with flavor.  Allows the configuration of different, random death flavors plus the ability to provide special emphasis, such as ansi and a beep if one of the people in your configured list of favorites dies.

## Mage HP Bar Configuration

`
confhp  HP: #$CCH#/#$MHP# SP: #$CCS#/#$MSP#/#$CPC#/#$GEM# Sat: #$CSAT# Cnc: #$CCNC# G/MI: #$NGM#/#$NMI#/#$GPC# ER: #$NSM#/#$SPC# G2N: #$G2N#$NWL# PROT: [#$PRO#] COF:#$COF# #$ST
`

*Note: there are two spaces after confhp*

>HP: 2526/1261 SP: 9944/11962/97%/70% Sat: 78 Cnc: 0 G/MI: 3/2/10% ER: 0/73% G2N: 40%   
PROT: [ A MS SS M PS MG PE MB \<P\> ST] COF:30/30 !!! Mon(Zom):bru

*cast monitor* for the monster status on the end of line 2.
