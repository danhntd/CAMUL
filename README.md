# CAMUL

This repository is the official implementation of the paper entitled: **CAMUL: Context-Aware Multi-conditional Instance Synthesis for Image Segmentation**, accepted to IEEE MultiMedia 2025. <br>
**Authors:** Thanh-Danh Nguyen, Trong-Tai Dam Vu, Bich-Nga Pham, Thanh Duc Ngo, Tam V. Nguyen, and Vinh-Tiep Nguyen†.

[[Paper]](https://doi.org/) [[Code]](https://github.com/danhntd/CAMUL) [[Project Page]](https://danhntd.github.io/projects.html#CAMUL)

## Updates:
- [June 10, 2025] The GitHub repository is initialized. Please stay tuned for further updates!
- [June 10, 2025] The manuscript is accepted to **IEEE Multimedia 2025**. :zap::zap:


## 1. Environment Setup
Download and install Anaconda with the recommended version from [Anaconda Homepage](https://www.anaconda.com/download): [Anaconda3-2019.03-Linux-x86_64.sh](https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh) 
 
```
git clone https://github.com/danhntd/CAMUL.git
cd CAMUL
curl -O https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh
bash Anaconda3-2019.03-Linux-x86_64.sh
```

After completing the installation, please create and initiate the workspace with the specific versions below. The experiments were conducted on a Linux server with a single `GeForce RTX 2080Ti GPU`, CUDA 10.2, Torch 1.7.

```
conda create --name CAMUL python=3
conda activate CAMUL
conda install pytorch==1.7.0 torchvision==0.8.0 torchaudio==0.7.0 cudatoolkit=10.2 -c pytorch
conda env update -f enviroment.yml --prune
```


## 2. Data Preparation
In this work, we mainly use [NEXET](https://www.kaggle.com/datasets/solesensei/nexet-original) and [Cityscapes](https://github.com/mcordts/cityscapesScripts.git) for the training process of the whole framework.

For ADE20K dataset:
```
cd ../ADE20K-process-official

# prepare instance 
python pre_process_instances.py --k 3 --save_folder_name "ADE20K_2021_17_01_official"

# integrated instance & random crop
python post_process_instances.py  --inpainting_dir '../ADE20K_dataset/p2p/version01' --metadata_path '../ADE20K_dataset/ADE20K_2021_17_01_official/metadata_ade20k.json' --save_folder_name "ADE20K_2021_17_01_official"

```

For Cityscape dataset:
```
# Define dataset paths
DATASET_PATH="../cityscapes_synthesis_temp/"                           # Path to cityscape dataset download from web
INPAINTING_PATH="../cityscapes_synthesis/DiffPainting/version01"       # Path to inpainted image from cityscape dataset
OUTPUT_DATASET_PATH="cityscapes_diffpainting"                          # Path to new dataset

# Step 1: Instance selection
# k :  number of instance to be selected 

python process.py \
  --k 3 \
  --train_folder "$DATASET_PATH"

# Step 2: Integrate inpainted images with metadata and cropped region coordinates 

python inpainted_integration.py \
  --inpainting_dir "$INPAINTING_PATH" \
  --metadata_path "${DATASET_PATH}metadata.json" \
  --cropped_region_coordinates "${DATASET_PATH}cropped_region_coordinates.json" \
  --output_folder_name "$OUTPUT_DATASET_PATH"

# Step 3: Move Raw Data to the Output Dataset Folder

python move_folder.py \
  --src_path "$DATASET_PATH" \
  --des_path "$OUTPUT_DATASET_PATH"

# Step 4: Crop raw color & labelIds in the raw city folder to match new image pairs

python drop_color_and_labelIds.py \
  --cropped_region_coordinates_path "${OUTPUT_DATASET_PATH}/cropped_region_coordinates.json"

# Step 5: Calculate Polygons for Inpainted Images
cd cityscapes-to-coco-conversion
python main_defined.py \
  --dataset cityscapes \
  --datadir "../$OUTPUT_DATASET_PATH"
```




## 3. Training Pipeline
Our proposed CAMUL is described as follows:
<img align="center" src="https://danhntd.github.io/images/camul_project.jpg">


<!-- ## 4. Visualization -->



## Citation
Please use this bibtex to cite this repository:
```
@article{nguyen2025camul,
  title={CAMUL: Context-Aware Multi-conditional Instance Synthesis for Image Segmentation},
  author={Nguyen, Thanh-Danh and Vu, Trong-Tai Dam and Pham, Bich-Nga and Ngo, Thanh Duc and Nguyen, Tam V and Nguyen, Vinh-Tiep},
  journal={IEEE MultiMedia},
  year={2025},
  publisher={IEEE}
}
```


<!-- ## Acknowledgements -->
