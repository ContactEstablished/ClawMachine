# ClawMachine — Project Plan and Discussion Notes

Last captured: October 15, 2026

## Project status

The project is paused for a few days at the concept and planning stage. This file preserves the discussion so work can resume without losing the original intent.

No game implementation has been started in this conversation. The technology choices and design suggestions below are recommendations, pending confirmation. Machine dimensions, artwork, target devices, and several gameplay details remain open.

## Vision

Build a fun, engaging, polished 3D claw machine game playable in a browser, using Three.js. The project is inspired by the creator's daughter, who enjoys claw machines.

Players move and position a mechanical claw above a pit of prizes, grab a prize, and successfully transport it to the prize collection portal. The game should combine the anticipation of a physical claw machine with playful features that are only possible in a video game.

## Requirements established by the creator

### Core gameplay

- Players control the positioning and movement of a mechanical claw.
- The claw operates over a pit containing potential prizes.
- The goal is to grab a prize and move it successfully into the collection portal.
- The game should be dynamic and allow different machine sizes and interchangeable prizes.
- Three.js is the intended 3D library; supporting tools and platforms are open for discussion.

### Machine sizes

Support six sizes:

1. X-small
2. Small
3. Medium
4. Large
5. X-large
6. Super

The meaning and scale of these sizes have not been settled. In particular, it remains undecided whether larger sizes mean larger cabinets with more prizes, larger prizes and claws as well, or different themed play areas.

### Interchangeable prizes

Allow the prizes inside a machine to be swapped out. Specific prize collections, models, and customization controls have not been selected.

### Special feature: “The Kobe”

This is a deliberately fantastical rescue mechanic that does not need to follow real-world physics.

- The character is a video game basketball player resembling Kobe Bryant.
- A rescue opportunity can appear when the claw has grabbed a prize and reached the top of the machine.
- The player may use it when concerned that the claw will drop the prize before reaching the collection portal.
- The opportunity appears only occasionally; it is not always available.
- When available, the Kobe button lights up.
- The player has approximately **1.5 seconds** to tap or click it.
- On activation, the character emerges from the prize pit and jumps to slam dunk the prize into the collection portal.
- The rescue has an **85% success rate**.

The frequency of opportunities has not been specified. The chance of an opportunity appearing is separate from the 85% success chance after activation.

## Recommended technology stack

These are the assistant's recommendations, not yet confirmed implementation decisions.

| Area | Recommendation | Intended role |
| --- | --- | --- |
| 3D rendering | Three.js | Render the machine, claw, prizes, character, lighting, and effects |
| Scene composition | React Three Fiber | Organize reusable 3D components and integrate the scene with React |
| Scene utilities | Drei | Camera helpers, model loading, lighting helpers, and effects |
| Physics | Rapier through React Three Rapier | Prize piles, collisions, falling objects, and collection detection |
| Application and interface | React + TypeScript | Menus, controls, collection gallery, and configurable game rules |
| Development and builds | Vite | Local development and browser production builds |
| Models and animation | Blender, exporting GLB assets | Create the cabinet, claw, prizes, and basketball player animations |
| Audio | Howler.js | Motor sounds, countdown cues, prize impacts, and celebrations |
| Hosting | Cloudflare Pages | Shareable browser URL and development preview deployments |

### Rendering recommendation

Start with Three.js's WebGLRenderer. At the time of the initial tooling review, the Three.js documentation recommended it for WebGL 2 applications and described WebGPURenderer as experimental. WebGPURenderer offers automatic WebGL 2 fallback and can be evaluated later.

Recheck renderer guidance and compatible dependency versions when implementation begins. Exact package versions have not been selected.

### Storage and backend

The proposed first version runs entirely in the browser and saves settings and collected prizes locally. Accounts, a backend, and cloud saves can be added later if progress needs to move between devices.

### Browser verification

Playwright was considered as a browser testing tool. The eventual game also needs hands-on testing on the actual target devices, particularly for touch input, responsiveness, frame rate, and the timed Kobe opportunity.

## Proposed gameplay design

The following design details are suggestions for discussion and prototyping.

### Overall play cycle

**Aim → lower → close → lift → carry → release into the portal → celebrate.**

Use a shadow or targeting marker under the claw to help players judge depth. Touch and keyboard controls should drive the same underlying movement rules. The exact controls and how much of the carry phase is manual remain to be decided.

### Physics and grip

Use believable physics with controlled arcade rules:

- Loose prizes tumble, collide, settle, and fall using physics.
- The claw uses a controlled grip system so a successful grab can be tuned reliably.
- Grip quality can depend on positioning, prize shape, weight, and how securely the fingers surround the prize.
- Good positioning should feel consistently rewarded.
- Difficulty can be tuned without depending entirely on a delicate physical simulation of claw fingers.

The exact grip model, slip behavior, and difficulty settings have not been designed or implemented.

### Proposed Kobe event sequence

