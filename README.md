# UniCrowd
[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/)

Simulation is a powerful tool to easily generate annotated data, and a highly desirable feature, especially in those domains where learning models need large training datasets. Real-world data, in fact, do not often comply with the needs of machine learning and deep learning solutions, which have proven to be extremely data-hungry. Despite the initial skepticism of a portion of the scientific community, the potential of simulation has been largely confirmed in many application areas, and the recent developments in terms of rendering and virtualization engines, have shown a good ability also in representing complex scenes. This includes environmental factors, such as weather conditions and surface reflectance, as well as human-related events, like human actions and behaviors.
We present a human crowd simulator, called UniCrowd, and its associated validation pipeline. We show how the simulator can generate annotated data, suitable for vision-based tasks, as crowd counting, detection, and tracking, as well as behavior-oriented tasks like trajectory prediction and anomaly detection.

![banner](https://github.com/mmlab-cv/UniCrowd/blob/main/images/unicrowd_schema.png)

## UniCrowd Dataset
The UniCrowd dataset comprises 9 scenes recorded in the `UniCrowd simulator`, developed with Unity. Each scene includes 5 videos, each captured by a different camera. These scenes vary in terms of the time of day they were recorded and the size of the simulated crowd.

![banner](https://github.com/mmlab-cv/UniCrowd/blob/main/images/unicrowd_gt.png)

You can download the UniCrowd dataset <font size=5>[HERE](https://drive.google.com/drive/folders/16Nb90YyOjfzDb9nolb6_efvl9syr6tAn?usp=sharing)</font>. By downloading the dataset, you agree to adhere to the license conditions.

### What You Will Find
The dataset consists of 9 folders, each identified by a unique name, and a `Readme.txt` file. Each folder contains:
 - 5 folders, each containing one RGB video recorded by a camera
 - 5 folders, each containing one video representing the semantic segmentation of the corresponding RGB video
 - 1 folder with annotations in JSON files

#### Extract frames
To extract individual frames from the videos, you need to meet the following requirements:
- ffmpeg 2020-2022 built with gcc 12.1.0

To extract the frames from a video losslessly execute the following command lines:
- (for low density simulations): `ffmpeg -i $VideoFilename.mp4 -start_number 256 $ImageFilename_%03d.png`
- (for medium density simulations): `ffmpeg -i $VideoFilename.mp4 -start_number 636 $ImageFilename_%03d.png`
- (for high density simulations): `ffmpeg -i $VideoFilename.mp4 -start_number 952 $ImageFilename_%03d.png`

Choose `$ImageFilename` depending on whether the video you are processing is `rgb` or `segmentation`.

#### Annotations
The annotations are included in JSON files, falling into two main categories of data:
- Captures: These contain ground truth information about segmentation, bounding boxes, and pedestrian poses.
- Metrics: These contain ground truth data related to people counting.

Due to the size of the dataset, there are multiple files for each simulation.

For each CAPTURE file, the structure is as follows:
- Id: Capture ID
- Sequence ID: Sequence identifier
- Step: Capture step number
- Timestamp: Capture timestamp
- Sensor Data: Camera-related data
  - Id: Sensor ID (e.g., Pro Cam1, Pro Cam2, ...)
  - Ego ID: Ego identifier for the camera
  - Modality: Camera modality (always set as "camera")
  - Translation: Sensor position within the simulation world
  - Rotation: Sensor rotation within the simulation world
  - Camera Intrinsic: Intrinsic camera parameters
  - Projection: Camera projection type (always set as "perspective")
- Ego: Represents the GameObject to which the sensor is attached (in our case, the camera stands alone, so ego information can be ignored)
  - Id: Ego ID
  - Translation: GameObject position within the simulation world
  - Rotation: GameObject rotation within the simulation world
  - Velocity: GameObject velocity
  - Acceleration: GameObject acceleration
- Filename: Comprises the parent folder and the image name
- Format: Image format (always PNG)
- Annotations: Lists various ground truth data obtained from the capture, including Segmentation image, Bounding Box data, and Pose data
  - Id: Segmentation capture ID
  - Annotation Definition: Annotation information for segmentation capture (usually identical to the ID)
  - Filename: Comprises the parent folder and the image name
  - Id: Bounding box ID
  - Annotation Definition: Annotation information for bounding box data (usually identical to the ID)
  - Values: Bounding box values for each pedestrian within the scene
    - Label ID: Pedestrian label ID
    - Label Name: Pedestrian name
    - Instance ID: ID of the pedestrian instance within the simulation
    - X: X-coordinate of the bounding box origin
    - Y: Y-coordinate of the bounding box origin
    - Width: Width of the bounding box
    - Height: Height of the bounding box
  - Id: Skeleton ID
  - Annotation Definition: Annotation information for skeleton data (usually identical to the ID)
  - Values: Skeleton data for each pedestrian within the scene
    - Label ID: Pedestrian label ID
    - Instance ID: ID of the pedestrian instance within the simulation
    - Template GUID: ID of the skeleton instance
    - Pose: Ground truth pose information
    - Keypoints: Collection of joint data following the COCO skeleton structure
      - Index: Joint index
      - X: X-coordinate of the joint
      - Y: Y-coordinate of the joint
      - State: Joint visibility status (0: does not exist, 1: not visible, 2: visible) within the scene

For each METRIC, the structure is as follows:
- Capture ID: ID of the capture (corresponding to the one in the capture file)
- Annotation ID: Annotation information about the capture
- Sequence ID: Sequence identifier
- Step: Capture step number
- Metric Definition: Metric definition
- Values: People counting ground truth values within the simulation
  - Label ID: Pedestrian label ID
  - Label Name: Pedestrian name
  - Count: Indicates whether the pedestrian is visible in the scene or not

## License
The UniCrowd Dataset is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International License](http://creativecommons.org/licenses/by-nc/4.0/).

## Citation
Related paper:

Soon available

## Summary
UniCrowd offers a valuable dataset for vision-based tasks and behavior-oriented analysis. Download the dataset, extract frames as needed, and explore the rich annotations to advance your research in crowd analysis and related fields.
