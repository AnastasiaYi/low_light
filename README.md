# low_light
## Model structure
The proposed low-light model combines three modified models: MixForm, SUNet, and EnlightenGAN, to create a unified workflow for object tracking in low-light scenes. The workflow begins with denoising using SUNet, followed by video enhancement using EnlightenGAN, and finally object tracking using MixFormer. The directory structure for the project is as follows:
   ```
   ${low_light_ROOT}
  -- EnlightenGAN
  -- MixFormer
      -- SUNet
      ...
   ```
## Install the environment
Use the Anaconda
```
conda create -n mixformer python=3.6
conda activate mixformer
bash install_pytorch17.sh
```
## Data preparation
The model is trained using the GOT-10k dataset, which can be found at http://got-10k.aitestunion.com/downloads.

Download the dataset and place the validation part of the dataset in the ```./MixFormer/data/got10k ``` directory. The validation set of the GOT-10k dataset is used for training due to memory constraint.

The directory structure should look like this:
   ```
   ${low_light_ROOT}
  -- MixFormer
      -- data
          -- got10k
              |-- val
   ```

In ```/MixFormer/lib/train/admin/local.py```, please specify: 
```
# input path
self.got10k_dir = '/content/low_light/MixFormer/data/got10k/val'
# target path
self.got10k_gt_dir = '/content/low_light/MixFormer/data/got10k/val'
```




## Set project paths
Run the following command in ./MixFormer folder to set paths for this project

```
python tracking/create_default_local_file.py --workspace_dir . --data_dir ./data --save_dir .
```
After running this command, you can also modify paths by editing these two files
```
/MixFormer/lib/train/admin/local.py  # paths about training
```

## Train the model

Training with multiple GPUs using DDP. Train_mixformer_cvt is used as the backbone. More details of training settings can be found at ```/MixFormer/tracking/train_mixformer_cvt.sh```
``` 
bash tracking/train_mixformer_cvt.sh
```
