## Voxel-RCNN-OpenPCDet
Final course project for CS-539 Machine Learning Fall '21

### Introduction and Motive 

In today's rapidly automated world, state of the art 2D object detection algorithms have become very robust yeilding high accuracy with great precision. In autonomous driving scenarios, 2D detection is not sufficient as it lacks depth information of the corresponding detections. This data is imperative for making sense of the environment for localization and obstacle avoidance. Thus 3D object detection is needed along with 2D detection. This project improves upon the state of the art 3D object detection algorithm Voxel-RCNN for detection of pedestrians and cyclists along with the original cars that were being detected.

### Dataset 
For testing the perfromace of our improved architecture, we used the standard KITTI dataset which is used as a baseline for all 3D object detection algorithms. This dataset contains 7500 training and 7500 testing instances of RGB camera images and Velodyne LIDAR scans for numerous driving conditions. These instances have ground truth 3D bounding boxes for pedestrians, cyclists and car that we use for training our model.

The dataset is further split into 3 main categories based on the occulusion and trucation of the objects. Those categories are:
- EASY: Min. bounding box height: 40 Px, Max. occlusion level: Fully visible, Max. truncation: 15 %
- MODERATE: Min. bounding box height: 25 Px, Max. occlusion level: Partly occluded, Max. truncation: 30 %
- HARD: Min. bounding box height: 25 Px, Max. occlusion level: Difficult to see, Max. truncation: 50 %

### OpenPCDet
We used an open source library for 3D object detection package which has multiple state of the art 3D object detection models. It is also the official code release for PV-RCNN, PointRCNN and VoxelRCNN arhcitectures. All these models differ slightly from one another and share multiple similar components. This library runs the particular modules in a plug and play type of system to run each of the archtectures and shows in the image below:

### Other Libraries Used
- Pytorch
- Google Colab Pro*
- Mayavi - Point cloud visualization
- TensorBoardX
- SpConv: Spatially Saprce Convolution Library 

### Original Voxel-RCNN Archtecture


```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text
```
[Link](url) and ![Image](src)


For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/jashmehta20/Voxel-RCNN-OpenPCDet/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
