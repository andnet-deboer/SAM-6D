cd SAM-6D

##### 1. Download SAM (Base version, same as your laptop)
```mkdir -p Instance_Segmentation_Model/checkpoints/segment-anything``` \
```wget -P Instance_Segmentation_Model/checkpoints/segment-anything/ https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth```

##### 2. Download DINOv2
```mkdir -p Instance_Segmentation_Model/checkpoints/dinov2``` \
```wget -P Instance_Segmentation_Model/checkpoints/dinov2/ https://dl.fbaipublicfiles.com/dinov2/dinov2_vitl14/dinov2_vitl14_pretrain.pth```

##### 3. Run the Container
docker run --gpus all -it \
    -v $(pwd):/workspace/SAM-6D \
    --name sam6d_server \
    sam6d-custom:v1 \
    /bin/bash```

```cd /workspace/SAM-6D```

##### Export variables
```export CAD_PATH=$(pwd)/Data/Example/obj_000005.ply```\
```export RGB_PATH=$(pwd)/Data/Example/rgb.png```\
```export DEPTH_PATH=$(pwd)/Data/Example/depth.png```\
```export CAMERA_PATH=$(pwd)/Data/Example/camera.json```\
```export OUTPUT_DIR=$(pwd)/Data/Example/outputs```

##### Run
```chmod +x demo.sh```
```./demo.sh```