1. A held prize reaches the top of the machine.
2. The game decides whether a rescue opportunity appears, using a configurable appearance frequency.
3. If available, the button lights up, a sound plays, and a visible 1.5-second countdown begins.
4. A valid activation makes one outcome roll: 85% success and 15% miss.
5. The character emerges, leaps, takes the prize, and performs an animation corresponding to the outcome.
6. A successful dunk awards the prize exactly once.
7. On a miss, the suggested behavior is for the prize to fall back into the pit.

Additional recommendations:

- Briefly stabilize the grip during the opportunity so the player receives the full reaction window. This changes the normal slipping behavior and needs confirmation.
- Measure the input window using elapsed time so slower and faster devices receive the same duration.
- Resolve the outcome once per valid activation, rather than repeatedly rolling during animation or rendering.
- Prototype and verify both success and miss outcomes.

The miss behavior, opportunity frequency, timeout behavior, and grip stabilization are not confirmed requirements.

## Machine and prize configuration

### One configurable machine system

Implement the six sizes as configurations of the same machine system rather than six independent implementations. A size configuration could define:

- Cabinet dimensions and play area bounds
- Claw size, reach, and movement range
- Prize population and pile depth
- Collection portal dimensions and location
- Camera position and framing
- Travel distances and movement tuning

Keep size and difficulty independently adjustable. A super machine could be a generous playground, while a small machine could require more precise positioning.

Larger machines need performance limits so additional prizes do not make controls sluggish. Exact prize counts and performance targets are still open.

### Separate prize sets

Keep prize sets separate from machine size configurations. Suggested examples include plush animals, dinosaurs, basketballs, and fantasy creatures; none has been chosen yet.

Each prize definition should include its visual model, dimensions, weight, collision shape, and grip behavior. Swapping only the visual model would not account for how different prize shapes should behave.

The first version's method for selecting or swapping prize sets remains undecided.

## Visual and audio direction

The creator expects to provide sample artwork or references for design discussion. Finished artwork is not required to begin a gameplay prototype; simple placeholder models can establish the mechanics first.

Suggested art direction:

- A colorful toy arcade aesthetic
- Rounded shapes and expressive prizes
- Warm lighting
- Satisfying mechanical movement and sound
- Clear countdown and opportunity feedback
- An energetic, celebratory dunk animation

This direction has not been approved. Reference pictures can help establish cabinet styling, prize appearance, colors, character proportions, and the desired balance between realism and stylization.

## Proposed first playable milestone

Build a focused version containing:

- One medium cabinet
- One prize set
- The complete claw play cycle
- Collection detection and prize rewards
- The Kobe opportunity and timed activation
- Both Kobe success and miss outcomes

Have the creator's daughter play this version. Use her reactions to tune controls, challenge, grip reliability, rescue timing, and enjoyment before expanding to all six sizes.

The milestone is a recommendation; implementation has not been authorized by this documentation request.

## Open decisions for resuming work

1. **Primary device:** tablet/phone, laptop/desktop, or both from the first playable version? This was asked but not answered in the captured discussion.
2. **Meaning of size:** larger cabinets and more prizes, larger prizes and claws too, or different themed play areas? This was asked but not answered.
3. **Scale:** dimensions, prize counts, pile depth, and travel distances for all six sizes.
4. **Art references:** preferred cabinet style, prize themes, character style, and visual mood.
5. **Controls:** aiming, lowering, carrying, releasing, and camera behavior on the target devices.
6. **Kobe appearance frequency:** how often an eligible lift offers the opportunity.
7. **Kobe edge cases:** timeout behavior, whether the grip stabilizes during the input window, and what happens after a miss.
8. **Difficulty:** grip strength, slipping rules, movement speeds, and any optional assistance.
9. **Prize swapping:** how the player or creator chooses collections and which collection comes first.
10. **Progression:** collection gallery details, unlocks, rewards, and whether cross-device saves are eventually needed.

## Suggested next session

Resume by settling target devices and the meaning of machine sizes, then reviewing any available artwork references. Confirm the first playable scope and stack, define the medium machine's initial configuration, and begin the gameplay prototype.

## Reference links from the tooling discussion

- [React Three Fiber introduction](https://r3f.docs.pmnd.rs/getting-started/introduction)
- [Drei introduction](https://drei.docs.pmnd.rs/getting-started/introduction)
- [Rapier JavaScript getting started](https://rapier.rs/docs/user_guides/javascript/getting_started_js/)
- [React Three Rapier repository and compatibility guidance](https://github.com/pmndrs/react-three-rapier)
- [Three.js renderer guidance](https://threejs.org/manual/pages/webgpurenderer)
- [Vite static deployment guide](https://vite.dev/guide/static-deploy.html)
- [Blender GLB/glTF workflow](https://docs.blender.org/manual/en/5.1/addons/import_export/scene_gltf2.html)
- [Howler.js](https://howlerjs.com/)
- [Playwright](https://playwright.dev/)

These links record the research used for the initial recommendations. Revisit current documentation when development resumes.
