# Potential of reconstruction of GEDI AGBD model in Japanese evergreen needleleaf forest using simulated GEDI Data

## **Research Flow**

**Type1**



**Type2**



## How GEDI team develop the AGB models

Kellner, J. R., et al. (2023). "Algorithm theoretical basis document for GEDI footprint aboveground biomass density." Earth and Space Science 10(4): e2022EA002516.

Duncanson, L., et al. (2022). "Aboveground biomass density models for NASA’s Global Ecosystem Dynamics Investigation (GEDI) lidar mission." Remote Sensing of Environment 270: 112845.

Hancock, S., et al. (2019). "The GEDI simulator: A large‐footprint waveform lidar simulator for calibration and validation of spaceborne missions." Earth and Space Science 6(2): 294-310.



#### Filtering:

1. when AGBD was <1 Mg/ha and RH98 was >5 m, the footprint was excluded ;When AGBD was >150 Mg/ha and RH98 was <5 m, the footprint was excluded

2. Canopy-cover Fraction was 0 and RH98  >5  m, the footprint was excluded 

3. When the difference between measured or predicted height and RH98 was >10 m, the footprint was excluded

4. If a footprint contained at least one tree with a diameter, height, or wood specific gravity outside the range defined by the original data, the footprint was excluded

5. If >10% of the area of a simulated footprint was outside the boundaries of a field inventory plot, it 

   was excluded

#### GEDI algorithm:

<img src="C:\Users\l\AppData\Roaming\Typora\typora-user-images\image-20240302153956856.png" alt="image-20240302153956856" style="zoom:50%;" />

## Importance of reconstruction of GEDI AGB models in East Asia

Kellner, J. R., et al. (2023). "Algorithm theoretical basis document for GEDI footprint aboveground biomass density." Earth and Space Science 10(4): e2022EA002516.

**East Asia lacks of training data for developing GEDI AGB models.**



## ALS-DATA needed files

**Type 1:** 

1. **ALS-DATA performance report**. should include basic ALS info such as flight height, flight speed, **point density**, **sensor name**, aircraft name, **flight period**.  The info highlighted by bold refers required info.
2. **Point cloud data in LAS file.**
3. **ALS-derived tree points data.**
4. Ground referenced data and report in each prefecture.

Where 1 is used to demonstrate the ALS-based data could be used as referenced data for calibrating GEDI data. 2 is utilized for geolocation adjustment of GEDI footprints by waveform match using GEDI simulator. 3 is used to develop new GEDI AGB model with GEDI L2A,L2B data. 4 is used as cross-validate the novel models' accuracy. 

**Type2:**

1. **ALS-DATA performance report** like type1.
2. **High resolution DEM data (1-m ,2-m)**
3. **ALS-derived tree points data.**
4. Ground referenced data and report in each prefecture.

Where 1 is used to demonstrate the ALS-based data could be used as referenced data for calibrating GEDI data.2 is utilized for geolocation adjustment of GEDI footprints by DEM match using grid search. 3 is used to develop new GEDI AGB model with GEDI L2A,L2B data. 4 is used as cross-validate the novel models' accuracy. 

# Case study on Aichi prefecture

## Purpose:

1. Evaluate the two types of approaches for GEDI geolocation adjustment (i.e:, Type 1 and Type 2).
2. Potential Reconstruction of GEDI AGBD models in Japan.
3. Evaluate the model's errors.

## Timeline:

- 2024.03.01-2024.04.01: GEDI simulator leaning- Input GEDI L1B waveform and ALS-point cloud-output revised L1B geolocation and simulated waveform from ALS data.
- 2024.03.01-2024.05.01：Processing Aichi prefecture data.
- 2024.05.01-2024.06.01:    Data analysis
- 2024.06.01-2024.07.01:   Figures,tables and draft.
- 2024.07.15: preparing for submission.

### Phase1:Running the Simulator 

https://bitbucket.org/StevenHancock/gedisimulator/src/master/README.md

```shell
singularity exec --bind /home/lihantao/Simulator/ gediSingularity gediRat -help
##启动simulator linux
```

#### Function of simulator

##### GediRat

```shell
gediRat -/mnt/hgfs/LAS_temp/.06qe952_org.las -coord $lon $lat -output /mnt/hgfs/LAS_temp/mnt/hgfs/waveform.txt
```

