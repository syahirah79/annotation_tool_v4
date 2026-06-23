# Standard Operating Procedure (SOP)
# Advanced Vehicle Dataset Tool (AVDT)
**Version:** v25 | **Last Updated:** February 2026

---

## Table of Contents

1. [Overview](#1-overview)
2. [Setup & Opening the Tool](#2-setup--opening-the-tool)
3. [Interface Layout](#3-interface-layout)
4. [Loading Images](#4-loading-images)
5. [YOLO Pre-Labels Import](#5-yolo-pre-labels-import)
6. [Setting Image Properties](#6-setting-image-properties)
7. [Drawing Bounding Boxes](#7-drawing-bounding-boxes)
8. [Annotation Types & Colour Coding](#8-annotation-types--colour-coding)
9. [Filling the Vehicle Form](#9-filling-the-vehicle-form)
10. [Updating Annotations with Form Data](#10-updating-annotations-with-form-data)
11. [Multiple License Plates](#11-multiple-license-plates)
12. [Parent–Child Relationships](#12-parentchild-relationships)
13. [Chain of Thought (CoT)](#13-chain-of-thought-cot)
14. [Fullscreen Mode & Navigation](#14-fullscreen-mode--navigation)
15. [Saving & Exporting](#15-saving--exporting)
16. [Multi-Image Batch Workflow](#16-multi-image-batch-workflow)
17. [Browser Storage Management](#17-browser-storage-management)
18. [Undo / Redo](#18-undo--redo)
19. [Keyboard Shortcuts Reference](#19-keyboard-shortcuts-reference)
20. [JSON Output Format](#20-json-output-format)
21. [Screenshot Suggestions](#21-screenshot-suggestions)

---

## 1. Overview

AVDT is a single-file browser-based annotation tool for labelling vehicle images captured at road monitoring points (Toll, WIM, Freeflow). It produces structured JSON annotations compatible with the `imap_annotation` schema, pairing bounding boxes with rich vehicle metadata.

**Key capabilities:**
- Draw bounding boxes for vehicles, license plates, wheels, make/logos, and attachments
- Fill a structured form with vehicle attributes, mechanical properties, and license plate data
- Import YOLO `.txt` pre-label files to pre-populate bounding boxes
- Save annotated PNG images and JSON files to a chosen local directory
- Batch process entire image directories with auto-save to browser localStorage

---

## 2. Setup & Opening the Tool

### Requirements
- A modern Chromium-based browser (Chrome, Edge) — **required** for the File System Access API used for saving files
- No installation needed — the tool runs entirely in the browser
- JavaScript must be enabled

### Steps
1. Open `index_v25.html` directly in Chrome/Edge (double-click or drag into browser tab)
2. The tool loads with a blank upload state
3. No server or internet connection is required after the file is open

> **📸 Screenshot suggestion:** Full tool on first load — showing the upload zone, empty left panel, and right panel with Image Properties and Vehicle Properties forms.

---

## 3. Interface Layout

The tool has a **three-panel layout**:

```
┌─────────────────────────────────────────────────────────────────┐
│  Left Panel               │   Centre Panel    │  Right Panel    │
│  (Annotation Controls)    │   (Image Canvas)  │  (Forms)        │
│                           │                   │                 │
│  • Storage Status Bar     │  • Image + BBoxes │  • Image Props  │
│  • Upload Controls        │  • Zoom Controls  │  • Vehicle Form │
│  • Dataset Navigator      │  • Magnifier      │  • Quick Actions│
│  • Annotation Type Picker │  • Fullscreen btn │  • Selected Ann │
│  • Annotation List        │                   │                 │
│  • CoT Section            │                   │                 │
│  • Keyboard Shortcuts     │                   │                 │
└─────────────────────────────────────────────────────────────────┘
```

> **📸 Screenshot suggestion:** Annotated screenshot of the interface with all three panels labelled.

---

## 4. Loading Images

### Single Image

1. Click **"📂 Choose Image File"** in the left panel, or drag and drop an image onto the upload zone
2. The image will appear in the centre canvas
3. If this image was previously annotated (saved in browser storage), annotations will automatically restore

### Image Directory (Batch Mode)

1. Click **"📁 Choose Image Folder"** to load an entire folder of images
2. A dataset index table appears in the left panel listing all images with status icons:
   - `🟡` — image only, not yet annotated
   - `✅Y` — image paired with a YOLO label file
   - `✅` — already annotated and saved in storage
3. Click any row in the dataset table to jump to that image
4. Navigate between images using **Arrow Left/Right** (in fullscreen) or the dataset table

> **📸 Screenshot suggestion:** Dataset index table showing multiple images with mixed status icons (🟡, ✅Y, ✅).

---

## 5. YOLO Pre-Labels Import

Use this feature when YOLO `.txt` label files have been pre-generated to provide initial bounding boxes.

### Steps

1. **Load images first** (single or folder)
2. Click **"🏷️ Choose YOLO Labels Folder"** and select the folder containing `.txt` files
3. The tool automatically matches `.txt` files to images by base filename (e.g., `image001.jpg` ↔ `image001.txt`)
4. Matched images show `✅Y` status in the dataset table
5. When you navigate to a paired image, YOLO boxes are automatically drawn on the canvas with class labels shown as tooltips
6. **YOLO boxes default to type `vehicle`** — change the type using number keys `1–6` after selecting a box

> **📸 Screenshot suggestion:** Canvas showing pre-loaded YOLO bounding boxes drawn on an image, with class label tooltips visible.

### YOLO Class Mapping
YOLO class IDs are mapped to readable labels automatically. Each YOLO-imported box shows the detected class name in the annotation list.

---

## 6. Setting Image Properties

Fill in the **Image Properties** form in the **right panel** for every image.

| Field | Options |
|-------|---------|
| **Site Location** | Toll / Weight in Motion (WIM) / Freeflow |
| **Scene** | Day / Night / Twilight |
| **Camera Position** | Front / Rear / Side |
| **Image Type** | RGB / gray |
| **Rain** | Checkbox — tick if rain is visible |

> **⚠️ Important:** These fields are per-image and must be set before exporting. They are saved automatically with annotations.

> **📸 Screenshot suggestion:** Close-up of the Image Properties section in the right panel with dropdowns expanded.

---

## 7. Drawing Bounding Boxes

### Entering Drawing Mode

- Press **`W`** to toggle drawing mode ON/OFF
- The cursor changes to a crosshair when drawing mode is active
- Drawing mode is also indicated by the active annotation type button being highlighted

### Drawing a Box

1. Press **`W`** to enter drawing mode
2. Select the annotation type from the type picker buttons (Vehicle, License Plate, Wheel, Attachment, Make)
3. **Click and drag** on the image to draw a rectangle
4. Release the mouse to confirm the box — it appears immediately with a colour-coded border
5. Press **`W`** again or **`Escape`** to exit drawing mode

### Editing an Existing Box

- **Click** any box to select it (it gains a highlighted border)
- **Drag** the box to reposition it
- **Drag corner/edge handles** to resize it
- Press **`Delete`** or **`Backspace`** to remove the selected box

> **📸 Screenshot suggestion:** Canvas in drawing mode with a box being drawn (dashed outline) and another completed box already visible.

### Zoom Controls (Canvas Overlay)

The bottom-left corner of the image shows zoom controls:
- **`+`** — zoom in
- **`−`** — zoom out
- **`⌂`** — reset zoom to fit
- **`🔍`** — toggle magnifier lens (follows cursor for precision drawing)

> **📸 Screenshot suggestion:** Zoom control buttons and magnifier active on canvas, showing the circular magnifier lens near the cursor.

---

## 8. Annotation Types & Colour Coding

Select the annotation type **before drawing** using the type picker buttons in the left panel:

| Key | Type | Colour | Description |
|-----|------|--------|-------------|
| `1` | **Vehicle** | 🟢 Green | The main vehicle body |
| `2` | **License Plate** | 🔵 Blue | Any visible license plate |
| `3` | **Wheel (Single)** | 🟠 Orange | Individual wheel |
| `4` | **Wheel (Double)** | 🟠 Orange | Dual/tandem wheel pair |
| `5` | **Attachment** | 🟣 Purple | Trailer, container, towed item |
| `6` | **Make/Logo** | 🩷 Pink | Vehicle brand emblem/logo |

> **Note:** Number keys `1–6` only work **when a box is already selected** — they change the type of the selected annotation. To draw a box of a specific type from scratch, use the type picker buttons in the panel.

> **📸 Screenshot suggestion:** All annotation types visible on one image — green vehicle box, blue plate box, orange wheel box, purple attachment box, pink make box — all colour coded.

---

## 9. Filling the Vehicle Form

The **Vehicle Properties** form is in the **right panel**. Fill this form after drawing vehicle boxes.

### 9.1 Basic Information

| Field | Options / Notes |
|-------|-----------------|
| **Category** | 18 categories: Car, SUV, MPV_Small, MPV_Big, Pickup_Truck, Bus, Motorcycle, Vans, Lorry, Small_Truck, Medium_Truck, Large_Truck, Heavy_Truck, Construction/Industrial, Emergency_Vehicle, Tanker, Trailer, Container |
| **Subcategory** | Depends on category. Select after choosing category. |
| **Color** | White, Black, Silver, Gray, Red, Blue, Green, Yellow, Brown, Orange, Purple, Gold, Maroon, Other |
| **Direction** | Northbound / Southbound / Eastbound / Westbound |

#### Bus / Van Special Logic

- **Bus category** → `is_bus` is automatically set to `true`; a **Bus Type** dropdown appears (City, School, Tour, Double Decker, Shuttle)
- **Vans category** → an **"Is Bus"** checkbox appears; tick it if the van is operated as a bus; Bus Type selector appears beside the checkbox when ticked
- **All other categories** → `is_bus` is `false`, no checkbox shown

### 9.2 License Plate Information

See [Section 11](#11-multiple-license-plates) for the full multi-plate workflow.

### 9.3 Mechanical Properties

| Field | Notes |
|-------|-------|
| **Inferred Axles** | Number of axles visible/inferred (integer) |
| **Raised Axles** | Number of axles lifted off the road (integer) |
| **Trailer Labels Visible** | Checkbox — company/cargo markings visible on trailer |
| **Cargo Present** | Checkbox — vehicle is carrying visible cargo/load |

### 9.4 Vehicle Attributes

| Field | Description |
|-------|-------------|
| 🚕 **Taxi** | Vehicle is a taxi |
| 🚨 **Emergency** | Police / Fire / Ambulance |
| ⚡ **Electric** | EV or hybrid vehicle |

### Quick Presets

Use the preset buttons at the bottom of the Vehicle Properties section:
- **🔄 Reset Form** — clear all vehicle form fields to defaults
- **🚛 Heavy Vehicle Preset** — sets category to Heavy_Truck, 5 axles, cargo present, trailer labels visible
- **🚌 Bus Preset** — sets category to Bus, is_bus=true, bus_type=city, 3 axles

> **📸 Screenshot suggestion:** The full Vehicle Properties form filled out for a heavy truck — category, axles, cargo checked, with the "Update Selected Annotation" button highlighted.

---

## 10. Updating Annotations with Form Data

> **⚠️ Critical step — do not skip!**

After filling the vehicle form, you must **link the form data to a specific vehicle bounding box**.

### Steps

1. **Click** the vehicle bounding box on the canvas to select it (it highlights)
2. Verify the box is selected — the **"📝 Update Selected Annotation ✅"** button becomes active (amber/orange)
3. Click **"📝 Update Selected Annotation ✅"**
4. The form data is now saved to that specific annotation object

> **If no annotation is selected**, the button shows ❌ and is disabled.

> **⚠️ Note:** This button only works for `vehicle` and `make` type annotations. For other types (plate, wheel, attachment), use the **type dropdown** in the "Selected Annotation" panel that appears when you click an annotation, or use keys `1–6` to change type.

> **📸 Screenshot suggestion:** Vehicle box selected (highlighted border) with the amber "Update Selected Annotation ✅" button visible and active in the right panel.

---

## 11. Multiple License Plates

Each vehicle can have **one or more license plates** stored in the form.

### Adding Plates

1. The License Plate section starts with **Plate #1** already shown
2. Fill in the fields for Plate #1
3. To add another plate, click **"+ Add Plate"**
4. A new **Plate #2** card appears — fill it in the same way
5. Repeat as needed for vehicles with multiple plates (e.g., truck + trailer)

### Plate Fields (per plate)

| Field | Options |
|-------|---------|
| **Plate Text** | Free-text input — enter the plate number exactly |
| **Country** | MY / SG / TH / ID |
| **Format** | License_Plate_Normal / (other formats) |
| **Visible** | Checkbox — plate is visible in the image |
| **Blurred** | Checkbox — plate is blurred / obscured |

### Removing a Plate

- Click **"✕ Remove"** on the plate card header (only shown when there are 2+ plates)

### Matching Plates to Bounding Boxes

> **⚠️ Order matters!**

Plate data is matched to plate bounding boxes **by position order**:
- The **1st license_plate bbox drawn** receives data from **Plate #1** in the form
- The **2nd license_plate bbox drawn** receives data from **Plate #2** in the form
- And so on…

**Best practice:** Draw plate boxes in the same order as you fill the plate form entries (top-to-bottom, left-to-right, or front plate first then rear plate).

> **📸 Screenshot suggestion:** License Plate section showing two plate cards (Plate #1 and Plate #2) expanded side by side, with fields filled in.

---

## 12. Parent–Child Relationships

The tool automatically determines which child boxes (plates, wheels, make logos, attachments) belong to which vehicle based on **spatial containment** — a child box whose centre point falls inside a vehicle box is linked to that vehicle.

**No manual linking is required**, but follow these guidelines:

- Always draw **vehicle boxes first** before drawing child boxes
- Ensure plate/wheel/logo boxes are **fully contained within** the vehicle box
- For attachments (trailers), draw the trailer box so its centre is within the truck/vehicle box
- If a box cannot be matched to any vehicle (e.g., a standalone plate), it becomes an **orphan** annotation in the export

### Export ID Scheme

| Type | ID Format | Example |
|------|-----------|---------|
| Vehicle | `vehicle_XX` | `vehicle_01` |
| License Plate | `licenseplate_XX` | `licenseplate_01` |
| Wheel | `wheel_XX` | `wheel_01` |
| Make/Logo | `make_XX` | `make_01` |
| Attachment | `attach_XX` | `attach_01` |
| Fallback component | `component_XX` | `component_01` |
| Orphan (no parent) | `orphan_XX` | `orphan_01` |

> **📸 Screenshot suggestion:** Diagram or screenshot showing a truck with vehicle box, plate box inside it, and wheel boxes inside it — with arrows showing the parent-child relationship.

---

## 13. Chain of Thought (CoT)

The CoT section in the **left panel** lets you write free-text notes describing your reasoning for the annotation.

### Usage

1. Scroll down in the left panel to find **"🧠 Chain of Thought (CoT)"**
2. Type notes in the text area — **each line becomes a separate entry** in the JSON output
3. Click **"+ Add Line"** to add a blank line
4. Click the **"?"** info button for examples

### Example CoT entries

```
Volvo prime mover detected pulling a box-type trailer.
Main vehicle plate WXD1023 and trailer plate TRL9087 are both visible.
Total of 10 wheels seen; one axle appears lifted.
Trailer is loaded (cargo flag = true).
No bus, taxi, or emergency markings detected.
```

These notes are saved in the JSON as:
```json
"CoT": [
  "Volvo prime mover detected pulling a box-type trailer.",
  "Main vehicle plate WXD1023 and trailer plate TRL9087 are both visible.",
  ...
]
```

> **📸 Screenshot suggestion:** Chain of Thought textarea filled with example notes, "Add Line" button visible below.

---

## 14. Fullscreen Mode & Navigation

### Entering Fullscreen

- Press **`F`** or click the **⛶** button in the canvas overlay
- The image expands to fill the screen with a dark overlay
- All bounding boxes remain visible and interactive

### In Fullscreen

- All drawing and selection functions work the same way
- Press **`Escape`** to exit fullscreen
- Press **Arrow Left / Arrow Right** to navigate to previous/next image in the directory

> **📸 Screenshot suggestion:** Image displayed in fullscreen mode with boxes visible, zoom controls floating at bottom-left, and Escape hint visible.

---

## 15. Saving & Exporting

### Step 1: Select Save Directory

1. Press **`S`** (or click the directory picker button)
2. A folder picker dialog appears — select or create a destination folder
3. The selected folder name is shown in the UI

> This only needs to be done once per session. The directory is remembered until the page is closed.

### Step 2: Save Current Image

- Press **`Ctrl+S`** to save the current image
- Two files are saved to the chosen directory:
  - `<filename>.json` — structured annotation data
  - `<filename>_annotated.png` — the original image with bounding boxes drawn on it

### Step 3: Save All Annotated Images (Batch)

- Press **`Ctrl+Shift+S`** to save **all** annotated images at once
- The tool iterates over every image in `imageAnnotationsMap` that has at least one annotation
- A progress counter is shown: `⏳ Saving... 3 / 12`
- Completion message: `🎉 Saved X images (JSON + PNG) to <folder name>`

> **⚠️ Important:** The current image is force-saved to the map before batch export, so you do not need to manually save it first.

> **📸 Screenshot suggestion:** File explorer showing output folder with both `.json` and `_annotated.png` files listed side by side.

---

## 16. Multi-Image Batch Workflow

This is the recommended workflow for annotating large image sets:

```
1. Open tool in Chrome
2. Press S → select output directory
3. Click "Choose Image Folder" → select images directory
4. Click "Choose YOLO Labels Folder" → select YOLO txts directory (if available)
5. For each image (navigate via dataset table or Arrow keys in fullscreen):
   a. Review / adjust YOLO bounding boxes
   b. Set Image Properties (site, scene, camera, type, rain)
   c. Draw missing boxes (Vehicle, Plate, Wheel, Make, Attachment)
   d. Fill Vehicle Properties form
   e. Click "Update Selected Annotation" for each vehicle
   f. Fill CoT notes if required
6. Press Ctrl+Shift+S to bulk save all annotated images
```

### Dataset Table Status Icons

| Icon | Meaning |
|------|---------|
| `🟡` | Image loaded, not yet annotated |
| `✅Y` | Paired with YOLO label, ready to refine |
| `✅` | Annotated and saved in browser storage |

> **📸 Screenshot suggestion:** Dataset table with multiple rows showing all three status types, with the currently active image row highlighted.

---

## 17. Browser Storage Management

Annotations are **automatically saved to browser localStorage** every 3 seconds and on page close. This allows you to safely close and reopen the tool without losing work.

### Storage Status Bar

A status bar is **always visible** at the top of the left panel:
- Shows: `🗄️ X image(s) in browser storage`
- Background is amber when data exists, grey when empty

### Clearing Storage

- Click **"🗑️ Clear"** in the storage status bar
- A confirmation dialog appears: `"Clear all saved annotation data from browser storage? This cannot be undone."`
- Confirm to wipe all stored annotations

> **⚠️ Use this when:** Starting a new batch session and you want to ensure no old data interferes. Always **save your work first** (Ctrl+Shift+S) before clearing.

> **📸 Screenshot suggestion:** Storage status bar showing "4 image(s) in browser storage" with the red Clear button, and the confirmation dialog.

---

## 18. Undo / Redo

| Action | Keyboard | Button |
|--------|----------|--------|
| Undo | `Ctrl+Z` | ↶ Undo (in Quick Actions) |
| Redo | `Ctrl+Y` or `Ctrl+Shift+Z` | ↷ Redo (in Quick Actions) |

- Each draw, delete, move, or resize action is saved to the undo history
- History is maintained for the current session (up to a fixed limit)
- Undo/Redo buttons are greyed out when no history is available

---

## 19. Keyboard Shortcuts Reference

| Shortcut | Action |
|----------|--------|
| **`W`** | Toggle drawing mode on/off |
| **`S`** | Select save directory |
| **`F`** | Enter fullscreen mode |
| **`Escape`** | Exit fullscreen / cancel drawing / deselect annotation |
| **`Delete` / `Backspace`** | Delete selected annotation |
| **`1`** | Change selected annotation → Vehicle |
| **`2`** | Change selected annotation → License Plate |
| **`3`** | Change selected annotation → Wheel (Single) |
| **`4`** | Change selected annotation → Wheel (Double) |
| **`5`** | Change selected annotation → Attachment |
| **`6`** | Change selected annotation → Make/Logo |
| **`Ctrl+Z`** | Undo |
| **`Ctrl+Y`** | Redo |
| **`Ctrl+Shift+Z`** | Redo (alternative) |
| **`Ctrl+S`** | Save current image (JSON + annotated PNG) |
| **`Ctrl+Shift+S`** | Save all annotated images (bulk export) |
| **`Arrow Left`** | Previous image (fullscreen mode) |
| **`Arrow Right`** | Next image (fullscreen mode) |

> **Note:** Keyboard shortcuts are **disabled when focus is inside a text input or dropdown** — click outside the form fields first if shortcuts are not responding.

---

## 20. JSON Output Format

Each saved `.json` file follows this structure:

```json
{
  "image": {
    "filename": "image001.jpg",
    "width": 1920,
    "height": 1080,
    "site_location": "Toll",
    "scene": "day",
    "camera_position": "front",
    "image_type": "RGB",
    "is_rain": false
  },
  "objects": [
    {
      "id": "vehicle_01",
      "type": "vehicle",
      "bbox": [120, 80, 860, 520],
      "vehicle_info": {
        "category": "Car",
        "subcategory": "Sedan",
        "color": "White",
        "direction": "Northbound",
        "is_taxi": false,
        "is_bus": false,
        "is_emergency_vehicle": false,
        "is_electric": false
      },
      "mechanical": {
        "number_of_axles_inferred": 2,
        "number_of_axles_raised": 0,
        "truck_trailer_labels_visible": false,
        "cargo_present": false
      },
      "license_plates": [
        {
          "id": "licenseplate_01",
          "type": "license_plate",
          "parent_id": "vehicle_01",
          "bbox": [340, 460, 200, 60],
          "text": "WXD1023",
          "country": "MY",
          "format": "License_Plate_Normal",
          "blurred": false,
          "visible": true
        }
      ],
      "wheels": [...],
      "make": [...],
      "attachments": [...],
      "CoT": [
        "White sedan, front view, plate clearly visible."
      ]
    }
  ]
}
```

### Key Notes on the Output

- **`bbox` format:** `[x, y, width, height]` in pixels (XYWH, top-left origin)
- **`license_plates`** is an array — supports multiple plates per vehicle
- **`CoT`** is an array of strings (one per line entered)
- **`parent_id`** links child boxes to their parent vehicle
- Orphan boxes (not inside any vehicle) appear under a top-level `"orphans"` key

---

## 21. Screenshot Suggestions

The following screenshots are recommended for a visual SOP or training guide:

| # | Step | What to Capture |
|---|------|-----------------|
| 1 | Tool on first load | Full 3-panel interface, empty state, upload zone |
| 2 | Loading a single image | Image displayed in canvas, Image Properties form visible |
| 3 | Loading an image folder | Dataset index table with multiple rows and status icons |
| 4 | YOLO import paired | Canvas with auto-drawn YOLO boxes and dataset table showing ✅Y |
| 5 | Image Properties filled | Right panel, Image Properties section, all fields set |
| 6 | Drawing a vehicle box | Canvas with crosshair cursor, partial box being drawn |
| 7 | All annotation types visible | One image with all 5 types drawn and colour coded |
| 8 | Magnifier active | Canvas with circular magnifier lens near a license plate |
| 9 | Vehicle form filled | Right panel showing Category, Color, Direction all set |
| 10 | Is Bus checkbox (Vans) | Vans selected, Is Bus checkbox ticked, Bus Type dropdown beside it |
| 11 | Multiple license plates | Plate #1 and Plate #2 cards both expanded with fields filled |
| 12 | Update Selected Annotation | Vehicle box selected + amber Update button active |
| 13 | Chain of Thought entries | CoT textarea with 3-4 lines of notes |
| 14 | Fullscreen mode | Image filling screen, zoom controls floating, arrow nav hint |
| 15 | Save directory selected | Directory picker confirmation message on screen |
| 16 | Bulk save complete | Success toast: "🎉 Saved 8 images (JSON + PNG) to output_folder" |
| 17 | Output folder contents | File Explorer showing .json and _annotated.png file pairs |
| 18 | Storage status bar | Amber bar showing "4 image(s) in browser storage" + Clear button |
| 19 | Annotated PNG output | The exported PNG with coloured bounding boxes drawn on image |
| 20 | JSON output sample | The exported JSON opened in a text editor or VS Code |

---

*End of SOP — ATHY Annotation Tool*
