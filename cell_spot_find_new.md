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
cd /media/zenglab/result/YourDirectory//spotfinder
```

Run the script
```batchfile
chmod +x countspots2drun.sh
./countspots2drun.sh /media/zenglab/result/lingyuan/spotcount/T
```

During the execution, you'll see logs similar to the following:
```batchfile

```

If you have a large number of images to process, I strongly recommend using the `nohup` command or `tmux` to run the program in the server backend.

```batch
$ nohup ./countspots2drun.sh /media/zenglab/result/lingyuan/spotcount/T > count.log &
```

Use `top` or `htop` to monitor the process.





