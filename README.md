[[Paper]](https://arxiv.org/pdf/1804.03641.pdf)
[[Project page]](http://andrewowens.com/multisensory)

Code for the [paper](https://arxiv.org/pdf/1804.03641.pdf):

Andrew Owens, Alexei A. Efros. Audio-Visual Scene Analysis with Self-Supervised Multisensory Features. arXiv, 2018

## Contents
This release includes:
- On/off-screen source separation
- Blind source separation
- Sound source localization
- Self-supervised features for the audio-visual network

## Setup
- Install [Python 2.7](https://www.python.org/download/releases/2.7)
- Install [ffmpeg](https://www.ffmpeg.org/download.html)
- Install [TensorFlow](https://www.tensorflow.org/), e.g. through pip:
```bash
pip install tensorflow     # for CPU evaluation only
pip install tensorflow-gpu # for GPU support
```
- Install other python dependencies
```bash
pip install numpy matplotlib pillow scipy
```
- Download the pretrained models and sample data
```bash
./download_models.sh
./download_sample_data.sh
```

## Audio-visual source separation
To try the on/off-screen source separation model, run:
```bash
python sep_video.py ../data/translator.mp4 --model full --duration_mult 4 --out ../results/
```
This will separate a speaker's voice from that of an off-screen speaker. It will write the separated video files to `../results/`, and will also display them in a local webpage, for easier viewing. Result:

Input:  <a href = "https://youtu.be/4kVNzxFeboo"><img src = "doc/translator.jpg" width = 200><a>. On-screen audio: <a href = "https://youtu.be/XvJVXsHyBKw"><img src = "doc/translator.jpg" width = 200></a>
Off-screen audio: <a href = "https://youtu.be/NFll7nfmwO8"><img src = "doc/translator.jpg" width = 200></a>

We can visually mask out one of the two on-screen speakers, thereby removing their voice:
```bash
python sep_video.py ../data/crossfire.mp4 --model full --mask l --out ../results/
python sep_video.py ../data/crossfire.mp4 --model full --mask r --out ../results/
```
This produces the following videos:
Source: <a href = "https://youtu.be/H9CgWJToF_s"><img src="doc/crossfire_src.jpg" width="200"/></a> Left: <a href = "https://youtu.be/9jPaA8ttI6A"><img src="doc/crossfire_l.jpg" width="200"/></a> Right: <a href = "https://youtu.be/M4ACgIWuiWM"><img src="doc/crossfire_r.jpg" width="200"/></a>

## Blind (audio-only) source separation
This baseline trains a [u-net](https://arxiv.org/pdf/1505.04597.pdf) network to minimize a [permutation invariant](https://arxiv.org/pdf/1607.00325) loss.
```bash
python sep_video.py ../data/translator.mp4 --model unet_pit --duration_mult 4 --out ../results/
```
The model will output the two separated streams in an arbitrary order.

## Citation
```
@article{multisensory2018,
  title={Audio-Visual Scene Analysis with Self-Supervised Multisensory Features},
  author={Owens, Andrew and Efros, Alexei A},
  journal={arXiv preprint arXiv:1804.03641},
  year={2018}
}
```

## Sound-source localization

```bash
python sep_video.py ../data/translator.mp4 --model full --cam --out ../results/
```
<a href = "https://youtu.be/u99MdLBDnJc"><img src="doc/crossfire_cam.jpg" width="200"/></a>

## Acknowledgements
Our *u*-net implementation draws from [this implementation](https://github.com/affinelayer/pix2pix-tensorflow) implementation of [pix2pix](https://arxiv.org/abs/1611.07004).