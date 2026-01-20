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
local function OnPrimaryAttackInput()
	if CheckHuman() ~= true then return end

	if not ALLOWED_TO_SHOOT[GetState()] then return end
	
	if NoAmmo then
		return
	end
	
	if unequipped then
		Dictonary[CurrentWeapon.Value]:Start()
		unequipped = false
		return
	end
	
	Dictonary[CurrentWeapon.Value]:AttackInput()
	if Dictonary[CurrentWeapon.Value]["WeaponType"] ~= "Melee" then
		if NoAmmo then
			return
		end
		local stats = Dictonary[CurrentWeapon.Value]
		local recoilVector = Ads and stats["RecoilSpring2"] or stats["RecoilSpring1"]
		RecoilSpring:shove(recoilVector / recoilModifier)

	elseif Dictonary[CurrentWeapon.Value]["WeaponType"] == "Melee" then
		OTSModule:ToggleShiftLock(true)
		
		if meleeCooldown then
			task.cancel(meleeCooldown)
		end
		
		meleeCooldown = task.delay(meleeOTSCooldown, function()
			OTSModule:ToggleShiftLock(false)
			meleeCooldown = nil
		end)
	end
end
