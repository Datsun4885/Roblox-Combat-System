# Roblox-Combat-System

## Overview
- A third person combat system for a passion project of mine

## Features
- Melee weapons,
- Ranged weapons (guns, etc..),
- Quick weapons swapping, ability to choose weapons,
- Can be controlled by mission scripts,
- Made for r6

## Example
![combat demo](media/shooting-reloading.gif)
![swapping demo](media/weapon-swapping.gif)

## Code Example
due to not wanting to leak my code I am providing a small example from the main combant handler

```lua
-- Primary Attack Input
function OnPrimaryAttackInput()
	if CheckHuman() ~= true then return end

	if peaceful then return end
	
	if unequipped then
		Dictonary[CurrentWeapon.Value]:Start()
		unequipped = false
		return
	end
	
	Dictonary[CurrentWeapon.Value]:AttackInput()
	if Dictonary[CurrentWeapon.Value]["WeaponType"] ~= "Melee" then

		if Ads and NoAmmo == false then
			RecoilSpring:shove(Dictonary[CurrentWeapon.Value]["RecoilSpring2"])
		elseif Ads == false and NoAmmo == false then
			RecoilSpring:shove(Dictonary[CurrentWeapon.Value]["RecoilSpring1"])
		end

		coroutine.wrap(function()
			wait(Dictonary[CurrentWeapon.Value]["FireRate"])
			RecoilSpring:shove(Vector3.new(-2.8, math.random(-1,1), -5))
		end)
	end
end
