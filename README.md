## Voxel-RCNN-OpenPCDet
Final course project for CS-539 Machine Learning Fall '21

### Introduction and Motive 

In today's rapidly automated world, state of the art 2D object detection algorithms have become very robust yeilding high accuracy with great precision. In autonomous driving scenarios, 2D detection is not sufficient as it lacks depth information of the corresponding detections. This data is imperative for making sense of the environment for localization and obstacle avoidance. Thus 3D object detection is needed along with 2D detection. This project improves upon the state of the art 3D object detection algorithm Voxel-RCNN for detection of pedestrians and cyclists along with the original cars that were being detected.

### Dataset 
For testing the perfromace of our improved architecture, we used the standard KITTI dataset which is used as a baseline for all 3D object detection algorithms. This dataset contains 7500 training and 7500 testing instances of RGB camera images and Velodyne LIDAR scans for numerous driving conditions. These instances have ground truth 3D bounding boxes for pedestrians, cyclists and car that we use for training our model.

![kitti](/Assets/kitti.jpg)

The dataset is further split into 3 main categories based on the occulusion and trucation of the objects. Those categories are:
- EASY: Min. bounding box height: 40 Px, Max. occlusion level: Fully visible, Max. truncation: 15 %
- MODERATE: Min. bounding box height: 25 Px, Max. occlusion level: Partly occluded, Max. truncation: 30 %
- HARD: Min. bounding box height: 25 Px, Max. occlusion level: Difficult to see, Max. truncation: 50 %

### OpenPCDet
We used an open source library for 3D object detection package which has multiple state of the art 3D object detection models. It is also the official code release for PV-RCNN, PointRCNN and VoxelRCNN arhcitectures. All these models differ slightly from one another and share multiple similar components. This library runs the particular modules in a plug and play type of system to run each of the archtectures and shows in the image below:

![openpcarchi](/Assets/image6.jpg)

### Other Libraries Used
- Pytorch
- Google Colab Pro*
- Mayavi - Point cloud visualization
- TensorBoardX
- SpConv: Spatially Sparce Convolution Library 

### Original Voxel-RCNN Architecture (Only Detects CARS)
Given the point cloud data, we perform the following steps in this architecture. 
1. The point clouds are first divided into regular voxels and fed into the 3D backbone network for feature extraction. 
2. Then, the 3D feature volumes are converted into Birds Eye View (BEV) representation, on which they applied the 2D backbone and Region Proposal Network for region proposal generation. 
3. Subsequently,  Voxel Region of Interest (RoI) pooling directly extracts RoI features from the 3D feature volumes. 
4. Finally, the RoI features are exploited in the detect head for further box refinement.

![vrcnn](/Assets/image4.jpg)

### Updated Voxel-RCNN, Our Model (Detects CARS, PEDESTRIANS and CYCLISTS) 
We improved upon the original architeture which only detected cars compared to ours detecting pedestrians and cyclists as well. We took inspriation from the SECOND architecture to extend the capabilities of the original model. The main changes are: 
1. In data preparation filter by minimum points by 5 for pedestrian and cyclist.
2. 2D backbone: Increased number of filters to (128,256).
3. No. of upsample filter changed from 128,128 to 256,256.
4. Added anchor generator config for pedestrian and cyclist based on SECOND model.

![update](/Assets/image13.jpg)

### Results
We tested our model that was trained for 20 epochs on google colab pro. We noticed a slight degradation of ~2% in terms of the detection of cars but we gained good accuracy of detecting pedestrians and cyclists. Our model performs at 89% for EASY data, 85% for MODERATE data and 79% for HARD data. The following plots shows accuracy over the number of epochs: 

![plot](/Assets/image12.jpg)

The training loss evaluation over no. of epochs: 

![loss](/Assets/image2.jpg)

### Visualization
The first scene tested was: 

![scene1](/Assets/image10.jpg)

Output:

![ans](/Assets/image1.jpg)

Voxel-RCNN vs Updated Voxel-RCNN

The second scene tested was: 

![scene2](/Assets/image5.jpg)

Output:

![ans2](/Assets/image9.jpg)

Voxel-RCNN vs Updated Voxel-RCNN

### Instruction To Run Code

**Requirements** 
- Linux (tested on Ubuntu 18.04/20.04
- Python 3.6+
- PyTorch 1.1 or higher (tested on PyTorch 1.1, 1,3, 1,5~1.10)
- CUDA 9.0 or higher (PyTorch 1.3+ needs CUDA 9.2+)
- spconv v1.0 (commit 8da6f96) or spconv v1.2 or spconv v2.x

Run the following commands:
- Step 1
```
  git clone https://github.com/jashmehta20/Voxel-RCNN-OpenPCDet
```
- Step 2: Zip the cloned folder and add to drive to run on Google Colab.
- Step 3: Open Google Colab and the notebook named ColabNotebookRun.ipynb
- Step 4: Mount Drive onto colab notebook.
- Step 5: Download the Kitti Data set from [here](http://www.cvlibs.net/datasets/kitti/eval_object.php?obj_benchmark=3d) and upload it into GDrive in the following format:
```
OpenPCDet
├── data
│   ├── kitti
│   │   │── ImageSets
│   │   │── training
│   │   │   ├──calib & velodyne & label_2 & image_2 & (optional: planes) & (optional: depth_2)
│   │   │── testing
│   │   │   ├──calib & velodyne & image_2
├── pcdet
├── tools
```

- Step 6: Generate the data infos by running the following command:
```
python -m pcdet.datasets.kitti.kitti_dataset create_kitti_infos tools/cfgs/dataset_configs/kitti_dataset.yaml
```
- Step 7: Download the following path files for our trained model to see the test results.
- Step 8: Follow the steps in the colab notebook and run the commands mentioned.
```
%cd drive/MyDrive/OpenPCDetTrain/OpenPCDet/
```
```
!pip install -r requirements.txt
```
```
!pip install spconv-cu111
```
```
!python setup.py develop
```
```
%cd tools/
```
```
%load_ext tensorboard
```
```
%tensorboard --logdir logs
```
```
!python train.py --cfg_file cfgs/kitti_models/voxel_rcnn_cpc.yaml --epochs 20 --batch_size 8
```
```
!python test.py --cfg_file cfgs/kitti_models/voxel_rcnn_cpc.yaml --batch_size 8 --eval_all
```

- Step 9: Please note that the 3D image display does not work in Colab and has to be done on the local system.
 Download the provided pretrained models from the [link](https://drive.google.com/drive/folders/14sUfunEwXeD0WIjfeKvoSEJYs87q1xWu?usp=sharing)

```
pip install mayavi
```
```
python demo.py cfgs/kitti_models/voxel_rcnn_cpc.yaml \
    --ckpt  VoxelRCNNAll20ep.pth \
    --data_path /{dircetory for training/testing data bin file ex. 000008.bin}
```

### Key Takeaways From The Project
In this project we learnt how powerful deep learning architectures are compared to ML model for 3D obstacle detection. Our developed architecture performed at par with the original model but could detect even more classes accuratetly. We successfully learnt the differences in two main models (Voxel-RCNN and SECOND) to create a new model which allows us for robust detection of pedestrians and cyclists. Thank you so much for taking the time to read about our project!

