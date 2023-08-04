# Vehicle Tracking using YOLOv7 + DeepSORT

## Method
- Use [YOLOv7]((https://github.com/WongKinYiu/yolov7)) for vehicle detection task, only considers objects in Region of Interest (ROI)
- Use [DeepSORT](https://arxiv.org/abs/1703.07402) for car tracking, not need to retrain this model, only inference
- Use Cosine Similarity to assign object's tracks to most similar directions.
- Count each type of vehicle on each direction.

## Notebook
- For inference, use this notebook [![Notebook](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1ewPDoJzp7xfbUqEPOA-OBxZsEmKh5VmR?usp=sharing)
- To retrain detection model, follow instructions from [original Yolov5](https://github.com/ultralytics/yolov5)

-----------------------------------------------------------

## Dataset
- AIC-HCMC-2020: [link](https://drive.google.com/drive/folders/1VPSEn1thUa-Y1NtzSiBbinpwOjYW_GL5?usp=share_link)
- Direction and ROI annotation format:
```
cam_01.json # match video name
{
    "shapes": [
        {
            "label": "zone",
            "points": [[x1,y1], [x2,y2], [x3,y3], [x4,y4], ... ] #Points of a polygon
        },
        {
            "label": "direction01",
            "points": [[x1,y1], [x2,y2]] #Points of vector
        },
        {
            "label": "direction{id}",
            "points": [[x1,y1], [x2,y2]]
        },...
    ],
}
```

<div align="center"><img width="1000" alt="screen" src="demo/dataset.png"></div>

## Pretrained weights
- Download finetuned models from on AIC-HCMC-2020 dataset:
- pretrained yolov7 weights: https://github.com/WongKinYiu/yolov7/releases
- change weights on networks/yolo.py

## **Inference**

- File structure
```
this repo
│   detect.py
└───configs
│      configs.yaml           # Contains model's configurations
│      cam_configs.yaml       # Contains DEEPSORT's configuration for each video
```

- Install dependencies by ```pip install -r requirements.txt```
- To run full pipeline:
```
python run.py --input_path=<input video or dir> --output_path=<output dir> --weight=<trained weight>
```
- **Extra Parameters**:
    - ***--min_conf***:     minimum confident for detection
    - ***--min_iou***:      minimum iou for detection

-----------------------------------------------------------

## Results

- After running, a .csv file contains results has following example format:

track_id |	frame_id |	box	| color |	label |	direction |	fpoint |	lpoint |	fframe |	lframe
--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
2	| 3	| [607, 487, 664, 582]	| (144, 238, 144) |	0	| 1	| (635.5, 534.5)	| (977.0, 281.5)	| 3	| 109
2	| 4	| [625, 475, 681, 566]	| (144, 238, 144)	| 0	| 1	| (635.5, 534.5)	| (977.0, 281.5)	| 3	| 109
2	| 5	| [631, 471, 686, 561]	| (144, 238, 144)	| 0	| 1	| (635.5, 534.5)	| (977.0, 281.5)	| 3	| 109

- With:
  - `track_id`: the id of the object
  - `frame_id`: the current frame
  - `box`: the box wraps around the object in the corresponding frame
  - `color`: the color which is used to visualize the object
  - `direction`: the direction of the object
  - `fpoint`, `lpoint`: first/last coordinate where the object appears 
  - `fframe`, `lframe`: first/last frame where the object appears 
  
| Visualization result |
|:-------------------------:|
|<img width="1604" alt="screen" src="demo/cam_04.gif">|
|<img width="1604" alt="screen" src="demo/cam_07.gif">|


## References
- DeepSORT from https://github.com/ZQPei/deep_sort_pytorch
- YOLOv7 from https://github.com/WongKinYiu/yolov7
