## Image naming format

Feel free to name the folder as you please.
For images within the folder, follow the format "*ch00.tif", "*ch01.tif", "*ch02.tif"... The **DAPI** channel must be named "**ch00**," while the other fluorescence channels could be named "ch01" and "ch02"... in your preferred order.

## Transfer raw data

Transfer raw data into /media/zenglab/data/YourDirectory/ImageDirectory
[FileZilla](https://filezilla-project.org) is a good choice for both windows and macOS user to transfer files.

For detailed instructions, please see instructions in the [Zilla.md](https://github.com/ZenghuPKU/cell_spot_finding/blob/main/Zilla.md) page

## Login 
For windows users, use MobaxTerm/cmd.

For macOS users, use Termius/Terminal.

Please see instructions in the [SSH&DataManagement.md](https://github.com/ZenghuPKU/zenglab_server/blob/main/SSH%26DataManagement.md) page

> NOTE: Remote ssh in vscode is a good choice for visualization.

## Preparation

Create a symbolic link.

```batch
ln -s /media/zenglab/data/YourDirectory/ImageDirectory /media/zenglab/result/YourDirectory/ImageDirectory
```

Copy script to your own space.
**You only need to copy this once; later, you can just use it in your own directory.**
```batchfile
cp -r /media/zenglab/script/yly/spotfinder /media/zenglab/result/YourDirectory
```

Activate public conda environment.

```batch
source /media/zenglab/anaconda3/etc/profile.d/conda.sh
conda activate cellpose
```

## Run the script
Go to the script

```batchfile
cd /media/zenglab/result/YourDirectory/spotfinder
```

Run the script
```batchfile
chmod +x countspots2drun.sh
./countspots2drun.sh /media/zenglab/result/YourDirectory 0.2
```
> 0.2 is a suitable threshold in most cases. If you feel you’re missing some counts, please lower the threshold; if you think you’re counting too many, please raise the threshold.

During the execution, you'll see logs similar to the following:
```batchfile
Processing dapi directory with pose.py
>>> GPU activated? YES
Cell counts have been saved to /media/zenglab/result/lingyuan/spotcount/Ttest/dapi_counts.csv
Mask images have been saved to /media/zenglab/result/lingyuan/spotcount/Ttest/ProcessedDapi
Processing spot directory with RunAndPlotSpots.m

                                      < M A T L A B (R) >
                            Copyright 1984-2023 The MathWorks, Inc.
                       R2023b Update 6 (23.2.0.2485118) 64-bit (glnxa64)
                                       December 28, 2023

 
To get started, type doc.
For product information, visit www.mathworks.com.
 
Now reading /media/zenglab/result/lingyuan/spotcount/Ttest/spot/A1-1_RGB_ch02.tif
Now reading /media/zenglab/result/lingyuan/spotcount/Ttest/spot/A1-1_betotime_ch02.tif
Now reading /media/zenglab/result/lingyuan/spotcount/Ttest/spot/A1-1_tempo_ch02.tif
Now reading /media/zenglab/result/lingyuan/spotcount/Ttest/spot/A1-2_RGB_ch02.tif
Now reading /media/zenglab/result/lingyuan/spotcount/Ttest/spot/B1_3_5ADVMLE.vsi_rgb_CH(CON640)_1CH_ch03.tif
All processing completed successfully.
```

If you have a large number of images to process, I strongly recommend using the `nohup` command or `tmux` to run the program in the server backend.

```batch
nohup ./countspots2drun.sh /media/zenglab/result/YourDirectory 0.2 > count.log &
```

Use `top` or `htop` to monitor the process.

> If you encounter a bug in the MATLAB interface during execution, please enter ‘exit()’ to exit.

## Result data

You are expected to see result files with the following structure.

```batchfile
YourDirectory/
│
├── dapi/
│   ├── *ch00.tif
│
├── spot/
│   ├── *ch01.tif
│   ├── *ch02.tif
│   ├── ...
│
├── ProcessedDapi/
│   ├── *mask.png
│   
├── ProcessedSpot/
│   ├── *spots.png
│
├── dapi_counts.csv
│
└── spot_counts.csv
```

- The ‘dapi’ directory contains the source images for ch00, and the ‘spot’ directory contains the source images for other channels.
- The ‘ProcessDapi’ directory contains the images after DAPI counting, and the ‘ProcessedSpot’ directory contains the images after fluorescence spot counting, which are used to assess counting accuracy.
- The dapi_counts CSV file contain the final statistics for cell count.
- The spot_counts CSV file contain three information:
  1. final statistics for spot count
  2. the total fluorescence intensity of the counted points
  3. the fluorescence intensity of the entire image (the same as fiji)


