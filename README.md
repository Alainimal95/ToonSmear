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
  <summary><b>Skinned meshes</b></summary>
  
  - Select at least 2 target joints or controls in an orderly fashion (ex. elbow to wrist, hip to foot etc.). Selected joints do not have to be connected to each other, but they should follow the same motion
  - Run the script
  - Name the smear geo when prompted
  </details>

  <details>
  <summary><b>Transforming meshes</b></summary>
  
  - Place locators where you want the smear to follow. Minimum 2 required
  - Parent the locators under the meshes
  - Select the target locators in an orderly fashion
  - Run the script
  - Name the smear geo when prompted
  </details>
  
  <details>
  <summary><b>Deforming meshes</b></summary>
  
  - Determine a selection of components (can be verts, edges, faces, or any combination of them). Minimum 2 required
  - For each desired component, select it then place a rivet constraint. you should see a locator appear
  - Select the target locators in an orderly fashion
  - Run the script
  - Name the smear geo when prompted
  </details>

## Notes/warnings
- If the object is static (ex. when added on the first frame of an animation) tesselation errors may appear even on a successful operation. Verify the success of the operation by playing through the animation
- Selection order matters -- try to follow a path or the smear may come out distorted
- A minimum of 2 target anchors are required for running the script

## How it works:
