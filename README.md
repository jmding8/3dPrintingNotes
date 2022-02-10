# 3dPrintingNotes

A collection of notes and lessons-learned related to 3d printing

## Slicer Comparison
Pros and cons associated with popular slicers, approximately ranked by importance
### Cura
Pros
1. Tree supports use less material and can support areas that are "blocked" by lower geometry.
1. ~~Fuzzy skin is useful for hiding surface quality issues~~ PrusaSlicer now also has this feature.
Cons
1. For retractions at the end of infill lines, Cura cannot insert wipe moves. Cura performs the retraction with the nozzle in-place. The lack of wipe leads to excess stringing.

### PrusaSlicer
Pros
1. Wipe and retraction are simultaneous, resulting in reduced stringing.
Cons
1. Inner perimeters and top/bottom solid infill are not supported by support structures. Only outer perimeters are supported.
1. With the legacy bridging behavior (thick_bridges: true), bridge lines are extruded at a volumetric flow rate given by {bridge_flow_rate (mm^3 / mm) = bridge_flow_ratio * nozzle_diameter^2 * Pi / 4}. It would be much better to be able to directly specify the ratio {bridge_line_width / layer_height ~= 120%}. This is because bridge lines are in free-air, so the lines have a circular cross-section. The diameter of this circle cross-section needs to be slightly larger than the layer_height to ensure adhesion with the layers above and below, and should not be too large otherwise the bridge will come out much thicker than specified input geometry (the stl). Although I don't thoroughly understand the new bridging behavior (thick_bridges: false), it seems to be barking up the wrong tree altogether.
1. 