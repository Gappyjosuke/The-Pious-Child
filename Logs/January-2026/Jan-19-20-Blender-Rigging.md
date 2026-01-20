[← Back to January Index](./README.md)

# Daily Study Log
**Date:** January 19, 2026  
**Focus:** Fundamental Character Rigging Concepts & Inverse Kinematics in Blender

---

## Table of Contents
* [Chapter 1: Understanding What Rigging Actually Is](#chapter-1-understanding-what-rigging-actually-is)
* [Chapter 2: Armatures, Bones, and Hierarchies](#chapter-2-armatures-bones-and-hierarchies)
* [Chapter 3: Building a Functional Skeleton for a Low-Poly Character](#chapter-3-building-a-functional-skeleton-for-a-low-poly-character)
* [Chapter 4: Parenting, Control Bones, and Separation of Concerns](#chapter-4-parenting-control-bones-and-separation-of-concerns)
* [Chapter 5: Inverse Kinematics as a Control System](#chapter-5-inverse-kinematics-as-a-control-system)
* [Chapter 6: Mirroring, Naming Conventions, and Automation](#chapter-6-mirroring-naming-conventions-and-automation)
* [Chapter 7: Binding the Mesh and Testing the Rig](#chapter-7-binding-the-mesh-and-testing-the-rig)
* [Chapter 8: Reflection on Rigging as a System Design Problem](#chapter-8-reflection-on-rigging-as-a-system-design-problem)

---

<a name="chapter-1-understanding-what-rigging-actually-is"></a>
## Chapter 1: Understanding What Rigging Actually Is

Today’s learning shifted away from modeling and texturing and into **[rigging](https://en.wikipedia.org/wiki/Skeletal_animation)**, which fundamentally changes how we think about characters. Rigging is not about animation itself; it is about building a **control system** that allows animation to happen efficiently and predictably.

A rig is essentially an interface between the animator and the mesh. Instead of directly manipulating vertices, we manipulate **[bones](https://en.wikipedia.org/wiki/Bone_(computer_graphics))**, and those bones influence the mesh through **[vertex weights](https://docs.blender.org/manual/en/latest/animation/armatures/skinning/index.html)**. The quality of a rig determines how easy or painful animation will be later. This made it clear that rigging is closer to system design than to art.

---

<a name="chapter-2-armatures-bones-and-hierarchies"></a>
## Chapter 2: Armatures, Bones, and Hierarchies

Rigging in Blender starts with an **[Armature](https://docs.blender.org/manual/en/latest/animation/armatures/index.html)**, which is a container for bones. Bones are not geometry; they are abstract transforms that define rotation points and movement directions.

We learned that bones exist in a **[hierarchy](https://en.wikipedia.org/wiki/Scene_graph)**. A parent bone passes its transformation to its children, while child bones can move independently without affecting the parent. This hierarchy is the backbone of all character motion.

The most important conceptual distinction here is between:
- The **armature** (the system)
- Individual **bones** (the components inside the system)

Naming and organizing bones correctly is critical, because Blender treats the armature and the bones very differently at a data level.

---

<a name="chapter-3-building-a-functional-skeleton-for-a-low-poly-character"></a>
## Chapter 3: Building a Functional Skeleton for a Low-Poly Character

The rig was built from the ground up, starting with a **root bone** placed at the base of the character. This root acts as the global controller, allowing the entire character to be moved, rotated, or reset easily.

From the root, a hip bone was created, followed by a simple spine chain: spine → chest → neck → head. Because the character is low-poly, this chain was intentionally minimal. The goal was not anatomical accuracy, but believable deformation and clean control.

Limbs were built starting from the hip and chest outward. Each limb was constructed on only one side first, reinforcing the idea that **[symmetry](https://docs.blender.org/manual/en/latest/animation/armatures/bones/editing/symmetrize.html)** should be automated, not manually duplicated.

---

<a name="chapter-4-parenting-control-bones-and-separation-of-concerns"></a>
## Chapter 4: Parenting, Control Bones, and Separation of Concerns

One of the most important ideas we learned today is the difference between **deformation bones** and **control bones**.

Deformation bones directly influence the mesh. Control bones do not deform the mesh at all; they exist purely to make animation easier. This separation prevents control logic from polluting deformation logic.

Control bones were explicitly marked as **non-deforming** using Blender’s **[Bone Properties](https://docs.blender.org/manual/en/latest/animation/armatures/bones/properties/index.html)** and disconnected from the deformation hierarchy. They were then parented directly to the root, ensuring predictable global behavior regardless of limb motion.

This separation mirrors good software design: logic and data are kept independent.

---

<a name="chapter-5-inverse-kinematics-as-a-control-system"></a>
## Chapter 5: Inverse Kinematics as a Control System

**[Inverse Kinematics (IK)](https://en.wikipedia.org/wiki/Inverse_kinematics)** was the most important concept learned today. Instead of rotating each bone manually (**[Forward Kinematics](https://en.wikipedia.org/wiki/Forward_kinematics)**), IK allows a chain of bones to be controlled by moving a single target.

By assigning **[IK constraints](https://docs.blender.org/manual/en/latest/animation/constraints/tracking/ik_solver.html)** to the lower leg and lower arm, movement became intuitive. Moving an IK bone automatically rotated the upper and lower segments in a physically believable way.

A **[pole target](https://docs.blender.org/manual/en/latest/animation/constraints/tracking/ik_solver.html#pole-target)** was added to the leg to control the direction of the knee bend. This clarified that IK is not magic—it is math with constraints, and those constraints must be guided to avoid ambiguity.

---

<a name="chapter-6-mirroring-naming-conventions-and-automation"></a>
## Chapter 6: Mirroring, Naming Conventions, and Automation

Rigging efficiency depends heavily on **[naming conventions](https://docs.blender.org/manual/en/latest/animation/armatures/bones/editing/naming.html)**. By correctly naming bones with `.L` suffixes, Blender was able to automatically mirror the entire rig to the opposite side.

This demonstrated that rigging is not just about placement, but about **data discipline**. A single incorrect character in a bone name breaks automation completely.

Once mirrored, Blender automatically generated the `.R` counterparts, saving significant time and ensuring symmetry across the rig.

---

<a name="chapter-7-binding-the-mesh-and-testing-the-rig"></a>
## Chapter 7: Binding the Mesh and Testing the Rig

With the rig complete, the mesh was bound using **[automatic weights](https://docs.blender.org/manual/en/latest/animation/armatures/skinning/parenting.html#with-automatic-weights)**. This step connects the armature to the mesh by calculating how much influence each bone should have over nearby vertices.

After correcting a missing parent relationship between the hip and root, the rig behaved as expected. The character could now:
- Move globally using the root
- Bend naturally using IK controls
- Rotate limbs and feet independently
- Reset poses cleanly using **[pose transforms](https://docs.blender.org/manual/en/latest/animation/armatures/posing/editing/clear.html)**

For a low-poly character, the automatic weighting was sufficient without manual **[weight painting](https://docs.blender.org/manual/en/latest/sculpt_paint/weight_paint/index.html)**.

<div align="center">
  <img src="/Media/Full_Rig_Pose_Test.gif" width="600" alt="Full_Rig_Pose_Test">
  <br>
  <b>Full Rig Pose Test</b>
</div>

---

<a name="chapter-8-reflection-on-rigging-as-a-system-design-problem"></a>
## Chapter 8: Reflection on Rigging as a System Design Problem

Rigging fundamentally changed how we view character creation. It is not an artistic afterthought—it is a mechanical system that must be designed with intention. Poor rigging decisions compound into animation pain later, while good rigs disappear into the background and let creativity flow.

What stood out most today is how closely rigging resembles engineering: hierarchy, constraints, control flow, and abstraction all matter. Understanding this makes animation feel less mysterious and more deterministic.

Tomorrow, this rig will become the foundation for animation work. Today was about building the control system that makes that possible.

---
