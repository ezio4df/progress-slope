Based on the detailed notebook content you provided, here is a **comprehensive, professional README** for the `progress-slope` project:

---

# 📈 Progress Slope

**Visualize your learning progress against your scheduled routine—and let your desktop wallpaper reflect your momentum.**

`progress-slope` is a personal analytics tool that quantifies your video-based course progress in relation to your planned study routine over time. It automatically:
- Scans structured course directories to compute total video durations,
- Tracks your current position in each course,
- Calculates how much of your routine window has elapsed,
- Plots your actual learning progress vs. ideal linear progress,
- Saves the graph as a high-resolution image, and
- **Sets it as your desktop wallpaper** (KDE Plasma support included).

Ideal for disciplined self-learners who follow time-boxed study schedules and want real-time, visual feedback on their adherence and advancement.

---

## ✨ Features

- **Automated Video Duration Analysis**: Uses `ffprobe` (from FFmpeg) to accurately measure `.mp4` durations in a strict hierarchical course structure.
- **Progress Tracking**: Record your current lesson/video via a simple dialog, with chronological and logical validation.
- **Routine-Aware Time Modeling**: Define daily study blocks (e.g., 06:00–08:00), and the system computes how much of your *available routine time* has passed between a global start and end date.
- **Dual-Axis Progress Metrics**:
  - **X-axis**: % of scheduled routine time elapsed (0 → 100%).
  - **Y-axis**: Cumulative % of total course content completed (across all tracked courses).
- **Stepped Progress Visualization**: Reflects discrete learning milestones with a step-wise line graph.
- **Dynamic Wallpaper Integration**: Automatically updates your KDE Plasma desktop background with the latest progress graph.
- **Persistent Caching**: Video durations and progress logs are cached via `pickle` for fast subsequent runs.

---

## 🗂️ Assumed Course Structure

The system expects courses to follow this filesystem layout:

```
course_root/
├── 1_intro/
│   ├── 1_welcome.mp4
│   ├── 2_setup.mp4
│   └── ...
├── 2_basics/
│   ├── 1_variables.mp4
│   └── ...
└── ...
```

- Directories and files **must start with a number** (e.g., `1_intro`, `5_backprop.mp4`).
- Only `.mp4` files are processed.
- Ordering is **numeric**, not alphabetical.

---

## 🛠️ Dependencies

- **Python 3.7+**
- **FFmpeg** (`ffprobe` must be in `PATH`)
- **KDE Plasma** (for `kdialog` input and `plasma-apply-wallpaperimage` integration)
- Python libraries:
  - `python-dotenv`
  - `matplotlib`
  - `seaborn`
  - `pillow`
  - `zoneinfo` (built-in in Python 3.9+, install `tzdata` for earlier versions)

Install Python deps with:
```bash
pip install python-dotenv matplotlib seaborn pillow
```

---

## ⚙️ Configuration

### Environment Variables (`.env` file)

```env
TIMEZONE=Asia/Dhaka

# Define your courses (slug → path)
COURSE_PATHS__ml_fundamentals=/home/user/courses/ml_fundamentals
COURSE_PATHS__python_abc=/home/user/courses/python_abc

# Routine period
START_DATETIME=2025-11-01T00:00:00
END_DATETIME=2026-03-31T23:59:59
```

> **Note**: Variable names for courses must start with `COURSE_PATHS__`, followed by a unique slug.

---

## 📊 Workflow

1. **Scan courses**: Video durations are extracted and cached.
2. **Log progress**: Use the interactive `kdialog` prompt to input your current position as `course_slug/lesson/video`.
3. **Compute metrics**:
   - For each logged timestamp, calculate:
     - `% of routine time elapsed` (X)
     - `% of total course content completed` (Y)
4. **Generate plot**: A stepped line graph is created comparing your actual progress to the ideal 1:1 slope.
5. **Update wallpaper**: The graph is resized to 1920×1080 and set as your desktop background.

---

## 📁 Output

- `course_cache.pkl`: Cached video durations.
- `course_progress.pkl`: Serialized progress log (timestamps + positions).
- `course_progress_XXXX.png`: Latest progress graph (set as wallpaper).

