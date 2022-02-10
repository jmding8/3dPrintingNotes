# 3dPrintingNotes

A collection of notes and lessons-learned related to 3d printing

## Slicer Comparison
Pros and cons associated with popular slicers, approximately ranked by importance
### Cura
Pros
1. Tree supports use less material and can support areas that are "blocked" by lower geometry.
1. ~~Fuzzy skin is useful for hiding surface quality issues~~ PrusaSlicer now also does fuzzy skin, this is no longer a positive point unique to Cura.

Cons
1. For retractions at the end of infill lines, Cura cannot insert wipe moves. Cura performs the retraction with the nozzle in-place. The lack of wipe leads to excess stringing.

### PrusaSlicer
Pros
1. Paint on supports.
1. Paint on seams.
1. Wipe and retraction are simultaneous, resulting in reduced stringing.
1. If-statements in custom gcode.

Cons
1. Inner perimeters and top/bottom solid infill are not supported by support structures. Only outer perimeters are supported.
1. With the legacy bridging behavior (thick_bridges: true), bridge lines are extruded at a volumetric flow rate given by {bridge_flow_rate (mm^3 / mm) = bridge_flow_ratio * nozzle_diameter^2 * Pi / 4}. It would be much better to be able to directly specify the ratio {bridge_line_width / layer_height ~= 120%}. This is because bridge lines are in free-air, so the lines have a circular cross-section. The diameter of this circle cross-section needs to be slightly larger than the layer_height to ensure adhesion with the layers above and below, and should not be too large otherwise the bridge will come out much thicker than specified input geometry (the stl). This is inconvenient and unintuitive, but can be worked around.
  1. Although I don't thoroughly understand the new bridging behavior (thick_bridges: false), it seems to be barking up the wrong tree altogether, solving non-existant problems and introducing new ones.
1. Wipe moves are performed at travel_speed. This is too fast, they should really be performed at the same speed as whatever feature was being printed just prior to the wipe.
1. PrusaSlicer doesn't align seams very consistently. Even with paint-on seams, adjacent layers' seams are typically misaligned by up to ~1mm. This produces a jagged that is more visually distracting than a precisely aligned one. Additionally, the surface quality of the area just downstream / counter-clockwise of the seam is also notably worse due to the extrusion flow re-stabilization occuring out-of-sync between layers.
1. Extrusion line-width must be the same for both the regular / bulk support structure and the support interface layers. Ideally, the bulk support structure should be printed with a relatively sane line-width (roughly 2x layer height) for solidity and reliability. Meanwhile, support interface should be printed with borderline, excessively narrow line-widths (roughly 1.2x layer height) to reduce layer adhesion with the main part. There's no way to achieve this currently. The best workaround is to print the entire support structure with layer height ~1.2x layer height, and relatively slowly (20 mm/s) to prevent the very frail bulk support structure from breaking apart during printing.