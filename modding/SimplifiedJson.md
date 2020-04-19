v0.27191

# .sjson files: simplified json
Throughout the main Contents section of hades, you'll be dealing with .json fiels, or simplified json files.
These files are pure data files that configure various objects in the game. JSON is a very intuitive syntax to read,
and simplified json makes it even more intuitive.

## Keys and values

Every sjson file is a collection of keys and values. As an example consider this file as simplehero.sjson:
```
{
  heroHealth: 100
  heroName: "Zagreus"
  heroGraphic: "fx/Hero"
}
```

This sjson has 3 keys, or properties: `heroHealth` `heroName` and `heroGraphic`. They've been assigned the values of `100`,
`"Zagreus"` and '"fx/Hero"` respectively. If you loaded this sjson into a variable such as `heroData`, you could access the hero's
health with the call `heroData.heroHealth` and expect a value return of 100.

Values themselves are generally either integers, floats, booleans (true or fals), or Strings. Strings require the `""` quote marks
around them to wrap them. If you are familiar with normal json, you'll notice that we didn't have to seperate the properties by commas.

## Lists in sjson
Let's add to our hero file to give him a set of traits/boons
```

{
  heroHealth: 100
  heroName: "Zagreus"
  heroGraphic: "fx/Hero"
  heroTraits: [
      "ZeusWeaponTrait"
      "AresSecondaryTrait"
      "ArtemisRushTrait"
  ]
}
```
the `[]` block means that instead of a single value, the value is instead a list. In this case, our hero has 3 boons with him.
To access the first trait, we would use the syntax `heroData.heroTraits[0]`
It is recommended to tab over a block of data within a key to show that these elements are inside with each other.

## Objects/Maps in sjson

The `{}` block means that instead of a single value, you can instead do a mapping of value, or an object-like data structure.
If you noticed, we were allready using the `{}` block as the starting and ending characters of the file. That means that you
were allready working with objects in sjson!. Let's look at a real world example from **Content/Game/Units/NPCs.sjson**
```
{
  Name = "BaseNPC"
  AutoLockable = false
  DisplayInEditor = false
  Life = 
  {
    HitFx = "HitSparkB"
    HomingEligible = false
    Invulnerable = true
    JumpTargetEligible = false
  }
  Thing = 
  {
    CanBeOccluded = true
    EditorOutlineDrawBounds = false
    Graphic = "NPCFemaleThinking"
    Grip = 99999.0
    SelectionHeight = 240.0
    SelectionShiftY = -30.0
    SelectionWidth = 180.0
    SpeechCapable = true
    UseBoundsForSortDrawArea = true
    Interact = 
    {
      Cooldown = 2.0
      Distance = 250.0
      OffsetY = -50.0
    }
    Points = [
      { X = 0 Y = 32 }
      { X = 64 Y = 0 }
      { X = 0 Y = -32 }
      { X = -64 Y = 0 }
    ]
  }
}
```

This is the baseNPC definition currently in the game. It has 5 top level fields: `Name`, `AutoLockable`, `DisplayInEditor`,
`Life` and `Thing`. The `Life` and `Thing` fields are actual objects within themselves to. For example, if we loaded this
file as baseNPCData, then we could find if the unit can take damage by accessing the `baseNPCData.Life.Invulnerable` field.
Likewise, if we wanted to know if the NPC could be talked to, we can check the `baseNPCData.thing.SpeechCapable` value and
see that it's set to true.

Finally as a complicated example check the `BaseNPC.Thing.Points` structure:
```
Points = [
  { X = 0 Y = 32 }
  { X = 64 Y = 0 }
  { X = 0 Y = -32 }
  { X = -64 Y = 0 }
]
```
This is actually a list of structures, in this case; points that define the bounding hitbox for the NPC! It makes a rectangle
from -64 to 64 in the x axis, and -32 to 32 in the y axis

## Advanced Note: InheritsFrom

Most sjson data structures within the game's files also have an `InheritFrom` field. Such as this Hypnos NPC definition:
```
{
      Name = "NPC_Hypnos_01"
      InheritFrom = "BaseNPC"
      DisplayInEditor = true
      Thing = 
      {
        AttachedAnim = "NPCShadow"
        EditorOutlineDrawBounds = false
        Graphic = "HypnosAwakeLoop"
        OffsetZ = 30.0
        Tallness = 150.0
        Points = [
          { X = 9 Y = 20 }
          { X = 41 Y = 3 }
          { X = 10 Y = -12 }
          { X = -22 Y = 4 }
        ]
        SubtitleColor = { Red = 0.44 Green = 0.50 Blue = 0.83 }
        Using = [
          { Name = "HypnosIdle" }
        ]
      }
    }
 ```
 
 
## More resources:
This is the first project I've seen that uses sjson, but it seems remarkably similar to json, so any json tutorial will do.
I recommend [Mozilla Developer's Network example for the web.](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON)
