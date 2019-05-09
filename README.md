### PointNet++ Implatation on Several Scene Segmentatin Datasets
The original project PointNet++ is created by <a href="http://charlesrqi.com" target="_blank">Charles R. Qi</a>, <a href="http://stanford.edu/~ericyi">Li (Eric) Yi</a>, <a href="http://ai.stanford.edu/~haosu/" target="_blank">Hao Su</a>, <a href="http://geometry.stanford.edu/member/guibas/" target="_blank">Leonidas J. Guibas</a> from Stanford University.

![prediction example](https://github.com/charlesq34/pointnet2/blob/master/doc/teaser.jpg)

### Installation

Install <a href="https://www.tensorflow.org/install/">TensorFlow</a>. The code is tested under TF1.2 GPU version and Python 2.7 (version 3 should also work) on Ubuntu 14.04. There are also some dependencies for a few Python libraries for data processing and visualizations like `cv2`, `h5py` etc. It's highly recommended that you have access to GPUs.

#### Compile Customized TF Operators
The TF operators are included under `tf_ops`, you need to compile them (check `tf_xxx_compile.sh` under each ops subfolder) first. Update `nvcc` and `python` path if necessary. The code is tested under TF1.2.0. If you are using earlier version it's possible that you need to remove the `-D_GLIBCXX_USE_CXX11_ABI=0` flag in g++ command in order to compile correctly.

To compile the operators in TF version >=1.4, you need to modify the compile scripts slightly.

First, find Tensorflow include and library paths.

        TF_INC=$(python -c 'import tensorflow as tf; print(tf.sysconfig.get_include())')
        TF_LIB=$(python -c 'import tensorflow as tf; print(tf.sysconfig.get_lib())')
        
Then, add flags of `-I$TF_INC/external/nsync/public -L$TF_LIB -ltensorflow_framework` to the `g++` commands.

Compile

        cd tf_ops/3d_interpolation/
        sh tf_interpolate_compile.sh
        ...

### Usage

#### Semantic Scene Parsing

See `scannet/README` and `scannet/train.py` for details.

#### Visualization Tools
We have provided a handy point cloud visualization tool under `utils`. Run `sh compile_render_balls_so.sh` to compile it and then you can try the demo with `python show3d_balls.py` The original code is from <a href="http://github.com/fanhqme/PointSetGeneration">here</a>.

#### Prepare Your Own Data
You can refer to <a href="https://github.com/charlesq34/3dmodel_feature/blob/master/io/write_hdf5.py">here</a> on how to prepare your own HDF5 files for either classification or segmentation. Or you can refer to `modelnet_dataset.py` on how to read raw data files and prepare mini-batches from them. A more advanced way is to use TensorFlow's dataset APIs, for which you can find more documentations <a href="https://www.tensorflow.org/programmers_guide/datasets">here</a>.

#### Tips
Fix missing files: `eulerangles.py` and `plyfile.py`