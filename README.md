# Printed Text Scanner (PyTesseract + PyQt5)

Lightweight GUI to load images or capture webcam frames, draw a rectangular ROI, and extract printed text using Tesseract OCR. The app is implemented in a single script, `test.py`, using PyQt5 for the interface and OpenCV + pytesseract for image processing and OCR.

**Key features (as implemented in `test.py`):**
- **Load images** (PNG/JPG/JPEG/BMP) using a file dialog. Uses `cv2.imdecode` + `numpy.fromfile` to support long/Unicode paths on Windows.
- **Webcam support**: start/stop camera with `cv2.VideoCapture` and a `QTimer` to show live frames.
- **Drawable ROI**: click-and-drag on the image to draw a rectangular ROI. The `ImageLabel` class converts between display and image coordinates so the ROI maps correctly to the underlying image.
- **OCR with preprocessing**: selected region (or full image) is converted to grayscale, filtered with `cv2.bilateralFilter`, and binarized with `cv2.adaptiveThreshold` before calling Tesseract.
- **Bounding boxes & overlays**: high-confidence OCR words are drawn as rectangles with text overlays on the image.
- **Text output**: full OCR text is placed in the right-side text panel.
- **Save overlay**: save the current overlay image to disk (PNG) using `cv2.imencode` and `tofile`.

**Requirements:**
- Python 3.8+
- A Tesseract OCR binary installed (and available on PATH, or set manually)
- Python packages:
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

Or create a `requirements.txt` and run:

```powershell
pip install -r requirements.txt
```

**Tesseract on Windows:**
- Install Tesseract (e.g. UB Mannheim build) and ensure `tesseract.exe` is on your PATH.
- If Tesseract isn't on PATH, set it in `test.py` before calls to `pytesseract`:

```python
import pytesseract
pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
```

**Run the app:**

```powershell
python test.py
```

Usage flow in the GUI:
- Click `Load Image` or `Start Camera` to get an image into the viewer.
- Draw a rectangle by dragging with the left mouse button to define an ROI (optional).
- Click `Capture Frame` to mark the current frame (shows a confirmation message).
- Click `Run OCR` to run preprocessing + Tesseract on the ROI or full image.
- OCR results appear in the right-side text area; high-confidence words are overlaid on the image.
- Click `Save Overlay` to export the displayed image with overlays as a PNG.

**Implementation / behavior notes (from the code):**
- ROI mapping: `ImageLabel` computes scaling between the pixmap and the label size. The ROI saved in image coordinates so overlays and OCR operate on the correct pixels.
- Preprocessing pipeline (in `run_ocr`): grayscale -> bilateral filter -> adaptive Gaussian thresholding. Output of preprocessing is passed to Tesseract via Pillow image conversion.
- OCR box filtering: the OCR output uses `pytesseract.image_to_data` and filters words by confidence `> 40` (integer-conf). Change this value in `test.py` to relax or tighten filtering.
- Saving images: `display_image` is kept as RGB; when saving the code converts to BGR and writes via `cv2.imencode(...).tofile(path)` to support Unicode/long paths on Windows.

**Troubleshooting & tips:**
- If images fail to load, confirm the path has read permissions and contains supported formats. Using non-ASCII characters in file paths is supported by the current `cv2.imdecode` approach but may still cause issues in some environments.
- If OCR returns nothing or poor text, try:
  - Increasing image contrast or experimenting with different preprocessing (e.g., denoising, resizing).
  - Lowering the confidence threshold in the code to see more raw detections.
  - Installing language packs for Tesseract if you expect non-English text.

**Where the code lives:**
- Main GUI and OCR logic: `test.py`

**Next steps I can help with:**
- Add a `requirements.txt` and a small `run.ps1` helper script. 
- Update `test.py` to auto-detect Tesseract and set `pytesseract.pytesseract.tesseract_cmd` when missing (with a helpful UI message).
- Add a command-line mode to run OCR headless on images or directories.

If you want any of those, tell me which and I will implement it.