.. _AssetsQuickStart:

******************
Assets Quick Start
******************

nanos.world provides fully capabilities of modifying, including and customizing the game. In this section we will explain how to include custom Assets in your server.


Structure
---------

Assets can be included in a folder called ``Assets/`` in the root server folder. All files in there will be sent to the clients and will be able to be referenced in your scripting code.

.. code-block:: javascript

	NanosWorldServer.exe
	Packages/
	Assets/
	|   My_Asset_Pack_01/
	|   |   My_Asset_01.uasset
	|   |   My_Asset_02.uasset
	|   |   My_Big_Map.umap
	|   |   ...
	|   |   Assets.toml
	|   Awesome_Weapons/
	|   |   Big_Fucking_Gun.uasset
	|   |   AwesomeWeaponBundle.pak
	|   |   ...
	|   |   Assets.toml


Creating your own Assets
------------------------

If you are already familiar with `Unreal Engine 4 <https://www.unrealengine.com>`_, creating custom assets is very straightforward. Otherwise, you just need to follow some steps.

All custom Assets must be cooked and packed through Unreal Engine 4. It is not (yet?) possible to import custom assets (meshes, images, sounds) directly to the game. So because of that, you will need Unreal Engine 4 installed just to import your assets and export in the proper format. This guide will teach you how to download the most light version of Unreal Engine and the steps needed to export and use your assets properly.


Installing Unreal Engine 4
~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Download and Install `Epic Games Launcher <https://www.unrealengine.com/en-US/download/ue_non_games>`_ (follow the steps for agreeing to Unreal Engine terms).
2. Install Unreal Engine from inside Epic Games Launcher -> Unreal Engine tab -> Library -> ``+`` Engine Version (We are always using the latest version ;)). Before starting the download, select Options in the dropdown menu and deselect all items, so your installation will be lean with no unecessary stuff.

.. tip:: Please refer to `Unreal Official Documentation <https://docs.unrealengine.com/en-US/GettingStarted>`_ for more information and tutorials.


Exporting Assets from Unreal Engine
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note:: For the sake of this tutorial, we are not covering further details on Unreal's peculiarities. The following guide is using a default's blueprint's blank Unreal Project, without any Starting Content.

.. attention:: All your Asset Pack content must be in a folder with the name of your Asset Pack inside the ``Content/`` Folder.

.. attention:: Do not use Engine's Content in your assets (e.g. Engine's Materials or Assets). If you wants to use them, always copy them to your Content/Your_Pack folder.

For this tutorial, we are going to use a simple Cube (Static Mesh) and a Material (applied to the cube) for our Assets. We've also created and placed them in a folder called MyPack which will help us afterwards.

.. image:: https://i.imgur.com/H5x15qE.png

.. image:: https://i.imgur.com/jcwWd0L.png

For exporting them in a recognizable way by nanos.world, you need to "Package the Project" (i.e. cooking and packaging it).

Before moving on, we just need to change three settings, for that open the Packaging Settings:

.. image:: https://i.imgur.com/CKWmKwl.png

And deselect ``Use Pak File``, ``Share Material Shader Code`` and ``Allow Static Lighting`` options:

.. image:: https://i.imgur.com/0dqzFh9.png

And you are done! Now you just need to Package it! For that just select the following option and select any folder in your computer and wait it finishes, it may take some minutes:

.. image:: https://i.imgur.com/h0fMzGa.png

.. image:: https://i.imgur.com/ImGjShf.png

After finishing, you will get a folder like that:

.. image:: https://i.imgur.com/FnUex4P.png


Importing and Using Assets in your Server
-----------------------------------------

After packaging your project, we will manually copy the exported folder from it, the one we are looking for will be located at ``NanosWorldAssets/Content/`` (or whatever is the name of your project). As we created a folder called ``MyPack``, our exported assets will be at ``NanosWorldAssets/Content/MyPack/``:

.. attention:: It is important to keep your main Asset Folder (in this case ``MyPack``) with the same name when copying to, otherwise internal references will not work.

.. image:: https://i.imgur.com/BIzXctE.png

And thats it! You must now just copy ``MyPack/`` folder inside your Server's ``Assets/`` folder and reference your Cube like ``Prop(Vector(0, 0, 0), Rotator(0, 0, 0), "MyPack/SM_Cube")`` when loading a Prop for example.

.. image:: https://i.imgur.com/H0B7WWp.png

.. note:: Please do NOT rename or change any file or folder in your exported folder after you Packaged it, it will break all internal references used by your assets.


