# Documentation Rules

[← Back to Main](../Main.md)

This document defines standards for writing technical documentation for Merge Toolkit.

## 1. General Principles
- **Language**: Ukrainian.
- **Style**: Concise, technical, structured.
- **Format**: Markdown.

## 2. Document Structure
Each document must have a clear structure:
- **Navigation**: Link [← Back to Main](../Main.md) at the very beginning.
- **Title (# Title)**: Name of component or system.
- **Description**: Brief explanation of what it is and why it is needed.
- **Main Properties**: List of key parameters or characteristics.
- **Interaction**: Description of how this component interacts with other parts of system.
- **Visual Effects (Optional)**: Description of visual support.

## 3. Specific Rules

### 3.1. Chip Documentation
For each chip type (for example, `ChipGenerator`, `ChipContainer`) there must be a separate file in `Docs/Chips/` folder.
- **File Name**: Must match class name (for example, `ChipGenerator.md`).
- **Unique Effects**: If chip has unique visual effects, they are described in the same document.
    > **Important**: General document `Docs/Effects.md` must contain a link to this document, not duplicate description.

### 3.2. Links
- Use relative links to other documents (for example, `[Tech Design](../Technical/TechnicalDesign.md)`).
- Reference code where possible.

### 3.3. Component Structure
Describe components through their classes and specific purpose, avoiding abstract architectural terms (View, Controller, Model).
- **Architecture and Responsibility**:
  - **`ClassName.cs`**: Brief description of what this class does.
  - **`DataClassName.cs`**: Description of settings.
  - **`LogicClassName.cs`**: Description of extracted logic.
- **Process (Flow)**: Step-by-step description of technical flow (Input -> Method -> Event).

### 3.4. Technical Design
`Docs/Technical/TechnicalDesign.md` document is the central place for architecture description.
- **Requirement**: This document must describe all interfaces from `Modules/Merge2/Scripts/VContainer/Interfaces` folder.
- **Content**: For each interface, its purpose and concrete implementation (class) must be specified.

### 3.5. Testing Documentation
`Docs/Technical/Testing.md` document describes testing approach and test infrastructure.
- **Requirement**: Document must contain description of **every test** from test classes.
- **Test description structure**:
  - Test method name.
  - What exactly test verifies (test goal).
  - Which conditions/scenarios it covers.
- **Update**: When adding a new test method, its description must be added to `Testing.md`.


### 3.6. EN/UA Heading and Anchor Consistency
- For paired files in `EN/` and `UA/` (same relative path), heading structure must be synchronized (`#`, `##`, `###` and order).
- Heading text in paired files must be identical so generated Markdown anchors match.
- Do not duplicate translation within one heading (for example: `Container (Container)`).
- If heading changes in one language, update all internal `#fragment` links and make the same change in paired file.

### 3.7. Visual Effects Documentation
- In `Docs/Visuals/Effects.md` document, numbering and order of detailed effect descriptions must strictly match their IDs in `EffectConsts` constants table.
- Each constant name in table must be a link to its corresponding description section.

## 4. Support and Maintenance
Documentation must be "live" and match current code state.

### When to Update
- **Logic Change**: When component logic changes (for example, spawn algorithm).
- **Data Extention**: When adding new fields to data (for example, new parameters in `ChipData`).
- **Refactoring**: When changing architectural approaches or interfaces.

### What to Update
1. **Local files** (`Docs/Chips/`, `Docs/Features/`):
   - Always updated on changes in corresponding classes.
   - If new functionality is added — add new section.

2. **Technical Design** (`Docs/Technical/TechnicalDesign.md`):
   - Mandatory update when adding/changing **interfaces** in `VContainer/Interfaces`.
   - When dependency graph changes (LifetimeScope).

3. **Main.md**:
   - Updated **only** when new `.md` files are added, to add links to contents.

