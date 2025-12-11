# Printed Text Scanner (PyTesseract + PyQt5)

Simple GUI application to load images or capture frames from a webcam, draw a rectangular ROI, and extract printed text using Tesseract OCR.

This repository contains a single script, `test.py`, which implements a PyQt5-based interface and uses OpenCV + pytesseract for image processing and OCR.

**Features:**
- Load images (PNG/JPG/BMP).
- Start/stop webcam and capture frames.
- Draw a rectangular ROI on the image to limit OCR to a region.
- Run OCR and display recognized text in a side panel.
- Show bounding boxes and text overlay on the image.
- Save overlay image to disk.

**Requirements:**
- Python 3.8+
- Tesseract OCR installed and available on PATH (or provide explicit path)
- The following Python packages:
  - `PyQt5`
  - `opencv-python`
  - `pillow`
  - `pytesseract`
  - `numpy`

**Install dependencies (Windows PowerShell):**

```powershell
python -m pip install --upgrade pip
pip install PyQt5 opencv-python pillow pytesseract numpy
```

If you prefer a `requirements.txt`, create one with these packages and run:

```powershell
pip install -r requirements.txt
```

**Tesseract installation (Windows):**
- Install Tesseract (for example from the UB Mannheim builds) and ensure `tesseract.exe` is in your PATH.
- If Tesseract is not on PATH, set the path in code before calling pytesseract functions, for example near the top of `test.py`:

```python
import pytesseract
pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
```

**Run the app:**

```powershell
python test.py
```

Then use the GUI: Load an image or start the camera, draw a rectangle over the region you want to scan, then click "Run OCR". Recognized text will appear in the right-side text area and high-confidence words will be overlaid on the image.

**Notes & Tips:**
- The script pre-processes the selected region with bilateral filtering and adaptive thresholding to improve OCR accuracy for printed text.
- The OCR boxes filter by confidence (>40). Adjust the threshold in `test.py` if you want to include lower-confidence results.
- On Windows, OpenCV's `cv2.imdecode` is used to support Unicode paths; avoid using non-ASCII characters if you run into file-open issues.

**Where the code lives:**
- Main GUI and OCR logic: `test.py`

**Contributing:**
- This is a small personal tool. If you'd like improvements (extra preprocessing, layout changes, exporting results), open an issue or send a patch.

**License:**
- No license file included. Add a `LICENSE` if you want to make reuse terms explicit.

---

If you want, I can:
- Add a `requirements.txt` and a small `run.ps1` helper script.
- Update `test.py` to set `pytesseract.pytesseract.tesseract_cmd` automatically when missing.
- Tweak the README wording or add screenshots.

Tell me which of those you'd like next.