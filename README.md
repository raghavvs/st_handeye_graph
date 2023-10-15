## st_handeye_graph

This package provides a general hand-eye calibration method which can be applied to pinhole and source-detector camera models.

### Description

Unlike usual hand-eye calibration techniques, this method directly takes the images of the calibration pattern (including the chessboard and charuco board) and estimates the hand-eye transformation and the pattern pose such that the projection error of the pattern is minimized. Since it does not rely on algorithms dedicated for pinhole cameras, such as PnP algorithms, it can be easily adapted to different camera model by changing only the projection model.

### Dependencies

- g2o
- VISP
- handeye_calib_camodocal (optional, for only evaluation)

### Usage

Include "st_handeye/st_handeye.hpp" and call "st_handeye::spatial_calibration_graph()". See "st_handeye.hpp" for the details.

### Reproduce figures

```bash
git clone https://github.com/koide3/st_handeye_graph.git
git clone https://bitbucket.org/koide3/st_handeye_eval.git

mkdir st_handeye_graph/build
cd st_handeye_graph/build
cmake .. -DCMAKE_BUILD_TYPE=Release
make -j4

cd ../scripts
# run simulations and plot figures
./run_simulation.sh

# run chessboard calibration on real chessboard data and perform 3d reconstruction
# Note: change the PATTERN_ROWS and PATTERN_COLS variables in calibrate_charuco.cpp if the calibration target changes!
./run_calibration.sh

# run charuco board calibration on real charuco board data and perform 3d reconstruction
# Note: change the PATTERN_ROWS and PATTERN_COLS variables in calibrate_charuco.cpp if the calibration target changes!
./run_calibration_charuco.sh

# see the reconstructed point cloud
pcl_viewer data/points_3d_graph.pcd
```

### Paper
Kenji Koide and Emanuele Menegatti, General Hand-Eye Calibration based on Reprojection Error Minimization, IEEE Robotics and Automation Letters/ICRA2019 [[link](https://ieeexplore.ieee.org/document/8616862)].

## Contact
Original Author: Kenji Koide, Intelligent Autonomous Systems Laboratory, University of Padova, Italy. koide@dei.unipd.it.

Chessboard and Charuco Board modifications: Holly Dinkel, University of Illinois Urbana-Champaign. hdinkel2@illinois.edu.