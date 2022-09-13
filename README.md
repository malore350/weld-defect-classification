# Weld defect classification with various machine learning techniques

This repository includes general information, notes and personal files relevant to the research topic

## General overview of the issue
Weld defects can occur due to numerous and various reasons, which will be discussed here later. There are many types of weld defect, but the ones that we are interested in are the following: Crack (CR), Porosity (PO), Undercut (UC), Incomplete Penetration (IP), Slag (Sl) and Lack of Fusion (LF). These may later be referred to by the given acronyms. An accurate identification of the defect is essential, since the remedy depends solely on the defect type. In order to gather information about the particular weld, ultrasound testing is generally used:


| ![space-1.jpg](https://user-images.githubusercontent.com/63436458/186456502-ebffc417-b782-43fa-b6ba-5f78719dd912.jpg) | 
|:--:| 
| *A simplified ultrasound test set-up for reference* |

The obtained data is the 1D-signal information of the particular weld. These data can be mapped out over the weld area in order to obtain 2D-data about the weld. Here are the typical causes of the defect types of our interest:

### Crack
**Causes:**
* Use of hydrogen when welding ferrous metals.
* Residual stress caused by the solidification shrinkage.
* Base metal contamination.                                   <img align="right" src = "https://user-images.githubusercontent.com/63436458/186449212-a5368df4-4b3e-48fa-b5d2-274e8a4a1b8f.png">
* High welding speed but low current.
* No preheat before starting welding.
* Poor joint design.
* A high content of sulfur and carbon in the metal.


### Porosity
**Causes:**
* Inadequate electrode deoxidant.
* Using a longer arc.
* The presence of moisture.
* Improper gas shield.                                      <img align="right" src = "https://user-images.githubusercontent.com/63436458/186449919-ac8b945a-4ec9-4d4c-9857-7d832689b69c.png">
* Incorrect surface treatment.
* Use of too high gas flow.
* Contaminated surface.
* Presence of rust, paint, grease or oil.


### Undercut
**Causes:**
* Too high weld current.
* Too fast weld speed.
* The use of an incorrect angle, which will direct more heat to free edges.       <img align="right" src = "https://user-images.githubusercontent.com/63436458/186450108-c3a4a653-6dda-4ab4-995d-c980477be894.png">
* The electrode is too large.
* Incorrect usage of gas shielding.
* Incorrect filler metal.
* Poor weld technique.


### Incomplete Penetration
**Causes:**
* There was too much space between the metal you’re welding together.
* You’re moving the bead too quickly, which doesn’t allow enough metal to be deposited in the joint.
* You’re using a too low amperage setting, which results in the current not being strong enough to properly melt the metal.       <img align="right" src = "https://user-images.githubusercontent.com/63436458/186450625-99c48018-077f-469d-8142-c2ae25e57f5c.png">
* Large electrode diameter.
* Misalignment.
* Improper joint.


### Slag Inclusion
**Causes:**
* Improper cleaning.
* The weld speed is too fast.
* Not cleaning the weld pass before starting a new one.           <img align="right" src = "https://user-images.githubusercontent.com/63436458/186450667-0a19c2d7-e31f-4c1e-9f2a-c40470f468fb.png">
* Incorrect welding angle.
* The weld pool cools down too fast.
* Welding current is too low.


### Lack of Fusion
**Causes:**
* Low heat input.
* Surface contamination.
* Electrode angle is incorrect.                                 <img align="right" src = "https://user-images.githubusercontent.com/63436458/186450882-0b50d468-11ad-4306-b97b-d0b2ecb7e233.png">
* The electrode diameter is incorrect for the material thickness you’re welding.
* Travel speed is too fast.
* The weld pool is too large and it runs ahead of the arc.

<br/>

## Data Collection
Data obtained from the PAUT is the intensity level signal information, which is a 1D data. This data can be mapped over the range width of the metal to obtain the intensity map of the signal, which is the 2D data. The difference in data types provides a certain flexibility with respect to training methods.

The acquired data has a massive weight imbalance, with the data distribution being 0.88:0.03:0.02:0.04:0.01:0.01 for NO:CR:IP:LF:PO:Sl:UC. The huge amount of data for Normal category is natural, since only limited areas of the sample have defects. Still, Porosity, Undercut and Slug categories have a sizable lack of data with respect to rest of data, but this cannot be adjusted. The data weights had been manually balanced before training, but the unavailability of the data for several defects still presents inherent problems in training.

The disadvantage of both 1D convolutional neural network and simple 2D image classification is that it is naturally bounded with the binary outcome; however, it is technically possible that multiple defects are present on the same width of a sample.
## Possible approaches to this problem

### 1D signal classification
* In 1D convolution, kernel slides only in 1 direction
* 1D CNN can perform signal recognition and classification from the given categories of data.

*3 sample PAUT signal data for various defects* | *A kernel of a sample 1D convolution*
--- | ---
![](https://user-images.githubusercontent.com/63436458/186459776-46c7d9d7-4763-413e-8b24-a1e55c5c00cc.png) | ![](https://user-images.githubusercontent.com/63436458/186459792-284800ca-9430-461e-923d-e3d8f88ba19e.png)

#### Results:

**CR** | **IP** | **LF** | **NO** | **PO** | **Sl** | **UC** | **Total Accuracy**
--- | --- | --- | --- | --- | --- | --- | ---
0/96 | 0/64 | 140/171 | 290/290 | 0/30 | 0/20 | 0/28 | 61%

The high accuracy is the result of the data imbalance and high accuracy in categories with higher amount of data. Since most categories do not even get classified, the 1D classification was deemed to be an unsuitable method, and thus has been dropped.


### 2D data classification
For sake of possible deployment, I deemed implementing models that can detect multiple objects as essential. For this reason, I implemented the Facebook Detectron2 Faster R-CNN. Just like its alternatives like YOLO, it works around the region based CNN, with the extra Region Proposal Network (RPN) at the last layer, which allows region proposal.

| (![image](https://user-images.githubusercontent.com/63436458/189889635-6df0438f-88d2-4e4c-bbc3-e6bedb493578.png) | 
|:--:| 
| *A simplified ultrasound test set-up for reference* |
