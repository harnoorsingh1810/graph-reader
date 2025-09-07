Real-Time Chart Pattern Detector using YOLOv8
This project provides a real-time screen-capturing tool that uses a custom-trained YOLOv8 model to detect and classify stock chart patterns directly from your screen. It logs all detections into an Excel file and compiles an annotated video of the findings.

Features
Real-Time Screen Capture: Continuously captures a specific, user-defined region of the screen.

Periodic Detection: Runs inference using a YOLOv8 model at a set interval (e.g., every 60 seconds) to minimize resource usage.

Pattern Classification: Identifies predefined chart patterns (e.g., 'Head and shoulders', 'Triangle').

Automated Logging: Records every detection event with a timestamp, a reference to the annotated image, and the predicted pattern label in an Excel spreadsheet.

Video Summary: Creates an annotated video from the frames where patterns were detected, overlaying them with timestamps and labels.

Clean-up: Automatically removes temporary screenshot files after the session ends.

How it Works
Initialization: The script sets up necessary directories, loads the pre-trained YOLOv8 model, and defines the screen area to monitor.

Screen Grabbing: Using the mss library, the script enters an infinite loop to continuously capture the specified screen region. A live preview window is shown using OpenCV.

Timed Inference: Once every 60 seconds, the script saves the current screen view as a PNG image in the screenshots/ directory.

YOLOv8 Prediction: This saved image is passed to the YOLOv8 model. The model performs object detection and saves a new image with bounding boxes drawn around any detected patterns into the runs/detect/ directory.

Data Logging: The script extracts the class label of the detected pattern. It then appends a new row to classification_results.xlsx containing the timestamp, the file path to the annotated image, and the predicted label.

Video Frame Creation: The annotated image is loaded, and further text (timestamp and label) is drawn onto it. This new frame is then written to an MP4 video file.

Loop & Exit: The process repeats every 60 seconds. The application can be closed by pressing the 'q' key on the OpenCV display window.

Cleanup: Upon exiting, the script releases the video writer and deletes the temporary screenshots/ directory and its contents.

Prerequisites
Python 3.8 or higher

A trained YOLOv8 model file (e.g., model.pt)

Installation
Clone the repository:

Bash
git clone https://github.com/harnoorsingh1810/graphreader.git
cd graph reader
Create a virtual environment (recommended):

Bash
python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
Install the required dependencies:

Bash
pip install -r requirements.txt
If you don't have a requirements.txt file, you can create one with the following content or install the packages manually:

ultralytics
opencv-python
mss
numpy
openpyxl
And then run:

Bash
pip install ultralytics opencv-python mss numpy openpyxl
Add your model:
Place your trained YOLOv8 model file in the root directory and name it model.pt, or update the model_path variable in the script to point to your model file.

Configuration
Before running the script, you may need to adjust the following parameters in the Python file:

classes: A list of strings representing the names of the patterns your model can detect. The order must match the class indices used during model training.

Python
classes = ['Head and shoulders bottom', 'Head and shoulders top', 'M_Head', 'StockLine', 'Triangle', 'W_Bottom']
model_path: The path to your trained YOLOv8 model file.

Python
model_path = "model.pt"
monitor: The screen region to capture. You will need to adjust these values for your specific screen resolution and the location of the target window.

top: The pixel distance from the top of the screen.

left: The pixel distance from the left of the screen.

width: The width of the capture area in pixels.

height: The height of the capture area in pixels.

Python
monitor = {"top": 0, "left": 683, "width": 683, "height": 768}
Inference Interval: The time in seconds between each detection. This is controlled by the if current_time - last_capture_time >= 60: line. Change 60 to your desired interval.

fps: The frames per second for the output video. Since one frame is captured per interval, a low fps like 0.5 (1 frame every 2 seconds) or 1 is suitable.

Python
fps = 0.5
Usage
Position the application window you want to analyze (e.g., your trading platform) so it is visible within the coordinates defined by the monitor dictionary.

Run the script from your terminal:

Bash
Live Chart Vision Analyzer.py
A window titled "Screen Capture" will appear, showing the live view of the monitored region.

The script will now run in the background, saving detection results every minute (or per your configured interval).

To stop the script, make sure the "Screen Capture" window is active and press the 'q' key.

Output
The script generates the following files and directories:

classification_results.xlsx: An Excel file containing the log of all detections. The columns are:

Timestamp: The date and time of the detection.

Predicted Image Path: The file path to the saved image with the model's bounding boxes.

Label: The predicted class name for the detected pattern.

video/annotated_video.mp4: A video file compiling all the frames where a pattern was successfully detected. Each frame is annotated with the timestamp and the predicted label.

runs/detect/: This directory is automatically created by the ultralytics library to store the images with bounding box predictions.

screenshots/: A temporary directory used to store the raw screen captures before they are processed. This directory and its contents are deleted when the script exits successfully.

License
This project is licensed under the MIT License. See the LICENSE file for more details.
