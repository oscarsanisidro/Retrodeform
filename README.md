# RetroDeform

**An online tool for the digital retrodeformation of 2D images and 3D anatomical models.**

RetroDeform removes taphonomic deformation from bilaterally symmetric structures (e.g. skulls) using paired landmarks placed on both sides of the specimen. It runs entirely in the web browser: nothing to install, and your files are never uploaded to a server.

🔗 **Launch the app:** https://oscarsanisidro.github.io/RetroDeform/

---

## Features

- **3D mode:** load a mesh (**OBJ** or **PLY**), place landmarks on the surface and export the retrodeformed mesh as **PLY**.
- **2D mode:** load an image (**PNG** or **JPG**, e.g. a photograph in dorsal or ventral view) and export the retrodeformed image as **PNG**.
- **Landmark export in TPS format:**
  - landmarks as placed (`*_landmarks.tps`)
  - symmetrized target configuration (`*_landmarks_symmetrized.tps`)
  - 3D files use `LM3=`; 2D files use `LM=` in pixel coordinates with the origin at the bottom left, as in tpsDig. A `SCALE=` line is added when a scale bar is set.
- **Display options:** the estimated sagittal plane (3D) or midline (2D) and a vertex-displacement colour map (3D).
- **Physical units:** displacement statistics in model units or pixels, or in mm after drawing a scale bar of known length.
- **TPS regularization:** adjustable through the α parameter.

## How to use

1. **Load** a 3D model (OBJ/PLY) or a 2D image (PNG/JPG). The mode is chosen automatically from the file type.
2. **Place landmarks:** switch to *Landmark* mode and click a point on the **right** side (🔴), then its counterpart on the **left** side (🔵). Repeat for each pair. Click on a landmark to remove it.
   - You need at least 2 pairs. 5–10 well-distributed pairs usually give the best results.
3. **Retrodeform:** click **▶ Retrodeform**, then toggle *Original / Retrodeformed* to compare.
4. **Set a scale (optional):** draw a scale bar of known length to express displacements in mm.
5. **Export** the retrodeformed mesh (PLY) or image (PNG), and the landmarks (TPS).

Landmarks are exported in the order R₁, L₁, R₂, L₂ …, in the coordinate frame of the original file.

## Method

RetroDeform follows the bilateral-symmetry approach implemented in `Morpho::retrodeform()` (Schlager et al. 2018):

1. **Symmetry plane:** estimated from the mean direction of the vectors joining each right–left landmark pair.
2. **Symmetrization:** each left landmark is reflected across the plane and averaged with its right counterpart. The symmetric left landmark is then the mirror image of that new right landmark.
3. **Thin Plate Spline warp:** a TPS deformation maps the whole mesh or image from the original (asymmetric) landmark configuration to the symmetric one.
   - In 2D, the image is resampled by inverse mapping with bilinear interpolation.

## How to cite

If you use RetroDeform in your research, please cite:

> Sanisidro, O. (2026). *RetroDeform: An online 3D retrodeformation tool* [Software]. https://oscarsanisidro.github.io/RetroDeform/

You can also use GitHub's **"Cite this repository"** button or the **❝ How to cite** button inside the app.

Please also cite the method:

> Schlager, S., Rüdell, A., & Wachter, R. (2018). Morpho and Rvcg — Shape Analysis in R. In G. Zheng, S. Li & G. Székely (Eds.), *Statistical Shape and Deformation Analysis* (pp. 217–256). Academic Press.

## Technical notes

- Single self-contained HTML file built with [three.js](https://threejs.org/) and [three-mesh-bvh](https://github.com/gkjohnson/three-mesh-bvh), both loaded from a CDN.
- Works in any modern browser (Chrome, Firefox, Edge, Safari). Very large meshes (> ~50 MB) may be slow.
- Browsers cannot read TIFF images; convert them to PNG first.

## Author

**Oscar Sanisidro**

© 2026 Oscar Sanisidro. See the [LICENSE](LICENSE) file for terms of use.
