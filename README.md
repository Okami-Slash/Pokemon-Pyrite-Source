Pokemon-Pyrite-Source
=====================

Code source du jeu Pokémon Pyrite Online
---
This project is open source and open to any and all productive input. 

It is a fan project designed with the hope of creating a free and enjoyable
experience for fans of the games.

All artwork, sounds, and game mechanics are sole property of GameFreak, The
Pokemon Company, and Nintendo.

Pokedex provided graciously by Veekun. All credit goes to him. Check out his
website @ http://veekun.com/ to support him, and also to download the csvs for
our db.

Now that that's out of the way, time for to do list and progress!

TODO:
- Our goal right now is to establish a working battle system that can handle any
  Pokemon with any moves, held items, etc.

PROGRESS:
- Tenative battle system set up successfully, the skeleton of the structure exists, 
  now we need to fill in the details which handle things such as abilities and
  unique move types.
- Database is successfully implemented. Species Array can be created, entering
  the dex number into the array returns base stats, more info such as dex
  entries and stuff still required as of writing this.
- Battle math is complete save for the random modifier which requires more investigation
  to implement properly.
- PokemonFactory class complete. Inserting a species into the class along with
  EV/IV values will provide a pokemon with correct stats.
- Factory classes for moves and species complete and working as well.
  
DATABASE STRUCTURE FOR GENDER RATIOS:
-------------------------------------
Unfortunatley, veekun's pokedex db does not include a table detailing the meaning
behind the values for gender ratios in the 'pokemon' table. The following is what I
found to be correct, if there is any discrepency please adjust accordingly, but this
should be right:

-1 = Genderless
0 = Always male
1 = 87.5% male
2 = 75% male
4 = 50% - 50%
6 = 75% female
8 = Always female

Note that there is no 87.5% female, I checked and this is not a mistake.

NOTE: Earlier section on database structure for status ailments is outdated, new and improved system now.

ABILITY CODE STRUCTURE
----------------------
Abilities are stored as enumerators. They have no variables to them, the only thing the game cares about is
if the pokemon has the ability or not. However, a few abilities only activate in certain scenarios, and 
the move "Gastro Acid" can disable an ability (there are others in-game effects which affect abilities as
well). For this, the AbilityClass has been created. The ability class contains a final Ability member variable
which refers to the true ability of the pokemon. If the ability is altered in battle, a separate non-final
Ability member variable is set to that new, temporary ability. In battle, when you call pokemon.getAbility(),
you should call the battle specific method which will check if the ability has been changed, and if it has, it
will return the temp ability instead of the pokemon's true ability. This system allows us to change a pokemon's 
ability mid-battle (Gastro-Acid, Simple Beam, etc) while remembering the pokemon's trueability, so on battle's 
end (or when the pokemon is switched out) the true ability can be returned. 

TYPE NUMBERING IN DATABASE
--------------------------
The way our database numbers types differs from traditional listings of types as well as commonly used
matricies for determining type effectiveness. Our database uses the following:

1 Normal
2 Fighting
3 Flying
4 Poison
5 Ground
6 Rock
7 Bug
8 Ghost
9 Steel
10 Fire
11 Water
12 Grass
13 Electric
14 Psychic
15 Ice
16 Dragon
17 Dark

Note that these were divided into "physical" and "special" attack types (gens 1-3), which obviously
is handled differently now, and thank god =).


PROJECT STRUCTURE:
------------------

The project's root directory contains following subdirectory and files:

src/
	This is where the source code is located. This directory has subdirectories
	structured in the standard java way with the directory structure
	representing the package structure.

bin/
	This is where the compiled class files can be found.

assets/
	Everything that is not source-, class- and library files. This directory
	contains databases, images, sounds, etc.

libs/
	External libraries needed to compile this project.

README
	This readme file.

.gitignore
	The exclude file for git. Currently excludes the bin/ directory.

options
	Arg file for javac listing default options.

classes
	Arg file for javac listing all classes to be compiled.

HOW TO BUILD:
-------------
* Dependencies:

	- Java Developing Kit (JDK) 6

* If not already present create new folder called 'dir' at the root of
  this project.

* From the root of this project execute the following:

	javac @options @classes

* All compiled .class-files will be placed under bin/.

HOW TO RUN:
-----------
* From the root of this project execute the following:

* Linux:

       java -classpath \
       'libs/lwjgl-2.7.1/jar/lwjgl.jar:libs/sqlitejdbc-v056.jar:bin' \
       -Djava.library.path=libs/lwjgl-2.7.1/native/linux/ \
       com.pokemon.mmo.Main

* Windows:
		java -classpath \
       'libs/lwjgl-2.7.1/jar/lwjgl.jar:libs/sqlitejdbc-v056.jar:bin' \
       -Djava.library.path=libs/lwjgl-2.7.1/native/windows/ \
       com.pokemon.mmo.Main

* Mac:
		java -classpath \
       'libs/lwjgl-2.7.1/jar/lwjgl.jar:libs/sqlitejdbc-v056.jar:bin' \
       -Djava.library.path=libs/lwjgl-2.7.1/native/mac/ \
       com.pokemon.mmo.Main

* Solaris:
		java -classpath \
       'libs/lwjgl-2.7.1/jar/lwjgl.jar:libs/sqlitejdbc-v056.jar:bin' \
       -Djava.library.path=libs/lwjgl-2.7.1/native/solaris/ \
       com.pokemon.mmo.Main
