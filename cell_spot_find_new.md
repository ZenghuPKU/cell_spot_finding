source /media/zenglab/anaconda3/etc/profile.d/conda.sh
conda activate cellpose

ln -s /media/zenglab/data/YourDirectory/ImageDirectory /media/zenglab/result/YourDirectory/ImageDirectory

cp -r /media/zenglab/script/yly/spotfinder /media/zenglab/result/YourDirectory
cd YourDirectory/spotfinder

chmod +x countspots2drun.sh
./countspots2drun.sh /media/zenglab/result/lingyuan/spotcount/T
