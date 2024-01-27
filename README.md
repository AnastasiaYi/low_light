# low_light

## Install the environment
Use the Anaconda
```
conda create -n mixformer python=3.6
conda activate mixformer
bash install_pytorch17.sh
```
## Data Preparation
The model is trained using the GOT-10k dataset, which can be found at http://got-10k.aitestunion.com/downloads.

Download the dataset and place the tracking datasets in the ```./MixFormer/data/got10k ``` directory. 

The directory structure should look like this:
   ```
   ${low_light_ROOT}
  -- MixFormer
      -- data
          -- got10k
              |-- test
              |-- train
              |-- val
   ```



## Set project paths
Run the following command in ./MixFormer folder to set paths for this project

```
python tracking/create_default_local_file.py --workspace_dir . --data_dir ./data --save_dir .
```
After running this command, you can also modify paths by editing these two files
```
/MixFormer/lib/train/admin/local.py  # paths about training
/MixFormer/lib/test/evaluation/local.py  # paths about testing
```

## Train the model

Training with multiple GPUs using DDP. Train_mixformer_cvt is used as the backbone. More details of training settings can be found at ```/MixFormer/tracking/train_mixformer_cvt.sh```
``` 
bash tracking/train_mixformer_cvt.sh
```
