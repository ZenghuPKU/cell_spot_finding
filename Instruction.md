## Image naming format
Folder can be named whatever you want.
Images in folder should be named as "A1-1_*_ch00.tif" where A1 represents the well name, 1 represents position in a well, ch represents fluo channel.
**dapi channel** must be named as **ch00**, other two fluo channels should be named as ch01, ch02 in order you want.

## Login 
Please see instructions in the [SSH&DataManagement.md](https://github.com/ZenghuPKU/zenglab_server/blob/main/SSH%26DataManagement.md) page

## Activate public conda environment

```batchfile
source /media/zenglab/miniconda3/etc/profile.d/conda.sh 
conda activate publiccenv
```

## Copy script to your own space
cp /media/zenglab/script/yly/find_batch.py YourPath

## Run the script
Run help command for instruction

```batchfile
$python find_batch.py --help
```

Then you will see instruction like below

```batchfile
2024-06-14 10:57:09.944248: I tensorflow/core/platform/cpu_feature_guard.cc:210] This TensorFlow binary is optimized to use available CPU instructions in performance-critical operations.
To enable the following instructions: AVX2 AVX512F AVX512_VNNI FMA, in other operations, rebuild TensorFlow with the appropriate compiler flags.
2024-06-14 10:57:10.837019: W tensorflow/compiler/tf2tensorrt/utils/py_utils.cc:38] TF-TRT Warning: Could not find TensorRT
usage: find_batch.py [-h] --input_dir INPUT_DIR --output_image_dir OUTPUT_IMAGE_DIR --output_csv
                     OUTPUT_CSV [--alpha ALPHA] [--beta BETA] [--bin_threshold BIN_THRESHOLD]
                     [--area_threshold AREA_THRESHOLD]

Process images for cell number counting.

options:
  -h, --help            show this help message and exit
  --input_dir INPUT_DIR
                        Input directory containing .tif images.
  --output_image_dir OUTPUT_IMAGE_DIR
                        Output directory for processed images.
  --output_csv OUTPUT_CSV
                        Output CSV file for cell numbers.
  --alpha ALPHA         Contrast factor, the default value of alpha is 1.0, and you can adjust
                        this value as needed. Typically, the value of alpha ranges from 0.1
                        (very low contrast) to 3.0 (very high contrast).
  --beta BETA           Brightness factor, the default value of beta is 0, and you can adjust
                        this value as needed. Typically, the value of beta ranges from -100
                        (very dark image) to 100 (very bright image).
  --bin_threshold BIN_THRESHOLD
                        Binary threshold for image processing.
  --area_threshold AREA_THRESHOLD
                        Area threshold for image processing.
```

Here give an example
