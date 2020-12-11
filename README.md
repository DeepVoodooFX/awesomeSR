# awesomeSP


## Methods

| Name | Temporal Coherence | Tested | Testing Code | Training Code| Repo | Paper | Year | 
|---|---|---|---|---|---|---|---|
| ESRGAN  |  N | Y | Y | Y | [link](https://github.com/xinntao/BasicSR) | [link](https://arxiv.org/abs/1809.00219) | 2018 |
| ESRGANPAUL  |  N | Y | Y | Y | [link](https://github.com/victorca25/BasicSR) | [link](https://arxiv.org/abs/1809.00219) | 2018 |
| DFDNet  |  N | Y | Y | N | [link](https://github.com/lambdal/DFDNet) | [link](https://arxiv.org/abs/2008.00418) | 2020 |
| HiFaceGAN  |  N | Y | Y | Y | [link](https://github.com/Lotayou/Face-Renovation) | [link](https://arxiv.org/abs/2005.05005) | 2020 |
| TecoGAN  |  Y | Y | Y | Y | [link](https://github.com/chuanli11/TecoGAN.git) | [link](https://arxiv.org/abs/1811.09393) | 2018 |
| Zooming-Slow-Mo  |  Y | Y | Y | Y | [link](https://github.com/Mukosame/Zooming-Slow-Mo-CVPR-2020) | [link](https://arxiv.org/abs/2002.11616) | 2020 |



## Instruction

### ESRGAN

#### Install

```
git clone https://github.com/xinntao/BasicSR.git

cd BasicSR

virtualenv -p /usr/bin/python3.6 venv
. venv/bin/activate

pip install -r requirements.txt

python setup.py develop --no_cuda_ext

```


#### Data

```
python scripts/download_pretrained_models.py ESRGAN
```

#### Run

```
python inference/inference_esrgan.py \
--model_path experiments/pretrained_models/ESRGAN/ESRGAN_SRx4_DF2KOST_official-ff704c30.pth \
--folder /home/ubuntu/data/hifacegan/Trump512 \
--folder_output results/esrgan/Trump512
```

### ESRGAN Paul

#### Install

```
git clone https://github.com/victorca25/BasicSR.git
cd BasicSR/

virtualenv -p /usr/bin/python3.6 venv
. venv/bin/activate
pip install -r requirements.txt

pip install matplotlib
pip install ipython
```

#### Data

Get Paul's finetuned 2x model from [here](https://drive.google.com/file/d/196ABbvXSMw0gGYe9W_6NExAiK6GcanGT/view?usp=sharing)

Change `codes/options/test/test_ESRGAN.yml` 

```
name: RRDB_ESRGAN_x2
suffix: _ESRGAN
model: srragan # srragan | asrragan
scale: 2
gpu_ids: [0]

datasets:
  test_1: # the 1st test dataset
    name: seta
    mode: LR
    dataroot_LR: '/home/ubuntu/data/SR/Trump512'

path:
  root: '/home/ubuntu/Paul/BasicSR'
  pretrain_model_G: '../experiments/pretrained_models/410500_G.pth'
```

#### Run

```
cd codes
python test.py -opt options/test/test_ESRGAN.yml
```



### DFDNet

#### Install

```
conda create -n DFD-deepvoodoo python=3.6
conda install pytorch torchvision cudatoolkit=10.1 -c pytorch -n DFD-deepvoodoo
conda install -c conda-forge/label/cf202003 dlib -n DFD-deepvoodoo
conda activate DFD-deepvoodoo

git clone https://github.com/lambdal/DFDNet.git
```

#### Run

```
python test_FaceDict.py \
--test-dir /home/ubuntu/data/hifacegan/Trump128 \
--upscale 4

python test_FaceDict.py \
--test-dir /home/ubuntu/data/hifacegan/Trump256 \
--upscale 2

python test_FaceDict.py \
--test-dir /home/ubuntu/data/hifacegan/Trump512 \
--upscale 1
```


### TecoGAN

#### Install

```
git clone https://github.com/chuanli11/TecoGAN.git

cd TecoGAN

virtualenv -p /usr/bin/python3.6 venv
. venv/bin/activate

pip install tensorflow-gpu==1.15.3
```

#### Data

```
# Download the pretrianed model
python runGan.py 0
```

#### Run

```
# Set testpre in runGAN.py
python runGan.py 1

```

### HiFaceGAN

#### Install

```
virtualenv -p /usr/bin/python3.6 venv
. venv/bin/activate

pip install opencv-python
pip install tqdm
pip install imgaug
pip install torch==1.2.0 torchvision==0.4.0
pip install dill
pip install dominate

cd
git clone https://github.com/davisking/dlib.git
cd dlib
mkdir build; cd build; cmake ..; cmake --build .
cd ..
python setup.py install

cd
git clone https://github.com/Lotayou/Face-Renovation.git
```

#### Data

Download pretrained model from [here](https://yadi.sk/d/Pl_hxVZPa_PHew). Unzip it to the repo directory

Set the path to your testing in `degrade_lambda.py` and `config_hifacegan.py`.

#### Run

```
python degrade_lambda.py
python test_lambda.py
```


### Zooming-Slow-Mo

#### Install

```
git clone --recursive https://github.com/chuanli11/Zooming-Slow-Mo-CVPR-2020.git

virtualenv -p /usr/bin/python3.6 venv
. venv/bin/activate

pip install numpy opencv-python lmdb pyyaml pickle5 matplotlib seaborn

pip install torch==1.5.1
pip install torchvision==0.6.1

# Need to do the change here:
# https://github.com/CharlesShang/DCNv2/pull/58/files/9f4254babcd162a809d165fa2430a780d14761f4#diff-c49b9111a7857be19e44bbe3aacb89427c0d19009e1c27fbcbe51df9813c2d8a
cd codes/models/modules/DCNv2
bash make.sh
```


#### Data

Set `test_dataset_folder` in `test.py` accordingly.



#### Run

```
python test.py
``` 