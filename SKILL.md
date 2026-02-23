---
name: slicer
description: >
  Search and reason over 3D Slicer source code, extensions, and community discussions. 
  Use for questions about medical imaging, MRML scene graphs, VTK/ITK pipelines, 
  Slicer Python scripting, C++ module development, extension development, Qt-based 
  module UI, segmentation, volume rendering, DICOM workflows, and the Slicer build system.
version: "2.0"
---

# Slicer Skill

This skill provides an AI agent with the knowledge and strategy to answer questions about the [3D Slicer](https://www.slicer.org) application and its extension ecosystem.

**CRITICAL RULE: DO NOT ATTEMPT TO CLONE OR DOWNLOAD SLICER SOURCE CODE LOCALLY.** 
Instead, rely entirely on online search, GitHub repository search, and official documentation portals using your web search or web fetch tools.

## 🌐 Online Resources Strategy

When answering Slicer programming questions, use your web search tools to gather accurate information from the following primary sources:

### 1. GitHub Code Search (The Source of Truth)
Search the official Slicer repository for real-world implementation, API usage, and CMake patterns.
- **Repository:** `Slicer/Slicer` (https://github.com/Slicer/Slicer)
- **Strategy:** Search for specific class names, methods, or macros using GitHub search API or web search tools.
  - E.g., query for `arrayFromVolume` in `Slicer/Slicer`
  - E.g., query for `vtkMRMLScene` in `Slicer/Slicer`
  - E.g., search for examples in `Modules/Scripted` to find Python module references.

### 2. Slicer Documentation & Script Repository
The **Script Repository** is the most valuable resource for Python snippets.
- **Main Docs:** `https://slicer.readthedocs.io/`
- **Script Repository:** `https://slicer.readthedocs.io/en/latest/developer_guide/script_repository.html`
- **API Reference (Doxygen):** `https://apidocs.slicer.org/`
- **Strategy:** Search these domains for tutorials, Python cookbook recipes, and C++ class definitions. 
  - E.g., `site:slicer.readthedocs.io "Segment Editor"`

### 3. Slicer Discourse Forum
Community discussions often reveal the *why* behind Slicer's architecture and provide workarounds for common issues.
- **Forum:** `https://discourse.slicer.org/`
- **Strategy:** Search the forum for bug reports, feature explanations, or advanced usage.
  - E.g., `site:discourse.slicer.org "transform error"`

### 4. Extensions Index
To find examples of third-party extensions (plugins):
- **Repository:** `Slicer/ExtensionsIndex` (https://github.com/Slicer/ExtensionsIndex)
- **Strategy:** Look up how community extensions are configured or search for specific domain implementations (like `SlicerIGT` or `MONAILabel`).

---

## 🧠 Slicer Architecture & Common Pitfalls

Keep these fundamental concepts and common pitfalls in mind when generating Slicer code:

1. **MRML (Medical Reality Modeling Language)**
   - The scene graph that holds all data. Nodes are instances of `vtkMRML*Node` (e.g., `vtkMRMLVolumeNode`, `vtkMRMLModelNode`).
   - *Pitfall:* MRML node names are NOT unique identifiers. Always use `node.GetID()` for reliable identification, not `node.GetName()`.

2. **Python and Numpy (`slicer.util`)**
   - The `slicer.util` module contains the most common helper functions (e.g., `getNode()`, `arrayFromVolume()`).
   - *Pitfall:* `arrayFromVolume` returns a view, not a copy. After modifying the array in-place, you MUST call `slicer.util.arrayFromVolumeModified(volumeNode)` to notify the display pipeline. Forgetting this results in the UI not updating.
   - *Pitfall:* Volume axis ordering is KJI (slice, row, column), not IJK.

3. **Coordinate Systems**
   - Slicer uses RAS (Right-Anterior-Superior) internally. Many external tools use LPS (Left-Posterior-Superior). Pay close attention to sign flips when importing/exporting transforms or point data.

4. **UI Thread Blocking**
   - The Python console runs on the main Qt thread. Long-running operations will freeze the UI. Use `slicer.app.processEvents()` in loops or use background threads with `qt.QTimer.singleShot()` callbacks.

5. **Segment Editor**
   - It is a highly complex module. To automate it via Python, look for `slicer.modules.segmenteditor.widgetRepresentation()` and the `slicer.qMRMLSegmentEditorWidget` class.

## 📝 How to Process User Requests

1. **Understand the Goal:** Is the user writing a Scripted Module, a Loadable (C++) Module, or just running a snippet in the Python console?
2. **Search Before Guessing:** Do NOT hallucinate Slicer API calls. If you don't know the exact method, use web search to find examples in Slicer's documentation or the `script_repository.html`.
3. **Prefer Built-in Utilities:** Always check if `slicer.util` or VTK/ITK already provides the functionality before writing complex custom logic.