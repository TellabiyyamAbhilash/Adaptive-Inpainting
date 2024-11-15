<p align="center">
  <img src="./example/AIM.jpg">
</p>

# Adaptive Inpainting: Segment Anything Meets Image Inpainting with Style Transfer
Adaptive Inpainting can inpaint anything in **images** and transfers the style.
<!-- - Authors: Tao Yu, Runseng Feng, Ruoyu Feng, Jinming Liu, Xin Jin, Wenjun Zeng and Zhibo Chen. -->
<!-- - Institutes: University of Science and Technology of China; Eastern Institute for Advanced Study. -->
- [[Website](https://huggingface.co/spaces/InpaintAI/Inpaint-Anything)] [[Hugging Face Homepage](https://huggingface.co/InpaintAI)]
<p align="center">
  <img src="./example/MainFramework.png" width="100%">
</p>

TL; DR: Users can select any object in an image by clicking on it. With powerful vision models, e.g., [SAM](https://arxiv.org/abs/2304.02643), [LaMa](https://arxiv.org/abs/2109.07161) and [Stable Diffusion (SD)](https://arxiv.org/abs/2112.10752), **Adaptive Inpainting** is able to remove the object smoothly (i.e., *Remove Anything*). Further, prompted by user input text, Adaptive Inpainting can fill the object with any desired content (i.e., *Fill Anything*) or replace the background of it arbitrarily (i.e., *Replace Anything*).


## 🌟 Features
- [x] [**Fill** Anything](#fill-anything)
- [x] [**Replace** Anything](#replace-anything)


**Click** on an object in the image, and Adaptinve Inpainting will **remove** it instantly and transfers its style!
- Click on an object;
- [Segment Anything Model](https://segment-anything.com/) (SAM) segments the object out;
- Inpainting models (e.g., [LaMa](https://advimman.github.io/lama-project/)) fill the "hole".

### Installation
Requires `python>=3.8`
```bash
python -m pip install torch torchvision torchaudio
python -m pip install -e segment_anything
python -m pip install -r lama/requirements.txt 
```
In Windows, we recommend you to first install [miniconda](https://docs.conda.io/en/latest/miniconda.html) and 
open `Anaconda Powershell Prompt (miniconda3)` as administrator.
Then pip install [./lama_requirements_windows.txt](lama_requirements_windows.txt) instead of 
[./lama/requirements.txt](lama%2Frequirements.txt).

### Usage
Download the model checkpoints provided in [Segment Anything](./segment_anything/README.md) and [LaMa](./lama/README.md) (e.g., [sam_vit_h_4b8939.pth](https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth) and [big-lama](https://disk.yandex.ru/d/ouP6l8VJ0HpMZg)), and put them into `./pretrained_models`. For simplicity, you can also go [here](https://drive.google.com/drive/folders/1ST0aRbDRZGli0r7OVVOQvXwtadMCuWXg?usp=sharing), directly download [pretrained_models](https://drive.google.com/drive/folders/1wpY-upCo4GIW4wVPnlMh_ym779lLIG2A?usp=sharing), put the directory into `./` and get `./pretrained_models`.

For MobileSAM, the sam_model_type should use "vit_t", and the sam_ckpt should use "./weights/mobile_sam.pt".
For the MobileSAM project, please refer to [MobileSAM](https://github.com/ChaoningZhang/MobileSAM)



## <span id="fill-anything">📌 Fill Anything</span>
<table>
  <caption align="center">Text prompt: "a teddy bear on a bench"</caption>
    <tr>
      <td><img src="./example/fill-anything/sample1/with_points.png" width="100%"></td>
      <td><img src="./example/fill-anything/sample1/with_mask.png" width="100%"></td>
      <td><img src="./example/fill-anything/sample1/filled_with_mask.png" width="100%"></td>
    </tr>
</table>
<p align="center">Text prompt: "a teddy bear on a bench"</p>
<p align="center">
    <img src="./example/GIF/Fill-sample1.gif" alt="image" style="width:400px;">
</p>

**Click** on an object, **type** in what you want to fill, and Adaptive Inpainting will **fill** it!
- Click on an object;
- [SAM](https://segment-anything.com/) segments the object out;
- Input a text prompt;
- Text-prompt-guided inpainting models (e.g., [Stable Diffusion](https://github.com/CompVis/stable-diffusion)) fill the "hole" according to the text.

### Installation
Requires `python>=3.8`
```bash
python -m pip install torch torchvision torchaudio
python -m pip install -e segment_anything
python -m pip install diffusers transformers accelerate scipy safetensors
```

### Usage
Download the model checkpoints provided in [Segment Anything](./segment_anything/README.md) (e.g., [sam_vit_h_4b8939.pth](https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth)) and put them into `./pretrained_models`. For simplicity, you can also go [here](https://drive.google.com/drive/folders/1ST0aRbDRZGli0r7OVVOQvXwtadMCuWXg?usp=sharing), directly download [pretrained_models](https://drive.google.com/drive/folders/1wpY-upCo4GIW4wVPnlMh_ym779lLIG2A?usp=sharing), put the directory into `./` and get `./pretrained_models`.

For MobileSAM, the sam_model_type should use "vit_t", and the sam_ckpt should use "./weights/mobile_sam.pt".
For the MobileSAM project, please refer to [MobileSAM](https://github.com/ChaoningZhang/MobileSAM)
```
bash script/fill_anything.sh

```

Specify an image, a point and text prompt, and run:
```bash
python fill_anything.py \
    --input_img ./example/fill-anything/sample1.png \
    --coords_type key_in \
    --point_coords 750 500 \
    --point_labels 1 \
    --text_prompt "a teddy bear on a bench" \
    --dilate_kernel_size 50 \
    --output_dir ./results \
    --sam_model_type "vit_h" \
    --sam_ckpt ./pretrained_models/sam_vit_h_4b8939.pth
```

### Demo

<table>
  <caption align="center">Text prompt: "a camera lens in the hand"</caption>
    <tr>
      <td><img src="./example/fill-anything/sample2/with_points.png" width="100%"></td>
      <td><img src="./example/fill-anything/sample2/with_mask.png" width="100%"></td>
      <td><img src="./example/fill-anything/sample2/filled_with_mask.png" width="100%"></td>
    </tr>
</table>

<table>
  <caption align="center">Text prompt: "a Picasso painting on the wall"</caption>
    <tr>
      <td><img src="./example/fill-anything/sample5/with_points.png" width="100%"></td>
      <td><img src="./example/fill-anything/sample5/with_mask.png" width="100%"></td>
      <td><img src="./example/fill-anything/sample5/filled_with_mask.png" width="100%"></td>
    </tr>
</table>

<table>
  <caption align="center">Text prompt: "an aircraft carrier on the sea"</caption>
    <tr>
      <td><img src="./example/fill-anything/sample3/with_points.png" width="100%"></td>
      <td><img src="./example/fill-anything/sample3/with_mask.png" width="100%"></td>
      <td><img src="./example/fill-anything/sample3/filled_with_mask.png" width="100%"></td>
    </tr>
</table>

<table>
  <caption align="center">Text prompt: "a sports car on a road"</caption>
    <tr>
      <td><img src="./example/fill-anything/sample4/with_points.png" width="100%"></td>
      <td><img src="./example/fill-anything/sample4/with_mask.png" width="100%"></td>
      <td><img src="./example/fill-anything/sample4/filled_with_mask.png" width="100%"></td>
    </tr>
</table>


## <span id="replace-anything">📌 Replace Anything</span>
 <table>
  <caption align="center">Text prompt: "a man in office"</caption>
    <tr>
      <td><img src="./example/replace-anything/man/with_points.png" width="100%"></td>
      <td><img src="./example/replace-anything/man/with_mask.png" width="100%"></td>
      <td><img src="./example/replace-anything/man/replaced_with_mask.png" width="100%"></td>
    </tr>
</table> 
<p align="center">Text prompt: "a man in office"</p>
<p align="center">
    <img src="./example/GIF/Replace-man.gif" alt="image" style="width:400px;">
</p>

**Click** on an object, **type** in what background you want to replace, and Adaptive Inpainting will **replace** it!
- Click on an object;
- [SAM](https://segment-anything.com/) segments the object out;
- Input a text prompt;
- Text-prompt-guided inpainting models (e.g., [Stable Diffusion](https://github.com/CompVis/stable-diffusion)) replace the background according to the text.

### Installation
Requires `python>=3.8`
```bash
python -m pip install torch torchvision torchaudio
python -m pip install -e segment_anything
python -m pip install diffusers transformers accelerate scipy safetensors
```

### Usage
Download the model checkpoints provided in [Segment Anything](./segment_anything/README.md) (e.g. [sam_vit_h_4b8939.pth](https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth)) and put them into `./pretrained_models`. For simplicity, you can also go [here](https://drive.google.com/drive/folders/1ST0aRbDRZGli0r7OVVOQvXwtadMCuWXg?usp=sharing), directly download [pretrained_models](https://drive.google.com/drive/folders/1wpY-upCo4GIW4wVPnlMh_ym779lLIG2A?usp=sharing), put the directory into `./` and get `./pretrained_models`.

For MobileSAM, the sam_model_type should use "vit_t", and the sam_ckpt should use "./weights/mobile_sam.pt".
For the MobileSAM project, please refer to [MobileSAM](https://github.com/ChaoningZhang/MobileSAM)
```
bash script/replace_anything.sh

```

Specify an image, a point and text prompt, and run:
```bash
python replace_anything.py \
    --input_img ./example/replace-anything/dog.png \
    --coords_type key_in \
    --point_coords 750 500 \
    --point_labels 1 \
    --text_prompt "sit on the swing" \
    --output_dir ./results \
    --sam_model_type "vit_h" \
    --sam_ckpt ./pretrained_models/sam_vit_h_4b8939.pth
```

### Demo
<table>
  <caption align="center">Text prompt: "sit on the swing"</caption>
    <tr>
      <td><img src="./example/replace-anything/dog/with_points.png" width="100%"></td>
      <td><img src="./example/replace-anything/dog/with_mask.png" width="100%"></td>
      <td><img src="./example/replace-anything/dog/replaced_with_mask.png" width="100%"></td>
    </tr>
</table>

<table>
  <caption align="center">Text prompt: "a bus, on the center of a country road, summer"</caption>
    <tr>
      <td><img src="./example/replace-anything/bus/with_points.png" width="100%"></td>
      <td><img src="./example/replace-anything/bus/with_mask.png" width="100%"></td>
      <td><img src="./example/replace-anything/bus/replaced_with_mask.png" width="100%"></td>
    </tr>
</table>

<table>
  <caption align="center">Text prompt: "breakfast"</caption>
    <tr>
      <td><img src="./example/replace-anything/000000029675/with_points.png" width="100%"></td>
      <td><img src="./example/replace-anything/000000029675/with_mask.png" width="100%"></td>
      <td><img src="./example/replace-anything/000000029675/replaced_with_mask.png" width="100%"></td>
    </tr>
</table>

<table>
  <caption align="center">Text prompt: "crossroad in the city"</caption>
    <tr>
      <td><img src="./example/replace-anything/000000000724/with_points.png" width="100%"></td>
      <td><img src="./example/replace-anything/000000000724/with_mask.png" width="100%"></td>
      <td><img src="./example/replace-anything/000000000724/replaced_with_mask.png" width="100%"></td>
    </tr>
</table>




# Style Transfer using AdaIN model

The "Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization" model is used to achieve style transfer (transforming a content image in the style of any arbitrary image) in real-time.

## Requirements
Please install requirements by `pip install -r requirements.txt`

- Python 3.5+
- PyTorch 0.4+
- TorchVision
- Pillow

(optional, for training)
- tqdm
- TensorboardX

## Usage

Change directory to /AdaIN

### Download models
Download decoder.pth / vgg_normalized.pth from [models](https://drive.google.com/drive/folders/1989H09T2dlj-HLQjTsFNCQWSdCfFrgZE?usp=sharing) and put them under `models/`.

### Test
Use `--content` and `--style` to provide the respective path to the content and style image.
```
CUDA_VISIBLE_DEVICES=<gpu_id> python test.py --content input/content/cornell.jpg --style input/style/woman_with_hat_matisse.jpg
```

You can also run the code on directories of content and style images using `--content_dir` and `--style_dir`. It will save every possible combination of content and styles to the output directory.
```
CUDA_VISIBLE_DEVICES=<gpu_id> python test.py --content_dir input/content --style_dir input/style
```

This is an example of mixing four styles by specifying `--style` and `--style_interpolation_weights` option.
```
CUDA_VISIBLE_DEVICES=<gpu_id> python test.py --content input/content/avril.jpg --style input/style/picasso_self_portrait.jpg,input/style/impronte_d_artista.jpg,input/style/trial.jpg,input/style/antimonocromatismo.jpg --style_interpolation_weights 1,1,1,1 --content_size 512 --style_size 512 --crop
```

Some other options:
* `--content_size`: New (minimum) size for the content image. Keeping the original size if set to 0.
* `--style_size`: New (minimum) size for the content image. Keeping the original size if set to 0.
* `--alpha`: Adjust the degree of stylization. It should be a value between 0.0 and 1.0 (default).
* `--preserve_color`: Preserve the color of the content image.


### Train
Use `--content_dir` and `--style_dir` to provide the respective directory to the content and style images.
```
CUDA_VISIBLE_DEVICES=<gpu_id> python train.py --content_dir <content_dir> --style_dir <style_dir>
```

For more details and parameters, please refer to --help option.
