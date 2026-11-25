# Don Juan de Austria: exclusive custom skin for Marta (Hellish Quart)

A fan-made skin that works only for Marta. It runs on Marta's native 68-joint skeleton (Mafra5/Marfa), with her original moves and rapier. The look follows the portrait of Don Juan de Austria (1545-1578) in black-and-gold field armour with round besagews, a small lace ruff, a mail codpiece and paned trunk hose.

- **Armour is iron, not cloth.** The breast and back plates are one rigid piece on the chest (Spine1) and only articulate at the waist belt, where the tassets begin. Inside the waist band Hips and Spine keep their proportions, so the weights stay continuous with the tassets below. On the front and back of the chest the clavicle and upper-arm weights are handed to the chest, so a twist of the spine or a raised arm never bends the plates.
- **Arms rebuilt along Marta's bones.** Everything past the shoulder joint belongs to the arm, with short blends at the shoulder and elbow; next to the joint only the pauldron follows the arm, so the breastplate sides never dent. The hands keep their own weights.
- **Limbs and hips fitted to Marta's skeleton.** The game snaps every skin bone onto Marta's bone, so her joint positions rule. The source arms are much shorter than her bones (upper arm x1.7, forearm x1.3), so each clavicle, upper arm, forearm, thigh and shin is stretched evenly along its own bone while binding, and the lower trunk is fitted so the hip joint lands on Marta's. No length difference piles up at the shoulder, elbow or hip.
- **Rigid head and ruff.** The small ruff turns with the head; the armour gorget stays with the body.
- **Same proportions as the source model.** The whole model is scaled to Marta's shoulder height (x0.964), so the neck and torso are never stretched or squashed.
- **No ghost body when hit.** Marta's 17 wound overlay meshes (`DecalBloody`, `DecalSpine1`, ...) are shaped like her own body. The visibility controller keeps them hidden under the skin, together with her stock face.
- **Sharp textures within the game budget.** The 1M-triangle source is reduced to 80k triangles after welding its UV seams; each part stays under the 65k vertices the game's wound decal builder supports.

- **Fingers rebuilt from the geometry.** The AI-generated source model has five long fingers on each hand (plus the thumb), fused together past the knuckles, so the extra one cannot be cut out without leaving a hole. It was weighted to two different phalanges at once and would have stayed half straight on the sword grip. Every finger vertex now follows its two nearest finger chains, blended by distance, with the phalanx given by its position along the finger, so the extra finger curls exactly like its neighbours.

## Release

The zip contains only the four bundles (`skin_marta_donjuandeaustria`, `pic_marta_donjuandeaustria` and their `.manifest` files) and `README.txt`. There are no scripts. Install by copying the four files into `Hellish Quart_Data/StreamingAssets/`. `README.txt` also documents the optional manual first-person (FPP) adjustment for Marta's skin loader (4 bytes of `resources.assets`, game version of November 2026).

## Layout

- `model/DonJuanDeAustria_Marta.blend`: the fitted result on Marta's skeleton, editable in Blender 5.3.
- `model/references/`: the four reference views.
- `model/selector_portrait.png`: the selector picture (the period portrait).
- `model/DonJuanDeAustria_source.glb` is not in the repository because of its size (103 MB). Place it there to rebuild from scratch.
- `blender_work/`:
  - `step1_prepare.py` welds, decimates and cleans the source, transfers its rig weights and exports the textures;
  - `step3_rig_marta.py` scales the model, fits Marta's limbs and hands, rebuilds the arm weights, makes the cuirass rigid, stretches the limb segments and fits the hips while carrying the mesh into Marta's bind pose, and splits off the rigid head, with QA renders;
  - `verify_skeleton.py` checks the 68 bones and the weights.
- `unity_project/`: Unity 2019.4.0f1 project (menu **DonJuanDeAustria → Construir mod**).
- `release/README.txt`: the English text shipped in the zip.

## Rebuild

Copy `Hellish Quart_Data/Managed/PlayMaker.dll` from your own installation to `unity_project/Assets/Plugins/`, then run:

```powershell
$blender="C:\Program Files\Blender Foundation\Blender 5.3\blender.exe"
$skel="unity_project\Assets\DonJuanDeAustriaMod\Editor\MartaSkeleton.json"
& $blender -b --factory-startup -P blender_work\step1_prepare.py -- model\DonJuanDeAustria_source.glb model\DonJuanDeAustria_source.glb build
& $blender -b --factory-startup -P blender_work\step3_rig_marta.py -- build $skel
& $blender -b --factory-startup -P blender_work\verify_skeleton.py -- build\marta\Skin.fbx $skel
.\build.ps1
```

`build.ps1` is a developer build script and is never part of the release. It runs the Unity batch build, copies the four bundles into `release/` and creates `DonJuanDeAustria_Marta.zip`.