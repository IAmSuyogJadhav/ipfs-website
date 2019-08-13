---
layout: post
hero-bg-color: "#000"
thumb: "brainy.png"  # Use the same as filename for its thumbnail in image/
title:  "Brainy"
subtitle: "The virtual MRI Analyzer"
worktype: "3D MRI Segmentation using deep learning"
date:   2019-01-19
summary:    "Brainy is a virtual MRI analyzer. Just upload the MRI scan file and get 5 different classes of tumors detected and segmented."
categories: projects
progress: 50
github: Brainy
#permalink: /Brainy
---

<style type="text/css">
  .post>.measure {
    max-width: 50rem;
  }
</style>

<!-- ## Progress

<progress value="{{ page.progress }}" max="100"></progress>
{{ page.progress }}% -->
# Preview
<div>
<img src="assets/analyze.png" align="centre" width="500px" style="margin: 10 10 10 10;"><br />
	<p align="center"><b>A screenshot of the project interface</b></p>
</div>

# Installation Instructions

1. Clone this repository

   ```bash
   git clone https://github.com/IAmSuyogJadhav/Brainy.git
   ```

   or [Download](https://github.com/IAmSuyogJadhav/Brainy/archive/master.zip) and then extract its contents.

2. Download the model from [here](https://drive.google.com/open?id=1U6o7FfR7Fue6ukIg_ciUoN0rjZs6QfrW) and put the file inside `app/static/models/` folder. Do not change the name (It should be `Model_1.h5`).

3. From the root folder of te repository, run:
```bash
pip3 install -r requirements.txt
```
to install ll the dependencies. Note that this project is based on python3.

4. Start the app using following command, when in the main folder:

   ```bash
   flask run
   ```

   It may take a while.

5. Now open your browser and navigate to http://localhost:5000 and follow the instructions. It is recommended to have a Nvidia GPU, since it can speed up the prediction task manifolds.

## Sample MRI scans

There are 2 sample cases from the BraTS2018 dataset insides the `samples/` directory. You can use them to quickly test the app. These cases also contain a file (named in the format `xxx_seg.nii.gz`) that shows the ground truth segmentation map for that case.

# Project Overview

<div>
    <img src="/images/brainy/brain.png" align="left" style="margin: 10px 10px 10px 10px;" width='500'>
    <!-- <img src="" align="left" width="500" style="margin: 10 10 10 10;"> -->
    <p>
        MRI is a very popular technique to detect tumours. MRI creates cross-section pictures of your insides. But MRI uses strong magnets to make the images – not radiation. An MRI scan takes cross-sectional slices (views) from many angles, as if someone were looking at a slice of your body from the front, from the side, or from above your head.
MRI creates pictures of soft tissue parts of the body that are sometimes hard to see using other imaging tests.MRI is very good at finding and pinpointing some cancers. An MRI with contrast dye is the best way to see brain and spinal cord tumors.
    </p>
</div>

## Why this Project

<blockquote>
With a surge in the brain related ailments in the world, every year over 2,500 of the Indian children suffer from medulloblastoma, a pediatric malignant primary brain tumour which spreads through the cerebrospinal fluid (CSF) and frequently metastasize to different locations along the surface of the brain and spinal cord, doctors have said.

According to them, in India, every year 40,000-50,000 persons are diagnosed with brain tumour. Of these 20 per cent are children. Until a year ago, the figure was only somewhere around 5 per cent.

The doctors said that if the cases are detected early, then 90 per cent of the tumour cases are curable, provided the treatment protocol is followed correctly.
<div align="right"><i>-Source (“The Hindu “)</i></div>
</blockquote>

- Though the technology used in MRI seems to be quite technically advanced, for the detection of tumour in the MRI image, currently we mostly rely on human skill and expertise.

- Although this method proves to be accurate in most of the cases, it sometimes fails when the human eye fails to detect tiny irregularities
- In a country like India, there is a lack of skilled Oncologists and Neurologists present.

- According to a report in 2018, the country is facing a severe shortage of care-givers with merely 2000 Oncologists to look after around 10 Million patients.

- The condition is much more worse in the rural areas where the count of Oncologists is much less.

## Proposed Solution
​	In order to solve this problem, we propose Brainy, our Brain MRI segmentation tool. Not only would Brainy automatise the entire process of diagnosis for tumours , but it would also help in solving out those cases which are generally missed by the human eye.

#### About the Dataset

​	The data that we’ve used is the BRATS dataset. This dataset contains the segmented images of the brain tissues. After the image has been segmented, there are 3 classes namely Necrotic and Non-Enhancing tumour (NCR/NET), Peritumoral Edema (ED) and GD-Enhancing  tumour (ET). There are about 220 images in the training dataset.

#### The Model

<div>
      <img src="/images/brainy/model-pipeline.png" align="right" width='400' style="margin: 10px 10px 10px 10px;"><br />
      <p>
            To segment the brain MRI’s we use a popular network called <b>U-Net</b>.
The U-Net architecture is built upon the Fully Convolutional Network and modified in a way that it yields better segmentation in medical imaging. Compared to FCN-8, the two main differences are: <br /><br />
            <b>1.</b> U-net is symmetric and <br />
            <b>2.</b> The skip connections between the downsampling path and the upsampling path apply a concatenation operator instead of a sum. <br /><br />
These skip connections intend to provide local information to the global information while upsampling. Because of its symmetry, the network has a large number of feature maps in the upsampling path, which allows to transfer information. By comparison, the basic FCN architecture only had number of classes feature maps in its upsampling path.
        </p>
</div>
<div>
        <img src="/images/brainy/unet.jpg" align="left" style="margin: 10px 10px 10px 10px;" width='400'><br /><center><b>The U-Net Model</b></center>
        	<br /><br /><br /><p>The U-Net owes its name to its symmetric shape, which is different from other FCN variants. U-Net architecture is separated in 3 parts:<br /><br />
            <b>1.</b> The contracting/downsampling path <br />
            <b>2.</b> Bottleneck <br />
        	<b>3.</b> The expanding/upsampling path <br /><br />
	</p>
</div>

#### Metrics

<div>
<img src="/images/brainy/metric.png" align="right" style="margin:10px 10px 10px 10px;" width='200'><br />
<p>
        To quantify the performance of our image segmentation, <b>Dice Score</b> is used. The algorithm is validated by calculating the Dice score, which is a measure of how similar the objects are. So it is the size of the overlap of the two segmentations divided by the total size of the two objects. That is, the score for a particular class c is the size of the overlap between the predicted region and its true counterpart. We were able to achieve best coefficient of dice loss score <b>0.61</b> (higher the better).
</p>
</div>

# Team

| <a href="https://github.com/gktejus" target="_blank"><img src="/images/brainy/Gk.png" height="150px"></a> | <a href="https://github.com/IAmSuyogJadhav" target="_blank"><img src="/images/brainy/Suyog.png" height="150px"></a> | <a href="https://github.com/ubamba98" target="_blank"><img src="/images/brainy/Udbhav.png" height="150px"></a> |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|            [Gk Tejus](https://github.com/gktejus)             | [Suyog Jadhav](https://github.com/IAmSuyogJadhav) | [Udbhav Bamba](https://github.com/ubamba98) |

---

# References

- Brain Tumor Segmentation with Deep Neural Networks (<https://arxiv.org/pdf/1505.03540.pdf>)
- Generalised Dice overlap as a deep learning loss function for highly unbalanced segmentations (<https://arxiv.org/abs/1707.03237>)
- U-Net: Convolutional Networks for Biomedical Image Segmentation (<https://arxiv.org/pdf/1505.04597.pdf>)
- An Open Source Implementation of 3D-Unet (<https://github.com/96imranahmed/3D-Unet>)
- BRATS 2015 Dataset (<https://www.med.upenn.edu/>)

# Notebook
The training notebook can be accessed below:

<a href="https://colab.research.google.com/github/IAmSuyogJadhav/Brainy/blob/master/training.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

---

> This project was made as part of the **TCS PanIIT Conclave 2019 - Mission AI: Solve for India**, a 24 hour hackathon organised at IIT Delhi, India on 19th-20th January by the PanIIT organisation, Skillenza and TCS. Our project achieved 4th rank at the same.

---
