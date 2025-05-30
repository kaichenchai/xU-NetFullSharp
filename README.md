<div align="justify">

# xU-NetFullSharp: ANN for Chest X-Ray Bone Shadow Suppression

</div>
  
## Introduction

<div align="justify">
  
In this [paper](https://www.sciencedirect.com/science/article/pii/S1746809424010413?dgcid=author), an automated deep learning-based framework for bone shadow suppression from frontal CXRs is developed. The framework was inspired by U-Net-based convolutional neural networks (CNNs). Multiple deep learning-based CNN architectures were implemented to fulfill this task. Among those, a novel neural network architecture called xU-NetFullSharp was proposed. This network is inspired by the most modern U-NetSharp [8] architecture and combines different approaches to preserve as many details, as possible and accurately suppress bone shadows. Additionally, recent state-of-the-art CNN models from [9] and [3] designed for this task were used for comparison. Code for the utilized models is available in the `models` folder in this repository.

<div align="center">
  <img src="images/intro.png" alt>
</div>
  
## Quick Start Python Guide

This guide provides an overview of setting up and testing bone suppression models using Conda and Python scripts.

### Environment Setup

To create the Conda environment with necessary dependencies, run:

```bash
conda env create -f environment.yml
```

After creating the environment, activate it by executing:

```bash
conda activate bone_suppression
```

### Testing Models on Datasets

You can test individual models on either internal or external datasets. To do this, use the `test.py` script with the following command:

```bash
python test.py --model_name <desired_model> --test_variant <external | internal> --data_path <path_to_desired_dataset>
```

**Supported Models:**

- `U-Net`: `"UNET"` [10], `"ATT_UNET"` [7], `"DEEP_RESUNET"` [11]
- `U-Net++`: `"UNETPP"` [12], `"ATT_UNETPP"` [5]
- `U-Net3+`: `"UNET3P"` [2]
- `U-Net#`: `"UNET_SHARP"` [8]
- `xU-NetFullSharp` (Ours): `"XUNETFS"`, `"ATT_XUNETFS"`
- `Kalisz-Marczyk Autoencoder` [3]: `"KALISZ_AE"`
- `DeBoNet` [9]: `"UNET_RES18"`, `"FPN_RES18"`, `"FPN_EF0"` ==> Outputs from the proposed ensemble can be generated using the [Matlab script](https://github.com/sivaramakrishnan-rajaraman/Bone-Suppresion-Ensemble/blob/main/bone_suppression_ensemble.py) (commented out at the end of the file) provided by the authors.

#### Internal vs. External Testing

- **Internal Testing**:
  - Requires the dataset folder to contain subdirectories named `JSRT` and `BSE_JSRT`.
  - The `JSRT` folder should include the original X-ray images, while `BSE_JSRT` should contain the corresponding ground truth images.

- **External Testing**:
  - Automatically processes images in the specified folder, applies inference, and saves the output.
  - Outputs are saved in the `outputs` directory.

### Training Models

To train models, use the `train.py` script:

```bash
python train.py --model_name <desired_model> --data_path <dataset_directory>
```

If you want to initialize the model with pre-trained weights, include the `--weights_path` argument:

```bash
python train.py --model_name <desired_model> --data_path <dataset_directory> --weights_path <weights_path>
```

The `dataset_directory` should contain subdirectories named `train` and `val`. Both of these directories should contain `JSRT` and `BSE_JSRT` subdirectories with the same properties as described above.

## Downloads

- [Pretrained model weights](https://vutbr-my.sharepoint.com/:u:/g/personal/burgetrm_vutbr_cz/EaxYf0RZYCVClFDVpvfqdtsBL1DUV0B81pE1Hy_C1W7bOg?e=buBH8N)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## How to Cite

<pre>
Schiller, V., Burget, R., Genzor, S., Mizera, J., & Mezina, A. (2025). xU-NetFullSharp: The Novel Deep
Learning Architecture for Chest X-ray Bone Shadow Suppression. Biomedical Signal Processing and Control,
100, 106983. https://doi.org/10.1016/j.bspc.2024.106983.
[<a href="https://www.sciencedirect.com/science/article/pii/S1746809424010413?dgcid=author">link</a>]
</pre>
Bibtex:
<pre><code>@article{SCHILLER2025106983,
title = {xU-NetFullSharp: The Novel Deep Learning Architecture for Chest X-ray Bone Shadow Suppression},
journal = {Biomedical Signal Processing and Control},
volume = {100},
pages = {106983},
year = {2025},
issn = {1746-8094},
doi = {https://doi.org/10.1016/j.bspc.2024.106983},
url = {https://www.sciencedirect.com/science/article/pii/S1746809424010413},
author = {Vojtech Schiller and Radim Burget and Samuel Genzor and Jan Mizera and Anzhelika Mezina},
keywords = {Deep learning, Bone shadow suppression, X-ray images, Medical Imaging, Neural networks, Convolutional neural networks, U-Net, Image denoising},
}</code></pre>

## Details about proposed architecture

<div align="justify">
  
  The xU-NetFullSharp is based on the most recent U-NetSharp [8] architecture and utilizes bidirectional multi-scale skip connections like in the preceding U-Net3+ [2]. The ReLU activation is changed for more modern xUnit [4] activation to ensure more accurate activation maps.

</div>

<div align="center">
  <img src="https://github.com/xKev1n/xU-NetFullSharp/blob/main/images/models/xU-NetFS_EN.svg?raw=true" alt>
</div>
<div align="center">
  <em>The architecture of the proposed xU-NetFullSharp</em>
</div>

<div align="justify">
  
  <br> Blocks of the proposed architecture are made up of 2D convolutions with different dilation rates and xUnit [4] activation functions.
  
</div>

<div align="center">
  <img src="https://github.com/xKev1n/xU-NetFullSharp/blob/main/images/models/DilatedBlockEN.svg?raw=true" alt>
</div>
<div align="center">
  <em>The structure of dilated blocks, where BB denotes the base block consisting of a 2D convolution with growing dilation rate <i>d</i></em>
</div>

## Datasets

<div align="justify">
  
  The experiments utilized three datasets – extensively augmented JSRT, VinDr-CXR [6], and Gusarev DES [1] dataset. The JSRT dataset, as well as the VinDr-CXR datasets, is available in the `datasets` folder in the [cloud storage](https://drive.google.com/file/d/1f0LP05jhNPI0UjqkQhpAyBbp8KV_2y4Y/view?usp=drive_link). The Gusarev DES dataset can be obtained from the following [GitHub repository](https://github.com/diaoquesang/A-detailed-summarization-about-bone-suppression-in-Chest-X-rays). Firstly, the JSRT dataset containing bone shadow-suppressed CXRs was split into training, validation, and testing sets and was extensively augmented to achieve a sufficient amount of usable images and to ensure the model’s robustness (both original and augmented images are available in the `JSRT` subfolder). The second, VinDr-CXR, dataset was augmented by randomly applying inversion and used for independent testing (the used testing set is available in the `VinDrCXR` subfolder). From the third, Gusarev DES, dataset expert pulmonologists selected images where the rib shadows collide with pulmonary nodules. These images were then used to conduct a performance assessment focused on clinical applications of the models.
  
</div>

## Results

<div align="justify">
  
  The internal testing results (on the JSRT dataset) are available in the `internal_test` folder; external testing results (on the VinDr-CXR dataset) are present in the `external_test` folder. To reproduce the results, use the `test.py` file with the desired model and path to corresponding weights. Sample outputs from individual models can be seen in the `/images/xray` folder of this repository. The objective and subjective results we achieved on the individual datasets can be seen in the tables below.

</div>

### Objective results (JSRT dataset)

| **Models**                         | **MAE**    | **MSE**    | **SSIM**   | **MS-SSIM** | **UIQI**   | **PSNR [dB]** |
| ---------------------------------- | :--------: | :--------: | :--------: | :---------: | :--------: | :-----------: |
| **U-Net**                          | 0.0074     | 0.0004     | 0.9835     | 0.9865      | 0.9959     | 34.6081       |
| **Attention U-Net**                | 0.0080     | 0.0004     | 0.9834     | 0.9863      | 0.9960     | 34.4984       |
| **Deep Residual U-Net**            | 0.0120     | 0.0007     | 0.9686     | 0.9768      | 0.9903     | 31.5990       |
| **U-Net++**                        | 0.0073     | 0.0004     | 0.9834     | 0.9867      | 0.9959     | 34.6063       |
| **Attention U-Net++**              | 0.0076     | 0.0004     | 0.9835     | 0.9861      | 0.9957     | 34.4042       |
| **U-Net3+**                        | 0.0074     | 0.0004     | 0.9836     | 0.9868      | 0.9957     | 34.6300       |
| **U-NetSharp**                     | 0.0078     | 0.0004     | 0.9836     | 0.9868      | 0.9960     | 34.4829       |
| **xU-NetFullSharp**                | **0.0071** | **0.0003** | **0.9846** | **0.9870**  | **0.9961** | 34.5338       |
| **Attention xU-NetFullSharp**      | 0.0076     | **0.0003** | 0.9841     | 0.9869      | 0.9960     | **34.7016**   |
| **Kalisz Marczyk’s Autoencoder**   | 0.0097     | 0.0005     | 0.9722     | 0.9821      | 0.9948     | 33.3040       |
| **FPN-ResNet-18**                  | 0.0154     | 0.0006     | 0.9708     | 0.9781      | 0.9917     | 31.7906       |
| **FPN-EfficientNet-B0**            | 0.0164     | 0.0006     | 0.9694     | 0.9802      | 0.9917     | 31.6370       |
| **U-Net-ResNet-18**                | 0.0172     | 0.0008     | 0.9660     | 0.9713      | 0.9904     | 30.7114       |
| **DeBoNet**                        | 0.0159     | 0.0022     | 0.9312     | 0.9642      | 0.9953     | 27.0403       |

### Histogram comparison

| **Models**                         | **Correlation** | **Intersection** | **𝜒<sup>2</sup>** | **Bhattacharyya** |
| ---------------------------------- | :-------------: | :--------------: | :---------------: | :--------------: |
| **U-Net**                          | 0.9443          | 9.8999           | 6.1702            | 0.1292           |
| **Attention U-Net**                | 0.9449          | 9.8873           | 8.3925            | 0.1295           |
| **Deep Residual U-Net**            | 0.9358          | 9.5917           | 5.8152            | 0.1478           |
| **U-Net++**                        | 0.9394          | 9.8962           | **4.8495**        | 0.1309           |
| **Attention U-Net++**              | 0.9478          | 9.9733           | 12.5015           | 0.1202           |
| **U-Net3+**                        | 0.9329          | 9.9043           | 8.3654            | 0.1251           |
| **U-NetSharp**                     | 0.9414          | 9.9133           | 14.6634           | 0.1330           |
| **Attention xU-NetFullSharp**      | 0.9321          | 9.8601           | 8.9438            | 0.1396           |
| **xU-NetFullSharp**                | **0.9631**      | **10.0285**      | 7.0048            | **0.1155**       |
| **Kalisz Marczyk’s Autoencoder**   | 0.9580          | 9.8830           | 6.7579            | 0.1220           |
| **FPN-ResNet-18**                  | 0.8658          | 9.1395           | 5.1873            | 0.1976           |
| **FPN-EfficientNet-B0**            | 0.8763          | 9.3075           | 12.9367           | 0.1831           |
| **U-Net-ResNet-18**                | 0.8843          | 9.0170           | 26.0124           | 0.1973           |
| **DeBoNet**                        | 0.9273          | 9.6682           | 11.0798           | 0.1418           |

### Experts' rating of the results on the external VinDr-CXR dataset

<table>
    <tr>
        <td align='center'><b>Models</b></td>
        <td align='center'><b>Average rating</b><br>(best = 1)</td>
        <td align='center'><b>Expert’s comment</b></td>
        <td></td>
    </tr>
    <tr>
        <td><b>U-Net</b></td>
        <td align='center'>3.0</td>
        <td></td>
        <td rowspan=8, align='center'>Our models</td>
    </tr>
    <tr>
        <td><b>Attention U-Net</b></td>
        <td align='center'>2.8</td>
        <td></td>
    </tr>
    <tr>
        <td><b>U-Net++</b></td>
        <td align='center'>3.0</td>
        <td></td>
    </tr>
    <tr>
        <td><b>Attention U-Net++</b></td>
        <td align='center'><b>2.3</b></td>
        <td></td>
    </tr>
    <tr>
        <td><b>U-Net3+</b></td>
        <td align='center'>2.8</td>
        <td><b>Some problems with detail retention.</b></td>
    </tr>
    <tr>
        <td><b>U-NetSharp</b></td>
        <td align='center'><b>1.7</b></td>
        <td><b>Second best in bone shadow suppression. Great retention of details.</b></td>
    </tr>
    <tr>
        <td><b>xU-NetFullSharp</b></td>
        <td align='center'><b>1.2</b></td>
        <td><b>Consistently the best in bone shadow suppression. Great retention of details.</b></td>
    </tr>
    <tr>
        <td><b>Attention xU-NetFullSharp</b></td>
        <td align='center'>3.0</td>
        <td></td>
    </tr>
    <tr>
        <td><b>U-Net-ResNet-18</b></td>
        <td align='center'>4.5</td>
        <td></td>
        <td rowspan=4, align='center'>Concurrent models</td>
    </tr>
    <tr>
        <td><b>FPN-ResNet-18</b></td>
        <td align='center'>4.3</td>
        <td><b>Major deformation of the chest silhouette in one sample!</b></td>
    </tr>
    <tr>
        <td><b>FPN-EfficientNet-B0</b></td>
        <td align='center'>3.7</td>
        <td><b>Blurry details.</b></td>
    </tr>
    <tr>
        <td><b>Kalisz Marczyk’s Autoencoder</b></td>
        <td align='center'>3.5</td>
        <td></td>
    </tr>
</table>

### Experts' rating of the results on the external Gusarev DES dataset

<table>
    <tr>
        <td align='center'><b>Models</b></td>
        <td align='center'><b>Vessel visibility</b> <br> 3: Clearly visible <br> 2: Visible <br> 1: Not visible</td>
        <td align='center'><b>Airway visibility</b> <br> 3: Lobar and intermediate bronchi <br> 2: Main bronchus and rump <br> 1: Trachea</td>
        <td align='center'><b>Bone shadow suppression</b> <br> 3: Nearly perfect <br> 2: Less than 5 unsuppressed bones <br> 1: 5 or more unsuppressed bones</td>
        <td align='center'><b>Overall bone shadow suppression performance</b> <br> 1: Excellent <br> 3: Average <br> 5: Poor</td>
        <td align='center'><b>Nodule visibility</b> <br> 3: More apparent <br> 2: Equally as apparent <br> 1: Less apparent</td>
    </tr>
    <tr>
        <td><b>DES (Reference)</b></td>
        <td align='center'><b>3</b></td>
        <td align='center'><b>1.8</b></td>
        <td align='center'><b>2.7</b></td>
        <td align='center'><b>1.3</b></td>
        <td align='center'><b>2.9</b></td>
    </tr>
    <tr>
        <td><b>Attention U-Net</b></td>
        <td align='center'><b>3</b></td>
        <td align='center'>2.1</td>
        <td align='center'>1</td>
        <td align='center'>2.8</td>
        <td align='center'><b>2.3</b></td>
    </tr>
    <tr>
        <td><b>Attention U-Net++<b></td>
        <td align='center'><b>3</b></td>
        <td align='center'>2.2</td>
        <td align='center'>1</td>
        <td align='center'>2.3</td>
        <td align='center'>2.1</td>
    </tr>
    <tr>
        <td><b>Attention xU-NetFullSharp<b></td>
        <td align='center'><b>3</b></td>
        <td align='center'>2.3</td>
        <td align='center'>1</td>
        <td align='center'>2.4</td>
        <td align='center'>2</td>
    </tr>
    <tr>
        <td><b>DeBoNet</b></td>
        <td align='center'>2</td>
        <td align='center'>1.8</td>
        <td align='center'>1</td>
        <td align='center'>4.2</td>
        <td align='center'>1.4</td>
    </tr>
    <tr>
        <td><b>FPN-EfficientNet-B0<b></td>
        <td align='center'><b>3</b></td>
        <td align='center'>2.2</td>
        <td align='center'>1</td>
        <td align='center'>3.3</td>
        <td align='center'>1.5</td>
    </tr>
    <tr>
        <td><b>FPN-ResNet-18<b></td>
        <td align='center'><b>3</b></td>
        <td align='center'>2.3</td>
        <td align='center'>1</td>
        <td align='center'>3.5</td>
        <td align='center'>2</td>
    </tr>
    <tr>
        <td><b>U-Net-ResNet-18<b></td>
        <td align='center'><b>3</b></td>
        <td align='center'>2.3</td>
        <td align='center'>1</td>
        <td align='center'>4.9</td>
        <td align='center'>2.1</td>
    </tr>
    <tr>
        <td><b>Deep Residual U-Net<b></td>
        <td align='center'>2.2</td>
        <td align='center'>2</td>
        <td align='center'>1</td>
        <td align='center'>5</td>
        <td align='center'>1.9</td>
    </tr>
    <tr>
        <td><b>Kalisz Marczyk’s Autoencoder<b></td>
        <td align='center'><b>3</b></td>
        <td align='center'>2.3</td>
        <td align='center'>1</td>
        <td align='center'>3.1</td>
        <td align='center'>2.1</td>
    </tr>
    <tr>
        <td><b>U-Net</b></td>
        <td align='center'><b>3</b></td>
        <td align='center'>2.3</td>
        <td align='center'>1</td>
        <td align='center'>2.6</td>
        <td align='center'>2.2</td>
    </tr>
    <tr>
        <td><b>U-NetSharp</b></td>
        <td align='center'><b>3</b></td>
        <td align='center'><b>2.4</b></td>
        <td align='center'>1</td>
        <td align='center'>2.1</td>
        <td align='center'>2.2</td>
    </tr>
    <tr>
        <td><b>U-Net3+</b></td>
        <td align='center'><b>3</b></td>
        <td align='center'><b>2.4</b></td>
        <td align='center'>1</td>
        <td align='center'>2.5</td>
        <td align='center'>2.2</td>
    </tr>
    <tr>
        <td><b>U-Net++</b></td>
        <td align='center'><b>3</b></td>
        <td align='center'><b>2.4</b></td>
        <td align='center'>1</td>
        <td align='center'>2.2</td>
        <td align='center'>2.1</td>
    </tr>
    <tr>
        <td><b>xU-NetFullSharp</b></td>
        <td align='center'><b>3</b></td>
        <td align='center'><b>2.4</b></td>
        <td align='center'>1</td>
        <td align='center'><b>1.6</b></td>
        <td align='center'><b>2.3</b></td>
    </tr>
</table>

## References

<div align="justify">
  
[1] Gusarev, M., Kuleev, R., Khan, A., Ramirez Rivera, A., & Khattak, A. M. (2017). Deep learning models for bone suppression in chest radiographs. *2017 IEEE Conference on Computational Intelligence in Bioinformatics and Computational Biology (CIBCB)*, 1–7. <https://doi.org/10.1109/CIBCB.2017.8058543>

[2] Huang, H., Lin, L., Tong, R., Hu, H., Zhang, Q., Iwamoto, Y., Han, X., Chen, Y.-W., & Wu, J. (2020). UNet 3+: A Full-Scale Connected UNet for Medical Image Segmentation. *ICASSP 2020 - 2020 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)*, 1055–1059. <https://doi.org/10.1109/ICASSP40776.2020.9053405>

[3] Kalisz, S., & Marczyk, M. (2021). Autoencoder-based bone removal algorithm from x-ray images of the lung. *2021 IEEE 21st International Conference on Bioinformatics and Bioengineering (BIBE)*, 1–6.

[4] Kligvasser, I., Shaham, T. R., & Michaeli, T. (2017). xUnit: Learning a Spatial Activation Function for Efficient Image Restoration. *CoRR*, abs/1711.06445. <http://arxiv.org/abs/1711.06445>

[5] Li, C., Tan, Y., Chen, W., Luo, X., Gao, Y., Jia, X., & Wang, Z. (2020). Attention Unet++: A Nested Attention-Aware U-Net for Liver CT Image Segmentation. *2020 IEEE International Conference on Image Processing (ICIP)*, 345–349. <https://doi.org/10.1109/ICIP40778.2020.9190761>

[6] Nguyen, H. Q., Lam, K., Le, L. T., Pham, H. H., Tran, D. Q., Nguyen, D. B., Le, D. D., Pham, C. M., Tong, H. T. T., Dinh, D. H., Do, C. D., Doan, L. T., Nguyen, C. N., Nguyen, B. T., Nguyen, Q. V., Hoang, A. D., Phan, H. N., Nguyen, A. T., Ho, P. H., … Vu, V. (2022). VinDr-CXR: An open dataset of chest X-rays with radiologist’s annotations.

[7] Oktay, O., Schlemper, J., Folgoc, L. le, Lee, M. C. H., Heinrich, M. P., Misawa, K., Mori, K., McDonagh, S. G., Hammerla, N. Y., Kainz, B., Glocker, B., & Rueckert, D. (2018). Attention U-Net: Learning Where to Look for the Pancreas. *CoRR*, abs/1804.03999. <http://arxiv.org/abs/1804.03999>

[8] Qian, L., Zhou, X., Li, Y., & Hu, Z. (2022). UNet#: A UNet-like Redesigning Skip Connections for Medical Image Segmentation. *ArXiv Preprint*, ArXiv:2205.11759.

[9] Rajaraman, S., Cohen, G., Spear, L., Folio, L., & Antani, S. (2022). DeBoNet: A deep bone suppression model ensemble to improve disease detection in chest radiographs. *Plos One*, 17(3), e0265691.

[10] Ronneberger, O., Fischer, P., & Brox, T. (2015). U-Net: Convolutional Networks for Biomedical Image Segmentation.

[11] Zhang, Z., Liu, Q., & Wang, Y. (2017). Road Extraction by Deep Residual U-Net. *CoRR*, abs/1711.10684. <http://arxiv.org/abs/1711.10684>

[12] Zhou, Z., Siddiquee, M. M. R., Tajbakhsh, N., & Liang, J. (2018). UNet++: A Nested U-Net Architecture for Medical Image Segmentation. *CoRR*, abs/1807.10165. <http://arxiv.org/abs/1807.10165>

</div>
