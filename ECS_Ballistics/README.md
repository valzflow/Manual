# ECS Ballistics

ECS Ballistics build on Entity Component System(ECS) architecture for Unity, current supported version of Unity ECS is [Entities 1.0.16](https://docs.unity3d.com/Packages/com.unity.entities@1.0/manual/index.html).

## Presentation
[![](/ECS_Ballistics/images/ECS_Ballistics_Features_Video_Logo.png)](https://youtu.be/-IWIncKrCUw)

[Perfomace Demo](https://youtu.be/fsDqgnA4vZs)

## Demos
* [DEMO](https://drive.google.com/file/d/1XvV_WHClp0cr797lW3myZpHmyIvPgnE4/view?usp=drive_link)
* [DemoPerfomance](https://drive.google.com/file/d/1TuoD-VszM97OvZ4KPBhDYVxek7f-6qlR/view?usp=drive_link)
* [DemoPerfomanceHybrid](https://drive.google.com/file/d/1xs_xo9G4smFjQ2s3K3Zs3_wx8av630VY/view?usp=drive_link)

## Dictionary
* ECS
* Hybrid Mode
* Decal
* Sprite
## Get Started

Open Scene Assets > Valzflow > ECS_Ballistics > Demo > A_Scene > Demo
#### Create Bullet 
1. Create Cube (it will be our bullet)
2. Remove Collider and name it `MyBullet`
3. Drag our bullet to folder Assets > Valzflow > ECS_Ballistics > Demo > Prefabs > Ballistics > Bullets (prefab should be created)
4. Select MyBullet prefab
5. Set Transform Scale to X-0.1 Y-0.1 Z-0.6, Position X-999 Y-999 Z-999
6. Delete MyBullet from the Scene
7. Add following Components:
    * Bullet Authoring
    * Dynamic Buffer Configuration Authoring
    * Random Generator Authoring
8. Set components Values as on the picture
   
![](/ECS_Ballistics/images/MyBullet.png)

#### Create Weapon
1. Create Cube (it will be our weapon)
    * Remove Collider and name it `MyWeapon`
    * Add Child objects ShootEffect and NoAmmoEffect (Copy it from another weapon for simplicity)
    * Rename it to MyShootEffect and MyNoAmmoEffect respectfully
2. Drag our weapon to folder Assets > Valzflow > ECS_Ballistics > Demo > Prefabs > Ballistics > Weapons (prefab should be created)
3. Select MyWeapon prefab
4. Set Transform Scale to X-0.25 Y-0.25 Z-1, Position X-0 Y-0 Z-0
5. Delete MyWeapon from the Scene
6. Add following Components:
    * Weapon Authoring
    * Random Generator Authoring
7. Set components Values as on the picture
   
![](/ECS_Ballistics/images/MyWeapon.png)
  
8. Add MyWeapon to Player weapons
9. Drag MyWeapon into Scene as child of GameConfiguration-Player-Body-Hand
10. Add MyWeapon instance to Player Authoring Component in GameConfiguration-Player

![](/ECS_Ballistics/images/MyWeapon_Player.png)
  
Now you can start the Demo. You should see that the player has now `MyWeapon` as a new weapon which shoots `MyBullets`

![](/ECS_Ballistics/images/MyWeapon_Shooting.png)

#### Create Material
1. Open folder Assets > Valzflow > ECS_Ballistics > Demo > Prefabs > Ballistics > MaterialConfigurations
2. Press the right button on the mouse
3. Select Create->Scriptable Objects->Material Configuration
   
![](/ECS_Ballistics/images/NewMaterialConfiguration.png)

4. Name a new configuration as `MyMaterialConfiguration`
5. Set configuration as on the picture (We will temprory add `TargetRicochet` Asset Variant to ricichet mark, currently system not processes interaction with dynamic obstacle if mark is not set)

![](/ECS_Ballistics/images/NewMaterialConfigurationSetUp.png)

##### Create static obstacle
1. Create in the Environment sub-scene a new box (important, if you crate the box in the main scene it would not converted to entity)
2. Name it `MyPanel`
3. Add component `Material Authoring`
4. Set our `MyMaterialConfiguration` into Material Configuration field
5. Set configuration as on the picture

![](/ECS_Ballistics/images/CreateStaticObstacle.png)
  
Start the Demo and shoot `MyPanel` (Almost all bullets should ricochet from it because of high ricochet values of the material)

![](/ECS_Ballistics/images/NewMaterialConfigurationBulletBouncing.png)

##### Create dynamic obstacle
1. Create in the Environment sub-scene a new sphere (important, if you crate the sphere in the main scene it would not converted to entity)
2. Name it `MySphere`
3. Add component `Rigidbody`
4. Add component `Material Impulse Tag Authoring`
5. Add component `Material Authoring`
6. Set our `MyMaterialConfiguration` into Material Configuration field
7. Add component `Rigidbody`
8. Set configuration as on the picture

![](/ECS_Ballistics/images/CreateDynamicObstacle.png)

9. Start the Demo and shoot `MySphere` (The sphere should start to react on bullets collisions)

![](/ECS_Ballistics/images/MySphereRolling.png)

Note: Besides `MaterialImpulseTagAuthoring` the system has other tags that modifies its behaviour with materials: `MaterialNoMarkTagAuthoring` and `MaterialDamageTagAuthoring`.
##### Create custom impact mark
1. Open folder Assets > Valzflow > ECS_Ballistics > Demo > Prefabs > Ballistics > MaterialConfigurations > ImpactMarks
2. Open folder 0_Simple
3. Create impact mark, name it `MyMarkEntitySimple` (You can creat you own mark Prefab or copy existen and modify it) (ECS will directly convert this gameObjects with Sprite to entity)

![](/ECS_Ballistics/images/MyMarkEntitySimple.png)

4. Go one folder back and Open folder 1_Decal/GameObjectPrefabs (Here we will create impact mark that uses Decals. Decals is more visual appealing then sprites, it stick to surfaces of different curveage as a sticker and cut itself on the edges (The Sprites can't do that) but Decal works only in Hybrid mode in ECS)
5. Create Prefab with decal, name it `MyDecal`
6. Set the decal

![](/ECS_Ballistics/images/MyDecal.png)

7. Go one folder back (to the folder 1_Decal)
8. Create entity holder for `MyDecal`, name it `MyMarkEntityDecalHolder`
9. Set `MyMarkEntityDecalHolder` as on picture

![](/ECS_Ballistics/images/MyMarkEntityDecalHolder.png)

10. Go one folder back (to the folder ImpactMarks)
11. Create Asset Variant by pressing the right button on the mouse in the folder
12. Give it the name `MyAssetVariantMark` (this scriptable object allows to quickly switch between GameObject that will be used by the system, you can pass Prefabs directly too)
13. Set `MyAssetVariantMark` as on the picture

![](/ECS_Ballistics/images/MyAssetVariantSetUp.png)
 
##### Create custom impact effect
1. Open folder Assets > Valzflow > ECS_Ballistics > Demo > Prefabs > Ballistics > MaterialConfigurations > ImpactEffects > GameObjectPrefabs
2. Create or copy Prefab with ParticleSystem (ParticleSystem works only in Hybrid mode in ECS)
3. Name it `MyEffect`
4. Set it configuration (If you effect play particles inside the obstecle then set Y Rotation to 180 in ParticleSystem->Shape panel)

![](/ECS_Ballistics/images/MyEffect.png)

5. Go one folder back (to the folder ImpactEffects)
6. Create entity holder for `MyEffect`, name it `MyEffectEntityHolder`
7. Set `MyEffectEntityHolder`

![](/ECS_Ballistics/images/MyEffectEntityHolder.png)

##### Set impacts and effects to material configuration
Note: The next part is a bit messy. `Material Configuration` was made to accept `GameObject` or `AssetVariant` scriptable object as values for effects and marks that is why those fields of type `Object` and this made fields to be legit almost for anything in the project. For my self I found workeround by openning two `Project` tabs. In the first one I have folder with my Material Configuration in the second one folder with marks or impacts. After selecting correct material configuration, I drag and drop impacts/effects into material configuration.

![](/ECS_Ballistics/images/NewProjectTab.png)

1. Open folder Assets > Valzflow > ECS_Ballistics > Demo > Prefabs > Ballistics > MaterialConfigurations
2. Find there `MyMaterialConfiguration` that we [](#create-material)
3. Set materials and effects

![](/ECS_Ballistics/images/MyMaterialConfigurationFinish.png)

Start the demo and shoot your obstacles. You should see marks and effects when bullet interacts with obstacle.
Note: I set yellow holes to be ricochet marks that is why you see it when ricochet happens.

![](/ECS_Ballistics/images/MaterialWithMarksAndEffects.png)

## Overview 
### Dynamic Buffer
## Weapon
## Material
### Interaction Mark
### Interaction Effect
## Bullet
## Hybrid Mode (ECS + MonoBehavior)
## Crosshair
## Optimizations
* Entity destruction 
* Buffers size
* Tag components usage

## FAQ
### How to Disable V-Sync to unlock FPS?
https://www.youtube.com/watch?v=yFNLjh3c7YY
### Where does the bullet spawn?
In front of player Camera (Player->Body->Head->PlayerCamera.Position + MOVE_BULLET_FORWARD_COEFFICIENT constant)
### Why some Bullet-Material interactions clip out of material?
1) It's transperent material (URP doesn't support decals on transperent materials). That is why Demo uses sprites in those cases.
2) Assert Variant Configuration for the mark switched to sprite variant (Assert Variant - 0. Sprites is supported by pure Entity - more perfomance)
### Why are impact effects rotated by 180 Degree (Particle System->Shape->Rotation: 180)?
Impact marks and effects places by the same system. This system places this elements facing opsticle that is why we need to rotate effect to make it look like it coming out of impact.
### Why Bullets, Interections marks and effects prefabs has position (999, 999, 999)
It's done to set initial spawn position of prefab Entity instances (Player should not see what is not designed to be seen)
