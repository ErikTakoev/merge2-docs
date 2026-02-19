# Documentation Rules

[← Back to Main](../Main.md)

This document defines the standards for writing technical documentation for the Merge2 project.

## 1. General Principles
- **Language**: English.
- **Style**: Concise, technical, structured.
- **Format**: Markdown.

## 2. Document Structure
Each document should have a clear structure:
- **Navigation**: Link [← Back to Main](../Main.md) at the very beginning.
- **Title (# Title)**: Name of the component or system.
- **Description**: Brief explanation of what this is and what it's for.
- **Main Properties**: List of key parameters or characteristics.
- **Interaction**: Description of how this component interacts with other parts of the system.
- **Visual Effects (Optional)**: Description of visual accompaniment.

## 3. Specific Rules

### 3.1. Chip Documentation
Each chip type (e.g., `ChipGenerator`, `ChipContainer`) should have a separate file in the `Docs/Chips/` folder.
- **File Name**: Should match the class name (e.g., `ChipGenerator.md`).
- **Unique Effects**: If a chip has unique visual effects, they are described in this same document.
    > **Important**: The general document `Docs/Effects.md` should link to this document, not duplicate the description.

### 3.2. Links
- Use relative links to other documents (e.g., `[Tech Design](../Technical/TechnicalDesign.md)`).
- Link to code where possible.

### 3.3. Component Structure
Describe components through their classes and specific purpose, avoiding abstract architectural terms (View, Controller, Model).
- **Architecture and Responsibility**:
  - **`ClassName.cs`**: Brief description of what this class does.
  - **`DataClassName.cs`**: Description of settings.
  - **`LogicClassName.cs`**: Description of extracted logic.
- **Flow**: Step-by-step description of technical flow (Input -> Method -> Event).

### 3.4. Technical Design
The document `Docs/Technical/TechnicalDesign.md` is the central place for describing architecture.
- **Requirement**: This document should describe all interfaces from the `Modules/Merge2/Scripts/VContainer/Interfaces` folder.
- **Content**: For each interface, specify its purpose and concrete implementation (class).

### 3.5. Testing Documentation
The document `Docs/Technical/Testing.md` describes the testing approach and test infrastructure.
- **Requirement**: The document should contain a description of **each test** from the test classes.
- **Test Description Structure**:
  - Name of the test method.
  - What exactly the test checks (test goal).
  - What scenarios it covers.
- **Updates**: When adding a new test method, it must be documented in `Testing.md`.

## 4. Support and Maintenance
Documentation should be "living" and match the current state of the code.

### When to Update
- **Logic Change**: When changing the logic of component operation (e.g., spawn algorithm).
- **Data Extension**: When adding new fields to data (e.g., new parameters in `ChipData`).
- **Refactoring**: When changing architectural approaches or interfaces.

### What to Update
1. **Local files** (`Docs/Chips/`, `Docs/Features/`):
   - Updated whenever there are changes in the corresponding classes.
   - If new functionality is added — a new section is added.

2. **Technical Design** (`Docs/Technical/TechnicalDesign.md`):
   - Mandatory update when adding/changing **interfaces** in `VContainer/Interfaces`.
   - When changing the dependency graph (LifetimeScope).

3. **Main.md**:
   - Updated **only** when adding new `.md` files to add links to the table of contents.

