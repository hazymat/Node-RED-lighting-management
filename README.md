# Node-RED-lighting-management

See this youtube video, which shows the user interface, a demo of the working system (also using Alexa to trigger scenes, but you can use anything you like - it's just an MQTT input), and a brief code walkthrough: https://www.youtube.com/watch?v=wyuGSUfekQc

Scroll down for screenshots...

General
-------
- Management of light "fixtures", "zones", and "scenes"
- NR Dashboard user interface which auto-populates sliders, colour pickers, and scene selection buttons based on which fixtures are in the given zone we are controlling
- Front-end management of the system (i.e. from NR Dashboard), where you can create zones, assign fixtures to zones etc.
- All config info stored in global context (home.light.config).
- Light circuits or fittings (or individual bulbs in the case of e.g. Philips Hue) are called Fixtures
- Fixtures, zones, and mappings are 3 sections under the "manage lighting" tab
- Scene management is done as part of this, but UI for scenes (add/remove scene, populate or set scene colours or light levels) will be under the tab where lighting controls for a given zone are. This enables e.g. family members to create / update / delete lighting scenes for that zone, on the fly. (Buttons are updated dynamically so user can test their new scene and tweak quickly)

Fixtures
---
- Add fixture (user specifies ID, location, type, control type)
- Includes form validation and checking for duplicate IDs with feedback
- Show list of existing fixtures, with delete button to remove one
- Delete fixture also removes associated fixture-to-zone mappings
- Automatically update list when add/remove happens
- Upon creation of fitting, current settings (i.e. light level or RGB) are generated with default vals according to fitting type (RGB / RGBW etc)

Zones
---
- Add zone (user specifies zone ID, name)
- Form validation / check for duplicate IDs with feedback
- 3D table of zones
 - each row is a zone. Delete button removes zone and any fixture-to-zone mappings
 - each row shows existing mappings, each having its own button to remove
 - this enables user to view and delete mappings from the zone view

Assign fixture to zone
---
- Select from list of fixtures, and list of zones, to assign a fixture to a zone
- Form validation (in case of existing mapping) with feedback
- List showing all mappings with delete button to "unassign" (although this can be done from the zone view)

Scene Management
---
- This is done under the control template for each individual zone

Note about Configuration Storage
---
- The lighting system has a configuration which is stored in Node-RED's global context. It is therefore restored after power outage etc.
- Configuration information is stored in objects. When a fixture, zone, or scene is created, the user specifies an ID, this ID becomes the object key. As duplicates would technically be possible, we therefore check for duplicates when storing such items. The key for a fixture-to-zone mapping is automatically generated as a concatenation of the fixture and zone keys.
- Config info includes the following:
 - fixtures (including the fixture's capabilities such as dimmable single colour, dimmable RGB, dimmable RGBW, RGBWW, individually addressable LEDs etc, and the light fixture's current state in terms of its colour, brightness etc.)
 - zones. This also includes scene configuration as follows. When you start to add scenes to a given zone, the settings for each individual light for that scene - at the time of creating the scene - are stored in the zone. In fact, when we save a scene, we are storing a snapshop of all the fixture objects for those fixtures mapped to the zone at that time. This does mean that if we move a fixture from one zone to another then we'd need to manually update each scene, otherwise it will still change the light which has now been moved to another room. Updating the scenes is really quick and easy though, and is done again from front end UI.

Light control interface
---
- To be displayed on e.g. a tablet mounted on the wall in a given zone. Or just on your mobile device. Or you can interface with any other controller you like. For example I'll be using my own homemade scene controller wall boxes with multiple scene buttons that will correspond to the first 5 scenes you create.
- Easily remove the scene management section, if you prefer not to have this option in your zone controller

Screenshot of front end 1: ![frontend1](https://user-images.githubusercontent.com/7063284/77906908-a94d0980-7280-11ea-9722-7e968f1d118e.jpg)

Screenshot of front end 2: ![frontend2](https://user-images.githubusercontent.com/7063284/77906912-a9e5a000-7280-11ea-8e43-3de2368d6189.jpg)

Screenshot of flows: ![image](https://user-images.githubusercontent.com/7063284/77906644-317edf00-7280-11ea-902e-203010a559a2.png)
