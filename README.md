**Plant Disease Detection**

**Introduction**
This project focuses on making a modular pipeline for detecting and quantifying plant‐leaf diseases using classical image‐processing techniques and machine learning. Our goal was to take raw leaf images, isolate the leaf region, segment disease lesions, extract discriminative features, and train a classifier to predict disease categories and compute percent infection.

**Objectives**
•	Automate leaf‐disease detection from raw images.
•	Quantify infected areas as a percentage of total leaf area.
•	Provide a reusable, well‐documented codebase with clear functions and modules.
•	Showcase all steps in a professional, structured manner.

**Dataset Description**
1. Source and Link
•	Source: Kaggle
•	URL: [PlantVillage Dataset](https://www.kaggle.com/datasets/emmarex/plantdisease)

 2. Structure & Attributes
•	Classes: 7 crop species 
•	Image format: RGB JPG, varying resolutions (256×256)

  3. Sample Size & Division
•	Total images: ~7,700
•	Splits:
o	Training: 70% 
o	Validation: 15% 
o	Test: 15% 

**How to run**
1. Download both files, the python file and notebook file.
2. Run each cell of the notebook
3. After running all cells run flask python file
4. Upload images from the dataset

**Methodology**
1. Leaf Cropping
•	Convert BGR→HSV, threshold on green hues.
•	Find largest contour → convex hull → binary mask. 
•	Crop to bounding box; merge with alpha → PNG.

2. Image Indexing
•	Parsed all PNGs under Leaf_Crops_original/ to build image_index.csv.
•	Recorded: filepath, class_label, format, width, height.

3. Preprocessing & Splitting
•	Resized all crops to 256×256 pixels.
•	Performed stratified split: 70/15/15 for train/val/test.
•	Saved under Processed/{train,val,test}/{class}/.

4. Segmentation of Infected Regions
•	For each leaf image
•	Convert to grayscale and apply Otsu’s threshold (or color‐range) to detect spots.
•	Morphological open/close to remove noise.
•	Output: binary mask (Segmented/masks/) and overlay PNG (Segmented/overlays/).

5. Feature Extraction
•	Color features: mean & std of B, G, R channels.
•	Shape/texture: lesion area, perimeter, roundness, aspect_ratio.
•	Aggregated in features_all_splits.csv with columns: split, label, filename, feature values.

**Classification & Infection Quantification**
1.	SVM Classifier
o	StandardScaler + GridSearchCV (C=[0.1,1,10]; kernel=[linear,rbf]; gamma=[scale,auto]).
o	Evaluated on test set: accuracy, precision, recall, F1, confusion matrix.

2.	Percent Infection
o	Count lesion pixels vs. total leaf pixels → (lesion/leaf)×100.
o	Appended to results_with_infected_area_all_splits.csv.





