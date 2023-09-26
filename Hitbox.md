---
title: Hitbox
layout: default
---

**Initialization**
```lua
local hitboxController = require(replicatedStorage.Modules.Hitbox)
```

{: .fs-6 .fw-300 }
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
