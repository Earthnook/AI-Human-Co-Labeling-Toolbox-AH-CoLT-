# AI Human Co-Labeling Toolbox (AH-CoLT) 

This is the code for the following paper:

X. Huang, B. Rezaei and S. Ostadabbas, “AH-CoLT: An AI-Human Co-Labeling Toolbox to Augment Efficient Groundtruth Generation” 

The goal of the AH-CoLT is to provide an efficient and augmentative annotation tool to facilitate creating large labeled visual datasets.
This toolbox presents an efficient semi-automatic groundtruth generation framework for unlabeled images/videos. 
AH-CoLT enables accurate groundtruth labeling by incorporating the outcomes of state-of-the-art AI recognizers into a time-efficient human-based review and revise process.

We are trying to integrate different annotation models into toolbox. So far, we only imported Hourglass model for single-person 
image annotation. Multi-person image labeling or facial landmarks annotation will be imported into our toolbox.  

Contact: 

[Xiaofei Huang](huang.xiaof@husky.neu.edu),
[Behnaz Rezaei](brezaei@ece.neu.edu),
[Sarah Ostadabbas](ostadabbas@ece.neu.edu)


## Contents   
*  [Requirements](#requirements)
*  [Running the GUI](#running-the-gui)
    *  [Stage I: AI Labeler](#stage-i-ai-labeler)
    *  [Stage II: Human Reviewer](#stage-ii-human-reviewer)
    *  [Stage III: Human Reviser](#stage-iii-human-reviser)
*  [Citation](#citation)
*  [License](#license)
*  [Acknowledgements](#acknowledgements)


## Requirements 
The interface of toolbox is developed by tkinter in python3.7.

1. Install following libraries:
    *  (1) OpenCV
    *  (2) PyTorch (>= 0.4.1): GPU version is not necessary.
2. Download one of pretrained models (e.g. [8-stack hourglass model](https://drive.google.com/drive/folders/0B63t5HSgY4SQQ2FBRE5rQ2EzbjQ?usp=sharing)) 
and put the model folder into ./Models/Hourglass/data/mpii.
3. Download a weights file for YOLOv3 detector [here](https://pjreddie.com/media/files/yolov3.weights), and place it into ./Models/Detection/data.

## Running the GUI
Run `Toolbox.py` to launch main window of AH-CoLT, which includes three stages: AI Labeler, Human Reviewer, and Human Reviser.
For user convenience, each stage can be employed independently.

![MainMenu](doc/MainMenu.png)

### Stage I: AI Labeler
AI labeler interface allows users to load a video or a collection of images as the unlabeled data source 
and select an appropriate already trained model as the initial AI labeler. 
The types of labels outputted by the AI model is then displayed in the textbox.

Model Selection:
* Hourglass: 16 keypoints for single-person images. (In use)
* Mask R-CNN: 16 keypoints for multi-person images. (To do)
* OpenPose: for multi-person images. (To do)

The "AI Labeler" window
![AI_Labeler](doc/AI_Labeler.png)

#### Input
Choose a directory of images set or a file of video. Each image must be JPEG file. The format of video file could be MP4, AVI or MOV.
##### Example:
```
{ROOT}/img
```
#### Output
1. If resource is a video, the corresponding frames will be generated first in folder `${root}/video_name`, which is named the video file name. 
2. Predicted keypoints of all images/frames will be save in `dirname_model.pkl` under root path. Here `dirname` represents 
the name of images/frames set and `model` is the abbreviation of name of selected model.
##### Example:
```
${ROOT}/img_hg.pkl
```

### Stage II: Human Reviewer
In this stage, the output of the AI labeling is given to a human reviewer for his/her evaluation. Reviewer needs check keypoints in order.

#### Input
1. Folder of images/frames set
2. AI predicted keypoint file, which is generated by AI labeler.
#### Output
When all images have been reviewed, a flag list file will be save in `dirname_flag.pkl` under root path. Here `dirname` represents 
the name of images/frames set.
##### Example:
```
${ROOT}/img_flag.pkl
```
The image reviewing window
![Review_Window](doc/Review_Window.png)

The format of labels, which represent the AI predicted keypoints, is `poseindex_keypointindex`. 

#### Operations
| Operation | Description |
| --- | --- |
| Click left button of mouse| Accept current predicted keypoint |
| Click right button of mouse | Reject current predicted keypoint |
| Press 'i' on keyboard | Insert a keypoint |
| Press 'd' on keyboard | Delete current keypoint |
| Press 'u' on keyboard | Undo |
| Press 'y' on keyboard | Confirm reviewing of current image (ONLY work after checking all keypoints)
| Press 'n' on keyboard | Recheck current image |

#####Note:
As we default all keypoints visible, if considering the visibility of each keypoint, please mark obscured keypoints as error, 
so that they can be annotated as invisible.

### Stage III: Human Reviser
In this stage, only the AI model errors detected by the human reviewer need be revised.

#### Input
1. Folder of images/frames set
2. AI predicted keypoint file, which is generated by AI labeler.
3. Corresponding flag list file, which is generated by Human Reviewer.
#### Output
When all images have been revised, a groundtruth will be save in `dirname_gt.pkl` under root path. Here `dirname` represents 
the name of images/frames set.
##### Example:
```
${ROOT}/img_gt.pkl
```
The image revising window
![Revise_Window](doc/Revise_Window.png)

The keypoints, which need be revised, are displayed in red. After correcting all keypoints marked as errors in one image, 
human reviser needs to capture the head bounding box by holding and releasing left button to create a pink rectangle. 

#### Operations
| Operation | Description |
| --- | --- |
| Click left button of mouse| Capture the new position of red keypoint and set as 'visible' |
| Click right button of mouse | Capture the new position of red keypoint and set as 'invisible' |
| Press 'u' on keyboard | Undo |
| Hold and release left button of mouse | Create a rectangle box |
| Press 'y' on keyboard | Confirm revising of current image (ONLY work after revising all keypoints and capturing bounding box)




## Citation 

## License 
* This code is for non-commercial purpose only. 

* For other uses please contact [Augmented Cognition Lab (ACLab)](http://www.northeastern.edu/ostadabbas/) at Northeastern University. 


## Acknowledgements
The person detector is brought from [pytorch-yolo-v3](https://github.com/ayooshkathuria/pytorch-yolo-v3), which is based on 
[YOLOv3: An Incremental Improvement](https://pjreddie.com/media/files/papers/YOLOv3.pdf). And the code of hourglass model 
for AI Labeler comes from [pytorch-pose](https://github.com/bearpaw/pytorch-pose).

