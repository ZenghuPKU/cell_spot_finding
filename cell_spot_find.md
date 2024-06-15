## Image naming format

Feel free to name the folder as you please.
For images within the folder, follow the format "A1-1_*_ch00.tif," where "A1" denotes the well name, "1" indicates the position in the well, and "ch" represents the fluorescence channel. The **DAPI** channel must be named "**ch00**," while the other fluorescence channels could be named "ch01" and "ch02"... in your preferred order.

An example for change name from *C405.tif to *ch00.tif in batches.

> NOTE: BE CAREFUL! This action is irreversible!

```batch
rename 's/C405.tif$/ch00.tif/' *.tif
```

## Transfer raw data

Transfer raw data from microscopy to `/media/zenglab/data/YourDirectory` in one of the following ways.

#### scp

Just open cmd on Windows or Terminal on macOS — no software download needed.
```batch
scp ImageDirectoryInHardDrive YourUserName@10.128.243.62:/media/zenglab/data/YourDirectory
```

#### Mobaxterm

For windows user, [mobaxterm](https://mobaxterm.mobatek.net/) is a useful for terminal managment.

#### Termius

For macOS user, [Termius](https://termius.com) is a useful for terminal managment.

A GitHub Education version account is required for login.

#### FileZilla

[FileZilla](https://filezilla-project.org) is a good choice for both windows and macOS user to transfer files.

For detailed instructions, please see instructions in the [Zilla.md](https://github.com/ZenghuPKU/cell_spot_finding/blob/main/Zilla.md) page.

## Login 
For windows users, use MobaxTerm/cmd.

For macOS users, use Termius/Terminal.

Please see instructions in the [SSH&DataManagement.md](https://github.com/ZenghuPKU/zenglab_server/blob/main/SSH%26DataManagement.md) page

## Preparation

Go to your own working space.

```batch
cd /media/zenglab/result/YourDirectory
```

Create a symbolic link.

```batch
ln -s /media/zenglab/data/YourDirectory/ImageDirectory ImageDirectory
```
> The example data for this demonstration is located at this path: /media/zenglab/script/yly/ExampleImage

Copy script to your own space.

```batchfile
cp /media/zenglab/script/yly/find_final.py find_final.py
```

Activate public conda environment.

```batch
source /media/zenglab/miniconda3/etc/profile.d/conda.sh 
conda activate stardist
```


## Run the script
Invoke the 'help' command for guidance.

```batchfile
$python find_final.py --help
```

Then you will see the following prompt.

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

Here's an example using basic parameters without adjusting thresholds, contrast, and brightness. You can modify these according to your own images. The current default settings do not alter brightness and contrast, and utilize relatively universal thresholds.

```batchfile
$ python find_final.py --input_dir ExampleImage/nozstack --output_image_dir nozstack_res --output_csv nozstack.csv
```

During the execution, you'll see logs similar to the following:

```batch
2024-06-14 11:37:37.267706: I tensorflow/core/platform/cpu_feature_guard.cc:210] This TensorFlow binary is optimized to use available CPU instructions in performance-critical operations.
To enable the following instructions: AVX2 AVX512F AVX512_VNNI FMA, in other operations, rebuild TensorFlow with the appropriate compiler flags.
2024-06-14 11:37:37.889700: W tensorflow/compiler/tf2tensorrt/utils/py_utils.cc:38] TF-TRT Warning: Could not find TensorRT
Found model '2D_versatile_fluo' for 'StarDist2D'.
2024-06-14 11:37:38.752974: E external/local_xla/xla/stream_executor/cuda/cuda_driver.cc:282] failed call to cuInit: CUDA_ERROR_SYSTEM_DRIVER_MISMATCH: system has unsupported display driver / cuda driver combination
2024-06-14 11:37:38.753004: I external/local_xla/xla/stream_executor/cuda/cuda_diagnostics.cc:134] retrieving CUDA diagnostic information for host: langchao-NF5468M6
2024-06-14 11:37:38.753010: I external/local_xla/xla/stream_executor/cuda/cuda_diagnostics.cc:141] hostname: langchao-NF5468M6
2024-06-14 11:37:38.753099: I external/local_xla/xla/stream_executor/cuda/cuda_diagnostics.cc:165] libcuda reported version is: 550.67.0
2024-06-14 11:37:38.753115: I external/local_xla/xla/stream_executor/cuda/cuda_diagnostics.cc:169] kernel reported version is: 550.54.15
2024-06-14 11:37:38.753120: E external/local_xla/xla/stream_executor/cuda/cuda_diagnostics.cc:251] kernel version 550.54.15 does not match DSO version 550.67.0 -- cannot find working devices in this configuration
Loading network weights from 'weights_best.h5'.
Loading thresholds from 'thresholds.json'.
Using default values: prob_thresh=0.479071, nms_thresh=0.3.
processing images in ch00...
base.py (406): Predicting on non-float input... ( forgot to normalize? )
2024-06-14 11:37:45.490581: W external/local_tsl/tsl/framework/cpu_allocator_impl.cc:83] Allocation of 536870912 exceeds 10% of free system memory.
2024-06-14 11:37:45.604253: W external/local_tsl/tsl/framework/cpu_allocator_impl.cc:83] Allocation of 536870912 exceeds 10% of free system memory.
2024-06-14 11:37:46.394366: W external/local_tsl/tsl/framework/cpu_allocator_impl.cc:83] Allocation of 536870912 exceeds 10% of free system memory.
2024-06-14 11:37:54.718991: W external/local_tsl/tsl/framework/cpu_allocator_impl.cc:83] Allocation of 536870912 exceeds 10% of free system memory.
2024-06-14 11:37:54.771373: W external/local_tsl/tsl/framework/cpu_allocator_impl.cc:83] Allocation of 536870912 exceeds 10% of free system memory.
-------------------------counting for cell number DONE--------------------------
processing images in ch01...
-----------------------counting for spot in ch01 has DONE-----------------------
processing images in ch02...
-----------------------counting for spot in ch02 has DONE-----------------------
------------------------------all works have DONE!------------------------------
```

> NOTE:
> - Ignore any warnings that may appear.
> - Cell counting for ch00 may take longer, please be patient. (Approximately several tens of seconds per image).
> - When you see this message "all works have DONE!", it indicates that the script has finished running. Please review the image results to decide whether adjustments to thresholds, brightness, or contrast are needed. You can check the CSV file to view the final statistical results.

If you have a large number of images to process, I strongly recommend using the `nohup` command or `tmux` to run the program in the server backend.

```batch
$ nohup python find_final.py --input_dir ExampleImage/nozstack --output_image_dir nozstack_res --output_csv nozstack.csv > nozstack.log &
```

Use `top` or `htop` to monitor the process.

## z-stack
If your images have a z-axis, please first run this script to perform a maximum intensity projection(MIP). Then you can use the file generated by this script for cell counting or point counting.

```batchfile
cp /media/zenglab/script/yly/mip.py mip.py
```

Invoke the 'help' command for guidance.

```batchfile
$python mip.py --help
```

Then you will see the following prompt.

```batchfile
usage: mip.py [-h] --input_dir INPUT_DIR --output_dir OUTPUT_DIR

Maximum intensity projection of a stack of images.

options:
  -h, --help            show this help message and exit
  --input_dir INPUT_DIR
                        The directory containing the input images.
  --output_dir OUTPUT_DIR
                        The directory where the output images will be saved.
```

Here's an example.

```batchfile
$ python mip.py --input_dir ExampleImage/zstack --output_dir zstack_mip
```

After creating MIP images, you can now follow similar steps for counting.

```batchfile
$ python find_final.py --input_dir zstack_mip --output_image_dir zstack_res --output_csv zstack.csv
```


