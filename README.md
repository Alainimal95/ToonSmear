# Toon Smear
A Maya script which creates a cartoon-like smear trail following animated objects.

<details>
<summary><b>Setup</b></summary>
  
- Download and unzip the script at a location of your choosing
- Open the text file in a text editor (ex. Notepad++) 
- Open Maya's script editor and create a new MEL tab
- Select all of the text in the text editor and copy/paste it into the new MEL tab
- Optional: Add it as a shelf button for reuse (recommended)
  - Follow these instructions from Autodesk: https://download.autodesk.com/global/docs/maya2014/en_us/index.html?url=files/Running_MEL_Make_a_shelf_button_for_a_script.htm,topicNumber=d30e789357

Alternatively, the script can be run by pressing numpad enter. Select all text in the script first to keep it from disappearing from the script editor.
</details>

### Intended usage/workflow:
- Animate the target object(s)
- If creating multiple smears, repeat the steps below for each smear seperately
- To create a smear on:
  <details>
  <summary><b>Skeletons/rigs</b></summary>
  
  - Select at least 2 target joints or controls in the order you want them connected. Selected joints do not have to be connected to each other, but they should follow the same motion
  - Run the script
  - Name the smear geo when prompted in the dialog (necessary if making multiple smears)
  - Optional: expand the suffix menu in the dialog settings to change the suffixes of objects. This only affects the smear about to be made
  </details>

  <details>
  <summary><b>Transforming meshes</b></summary>
  
  - Place locators where you want the smear to follow. Minimum 2 required
  - Parent the locators under the meshes
  - Select the target locators in an orderly fashion
  - Run the script
  - Name the smear geo when prompted in the dialog (necessary if making multiple smears)
  - Optional: expand the suffix menu in the dialog settings to change the suffixes of objects. This only affects the smear about to be made
  </details>
  
  <details>
  <summary><b>Deforming meshes</b></summary>
  
  - Ensure there is no non deformer history on the mesh selection. Failure to delete it will likely result in the anchors not binding
  - Determine a selection of components (can be verts, edges, faces, or any combination of them). Minimum 2 required
  - Run the script
  - Name the smear geo when prompted in the dialog (necessary if making multiple smears)
  - Optional: expand the suffix menu in the dialog settings to change the suffixes of objects. This only affects the smear about to be made
  </details>

  <details>
  <summary><b>Mixing component and transform targets</b></summary>
  
  - Ensure there is no non deformer history on the mesh selection. Failure to delete it will likely result in the anchors not binding
  - Don't mix component and transform selections for the workflows above
  - Instead, for each desired component, select it then place a rivet constraint or proximity pin. You should then see a locator appear at that location
  - Select the locator(s) and any other transforms in the order you want them connected (same as you would for skeletons or transforming meshes) 
  - Run the script
  - Name the smear geo when prompted in the dialog (necessary if making multiple smears)
  - Optional: expand the suffix menu in the dialog settings to change the suffixes of objects. This only affects the smear about to be made
  </details>

## Notes/warnings
- Tested in Maya 2027
- Anchors from mesh components need Maya 2020 or later (proximityPin)
- If the object is static when the script is run (ex. when added on the first frame of an animation) tesselation errors may appear even on a successful operation.
Verify the success of the operation by playing through the animation. There is now an in view message as a reminder
- Selection order matters -- try to follow a path or the smear may come out distorted
- A minimum of 2 target anchors are required for running the script
- Use on simulations is currently not supported

## How it works:
- One live degree-1 curve has each CV driven by the world position of one selected transform (decomposeMatrix -> controlPoints).
- The live curve is snapshotted over the last few frames. The snapshot curves come back oldest -> newest, so they can be lofted directly.
- An expression keeps the snapshot window tied to the current frame (also needed so a smear created on a frame without motion still updates when you scrub).
- The loft is converted to polygons. Everything is parented under <name>_smear_<groupSuffix>, which should stay at identity (don't move it).
- Anchors (only when mesh components are selected)
  - Each component becomes a visible locator driven by a proximityPin node. The pin follows the closest point on the mesh through deformation, and needs no UVs. One proximityPin is made per mesh.
  - The pin binds against the mesh's original (pre-deformation) shape, so for meshes with deformers the rest position is read from that shape.
  A mesh that is driven by something but has no orig shape (e.g. an Alembic cache) gets a hidden, frozen copy made at the current frame to serve as its rest geometry.
  - Rest positions are read in the mesh's object space, so the mesh transform can be offset or animated.
  - After the anchors are built, each is compared with its component's current position, and a warning is shown if they don't match.
  - Pick order matters, so "Track selection order" must be on. If it is off the script turns it on and asks you to reselect.

### Naming
Every node is named \<name\>\_\<descriptor\>_\<suffix\>

e.g. with name "arm":
|||
|:------------------:|:------------------:|
|arm_smear_grp|top group|
|arm_smearLive_crv|live curve|
|arm_smearTrail_grp|snapshot group|
|arm_smearTrail1_crv|trail curves|
|arm_smearLoft_srf|loft surface|
|arm_smear_geo|final polygon geometry|

Fixed names (no suffix field): arm_snap, arm_smear_exp, arm_smearDM1...
