# low_light
## Model structure
The proposed low-light model combines three modified models: MixForm, SUNet, and EnlightenGAN, to create a unified workflow for object tracking in low-light scenes. The workflow begins with enhancement using EnlightenGAN, followed by denoising using SUNet, and finally object tracking using MixFormer. The directory structure (for now) for the project is as follows:
   ```
   ${low_light_ROOT}
  -- EnlightenGAN
  -- MixFormer
      -- SUNet
      ...
   ```
## Install the environment
Use Anaconda
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


## Set project paths
Run the following command in ./MixFormer folder to set paths for this project

```
python tracking/create_default_local_file.py --workspace_dir . --data_dir ./data --save_dir .
```
After running this command, in ```/MixFormer/lib/train/admin/local.py```, please specify: 
```
# input path -- synthesised low-light dataset.

self.got10k_dir = '/content/low_light/MixFormer/data/got10k/dark'
# target path -- clean daylight image used for training SUNet.
self.got10k_gt_dir = '/content/low_light/MixFormer/data/got10k/val'
```

## Train the model

### Download the pretrained CVT model
Download the cvt model ```CvT-21-384x384-IN-1k.pth``` from [here](https://onedrive.live.com/?authkey=%21AMXesxbtKwsdryE&id=56B9F9C97F261712%2115004&cid=56B9F9C97F261712). Create a ```pretrained``` folder and a ```trained``` folder for saving the MixFormer checkpoint files under the ```MixFormer``` directory. Once it's done, it should look like this:

   ```
   ${low_light_ROOT}
  -- MixFormer
      -- data
      ...
      -- pretrained
         -- CvT-21-384x384-IN-1k.pth
      -- trained
   ```

### Run training
CVT is used as the backbone. Configuration script can be found at ```/MixFormer/experiments/mixformer_cvt/baseline_1k.yaml```. Run ```/MixFormer/tracking/train_mixformer_cvt.sh``` to train. You can also specify the number of gpu used by modifying the ```--nproc_per_node``` in ```/MixFormer/tracking/train_mixformer_cvt.sh```.
``` 
bash tracking/train_mixformer_cvt.sh
```
