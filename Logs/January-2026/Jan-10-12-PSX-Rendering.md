[← Back to January Index](./README.md)

# Daily Study Log
**Date:** January 10 – January 12, 2026  
**Focus:** Rendering Pipelines & PS1 Aesthetic Engineering

---

##  Table of Contents
* [Chapter 1: Beginning the Project Through Visual Study](#chapter-1)
* [Chapter 2: Understanding Rendering and Post Processing](#chapter-2)
* [Chapter 3: Reducing Engine Rendering Quality Intentionally](#chapter-3)
* [Chapter 4: Learning About RHI and DirectX](#chapter-4)
* [Chapter 5: Understanding Vertex Snapping in Older Hardware](#chapter-5)
* [Chapter 6: Recreating Vertex Snapping Using Materials](#chapter-6)
* [Chapter 7: The Role of Post Processing in PS1 Rendering](#chapter-7)
* [Chapter 8: Color Banding and Dithering](#chapter-8)
* [Chapter 9: Final Visual Outcome](#chapter-9)
* [Chapter 10: Reflection on Learning](#chapter-10)

---

<a name="chapter-1"></a>
## <font color="#C0392B">Chapter 1</font>
> **Beginning the Project Through Visual Study**

We decided to begin the project by focusing on the visual appearance of the game rather than gameplay or level design. The reason for this choice was that the visual style defines the overall identity of the project, and the [PlayStation-1 aesthetic](https://en.wikipedia.org/wiki/PlayStation_graphics) is strongly shaped by how images are rendered rather than by complex geometry or textures. Before building any interactive systems, it was important for us to understand how older hardware produced its distinctive look and how that look can be recreated inside a modern engine.

The PS1 visual style is not the result of artistic intention alone, but rather the result of technical limitations. Understanding those limitations is necessary for us to recreate the style faithfully instead of merely applying surface-level filters.

---

<a name="chapter-2"></a>
## <font color="#2980B9">Chapter 2</font>
> **Understanding Rendering and Post Processing**



Rendering is the process by which a game engine converts three-dimensional data into a two-dimensional image on the screen. In modern engines, this process includes many advanced steps such as high-precision lighting, reflections, smooth edges, and post-processing effects. 

> [!Note]
> **[Post processing](https://docs.unrealengine.com/5.0/en-US/post-process-effects-in-unreal-engine/)** refers specifically to changes applied to the final image after the scene has already been drawn. This includes effects such as color adjustment, bloom, blur, and noise.

In older systems like the PlayStation-1, rendering was far simpler. The final image had limited color precision and lacked many of the smoothing techniques that are common today. Because of this, we recognized that post processing plays a central role in recreating the PS1 look, especially when it comes to controlling color depth and visual noise.

---

<a name="chapter-3"></a>
## <font color="#27AE60">Chapter 3</font>
> **Reducing Engine Rendering Quality Intentionally**

Modern versions of **Unreal Engine** enable many advanced rendering features by default in order to achieve realistic visuals. However, these features work against the PS1 aesthetic we desire. For this reason, the first practical step we took was to deliberately reduce the rendering quality of the engine.

[Ray tracing](https://en.wikipedia.org/wiki/Ray_tracing_(graphics)) was disabled because it simulates realistic light behavior that did not exist on older hardware. **Global illumination**, which allows light to bounce between surfaces, was also disabled in order to achieve flatter lighting. These changes remove modern softness and realism from the scene and help us restore the harsh lighting transitions characteristic of early 3D games.

---

<a name="chapter-4"></a>
## <font color="#8E44AD">Chapter 4</font>
> **Learning About RHI and DirectX**

While adjusting the rendering settings, we learned about the concept of the **[Rendering Hardware Interface (RHI)](https://docs.unrealengine.com/5.0/en-US/rhi-api-in-unreal-engine/)**. The RHI acts as a bridge between the game engine and the graphics hardware. It determines how drawing commands are sent to the GPU and which graphics API is used.

**DirectX** is one such graphics API used on Windows systems. Different versions of DirectX provide different levels of control and complexity:
* **DirectX 12:** Designed for modern hardware and advanced rendering techniques.
* **DirectX 11:** Follows a simpler and more predictable model. 

For this project, we chose DirectX 11 because its simpler behavior aligns better with older rendering styles and avoids modern optimizations that interfere with the desired visual output.

---

<a name="chapter-5"></a>
## <font color="#D35400">Chapter 5</font>
> **Understanding Vertex Snapping in Older Hardware**

One of the most noticeable visual traits of PlayStation-1 games is unstable geometry. When the camera moves, edges appear to wobble slightly and surfaces do not remain perfectly aligned. This behavior is known as **vertex snapping**.

[Vertices](https://en.wikipedia.org/wiki/Vertex_(computer_graphics)) are points in three-dimensional space that form the building blocks of all 3D models. In modern engines, vertex positions are stored with high precision using decimal values. In older hardware, numerical precision was limited, and vertex positions were often rounded to nearby values. As a result, smooth movement was not possible, and we observed that geometry appeared to shift slightly from frame to frame.

---

<a name="chapter-6"></a>
## <font color="#16A085">Chapter 6</font>
> **Recreating Vertex Snapping Using Materials**

<div align="center">
  <img src="/Media/Vertex_Snapping_.gif" width="600" alt="Vertex Snapping">
  <br>
  <b>Intentional Vertex Snapping & Geometric Instability</b>
</div>

#
To recreate this behavior, we implemented vertex snapping using the **[Material System](https://docs.unrealengine.com/5.0/en-US/unreal-engine-materials/)**. A material parameter collection was created to store a single global value that controls how strong the snapping effect is. This allows us to adjust the effect consistently across multiple materials.

We then created a material function to modify vertex positions. This function reduces the precision of the vertex coordinates by:
1. Dividing them.
2. Removing their decimal values.
3. Scaling them back.

The difference between the original and modified position is applied as an offset. When we connect this function to the **[World Position Offset](https://docs.unrealengine.com/5.0/en-US/unreal-engine-materials-for-world-position-offset/)** input of a material, the geometry begins to move in discrete steps rather than smoothly. 

---

<a name="chapter-7"></a>
## <font color="#2C3E50">Chapter 7</font>
> **The Role of Post Processing in PS1 Rendering**

After implementing vertex snapping, we shifted our attention to post processing. Post processing affects the final image rather than individual objects. This makes it ideal for controlling color behavior, which was a key limitation of PS1-era graphics.

Older hardware supported only a limited number of colors. Because of this, smooth gradients such as shadows or sky transitions appeared as visible steps rather than continuous blends. We identified this effect as **[Color Banding](https://en.wikipedia.org/wiki/Colour_banding)**.

---

<a name="chapter-8"></a>
## <font color="#E67E22">Chapter 8</font>
> **Color Banding and Dithering**

<div align="center">
  <img src="/Media/Post_Processing_Dithering_.gif" width="600" alt="Post Processing">
  <br>
  <b>Post Processing Effects</b>
</div>

#
To reproduce color banding, we created a post-process material that reduces the number of colors in the final image. This is done by forcing color values into fixed steps rather than allowing smooth transitions. While this creates an authentic look, it also produces harsh visual boundaries.

To soften these boundaries, we applied **[Dithering](https://en.wikipedia.org/wiki/Dither)**. Dithering alternates between nearby colors in a patterned way, spreading the error across multiple pixels. This does not increase color accuracy, but it makes transitions appear smoother to the human eye. We noted that this technique was widely used in PS1 games to hide hardware limitations.

---

<a name="chapter-9"></a>
## <font color="#7F8C8D">Chapter 9</font>
> **Final Visual Outcome**

<div align="center">
  <img src="/Media/Visual_Outcome_.gif" width="600" alt="Visual Outcome Demo">
  <br>
  <b>Demonstration: Final Visual Outcome</b>
</div>

#
By combining reduced engine rendering quality, vertex snapping, color banding, and dithering, we have achieved a final image that closely resembles the visual output of PlayStation-1 games. Geometry appears unstable during movement, colors are limited and noisy, and the overall image feels less precise and more mechanical.

We ensured these effects were not applied randomly; each one is the result of us deliberately recreating a specific hardware limitation from older systems.

---

<a name="chapter-10"></a>
## <font color="#34495E">Chapter 10</font>
> **Reflection on Learning**

The work we completed on January 12 focused on understanding how rendering limitations shape visual style. Rather than treating the PS1 look as a filter, we approached it as a technical problem rooted in precision, memory, and mathematical constraints. This understanding forms a strong foundation for us to continue the project in a structured and informed manner.