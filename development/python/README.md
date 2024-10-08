<br>

* [Python & Docker](#python--docker)
* [Tensorflow](#tensorflow)
  * [GPU (Graphics Processing Unit) Computing Environment](#gpu-graphics-processing-unit-computing-environment)
* [References](#references)

<br>

## Python & Docker

Notes:
* [Base Images](https://pythonspeed.com/articles/base-image-python-docker-images/)
* [Docker Official Images: Python](https://hub.docker.com/_/python/)
  * [A production set-up example](https://github.com/discourses/augmentation/blob/master/Dockerfile)

<br>

## Tensorflow

References:

* [Images](https://hub.docker.com/r/tensorflow/tensorflow/tags)
  * https://www.tensorflow.org/install/docker
* [Tensorflow & Windows Subsystem for Linux (WSL)](https://www.tensorflow.org/install/pip#windows-wsl2)
* [Hardware Requirements](https://www.tensorflow.org/install/pip#hardware_requirements)
* [Theoretical and advanced machine learning with TensorFlow](https://www.tensorflow.org/resources/learn-ml/theoretical-and-advanced-machine-learning)
* [Images](https://www.tensorflow.org/tutorials/load_data/images)

<br>

### GPU (Graphics Processing Unit) Computing Environment

Foremost, a virtual conda environment for tensorflow - within a WSL (Windows Subsystem for Linux) operating system ...

```shell
conda create --prefix /opt/miniconda3/envs/tensors python=3.11
```

Next, activate the environment then inspect ...

```shell
conda activate tensors
python -m pip list
conda list
```

Next, the core/background installations for tensorflow ... `cudatoolkit` & `cuDNN`

```shell
conda install -c conda-forge cudatoolkit=11.8.0
python -m pip install nvidia-cudnn-cu11==8.6.0.163

# Additionally (verification via ptxas --version)
conda install -c nvidia cuda-nvcc --yes
ptxas --version
```

... hence, the setting-up of their path variables

```shell
# ascertain that this variable has a value
echo $CONDA_PREFIX

# subsequently
mkdir -p $CONDA_PREFIX/etc/conda/activate.d

echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' \
  >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
  
echo 'export LD_LIBRARY_PATH=$CONDA_PREFIX/lib/:$CUDNN_PATH/lib:$LD_LIBRARY_PATH' \
  >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
```

... and probably

```shell
echo 'export XLA_FLAGS=--xla_gpu_cuda_data_dir=$CONDA_PREFIX/lib' \
  >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
```

Additionally ...

```commandline
cd C:\Windows\System32\lxss\lib
del libcuda.so
del libcuda.so.1
mklink libcuda.so libcuda.so.1.1
mklink libcuda.so.1 libcuda.so.1.1
```

<br>

Now, **install `tensorflow`**

```shell
pip install tensorflow==2.12.*
```

Perhaps [TensorRT](https://www.tensorflow.org/install/pip#windows-wsl2:~:text=improve%20latency%20and%20throughput%20for%20inference); [more](https://docs.nvidia.com/deeplearning/tensorrt/archives/tensorrt-861/index.html).

```shell
pip install --upgrade tensorrt
```


and [DASK](https://www.dask.org), [scikit-learn](https://scikit-learn.org/stable/), and code analysis packages; [pytest](https://docs.pytest.org/en/latest/), [coverage](https://coverage.readthedocs.io/en/7.3.3/), [pylint](https://pylint.readthedocs.io/en/latest/), [pytest-cov](https://pytest-cov.readthedocs.io/en/latest/), [flake8](https://flake8.pycqa.org/en/latest/).

```shell
pip install "dask[complete]"
pip install scikit-learn
pip install pytest coverage pylint pytest-cov flake8
```

<br>

## References

* [pip](https://pip.pypa.io/en/stable/)
* [Python Package Index](https://pypi.org)
* [Python & Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial)
* [NumFocus Sponsored Projects](https://numfocus.org/sponsored-projects)
  * [pandas](https://pandas.pydata.org)
* EXCEL
  * www.shellhacks.com/python-download-file-from-url-save/
  * https://stackoverflow.com/questions/25415405/downloading-an-excel-file-from-the-web-in-python
  * https://stackoverflow.com/questions/65958071/send-xls-file-to-s3-in-python
  * https://crashlaker.github.io/programming/2020/06/11/pandas_write_xlsx_to_s3.html

<br>
<br>

<br> 
<br>

<br> 
<br>

<br> 
<br>