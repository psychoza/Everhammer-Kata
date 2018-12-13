# Everhammer-Kata
A kata based on Warhammer instead of D&amp;D much like the Evercraft kata.   [https://github.com/PuttingTheDnDInTDD/EverCraft-Kata](https://github.com/PuttingTheDnDInTDD/EverCraft-Kata)

## Iteration 1 - Core Values

### Feature: Create a Character
  As a character, I want to have a name so I can distinguish myself from other characters
  * Get / Set name

### Feature: Attributes
  As a character, I want attributes to determine the things I am and am not good at
  * Attributes are Weapon Skill, Strength, Toughness, Willpower
  * They are defaulted to 10
  * They can be randomized by rolling 2 10 sided dice and adding their results
  * Attributes have a bonus which is the number in the tens place
    * ie a Strength of 17 has a Strength Bonus of +1

### Feature: Species
  As a character, I want the ability to be a human
  * As a human, they get a +20 bonus to all attribute scores.

### Feature: Wounds
  As a character, I want the ability to determine how much damage I can take
  * Your Wounds "attribute" is determined by Strength Bonus + (2x Toughness Bonus) + Willpower Bonus
  * Wounds can be reduced and once Wounds reaches 0, a character becomes Prone (discussed later?)
  * If you do not regain any Wounds within a number of Rounds = to your Toughness Bonus, you will pass out, gaining the Unconcious condition
  * Wounds can never be below 0

### Feature: Damage
  As a character, I want the ability to take damage and absorb hits
  * When taking damage, a character reduces the incoming damage by their Toughness Bonus
    * This can only reduce the damage to 1, it cannot negate it completely
  * Damage should never be tracked as more than the wounds you could take
  * Evenutally add hit locations...

### Feature: Critical Wounds
  As a character, I want the ability to suffer truly debilitating effects
  * When at 0 Wounds or taking more damage than Wounds you have left, you suffer a Critical Wounds
  * If you suffer fewer negative wounds than your Toughness Bonus (-4 wounds compared to 4 Toughness Bonus), you subtract -20 from the Critical Table result with a minimum result of 01.
  * Eventually add hit locations and critical wounds tables...
  
### Feature: Death
  As a character, I want the ability to be declared declared
  * When a character has the Unconcious condition, and wounds are equal to 0, then compare the number of Critical Wounds to your Toughness Bonus.
    * If you have more wounds than the toughness bonus, you are pronounced dead at the end of the round.

### Feature: The melee skill
  As a character, I want the ability to have knowledge in melee combat to determine my skill's level
  * The melee skill uses the Weapon Skill Score as its base number then adds "advances" from training to determine the skill's level.
  * 41 Weapon Skill + 5 Melee(Basic) = 46 Skill Level)

### Feature: Attack (Melee Skill Test)
  As a character, I want the ability to attack another character.
  * Roll 2 10 sided dice, one being the tens place, and one being the units place
  * The dice values are ranged between 0 - 9
  * Subtract the rolled result from your Melee Skill
  * Determine your attack's Success Level like attribute bonuses (46 Melee Skill Level - 34 Attack Roll = 12,  Math.Floor(12 / 10) = +1 SL)
  
### Feature: Deal damage
  As a character, I want the ability to land a hit on another character unopposed
  * Target an enemy character and deal damage equal to the success level of your Melee Skill Test + your Strength Bonus

### Feature: Fend off attacks
  As a character, I want the ability to fend off the attack when I am attacked
  * Make a Melee Skill Test as the previous Feature when attacked and return the success level to the attacker
  * Compare the attacker's Success Level versus the Defender's Success Level negate any damage if the defender is the higher
  * The defender takes no damage if the levels are the same

### Feature: Advantage
  As a character I want the ability to press my advantage in combat
  * After succeeding in an Opposed Melee Skill Test with an opponent, gain a point of Advantage
  * When determining future Melee Skill Tests with the same opponent, add a +10 to your Melee Skill Level for each point of Advantage
  * When your Melee Skill Test's Success Level is the same as your opponents, do not gain any advantage
  * When taking damage or leaving combat, lose all Advantage
  * When taking no damage but gaining no Advntage in a round of combat, lose 1 point of Advantage
  
