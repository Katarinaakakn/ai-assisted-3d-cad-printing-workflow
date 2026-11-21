# AI-Assisted 3D, CAD, and Printing Workflow

**Audience:** Makers, product designers, educators, and small teams evaluating AI-generated geometry for real fabrication.  
**Language:** English (`en`).  
**Release state:** local review only.

A 3D image, mesh, CAD model, slicer project, and machine program are not interchangeable. This guide follows one proposed part through those representations and asks what evidence must survive each conversion. Its goal is not to rank generators; it is to prevent a plausible render from bypassing engineering review.

## Choose by the part you need

| Reader need | Start with | Why this route | Do not substitute |
|---|---|---|---|
| A visual concept from one reference image | TripoSR or TripoSG; use InstantMesh when investigating its multi-view reconstruction approach | Each begins from visual input and yields candidate 3D geometry | An inferred back surface is not a measured interface |
| A text-conditioned shape experiment | Shap-E; review Tencent XR 3DGen for its separate PandoraX shape/texture research stack | Research output supports exploration, not dimensioned engineering | XR 3DGen is not the Hunyuan product family |
| Repeated functional geometry with named dimensions | CadQuery; OpenSCAD for declarative constructive geometry | Parameters remain inspectable and repeatable | A textured generator mesh does not encode design constraints |
| Interactive reconstruction or inspection | FreeCAD | Editable solids, dimensions and exchange-format inspection belong here | Opening a mesh is not automatic parametric recovery |
| Natural-language-to-CAD exploration | CADAM | It explicitly generates CAD-oriented outputs and offers STL/SCAD/DXF export | Generated executable geometry still needs code and dimension review |
| Architectural/BIM input | IfcOpenShell before part-specific CAD | Preserve IFC units/properties and identify the actual component | IFC semantics are not a printer manufacturing contract |
| Toolpath preparation after geometry review | Cura | Reviewed model plus exact machine/material profile become a slicing proposal | No generator supplies machine-specific G-code approval |

LGM offers a separate multi-view Gaussian research representation. Include it when Gaussian-to-mesh conversion is itself part of the experiment; omit that conversion when the decision is simply to create a parameter-first spacer. These choices are based on declared representations, not unmeasured speed or geometry rankings.

## Worked handoff: a cable spacer, not a load-bearing part

The [illustrative part passport](assets/part-passport-example.json) chooses CadQuery → FreeCAD inspection → Cura. It records an 8.5 mm slot with a ±0.3 mm illustrative tolerance and a three-position caliper method. Its observation array is deliberately empty: no print or measurement occurred. This is a completed planning example, not a fabricated manufacturing result.

1. Keep slot width as a named parameter rather than asking an image model to infer it.
2. Compare native CAD and STEP units, bounding boxes, and solid count before mesh conversion.
3. Inspect the mesh-to-slicer handoff for tessellation loss, machine profile, material and support contact.
4. Hold release until the recorded coupon dimensions and defect observations exist.

A figurine concept would select a generator and topology repair route instead; it would not inherit this spacer's tolerance. A structural bracket would need an additional engineering load/material review, so this illustrative route does not approve one.

## The part passport

Before any prompt or reference image enters a model, create the [manufacturability gate ledger](assets/manufacturability-gates.yaml). Record units, coordinate frame, interfaces, tolerance, loads, prohibited geometry, material, process, printer envelope, reference rights, and the person who owns the final engineering decision. The passport stays with the part through every revision.

## Gate 0 — a bounded design envelope

Record units, coordinate frame, interfaces, tolerances, load cases, forbidden geometry, material, process, printer envelope, and rights to every reference input. A missing tolerance remains unknown; it is never inferred from a screenshot. Separate cosmetic surfaces from functional faces. Mark holes, threads, bearings, snap fits, heat zones, food or skin contact, and every load path that needs domain review.

## Gate 1 — generation provenance, not design approval

Hash prompt, model/checkpoint, seed, configuration, generated files, and post-processing. Never infer hidden dimensions from a visually convincing surface. Record whether output is an image, point cloud, implicit representation, triangle mesh, or boundary representation. Preserve every intermediate hash. Model and weight terms may differ from repository code terms, and rights to a reference image do not automatically cover a generated derivative.

## Gate 2 — the geometry clinic

Inspect scale, coordinate orientation, open edges, non-manifold edges, self-intersections, inverted normals, disconnected shells, degenerate faces, minimum wall, minimum gap, and hidden cavities. A repair tool may close a surface by inventing geometry; keep before/after hashes and inspect the repaired region. If the generator inferred an occluded side, label that surface as unobserved rather than “reconstructed.”

## Gate 3 — recover editable intent

Use a parametric CAD or scripting system to recover datums, constraints, named dimensions, interfaces, wall thickness, and revision history. Recreate critical geometry around named datums and constraints. A mesh may remain as a visual reference, but interfaces, clearances, and load-bearing geometry need reviewed parameters. Export and re-import one neutral format, compare units and bounding boxes, and retain the native editable source.

## Gate 4 — slicing is a manufacturing proposal

Inspect manifoldness, normals, minimum features, clearances, overhangs, support, orientation, material profile, and generated machine-code provenance. Review orientation, support contact, seam, bridging, overhang, layer height, wall count, infill, temperatures, cooling, acceleration, travel, estimated material, and machine target. Store the exact printer and material profile. Never send generated G-code to a different machine merely because the file opens.

## Gate 5 — coupon, measurement, and disposition

