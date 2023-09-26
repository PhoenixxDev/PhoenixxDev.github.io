---
title: Hitbox
layout: default
---

{: .fs-6 .fw-300 }
**Initialization**
```lua
local hitboxController = require(replicatedStorage.Modules.Hitbox)
```

---

{: .fs-6 .fw-300 }
**Creating a Hitbox**
{: .fs-6 .fw-300 }
Initializing a hitbox structure, we start it at a later time (unless you call :Start() instantly after ig)
```lua
local myHitbox = hitboxController:Init(String: VariantTag, Model: Character, Vector3: Size, Bool: Visualize, <Optional> CFrame: CustomOffset)
```
<dl>
  <dt>VariantTag</dt>
  <dd>i.e. "Kamehameha", used in the Server Combat module to signify what the damage source is (if not passed connection func)</dd>
  <dt>Character</dt>
  <dd>Character of the player that is doing said move/hitting (specifics of the hitbox rely on this based on the type)</dd>
  <dt>Size</dt>
  <dd>Vector3 Size of the box (or Integer: Radius for SphereCasting)</dd>
  <dt>Visualize</dt>
  <dd>Creates a visual box to simulate to your eyes where the hitbox is (idk its just obvious)</dd>
  <dt>CustomOffset</dt>
  <dd>Can be `null`, but if given will offset the hitbox (usually from the HumanoidRootPart) just like a weld</dd>
</dl>

---

**Starting Hit Detection**
{: .fs-6 .fw-300 }
```lua
myHitbox:Start(<optional but you should use it> Function: onHitCallback)
```
<dl>
  <dt>onHitCallback</dt>
  <dd>A Function that you pass which will get called once a valid player is detected within the hitbox</dd>
</dl>

---

**Stopping Hit Detection**
{: .fs-6 .fw-300 }
```lua
myHitbox:Stop()
```
yes that's literally it, just call :Stop() to stop the hitbox (you can start it again)

---

**Full Example**
{: .fs-6 .fw-300 }
```lua
local hitboxController = require(replicatedStorage.Modules.Hitbox)
-- creating a hitbox object (so we can stop it at any time if you're wondering why :Init() isn't also just :Start()
local myHitbox = hitboxController:Init("SuperRush", player.Character, Vector3.new(5,5,5), false)

-- when we're ready to begin hit detection
myHitbox:Start(function(hit)
	local hitChar = hit.Parent

	-- use BindableEvent ClientAction in ServerStorage if calling from server (i.e. a skill), and calling the normal remote otherwise (explained in Networks page)
	-- all this damage stuff will be explained in the Combat module
	-- but basically we do this to validate that 1: call is genuinely from the server and was not somehow exploited by the client in anyway
	_G.validGuids = _G.validGuids or {}
	_G.validGuids[eventGuid] = {character = player.Character, skill = "SuperRush", expires = os.clock() + 1}
	task.delay(1, function()
		if _G.validGuids[eventGuid] then
			_G.validGuids[eventGuid] = nil
		end
	end)

	ClientAction:Fire(player, {
		Action = "Damage",
		Skill = "SuperRush",
		Variant = 1, -- first damage value in the damage table in stats for skill
		Var = hit,
		guid = eventGuid
	}, nil)
end)

-- when we want it to end (i.e. end of an animation or despawn time)
myHitbox:Stop()

```