### Feature: Critical Hit
  As a character, I want the ability to score a major hit on another character.
  * When rolling the 2 10 sided dice in an attack, if they are both the same and the attack was successful, the opponent suffers a critical wound

### Feature: Hit Location
  As a character, I want the ability to hit my opponent in specific locations
  * When you roll your opposed melee skill test and win, reverse the roll result for a hit location number
    * You rolled a 27 in your melee skill test, it translates to 72 for a hit location, in this case, the body.
  * 01-09 | Head
  * 10-24 | Left Arm
  * 25-44 | Right Arm
  * 45-79 | Body
  * 80-89 | Left Leg
  * 90-00 | Right Leg

## Iteration 2 - Species

### Feature: Species: Dwarf
  As a character, I want to be a rough and gruff Dwarf
  * My Weapon Skill and Toughness should improve to +30 instead of the human's +20
  * My Willpower should improve to +40 instead of the human's +20
  
### Feature: Species: Elf
  As a character, I want to be an agile Elf
  * My Weapon Skill and Willpower should improve to +30 instead of the human's +20

### Feature: Species: Halfling
  As a character, I want to be a small Halfling
  * My Weapon Skill and Strength should decrease to +10 instead of the human's +20
  * My Willpower should improve to +30 instead of the human's +20
  * The formula for my Wounds should be modified to omit a Strength Bonus
    * (2x Toughness Bonus) + Willpower Bonus

## Iteration 3 - Armor

### Feature: Skullcap
  As a character, I want the ability to wear a leather skullcap and protect my noggin
  * The skullcap can only be worn on my head.
  * It has an Armour Point value of 1
  * Armor Points can be reduced to 0 by being hit and becoming useless

### Feature: Mail Coat
  As a character, I want the ability to wear a mail coat and protect my body and arms
  * The mail coat can only be worn on body and both arms
  * It has an Armour Point value of 2
  * When tracking Armor Points, each individual location is tracked
    * Left arm can be reduced without reducing the protection for body and right arm

### Feature: Plate Leggings
  As a character, I want the ability to wear plate leggings to protect both of my legs
  * The plate leggings can only be worn on both legs
  * It has an Armour Point value of 2
  * When tracking Armor Points, each individual location is tracked
    * Left leg is separate from right leg
    
### Feature: Damage reduction
  As a character, now that I am protected, damage should be reduced
  * When taking damage from an attack in a location protected by armor, reduce that damage by the Armor Points in that location
  * Damage can be reduced to 0
  * When receiving a critical wound in a location, instead deal a point of damage to the armor in that location
  * If the armor is at 0 armor points, deal the critical wound as normal

## Iteration 4 - Weapons

### Feature: Hand Weapon (Sword)
  As a character, I want ability to wield a sword and slash my opponents to death
  * The sword has a damage bonus of 4
  * When dealing damage to an opponent, this bonus gets applied in addition to all previous rules
  * The sword can take damage and reduce its bonus by that amount
  * The sword will never deal less than +0.
  * The first time the sword reaches +0 it becomes an improvised weapon

## Feature: Improvised Weapon
  As a character I want the ability to continue using my weapon despite it being mangled byond all recognition
  * The weapon has a damage bonus of 1
  * The weapon has the undamagaing flaw
    * When hitting an opponent in a location that is covered in armor, that armor's armor points are doubled.
  * When the weapon takes damage it is broken and useless as a weapon 
  
## Feature: Unarmed
  As a character I want the ability to always be able to fight
  * When no weapons are equipped, you are equipped with the unarmed weapon
  * The damage bonus of unarmed is +0
  * The weapon cannot take damage and is not considered improvised
  * The weapon does have the undamaging flaw
  
## Feature: TBD
  As a character I want ...
  * ...
