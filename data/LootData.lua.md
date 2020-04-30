v0.27191
## Overview
Lootdata mainly control the top level data involved in the boons, and hammer upgrades in the game. It does NOT control the damage and mechanics of the upgrade, mainly the file defines loot as an interactible item within the game. When you pick up a boon, pom, or hammer, this data is used. If you are interested in the trait stats themselves, check the **TraitData.lua** file.

Every loot table in LootData.lua inherits from these 2 base classes:
```
BaseSoundPackage =
	{
		SelectionSound = "/SFX/Menu Sounds/GeneralWhooshMENU",
		ConfirmSound = "/SFX/Menu Sounds/GodBoonChoiceConfirm",
		DebugOnly = true,
	},

	BaseLoot =
	{
		UsePromptOffsetX = 75,
		UsePromptOffsetY = 44,
		GodLoot = true,
		DebugOnly = true,
	},
```
These 2 structures set music cues, interactible prompts, and set loot as default to being from a god.
Afterwords, each good gets a table defined `God`Upgrade. Using zeus as the example
## Basics
```
ZeusUpgrade =
	{
		InheritFrom = { "BaseLoot", "BaseSoundPackage" },
		CanReceiveGift = true,
		AlwaysShowDefaultUseText = true,
		Weight = 10,
		Icon = "BoonSymbolBase",
		DoorIcon = "BoonSymbolBaseIsometric",
		Color = { 250, 250, 215, 255 },
		LightingColor = {235, 206, 87, 255},
		LootColor = {255, 255, 64, 255},
		SubtitleColor = {1.000, 0.973, 0.733, 1.0},
		ColorGrade = "ZeusLightning",
		EventEndSound = "/SFX/ZeusBoonThunder",
		UpgradeSelectedSound = "/SFX/Player Sounds/ZeusLightningWrathImpact",

		RequiredMinCompletedRuns = 1,

		PriorityUpgrades = { "ZeusWeaponTrait", "ZeusSecondaryTrait", "ZeusRushTrait", "ZeusRangedTrait" },
		WeaponUpgrades = { "ZeusWeaponTrait", "ZeusRushTrait", "ZeusRangedTrait", "ZeusSecondaryTrait","ZeusShoutTrait" },
		Traits = { "RetaliateWeaponTrait", "SuperGenerationTrait", "OnWrathDamageBuffTrait" },
		Consumables = { },
```
`Icon` and `DoorIcon` are the graphics defined in the **GUIAnimations.sjson** package, which points to the location in the GUI content. `Color` controls general color palette for the boon. `LightingColor` controls the lighting color on the upgrade menu screen over zagreus. `LootColor` controls the lighting of the boon itself, and any other times the bloom-like effect might be used. `SubtitleColor` is for text subtitles for that god character. `EventEndSound` plays when the boon is picked up. `UpgradeSelectedSound` plays when the upgrade is selected.

`RequiredMinCompletedRuns` is the requirement needed for this boon to appear: in this case, you need 1 completed run to be done.

`PriorityUpgrades` are upgrades that must appear FIRST in a RUN. Zagreus must have one priority upgrade from any boon before others can be considered.

`WeaponUpgrades` are the primary weapon boon definitions: 1 each for weapon, dash, cast, special, and call. In this case, Rush=Dash, Ranged=Cast, and Shout=Call.

`Traits` are general utility traits that don't attach to a weapon. In this case, zeus' retailiate, call generation buff, and call damage buff boon options.

## LinkedUpgrades

```
ZeusUpgrade =
{
...
LinkedUpgrades =
		{
			ZeusBonusBounceTrait =
			{
				OneOf = { "ZeusWeaponTrait", "ZeusRangedTrait"},
			},
			ZeusLightningDebuff =
			{
				PriorityChance = 0.5,
				OneOf = { "ZeusSecondaryTrait", "ZeusShoutTrait", "ZeusRushTrait", "RetaliateWeaponTrait", "ZeusWeaponTrait", "ZeusRangedTrait" },
			},
      ...
			LightningCloudTrait =
			{
				OneFromEachSet =
				{
					{ "ZeusWeaponTrait", "ZeusSecondaryTrait", "ZeusRangedTrait", "ZeusRushTrait", "ZeusShoutTrait" },
					{ "DionysusRangedTrait", "DionysusDefenseTrait" },
				}
			},
```
`LinkedUpgrades` are upgrades that can only be obtained once certain conditions are met. For instance, the boon that gives lightning bolts more bounces requires you to have the weapon or ranged upgrade too, as those are the ones that can bounce. This is also where the Duo Boon requirements live too. We can see in the example that the Dionysis/Zeus duo boon requires a zues weapon trait, and a dionysis ranged trait, or his defense trait

## More presentation information
```
ZeusUpgrade =
{
...
		Speaker = "NPC_Zeus_01",
		Portrait = "Portrait_Zeus_Default_01",
		WrathPortrait = "Portrait_Zeus_Wrath_01",
		Gender = "Male",
		SpawnSound = "/SFX/ZeusBoonThunder",
		FlavorTextIds =
		{
			"ZeusUpgrade_FlavorText01",
			"ZeusUpgrade_FlavorText02",
			"ZeusUpgrade_FlavorText03",
		},
```
These values set some boon specific information, such as the portrait of the speaker, and a flavortext subtitle lint that can appear on the upgrade select screen zeus has a unique spawn sound as well.
## God Voice Line information
The next fields are voiceline structures. See voiceline.md for more information for how these are structured. TODO: Create voiceline.md
* ```DuoPickupTextLineSets``` define voicelines that play when a duo boon becomes available.
* ```PriorityPickupTextLineSets``` define voicelines that are contextual in nature and should be played before generic lines. ```PickupTextLineSets``` are generic fallback lines. ```BoughtTextlines``` are voicelines to play when the bought from the charon shop. ```RejectionTextLines``` are for when you spurn a god in the devotion test challenge room. ```RejectionVoiceLines``` are Zagerus follow up text lines. ```MakeupTextLines``` are for when the devotion challenge is completed. ```GiftTextLineSets``` are for when the character is given a nectar/gift. ```GiftGivenVoiceLines``` are for follow up
