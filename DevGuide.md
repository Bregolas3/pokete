# Development guide for Pokete
This guide only handles adding new types, attacks and Poketes to Pokete. For further information on class and function syntax, see the HTML documentation or `$ pydoc <file>`.

1. [Adding Poketes](#adding-poketes)
2. [Adding attacks](#adding-attacks)
3. [Adding types](#adding-types)
4. [Adding subtypes](#adding-subtypes)
5. [Adding maps](#adding-maps)

## Adding Poketes
Every Pokete has an entry in the `pokes` dictionary in [`pokete_data/poketes.py`](./pokete_data/poketes.py). Every entry needs to have this structure:

```Python
"steini": {  # This is the Poketes simplified name/identifier without spaces and in lowercase, which is used to refer to the Pokete in the code
        "name": "Steini",  # This is the Pokete's pretty name
        "hp": 25,  # The Pokete's max health points
        "atc": 2,  # The Pokete's attack points that will added to the Pokete's level
        "defense": 4,  # The Pokete's defense points that will added to the Pokete's level
        "attacks": ["tackle", "politure", "brick_throw"],  # The Pokete's starting attacks
        "pool": [],  # List of additional ungeneric attacks the Pokete can learn
        "miss_chance": 0,  # The chance a Pokete will miss an attack, this is added to the attacks individual `miss_chance`
        "desc": "A squared stone that can casually be found on the ground.",  # The Pokete's description
        "lose_xp": 3,  # The amount of experience the player gets by killing the Pokete
        "rarity": 1,  # Rarity
        "types": ["stone", "normal"],  # The Pokete's types
        "evolve_poke": "",  # The name/identifier of the Pokete, that this Pokete evolves to at a certain level
        "evolve_lvl": 0,  # The level the Pokete evolves at
        "initiative": 5,  # The Pokete's initiative points that will added to the Pokete's level, and determine what Pokete starts in a fight
        "ico": [{  # A list of dictionaries containing a displayed string and a color; all those strings will be layered over each other and represent the Pokete in the fight
            "txt": """ +-------+
 | o   o |
 |  www  |
 +-------+ """,
            "esc": None}],
    },
```

For more examples on the dictionaries see [`pokete_data/poketes.py`](./pokete_data/poketes.py).

### Types
Only the first type in the `"types"` list is the Poketes actual displayed type that determines the Pokete's effectivity. The other types in the list are just secondary types that determine what generic attacks a Pokete is able to learn. Those can also be subtypes. The main type can't!
For a list of all types and subtypes see [`pokete_data/types.py`](./pokete_data/types.py)

### Learning attacks
In addition the attacks in the `"attacks"` and the `"pool"` lists, a Pokete can learn all generic attacks of the Poketes types.


## Adding attacks
Every attack has an entry in the `attacks` dictionary in [`pokete_data/attacks.py `](./pokete_data/attacks.py). Every entry needs to have this structure:

```Python
"poison_bite": {  # This is the attack's simplified name/identifier without spaces and in lowercase, which is used to refer to the attack in the code
        "name": "Poison bite",  # The attack's pretty name
        "factor": 1,  # The attack factor that is used to calculate damage
        "action": "",  # A string that's executed when the attack is used, to effect the Pokete's or the enemy's values (don't use this)
        "world_action": "",  # An extra ability the attack can be used for
        "move": ["attack", "downgrade"],  # The moves the Pokete does using the attack
        "miss_chance": 0.3,  # The chance to miss the attack
        "min_lvl": 0,  # The minimal level a Pokete needs in order to learn the attack
        "desc": "Makes the enemy weaker.",  # The attack's description
        "types": ["poison"],  # The attack's types
        "effect": "poison",  # The effect the enemy gets when the attack is used, default is None
        "is_generic": False,  # Whether or not the attack can be learned by any Pokete of its types
        "ap": 10,  # Attack points; the amount of times the attack can be used by a Pokete until the Pokete has to be healed
    },
```

For more examples on the dictionaries see [`pokete_data/attacks.py`](./pokete_data/attacks.py).

### Types and learning
Only the first type in the `"types"` list is the attack's real type and determines its effectivity. The other type can only be subtypes. A generic attack can only be learned by a Pokete that has all its types. An ungeneric attack can only be learned if the attack is in the Pokete's `"pool"` or `"attacks"` list.

### Effects
The effect given in the `"effect"` attribute has to be the `c_name` of an effect listed in [`pokete_classes/types.py`](./pokete_classes/types.py ) or `None`.

### World action
An attacks `"world_action"` is some kind of extra ability which can be called from the Poketes detail view and be used to, for example, make the player fly. The string in `"world_action"` has to be a key in the `abb_funcs` list in [`pokete.py`](./pokete.py).


## Adding types
Every type has an entry inside the `types` dictionary in [`pokete_data/types.py`](./pokete_data/types.py). Every entry needs to have this structure:

```Python
"stone": {  # The type's name
        "effective": ["flying", "fire"],  # The types the type is effective against
        "ineffective": ["plant"],  # The types the type is ineffective against
        "color": ["grey"]  # The type's label color
    },
```

## Adding subtypes
Every subtype has an entry inside the `sub_types` list in [`pokete_data/types.py`](./pokete_data/types.py). They have no further attributes. Subtypes are only useful to determine what Poketes can use what generic attack in a type. For example, to avoid that `bato` learns the generic `flying` attack `pick`.

## Adding maps
Every map has an entry in the `maps` list in `pokete_data/maps.py`. Every map needs to have this structure:
```Python
"intromap": {
        "height": 15,
        "width": 30,
        "song": "03 Chibi Ninja.mp3",
        "pretty_name": "Your home",
        "extra_actions": None,
        "poke_args": None,
        "weather": None
    },
```
When creating maps you can use any of the provided map data which is found in `pokete_data/map_data.py`. You will have to create your map data and name it the same name as your map. Your map data should be created in this format:
```Python
     "playmap_1": {
         "hard_obs": {
         },
         "soft_obs": {
         },
         "dors": {
         },
         "balls": {
         }
     },
```
There are given buildings you can add such as:
```Python
CENTER = r""" __________
             /         /\
            /_Center!_/  \
            | # ___ # |  |
            |___| |___|__|"""

SHOP = r"""   __________
             /         /\
            /__Shop!__/  \
            | # ___ # |  |
            |___| |___|__|"""

HOUSE1 = r""" ________
             /       /\
            /_______/  \
            |# ___ #|  |
            |__| |__|__|"""
```
You would add them by adding this to the map:
```Python
            "pokecenter": {
                "x": 20,
                "y": 0,
                "txt": CENTER
            },
            "house1": {
                "x": 2,
                "y": 0,
                "txt": HOUSE1
            },
            "shop": {
                "x": 29,
                "y": 3,
                "txt": SHOP
            },
```
You can also add you own buildings if you want like this:
```Python
"hotel": {
                "x": 43,
                "y": 18,
                "txt": r"""    ..___________
                              /||          /\
                             / ||         /  \
                            /___Hotel!___/    \
                            | ##      ## | ## |
                            |    ____    |    |
                            |____|  |____|____|"""
            },
```
You should also add terrain such as:
```Python
"hard_obs": {
            "tree_group_1": {
                "x": 35,
                "y": 2,
                "txt": """ (()(()((())((()((()
())(())))())))()))(()
 || ||| ||||| |||||"""
            },
    }
```
You can also add soft obstacles where you can find poketes such as:
```Python
"soft_obs": {
            "meadow1": {
                "x": 1,
                "y": 8,
                "txt": """     ;;;;;;;
                            ;;;;;;;;;;;;
                         ;;;;;;;;;;;;;;;;
                        ;;;;;;;;;;;;;;;;;;;
                         ;;;;;;;;;;;;;;;;;
                           ;;;;;;;;;;;;;
                              ;;;;;;    """
            },
    }
```
If you want to add a door to a building you do that with the `dors:` portion of the dict such as:
```Python
"dors": {
            "dor_playmap_51_1": {
                "x": 90,
                "y": 12,
                "args": {"map": "playmap_51", "x": 1, "y": 28}
            },
        }
```
If you're adding doors to buildings you should also include a map of the inside of the building such as:
```Python
"playmap_22": {
        "hard_obs": {
            "inner_walls": {
                "x": 2,
                "y": 1,
                "txt": """________________________
                          |______________________|
                          |          a           |
                          |       ________       |
                          |                      |
                          |                      | |
                          |                        |
                          |                      __|
                          |__________  __________|
                          |_________|  |_________|"""
            },
        },
}
```
To add balls to pickup throughout the map you add them like this:
```Python
"balls": {
            "ball_1": {
                "x": 1,
                "y": 1
            },
            "ball_2": {
                "x": 7,
                "y": 12
            },
        }
```
