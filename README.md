# ✋ Hand Tracking Filters

A real-time computer vision project that uses **MediaPipe Hand Tracking** and **OpenCV** to create an interactive visual filter experience using hand gestures.

The application detects both hands through a webcam and creates a dynamic four-point **visual portal** between the user's thumbs and index fingers. Different image effects are rendered inside the portal, and users can switch between filters by performing a closing gesture.

---

## 📌 Overview

**Hand Tracking Filters** combines real-time hand landmark detection with image processing to create an interactive camera effect.

The application:

* Detects up to two hands using MediaPipe.
* Tracks hand landmarks in real time.
* Identifies the index-finger tips and thumbs.
* Creates a four-point portal between both hands.
* Applies visual filters inside the portal.
* Detects when the portal is closed.
* Automatically switches to the next filter.
* Displays the processed camera feed in real time.

The hand landmark indices used by the project include the thumb tip at landmark `4` and index-finger tip at landmark `8`.

---

## ✨ Features

### 🖐️ Real-Time Hand Tracking

The project uses **MediaPipe Hands** to detect and track up to two hands from the webcam.

The detection configuration uses:

* Maximum hands: `2`
* Detection confidence: `0.6`
* Tracking confidence: `0.6`

### 🌀 Interactive Portal

The positions of both hands are used to create a four-corner polygon:

```text
Left Index ───────── Right Index
     │                    │
     │      PORTAL        │
     │                    │
Left Thumb ───────── Right Thumb
```

The application calculates the portal width using the distance between the corresponding index and thumb points.

### 🎨 Multiple Visual Filters

The project includes a collection of real-time visual effects, including:

* Grid
* Color mapping
* Dot pattern
* RGB shift
* Sepia/vintage effect
* White/soft effect
* Pink dot effect
* Fire effect
* Neon edge effect
* Pixelation
* Inverted colors
* Pop-art effect

The filters are stored in a central `FILTERS` collection and can be cycled automatically.

### 🤏 Gesture-Based Filter Switching

The application monitors the width of the portal.

When the portal closes below the configured threshold, the next filter is selected. When the portal opens again, the detector resets and waits for the next closing gesture.

### 🎥 Real-Time Camera Processing

The webcam feed is continuously captured, mirrored, processed, and displayed using OpenCV.

---

## 🛠️ Technologies Used

| Technology    | Purpose                              |
| ------------- | ------------------------------------ |
| **Python**    | Core programming language            |
| **OpenCV**    | Webcam capture and image processing  |
| **MediaPipe** | Real-time hand landmark detection    |
| **NumPy**     | Numerical and image-array operations |

---

## 📂 Project Structure

```text
Hand-Tracking-Filters/
│
├── main.py
├── hand_tracking.py
├── geometry.py
├── filters.py
├── requirements.txt
└── README.md
```

### `main.py`

The main application entry point.

It:

1. Initializes MediaPipe Hands.
2. Opens the webcam.
3. Processes each camera frame.
4. Detects left and right hands.
5. Extracts hand landmarks.
6. Calculates portal width.
7. Detects the closing gesture.
8. Changes the active filter.
9. Renders the filter inside the portal.

### `hand_tracking.py`

Contains the MediaPipe landmark indices and helper functions related to hand landmarks.

For example:

```python
THUMB_TIP = 4
INDEX_TIP = 8
```

### `geometry.py`

Handles the portal geometry, gesture detection, polygon masking, and rendering.

The filter is applied only within the detected polygon using a mask and image blending.

### `filters.py`

Contains the visual effects used by the application.

Each filter receives an image region and returns a processed image.

### `requirements.txt`

Project dependencies:

```text
opencv-python
mediapipe==0.10.14
numpy
```

---

## 💻 Requirements

Before running the project, make sure you have:

* Python installed
* A working webcam
* Windows, macOS, or Linux
* A supported Python environment
* Internet access for installing dependencies

> **Important:** This project requires **MediaPipe 0.10.14**. Do not replace it with a newer MediaPipe version unless the code is migrated to the newer MediaPipe API.

---

## 🚀 Installation

### 1. Clone the repository

```bash
git clone https://github.com/YOUR-USERNAME/Hand-Tracking-Filters.git
```

Navigate into the project:

```bash
cd Hand-Tracking-Filters
```

### 2. Create a virtual environment

Windows:

```bash
python -m venv venv
```

Activate it:

```bash
venv\Scripts\activate
```

macOS/Linux:

```bash
source venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install -r requirements.txt
```

### 4. Verify MediaPipe

Because the project depends on a specific MediaPipe version, verify the installation:

```bash
python -c "import mediapipe as mp; print(mp.__version__)"
```

Expected:

```text
0.10.14
```

---

