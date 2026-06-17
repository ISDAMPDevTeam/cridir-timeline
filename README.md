# CriDIR Project Timeline Planner & Interactive Gantt Generator

An interactive, vector-based project roadmap planner designed for the **Critical Data Infrastructure Modernization (CriDIR)** program. This repository contains an automated workflow within a Jupyter Notebook to systematically transform tabular sprint mapping schedules into a clean, modern, high-fidelity Gantt chart exported as a PDF. 

Notably, this notebook embeds cross-platform clickable **Azure DevOps (ADO) hyperlinks** directly onto both the visual timeline bars and text labels, enabling interactive traceability from high-level visual representations directly back into active epics and tasks.

---

## 🚀 Features

- **Automated Timetable Calculation:** Dynamically translates standard project metadata (`start_sprint`, `sprints` duration) into absolute calendar timelines based on configured project anchors and standardized 3-week sprint intervals.
- **Cross-Platform PDF Interactivity:** Implements an advanced double-layer hyperlink embedding pattern (`matplotlib.patches.Rectangle` text-and-shape bindings). This guarantees that links remain clickable in web browsers, Adobe Acrobat Reader, and native platform preview utilities (including macOS Preview).
- **Phased Color Alignment:** Automatically groups and tracks tasks along standard modern SDLC workflows (*Analysis & Design*, *Development*, *Testing & Validation*, and *Deployment*).
- **Time Anchors & Progress Tracking:** Includes automatic layout bands, customized month/year gridlines, fiscal quarterly markers, and a dynamic "Today" milestone cursor.

---

## 📂 Repository Structure

* `cridir-timeline-planner.ipynb` - The primary Jupyter Notebook containing the data loading, date engineering, transformation logic, and `matplotlib` plotting engine.
* `CriDIR_Project_Roadmap.csv` - The input governance registry source data containing epics, task groupings, and sequential sprint mappings.

---

## 🛠️ Requirements & Installation

The script is written in Python 3 and requires standard data science and visualization dependencies. 

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/your-organization/cridir-timeline-planner.git](https://github.com/your-organization/cridir-timeline-planner.git)
   cd cridir-timeline-planner

```

2. **Install required packages:**
```bash
pip install pandas matplotlib jupyter

```


3. **Launch the environment:**
```bash
jupyter notebook cridir-timeline-planner.ipynb

```



---

## 📋 Input Data Format (`CriDIR_Project_Roadmap.csv`)

The notebook expects a structured CSV data file representing the program catalog. For accurate rendering, the sheet must include the following column headers (case and spaces are normalized automatically by the script):

| Column Name | Type | Description |
| --- | --- | --- |
| `epic_id` | `int` / `nullable` | The unique work item ID matching your Azure DevOps backlog (used to build URLs). |
| `task` | `string` | Title/description of the milestone or task group displayed on the Gantt bar. |
| `phase` | `string` | Operational phase. Must map strictly to: `Analysis & Design`, `Development`, `Testing & Validation`, or `Deployment`. |
| `start_sprint` | `int` | The relative sequence sprint number where the work initiates (1-based index). |
| `sprints` | `int` | Total duration allocated to the task, expressed in consecutive sprints. |

---

## ⚙️ Configuration & Project Constants

You can change core chronological anchors directly inside the configuration block of the notebook's setup cell to fit your current milestone windows:

```python
PROJECT_START = datetime(2025, 7, 30)  # Sprint 1 Day 1 Anchor date
CURRENT_DATE  = datetime(2026, 6, 10)  # The tracking timeline threshold/marker
SPRINT_DAYS   = 21                    # Standardized 3-week sprint rhythm

```

### Business Logic Rule Processing

* **Time Calculation:** Start and end boundaries are evaluated directly as:

$$\text{Start Date} = \text{PROJECT\_START} + \big((\text{start\_sprint} - 1) \times \text{SPRINT\_DAYS}\big)$$


* **Layout Sizing:** Bar labels are automatically processed using a length check threshold (`MIN_INSIDE_WIDTH = 40` days). If a task duration is too short for centered text, the code seamlessly moves the label to the right of the bar to preserve clean, un-cluttered formatting.

---

## 📖 How to Run & Use the Script

1. Open `cridir-timeline-planner.ipynb` inside your Jupyter workspace.
2. Verify that your input path `csv_path` correctly points to your tracking file location (by default, it references your working roadmap layout).
3. Select **Cell -> Run All** from the workspace top menu.
4. Upon successful run, the execution pipeline prints out the final target output summary:
```text
Saved PDF → cridir-timeline-linked-2026.pdf

```



---

## 🗺️ Architectural Legend Mapping

The visualization applies desaturated, professional tone profiles paired with strategic opacity blending to communicate clean priority hierarchies:

* 🟡 **Yellow (`#FFD966`)**: Analysis & Design Phase
* 🔵 **Blue (`#9BC2E6`)**: Code Construction & Infrastructure Development
* 🟢 **Green (`#A9D18E`)**: Testing & Validation / Proof-of-Parity Execution
* 🟠 **Peach (`#F4B183`)**: Release Management, Stabilization, & Decommissioning
* 🔴 **Red Dashed Line**: Dynamic present-day indicator line highlighting active work tracks.

```

***

### 💡 Tips for Customization
* **Repository Links:** Replace `https://github.com/your-organization/...` with your actual Git repo workspace URL.
* **Azure DevOps base URL:** If your ADO project group has an alternative root endpoint structure than the internal `https://dev.azure.com/bc-icm/FODIG/...` path inside cell 1, you can explicitly update that section under the `build_ado_url` block inside the codebase helper rules.

Jupyter notebook used to create a Gantt-style timeline from local CSV file.