Assets Configuration File
~~~~~~~~~~~~~~~~~~~~~~~~~

.. tip:: nanos.world Config files use **TOML** (Tom's Obvious, Minimal Language), please refer to https://github.com/toml-lang/toml for more information and syntax.

The Assets Configuration file ``Assets.toml`` is generated automatically when an Asset Pack is loaded for the first time. This file will always be overriden with the proper pattern after it's loaded.

It is extremely important to setup your ``Assets.toml`` file, in there you will need to list every asset contained in your Pack, as well the type of them, besides that, this is where Scripters can look into to find the assets they want to use.

.. code-block:: toml

	# Asset Pack Configurations
	[asset_pack]
		# Asset Pack Name
		name =				"My Pack"
		# Author
		author =			"Incredible Scripter"
		# Version
		version =			"1.1.0"

	# Assets Files
	[assets]
		# Maps
		[assets.maps]
			AwesomeAndBigMap = "Maps/BigMap_01"

		# Static Meshes
		[assets.static_meshes]
			# SM_Flower_01 = "MyFolder/SM_Awesome_Flower_01"
			# ...

		# Skeletal Meshes
		[assets.skeletal_meshes]
			# SK_Better_Man = "Characters/SK_BetterMan_3"
			# ...

		# Other Assets (for not yet categorized ones)
		[assets.others]
			# A_Audio_Rifle_Fire = "Audios/A_Audio_Rifle_Fire_03"
			# ...

As seen above, Assets can be set in a ``key = "value"`` pattern, the **key** being how Scripters reference it in their code, and the **value** being the path where the game will look for it. Note: all paths are relative to the Asset Pack folder.


Referencing Assets in Code
~~~~~~~~~~~~~~~~~~~~~~~~~~

The correct way of referencing assets in code is using the pattern: ``ASSET_PACK_NAME::ASSET_FILE_NAME``. So for example, if I want to reference to AwesomeAndBigMap as the above example, I would use: ``MyPack::AwesomeAndBigMap``

.. note:: The key ``ASSET_PACK_NAME`` is the Asset Pack's folder name.


Going Further
-------------

If you want to know more, please move to :ref:`Modding`.


NanosWorld's Default Asset Pack
-------------------------------

nanos world provides a default Asset Pack already included in the base game, feel free to use the assets the way you want:

.. tip:: The following list is constantly updated and it's presentation will be improved soon™.


.. code-block:: toml

	# Assets Files
	[assets]
		# Maps
		[assets.maps]
		BlankMap = "Maps/BlankMap/BlankMap"
		TestingMap = "Maps/Testing/NanosTestingMap"

		# Static Meshes
		[assets.static_meshes]
		SM_Cone = "Props/BasicShapes/SM_Cone"
		SM_Cube = "Props/BasicShapes/SM_Cube"
		SM_Cylinder = "Props/BasicShapes/SM_Cylinder"
		SM_Plane = "Props/BasicShapes/SM_Plane"
		SM_Sphere = "Props/BasicShapes/SM_Sphere"

		SM_Ball_VR = "Props/VRShapes/SM_Ball"
		SM_Cube_VR_01 = "Props/VRShapes/SM_Cube_01"
		SM_Cube_VR_02 = "Props/VRShapes/SM_Cube_02"
		SM_Cube_VR_03 = "Props/VRShapes/SM_Cube_03"
		SM_Pyramid_VR = "Props/VRShapes/SM_Pyramid"

		SM_Error = "Props/Utils/SM_Error"

		SM_PlasticBarrel_01 = "Art/City/Construction_Props/Mesh/SM_PlasticBarrel_01"
		SM_RockingChair = "Art/City/House_Props/Meshes/SM_RockingChair"
		SM_RoundStand = "Art/City/House_Props/Meshes/SM_RoundStand"
		SM_Bottle_01 = "Art/Rural/InteriorDecoration/SM_Bottle_01"
		SM_Bottle_02 = "Art/Rural/InteriorDecoration/SM_Bottle_02"
		SM_Bottle = "Art/Rural/Extra/SM_Bottle"
		SM_Bunny = "Art/City/House_Props/Meshes/SM_Bunny"
		SM_CupC = "Art/City/House_Props/Meshes/KitchenWare/SM_CupC"
		SM_OilLamp = "Art/Rural/InteriorDecoration/SM_OilLamp"
		SM_PlierSet_01 = "Art/City/Construction_Props/Mesh/SM_PlierSet_01"
		SM_PliersSet_07 = "Art/City/Construction_Props/Mesh/SM_PliersSet_07"
		SM_Saw_01 = "Art/City/Construction_Props/Mesh/SM_Saw_01"
		SM_BarrelTub = "Art/Rural/Extra/SM_BarrelTub"
		SM_CoffeeTable = "Art/City/House_Props/Meshes/SM_CoffeeTable"
		SM_Crate_Base_01 = "Art/City/Construction_Props/Mesh/SM_Crate_Base_01"
		SM_Crate_Lid_01 = "Art/City/Construction_Props/Mesh/SM_Crate_Lid_01"
		SM_Toolbox_01 = "Art/City/Construction_Props/Mesh/SM_Toolbox_01"
		SM_Toolbox_06 = "Art/City/Construction_Props/Mesh/SM_Toolbox_06"
		SM_VaseA = "Art/City/House_Props/Meshes/Vases/SM_VaseA"
		SM_CupD = "Art/City/House_Props/Meshes/KitchenWare/SM_CupD"
		SM_Crate_01 = "Art/City/Construction_Props/Mesh/SM_Crate_01"

		SM_Beard_Extra = "Characters/Common/BodyParts/Beard/SM_Beard_Extra"
		SM_Beard_Middle = "Characters/Common/BodyParts/Beard/SM_Beard_Middle"
		SM_Beard_Mustache_01 = "Characters/Common/BodyParts/Beard/SM_Beard_Mustache_01"
		SM_Beard_Mustache_02 = "Characters/Common/BodyParts/Beard/SM_Beard_Mustache_02"
		SM_Beard_Side = "Characters/Common/BodyParts/Beard/SM_Beard_Side"
		SM_Hair_Long = "Characters/Common/BodyParts/Hair/Male/SM_Hair_Long"
		SM_Hair_Short = "Characters/Common/BodyParts/Hair/Male/SM_Hair_Short"
		SM_Hair_Kwang = "Characters/Common/BodyParts/Hair/Kwang/SM_Hair_Kwang"

		SM_AK47_Mag_Empty = "Weapons/Rifles/AK47/SM_AK47_Mag_Empty"
		SM_AK74U_Mag_Empty = "Weapons/Rifles/AK74U/SM_AK74U_Mag_Empty"
		SM_GE36_Mag_Empty = "Weapons/Rifles/GE36/SM_GE36_Mag_Empty"
		SM_Glock_Mag_Empty = "Weapons/Pistols/Glock/SM_Glock_Mag_Empty"
		SM_DesertEagle_Mag_Empty = "Weapons/Pistols/DesertEagle/SM_DesertEagle_Mag_Empty"
		SM_AP5_Mag_Empty = "Weapons/Rifles/AP5/SM_AP5_Mag_Empty"
		SM_AR4_Mag_Empty = "Weapons/Rifles/AR4/SM_AR4_Mag_Empty"

		SM_T4_Sight = "Weapons/Common/Accessories/SM_T4_Sight"

		SM_WoodenTable = "Art/Rural/InteriorDecoration/SM_WoodenTable"
		SM_WoodenChair = "Art/Rural/InteriorDecoration/SM_WoodenChair"
		SM_TireLarge = "Art/Rural/Extra/SM_TireLarge"
		SM_Stool = "Art/Rural/InteriorDecoration/SM_Stool"
		SM_TeaPot_Interior = "Art/Rural/InteriorDecoration/SM_TeaPot_Interior"
		SM_OilDrum = "Art/Rural/ExteriorDecoration/SM_OilDrum"
		SM_Bucket5Gallon = "Art/Rural/Extra/SM_Bucket5Gallon"
		SM_Crate_07 = "Art/Rural/Extra/SM_Crate_07"
		SM_Crate_03 = "Art/Rural/InteriorDecoration/SM_Crate_03"
		SM_Crate_04 = "Art/Rural/InteriorDecoration/SM_Crate_04"
		SM_Pot_01 = "Art/Rural/InteriorDecoration/SM_Pot_01"
		SM_Pot_02 = "Art/Rural/InteriorDecoration/SM_Pot_02"
		SM_Plate_Interior = "Art/Rural/InteriorDecoration/SM_Plate_Interior"
		SM_Barrel_02 = "Art/Rural/Extra/SM_Barrel_02"
		SM_Bamboo_Roof45_Right = "Art/Rural/HouseModular/SM_Bamboo_Roof45_Right"
		SM_MetalBucket_Interior_02 = "Art/Rural/InteriorDecoration/SM_MetalBucket_Interior_02"
		SM_Basket_01 = "Art/Rural/InteriorDecoration/SM_Basket_01"
		SM_Bamboo_Woodplank_01 = "Art/Rural/Extra/SM_Bamboo_Woodplank_01"

		SM_Grenade_G67 = "Weapons/Grenades/G67/SM_G67"

		# Skeletal Meshes
		[assets.skeletal_meshes]
		SK_Female = "Characters/Female/SK_Female"
		SK_Male = "Characters/Male/SK_Male"
		SK_Mannequin = "Characters/Mannequin/SK_Mannequin"
		SK_PostApocalyptic = "Characters/PostApocalyptic/SK_PostApocalyptic"
		SK_ClassicMale = "Characters/ClassicMale/SK_ClassicMale"

		SK_Shirt = "Characters/Common/BodyParts/Clothes/Shirt/SK_Shirt"
		SK_Underwear = "Characters/Common/BodyParts/Clothes/Underwear/SK_Underwear"
		SK_Pants = "Characters/Common/BodyParts/Clothes/Pants/SK_Pants"
		SK_Shoes_01 = "Characters/Common/BodyParts/Clothes/Shoes/SK_Shoes_01"
		SK_Shoes_02 = "Characters/Common/BodyParts/Clothes/Shoes/SK_Shoes_02"
		SK_Tie = "Characters/Common/BodyParts/Clothes/Tie/SK_Tie"
		SK_CasualSet = "Characters/Common/BodyParts/Clothes/CasualSet/SK_CasualSet"
		SK_Sneakers = "Characters/Common/BodyParts/Clothes/Shoes/SK_Sneakers"

		SK_Error = "Props/Utils/SK_Error"

		SK_AK47 = "Weapons/Rifles/AK47/SK_AK47"
		SK_AK74U = "Weapons/Rifles/AK74U/SK_AK74U"
		SK_GE36 = "Weapons/Rifles/GE36/SK_GE36"
		SK_Glock = "Weapons/Pistols/Glock/SK_Glock"
		SK_DesertEagle = "Weapons/Pistols/DesertEagle/SK_DesertEagle"
		SK_AR4 = "Weapons/Rifles/AR4/SK_AR4"
		SK_Moss500 = "Weapons/Shotguns/Moss500/SK_Moss500"
		SK_AP5 = "Weapons/Rifles/AP5/SK_AP5"
		SK_SMG11 = "Weapons/SMGs/SMG11/SK_SMG11"

		# Sound Assets
		[assets.sounds]
		A_SMG_Dry = "Weapons/Common/Audios/A_SMG_Dry_Cue"
		A_Rifle_Dry = "Weapons/Common/Audios/A_Rifle_Dry_Cue"
		A_Pistol_Dry = "Weapons/Common/Audios/A_Pistol_Dry_Cue"
		A_Shotgun_Dry = "Weapons/Common/Audios/A_Shotgun_Dry_Cue"
		A_SMG_Load = "Weapons/Common/Audios/A_SMG_Load_Cue"
		A_Rifle_Load = "Weapons/Common/Audios/A_Rifle_Load_Cue"
		A_Pistol_Load = "Weapons/Common/Audios/A_Pistol_Load_Cue"
		A_Shotgun_Load_Bullet = "Weapons/Common/Audios/A_Shotgun_Load_Bullet_Cue"
		A_SMG_Unload = "Weapons/Common/Audios/A_SMG_Unload_Cue"
		A_Rifle_Unload = "Weapons/Common/Audios/A_Rifle_Unload_Cue"
		A_Pistol_Unload = "Weapons/Common/Audios/A_Pistol_Unload_Cue"
		A_AimZoom = "Weapons/Common/Audios/Rattle/A_AimZoom_Cue"
		A_Rattle = "Weapons/Common/Audios/Rattle/A_Rattle_Cue"
		A_M4A1_Shot = "Weapons/Common/Audios/A_M4A1_Shot_Cue"
		A_AK47_Shot = "Weapons/Common/Audios/A_AK47_Shot_Cue"
		A_Glock_Shot = "Weapons/Common/Audios/A_Glock_Shot_Cue"
		A_Rifle_Shot = "Weapons/Common/Audios/A_Rifle_Shot_Cue"
		A_DesertEagle_Shot = "Weapons/Common/Audios/A_DesertEagle_Shot_Cue"
		A_Shotgun_Shot = "Weapons/Common/Audios/A_Shotgun_Shot_Cue"
		A_LightMachine_Shot = "Weapons/Common/Audios/A_LightMachine_Shot_Cue"
		A_SMG_Shot = "Weapons/Common/Audios/A_SMG_Shot_Cue"
		A_Male_Death = "Characters/Common/Audios/Death/A_Male_Death_Cue"
		A_VR_Click_01 = "Effects/VR/A_VR_Click_01"
		A_VR_Click_02 = "Effects/VR/A_VR_Click_02"
		A_VR_Click_03 = "Effects/VR/A_VR_Click_03"
		A_VR_Close = "Effects/VR/A_VR_Close"
		A_VR_Confirm = "Effects/VR/A_VR_Confirm"
		A_VR_Grab = "Effects/VR/A_VR_Grab"
		A_VR_Ungrab = "Effects/VR/A_VR_Ungrab"
		A_VR_Negative = "Effects/VR/A_VR_Negative"
		A_VR_Open = "Effects/VR/A_VR_Open"
		A_VR_Teleport = "Effects/VR/A_VR_Teleport"

		# Other Assets (for not yet categorized ones)
		[assets.others]
		# Particles
		P_Bullet_Trail = "Weapons/Common/Effects/ParticlesSystems/Weapons/P_Bullet_Trail_System"
		P_Weapon_BarrelSmoke = "Weapons/Common/Effects/ParticlesSystems/Weapons/P_Weapon_BarrelSmoke_System"
		P_Weapon_Shells_12Gauge = "Weapons/Common/Effects/ParticlesSystems/Weapons/P_Weapon_Shells_12Gauge_System"
		P_Weapon_Shells_762x39 = "Weapons/Common/Effects/ParticlesSystems/Weapons/P_Weapon_Shells_762x39_System"
		P_Weapon_Shells_9x18 = "Weapons/Common/Effects/ParticlesSystems/Weapons/P_Weapon_Shells_9x18_System"
		P_Weapon_Shells_556x45 = "Weapons/Common/Effects/ParticlesSystems/Weapons/P_Weapon_Shells_556x45_System"
		P_Weapon_Shells_545x39 = "Weapons/Common/Effects/ParticlesSystems/Weapons/P_Weapon_Shells_545x39_System"
		P_Weapon_Shells_45ap = "Weapons/Common/Effects/ParticlesSystems/Weapons/P_Weapon_Shells_45ap_System"
		P_Weapon_Shells_9mm = "Weapons/Common/Effects/ParticlesSystems/Weapons/P_Weapon_Shells_9mm_System"

		# Animations
		AM_Mannequin_Reload_Pistol = "Characters/Common/Animations/Weapons/AM_Mannequin_Reload_Pistol"
		A_Character_Damage_Taken = "Characters/Common/Audios/Pain/A_Character_Damage_Taken"
		AM_Mannequin_Reload_Rifle = "Characters/Common/Animations/Weapons/AM_Mannequin_Reload_Rifle"
		AM_Mannequin_Reload_Shotgun = "Characters/Common/Animations/Weapons/AM_Mannequin_Reload_Shotgun"
		AM_Mannequin_Sight_Fire = "Characters/Common/Animations/Weapons/AM_Mannequin_Sight_Fire"
		AM_Mannequin_Sight_Fire_Heavy = "Characters/Common/Animations/Weapons/AM_Mannequin_Sight_Fire_Heavy"
		
		# Blueprints
		BP_Grabable_Torch = "Core/Items/BP_Grabable_Torch.BP_Grabable_Torch_C"
		BP_Vehicle_SUV = "Core/Vehicles/BP_Vehicle_SUV.BP_Vehicle_SUV_C"
		BP_Vehicle_Pickup = "Core/Vehicles/BP_Vehicle_Pickup.BP_Vehicle_Pickup_C"
		BP_Vehicle_Truck = "Core/Vehicles/BP_Vehicle_Truck.BP_Vehicle_Truck_C"
		BP_Vehicle_Truck_Chassis = "Core/Vehicles/BP_Vehicle_Truck_Chassis.BP_Vehicle_Truck_Chassis_C"
		BP_Vehicle_Hatchback = "Core/Vehicles/BP_Vehicle_Hatchback.BP_Vehicle_Hatchback_C"
		BP_Vehicle_SportCar = "Core/Vehicles/BP_Vehicle_SportCar.BP_Vehicle_SportCar_C"