## ▶️ Run the Project

Start the application with:

```bash
python main.py
```

Allow camera access if your operating system asks for permission.

Once the webcam opens:

1. Show both hands to the camera.
2. Position your thumbs and index fingers to create the portal.
3. Move your hands closer together to close the portal.
4. The filter changes when the closing gesture is detected.
5. Open the portal again.
6. Repeat to cycle through the available filters.

Press:

```text
Q
```

to exit the application.

---

## 🧠 How It Works

The application follows this processing pipeline:

```text
Webcam
   │
   ▼
OpenCV Frame Capture
   │
   ▼
MediaPipe Hand Detection
   │
   ▼
Hand Landmark Extraction
   │
   ├── Left Index Tip
   ├── Left Thumb Tip
   ├── Right Index Tip
   └── Right Thumb Tip
   │
   ▼
Portal Geometry Calculation
   │
   ▼
Closing Gesture Detection
   │
   ▼
Select Visual Filter
   │
   ▼
Apply Filter Inside Polygon
   │
   ▼
Display Result
```

The filter is rendered inside a polygon formed from four hand landmark points. A mask isolates the polygon region before blending the filtered image back into the camera frame.

---

## 🎨 Available Filters

The project currently includes:

|  # | Filter            |
| -: | ----------------- |
|  1 | Grid              |
|  2 | Color / Threshold |
|  3 | Dot Pattern       |
|  4 | RGB Shift         |
|  5 | Color Map         |
|  6 | Sepia + Vignette  |
|  7 | Soft White        |
|  8 | Pink Dot          |
|  9 | Fire / Inferno    |
| 10 | Neon              |
| 11 | Pixel             |
| 12 | Invert            |
| 13 | Pop Art           |

## The filter functions use OpenCV operations such as color conversion, color maps, edge detection, resizing, channel shifting, and image blending.

## ⚙️ Gesture Detection

The portal uses two thresholds:

```python
close_ratio = 0.16
open_ratio = 0.30
```

The thresholds are multiplied by the camera frame width to determine whether the portal is closed or open.

This prevents the filter from repeatedly changing while the hands remain close together.

---

## 🔧 Troubleshooting

### Camera does not open

If you see:

```text
Could not open the camera.
```

Check:

* Camera permissions
* Whether another application is using the webcam
* Whether your webcam is connected
* Whether the camera index needs to be changed

The application currently opens camera index `0`.

### MediaPipe error

Make sure the required version is installed:

```bash
python -m pip uninstall mediapipe -y
python -m pip install mediapipe==0.10.14
```

Then verify:

```bash
python -c "import mediapipe as mp; print(mp.__version__)"
```

### Pylance says `mp.solutions.hands` is unknown

Make sure VS Code is using the same Python interpreter where `mediapipe==0.10.14` was installed.

In VS Code:

```text
Ctrl + Shift + P
        ↓
Python: Select Interpreter
        ↓
Select your project virtual environment
```

Then reload VS Code.

---

## 📸 Demo

Add screenshots or a GIF of the application here:

```text
docs/
├── demo.gif
├── screenshot-1.png
└── screenshot-2.png
```

Example:

```markdown
![Hand Tracking Filters Demo](docs/demo.gif)
```

A short demo GIF is highly recommended for a computer-vision GitHub project because it immediately shows how the hand gesture and filters work.

---

## 🔮 Future Improvements

Possible future improvements include:

* [ ] Add more hand gestures
* [ ] Add filter selection using specific finger gestures
* [ ] Add a graphical user interface
* [ ] Add filter names on screen
* [ ] Add keyboard shortcuts for filters
* [ ] Add customizable gesture sensitivity
* [ ] Add more visual effects
* [ ] Add screenshot functionality
* [ ] Add video recording
* [ ] Improve performance and FPS
* [ ] Add a configuration file for filter settings
* [ ] Package the application as a standalone executable

---

## 🤝 Contributing

Contributions are welcome!

To contribute:

1. Fork the repository.
2. Create a new branch.

```bash
git checkout -b feature/your-feature
```

3. Make your changes.
4. Commit your changes.

```bash
git commit -m "Add new visual filter"
```

5. Push the branch.

```bash
git push origin feature/your-feature
```

6. Open a Pull Request.

---

## 📄 License

If you have not selected a license yet, choose one before publishing the repository.

A common choice for open-source projects is the **MIT License**.

---

## 👨‍💻 Author

**Souhaib Aziz**

Computer Science / Artificial Intelligence Student

GitHub: `https://github.com/YOUR-USERNAME`

---

## ⭐ Support

If you find this project useful or interesting, consider giving the repository a ⭐ on GitHub.

---

### Made with ❤️ using Python, OpenCV, MediaPipe, and NumPy.