```shell
singularity exec --bind /home/lihantao/Simulator/ gediSingularity gediRat -input /home/lihantao/Simulator/19042262CE006.las -coord $lon $lat -output /home/lihantao/Simulator/waveform.txt
##Mode 1: Single coordinate

singularity exec --bind /home/lihantao/Simulator/ gediSingularity gediRat -input /home/lihantao/Simulator/SERC_Jun2012_c2r0.las -coord 365090 4306825 -output /home/lihantao/Simulator/ wave.h5 -ground -hdf

singularity exec --bind /home/lihantao/Simulator/ gediSingularity gediRat -input 19042262CE006.las -coord -27101 -137102 -output my_wave.h5 -ground -hdf

--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE

sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity gediRat -input /mnt/hgfs/LAS_temp/LAS/LAS_G/text.las -output /mnt/hgfs/LAS_temp/LAS/LAS_G/text.h5 -hdf -ground -coord -13370.189431 -69642.858381 -pSigma 5.5 -pFWHM 15.6 -checkCover -gridBound 24000.0 25999.999 -105000.0 -103500.001

sudo singularity exec --bind /mnt/hgfs/LAS_temp/ gediSingularity gediMetric -input /mnt/hgfs/LAS_temp/LAS/LAS_G/text.h5 -readHDFgedi -ground -varScale 3.5 -sWidth 0.8 -rhRes 2 -laiRes 5  -outRoot /mnt/hgfs/LAS_temp/LAS/LAS_G/text

-photonCount -nPhotons

--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE

sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity gediRat -input /mnt/hgfs/LAS_temp/LAS_E/LAS/07oe8231_org.las -output /mnt/hgfs/LAS_temp/LAS/LAS_G/output/07md1514.h5 -ground -hdf -gridStep 50 -pSigma 5.5 -pFWHM 15.6 -gridBound -18613.643 -18000.001 -64500.0 -64054.16

sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity gediMetric -input /mnt/hgfs/LAS_temp/LAS/LAS_G/output_h5/07md1514.h5 -readHDFgedi -ground -varScale 3.5 -sWidth 0.8 -rhRes 1 -laiRes 5 -outRoot /mnt/hgfs/LAS_temp/LAS/LAS_G/output_txt/07md1514

--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE--NOTE
##Mode 2: list of coordinates
singularity exec --bind /home/lihantao/Simulator/ gediSingularity gediRat -input 19042262CE006.las  -output my_waves.h5 -ground -hdf -listCoord coordList.txt

##Mode 3: grid of footprints
sudo singularity exec --bind /mnt/hgfs/LAS_temp/ gediSingularity gediRat -input /mnt/hgfs/LAS_temp/LAS/LAS/xxx.las  -output /mnt/hgfs/LAS_temp/LAS/xxx.h5 -ground -hdf -gridStep 50 -pSigma 5.5 -pFWHM 15.6 -gridBound 19119.056 24581.526 -112637.599 -110078.442

sudo singularity exec --bind /mnt/hgfs/LAS_temp/ gediSingularity gediRat -input /mnt/hgfs/LAS_temp/LAS/LAS/T02_nk.las -output /mnt/hgfs/LAS_temp/LAS/LAS/output/T02_nk.h5 -ground -hdf -gridStep 50 -pSigma 5.5 -pFWHM 15.6 -gridBound 19150.586 24628.112 -112473.33 -109869.928



sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity gediRat -input /mnt/hgfs/LAS_temp/LAS/alsList.txt  -output waveGrid.h5 -ground -hdf -gridStep 50 -gridBound 19119.056 24581.526 -112637.599 -110078.442






sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity gediRat -inList /mnt/hgfs/LAS_temp/LAS/alsList.txt  -output waveGrid.h5 -ground -hdf -gridStep 2000 -pSigma 5.5 -pFWHM 15.6 -gridBound -31256.215 61101.077 -157710.903 -64132.7072
```

****

## gediMetric

```shell
sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity gediMetric -input /mnt/hgfs/LAS_temp/LAS/LAS_G/output_h5/xxx -readHDFgedi -ground -varScale 3.5 -sWidth 0.8 -rhRes 1 -laiRes 5 -outRoot /mnt/hgfs/LAS_temp/LAS/LAS_G/output_txt/xxx

```

## Co-locate

