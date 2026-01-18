[← Back to January Index](./README.md)

# Daily Study Log
**Date:** January 15 – January 18, 2026  
**Focus:** PS1 Character Modeling & Global Repository Maintenance

---

## Table of Contents
* [Chapter 1: Low-Poly Head Construction & Silhouette](#chapter-1-low-poly-head-construction--silhouettechapter-1)
* [Chapter 2: UV Mapping & Pixel-Conscious Texturing](#chapter-2-uv-mapping--pixel-conscious-texturing)
* [Chapter 3: Texture Projection & Resolution Scaling](#chapter-3-texture-projection--resolution-scaling)
* [Chapter 4: The Cleanup: Maintenance of Legacy Repositories](#chapter-4-the-cleanup-maintenance-of-legacy-repositories)
* [Chapter 5: Reflection and Future Commitment](#chapter-5-reflection-and-future-commitment)

---

<a name="chapter-1"></a>
## Chapter 1: Low-Poly Head Construction & Silhouette

The Blender work began with a deliberate focus on structure rather than detail. Using photographic references taken from the front, side, and back, I blocked out a human head starting from a low-poly round cube. The constraint was intentional: the goal was to remain under 300 polygons while still achieving a readable and recognizable silhouette.

Instead of extruding complex facial features, the face was kept largely flat, with dedicated geometry added only for the nose. This approach closely mirrors how characters were built on 1990s hardware, where polygon budgets were extremely limited. In those systems, form and lighting were carried by the mesh silhouette, while facial identity was communicated primarily through textures rather than geometry.

---

<a name="chapter-2"></a>
## Chapter 2: UV Mapping & Pixel-Conscious Texturing

UV unwrapping was treated as a precision problem rather than a routine step. To identify distortion, I applied a pixel grid texture and inspected how evenly the grid flowed across the surface. Even minor stretching inside Blender becomes highly visible once the model is rendered with low-resolution textures, so significant time was spent adjusting and pinning UV vertices to preserve square, uniform pixels.

Symmetry was intentionally broken during this phase. By allocating separate UV space to each side of the face, I allowed for subtle asymmetries in shading and detail. This avoided the artificial look of perfectly mirrored faces and helped the character feel more human, even within strict low-poly constraints.

<div align="center">
  <img src="/Media/UV Layout.gif" width="600" alt="UV Layout">
  <br>
  <b>UV Layout</b>
</div>


---

<a name="chapter-3"></a>
## Chapter 3: Texture Projection & Resolution Scaling

Facial textures were painted using the reference photographs as projection stencils. The initial painting was done at a 1K resolution to retain flexibility and detail during the process. Once the texture was complete, I began downscaling it to smaller resolutions.

Reducing the texture size to 256×256 and 128×128 is what actually produces the characteristic PS1 look. However, this process is not automatic. Each reduction introduced new artifacts along UV seams and edges, requiring manual cleanup. The final result depended on repeatedly revisiting the texture to ensure that it remained readable and intentional at low resolutions, rather than simply degraded.

<div align="center">
  <img src="/Media/Head Silhouette Blocking.gif" width="600" alt="Head Silhouette Blocking">
  <br>
  <b>Head Silhouette Model</b>
</div>

---

<a name="chapter-4"></a>
## Chapter 4: The Cleanup: Maintenance of Legacy Repositories

The latter part of this period was dedicated to clearing technical debt from older projects. I revisited my University **Compiler Design** repository to refactor disorganized code and fix broken logic that had been left unresolved for months.

More significantly, I completely overhauled my **Portfolio** codebase. The project had accumulated unused functions, dead code paths, and inconsistent structure over time. I reorganized everything into a clean directory layout, fixed GitHub Pages pathing issues, and finally eliminated a long-standing audio ghosting bug using a decisive `async/await` shutdown approach. Stabilizing these repositories was necessary to prevent them from continuing to pull attention away from current work.

---

<a name="chapter-5"></a>
## Chapter 5: Reflection and Future Commitment

These past few days were about clearing the deck. The Blender workflow for PS1-style characters is now established, and long-standing distractions in older repositories have been resolved and pushed.

From this point forward, I am done with combined multi-day logs. All future entries will be written as individual daily updates. With the backlog cleared, my focus is now fully on consistent, day-by-day progress toward building the game itself—no more catch-up, no more maintenance cycles, just forward motion.

---