Print a bounded coupon, record machine/material/environment, measure agreed features, and keep failures. Passing one coupon does not certify another machine or load case. Measure agreed dimensions with a named instrument and method. Record warping, layer adhesion, support damage, surface defects, mass, and failed attempts. A coupon can validate a bounded material/machine/profile combination; it cannot certify the final part's strength or regulated use.

## Toolchain by handoff

### Concept generators

- **[LGM](https://github.com/3DTopia/LGM) — image-to-3D research.** Generate a 3D representation from image inputs for concept exploration. **Gate:** Generated appearance does not establish scale, topology, closed volume, reverse-side correctness, or printable details. **License:** MIT.
- **[Tencent XR 3DGen](https://github.com/Tencent/Tencent-XR-3DGen) — 3D generation research stack.** Explore text/image-conditioned asset generation and related post-processing. **Gate:** Model weights, generated-asset rights, compute, topology, texture, and commercial-use terms require separate review. **License:** repository-specific and model-specific terms.
- **[TripoSG](https://github.com/VAST-AI-Research/TripoSG) — image-to-3D research.** Generate candidate geometry from visual references. **Gate:** A candidate mesh must pass topology, scale, interface, and manufacturability gates before CAD reconstruction. **License:** MIT repository code; verify model terms.
- **[TripoSR](https://github.com/VAST-AI-Research/TripoSR) — single-image 3D reconstruction research.** Create rapid concept meshes from one image. **Gate:** Occluded surfaces are inferred, not observed; preserve uncertainty and never derive safety-critical geometry from one view. **License:** MIT repository code; verify model terms.
- **[Shap-E](https://github.com/openai/shap-e) — conditional 3D generation research.** Explore generated implicit/mesh assets from prompts or images. **Gate:** Research checkpoints and samples are not dimensioned manufacturing models; verify licenses and conversion losses. **License:** MIT repository code; verify model terms.
- **[InstantMesh](https://github.com/TencentARC/InstantMesh) — single-image mesh generation research.** Generate multi-view-informed concept meshes. **Gate:** View synthesis and mesh reconstruction can invent hidden geometry; inspect topology and rebuild interfaces parametrically. **License:** Apache-2.0.

### Engineering reconstruction

- **[FreeCAD](https://github.com/FreeCAD/FreeCAD) — parametric CAD.** Reconstruct dimensions, constraints, assemblies, and editable design intent. **Gate:** Imported AI meshes do not become constrained solids automatically; verify units, shape healing, and export round trips. **License:** LGPL-2.1.
- **[CadQuery](https://github.com/CadQuery/cadquery) — scripted parametric CAD.** Express repeatable solids and dimensions in Python. **Gate:** A generated script remains untrusted code and geometric intent must be reviewed; test kernel/version reproducibility. **License:** Apache-2.0.
- **[OpenSCAD](https://github.com/openscad/openscad) — declarative solid modelling.** Create deterministic constructive geometry from reviewed parameters. **Gate:** Mesh import and rendering do not prove manifoldness or mechanical suitability; retain units and source parameters. **License:** GPL-2.0.
- **[IfcOpenShell](https://github.com/IfcOpenShell/IfcOpenShell) — open BIM/CAD semantics.** Inspect and transform IFC geometry and properties when building semantics matter. **Gate:** IFC semantics and tolerance rules differ from printable-part CAD; test schema, units, and tessellation boundaries. **License:** LGPL-3.0.
- **[CADAM](https://github.com/Adam-CAD/CADAM) — language-model CAD research.** Study an AI-assisted CAD generation approach and its declared evaluation surface. **Gate:** Research output is not a production CAD authority; verify model/license, executable output, dimensions, and failure cases independently. **License:** GPL-3.0.

### Fabrication handoff

- **[Cura](https://github.com/Ultimaker/Cura) — slicer.** Turn a reviewed model and machine/material profile into a toolpath preview and G-code. **Gate:** A successful slice is not a safe print; verify profile ownership, supports, estimated material, machine limits, and G-code target. **License:** LGPL-3.0.

## Failure atlas

| Failure | Where it enters | How it is detected | Disposition |
|---|---|---|---|
| Invented hidden surface | single-view generation | compare views and mark unobserved regions | rebuild or collect more views |
| Unit/scale loss | format conversion | bounding-box and named-dimension check | reject conversion |
| Non-manifold shell | generation or mesh repair | topology inspection | repair with reviewed delta or reject |
| Lost design intent | mesh-to-CAD handoff | missing datums/constraints | parametric reconstruction |
| Unsafe toolpath target | slicer/profile mismatch | machine/profile identity check | block export |
| False confidence from one print | coupon stage | repeat and measure against acceptance | keep state bounded, not certified |

## Release packet

A release packet contains the approved native CAD hash, neutral export hash, slicer project hash, generated machine-program hash, printer/material profile identity, coupon measurements, exceptions, reviewer, and revision supersession. The state remains `LOCAL_REVIEW_ONLY` until a separately authorized owner approves the exact bytes.

## Evidence discipline

The canonical source register is [`sources.json`](sources.json). Run `python3 scripts/verify.py` to check exact source/evidence binding, acceptance hashes, synthetic fixture invariants, English narrative, and the presence of the [manufacturability gate ledger](assets/manufacturability-gates.yaml). Reachability is transport evidence only. Re-read release guidance, licenses, model terms, and security notices before adoption.

## Source upkeep and reuse

The source register links exact-revision evidence excerpts. Recheck on upstream deprecation, license changes, representation changes or fixture drift; otherwise avoid date-only updates. Original guide and examples: CC BY 4.0; linked projects retain their terms.