```shell
collocateWaves -listALS alsList.txt -gedi waveforms.h5 -readHDFgedi -aEPSG 32622 -solveSofG -geoError 30 5 -fixFsig -writeWaves simulated.h5 -minDense 3 -minSense 0.9

sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity collocateWaves -listALS  /mnt/hgfs/LAS_temp/LAS/alsList.txt -listGedi /mnt/hgfs/LAS_temp/LAS/gediList.txt -readHDFgedi -aEPSG 2249 -geoError 30 5 -fixFsig -writeWaves simulated.h5 -minDense 3 -minSense 0.9

sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity collocateWaves -listALS  /mnt/hgfs/LAS_temp/LAS/alsList.txt -listGedi /mnt/hgfs/LAS_temp/LAS/gediList.txt -readHDFgedi -aEPSG 2249 -geoError 30 5 -fixFsig -writeWaves simulated.h5 -minDense 3 -minSense 0.9 -leaveEmpty



sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity collocateWaves -listALS /mnt/hgfs/LAS_temp/LAS/alsList.txt -gedi /mnt/hgfs/LAS_temp/LAS/GEDI/GEDI01_B_2021233193643_O15245_02_T07213_02_005_02_V002.h5 -readHDFgedi -aEPSG 2449 -geoError 50 5 -fixFsig -writeWaves simulated.h5 -minDense 3 -minSense 0.9

#!
sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity collocateWaves -listALS alsList.txt -gedi /mnt/hgfs/LAS_temp/LAS/GEDI/GEDI01_B_2019191204111_O03261_03_T05611_02_005_01 -readHDFgedi -aEPSG 2449 -geoError 30 5 -fixFsig -writeWaves /mnt/hgfs/LAS_temp/LAS/simulated.h5 -minDense 3 -minSense 0.9 -leaveEmpty

------------------------------------------------------------------------------
------------------------------------------------------------------------------
sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity collocateWaves -help
这玩意是对的
sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity collocateWaves -listALS /mnt/hgfs/LAS_temp/LAS/alsList.txt -gedi root_path -readHDFgedi -aEPSG 2249 -geoError 30 5 -fixFsig -writeWaves simulated.h5 -minDense 3 -minSense 0.9 -leaveEmpty
以下都是不可
while IFS= read -r gedi_file; do
  p="$gedi_file"  # 这里应该使用等号(`=`)和引号将变量值引起来
  echo "${p}"
done < "/mnt/hgfs/LAS_temp/LAS/gediList.txt"
---------------------------------------------------------------------------------------------------------
while IFS= read -r gedi_file; do
  p="$gedi_file"  # 这里应该使用等号(`=`)和引号将变量值引起来
  echo "${p}"
  sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity collocateWaves -listALS /mnt/hgfs/LAS_temp/LAS/alsList.txt -gedi index_root -readHDFgedi -aEPSG 2249 -geoError 30 5 -fixFsig -writeWaves simulated.h5 -minDense 3 -minSense 0.9 -leaveEmpty
done < "/mnt/hgfs/LAS_temp/LAS/gediList.txt"
---------------------------------------------------------------------------------------------------------
while IFS= read -r gedi_file
do
  echo "${gedi_file}"
done < "/home/lihantao/Simulator/gediList.txt"
---
while IFS= read -r gedi_file
do
p=${gedi_file}
echo ${p}
sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity collocateWaves -listALS /mnt/hgfs/LAS_temp/LAS/alsList.txt -gedi ${p} -readHDFgedi -aEPSG 2249 -geoError 30 5 -fixFsig -writeWaves simulated.h5 -minDense 3 -minSense 0.9 -leaveEmpty
echo ${p}
done < "/mnt/hgfs/LAS_temp/LAS/gediList.txt"

#memo: 问题所在是变量地址传输不到循环里
```

```shell
sudo singularity exec --bind /mnt/hgfs/LAS_temp/ gediSingularity gediRat -input /mnt/hgfs/LAS_temp/LAS/T01_nk.las  -output waveGrid1.h5 -ground -hdf -gridStep 50 -pSigma 5.5 -pFWHM 15.6 -gridBound 19119.056 24581.526 -112637.599 -110078.442

sudo singularity exec --bind /mnt/hgfs/LAS_temp/LAS/ gediSingularity gediRat -input /mnt/hgfs/LAS_temp/LAS/T01_nk.las  -output T01_nk.h5 -ground -hdf -gridStep 50 -pSigma 5.5 -pFWHM 15.6 -gridBound 19119.056 24581.526 -112637.599 -110078.442
```

