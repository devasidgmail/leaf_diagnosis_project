## Leaf Disease Diagnosis Project
Empowering Home Gardeners: AI-Driven Disease Detection for Proactive Plant Care. 

### Business Understanding 
The business goal of this project is to enable home gardeners with a tool to detect diseases in common household plants. By using AI, this project lowers the barrier to expert level botanical knowledge, allowing users to take proactive corrective action before the plant is lost.  

### Technical Value Proposition
The project transitions from a "laboratory" success to a practical tool by addressing specific operational challenges:
* High Precision: Achieving a 99.22% Test Accuracy using a fine-tuned ResNet50 architecture ensures users receive reliable diagnoses.
* Real-World Usability: The inclusion of a Top-K Prediction system (showing the top 3 likely diagnoses) acknowledges that nature is rarely 100% certain, providing users with a nuanced confidence percentage.


### Project Workflow
This project implements a complete machine learning pipeline—from raw data preprocessing to real-world inference—to identify 38 different classes of plant diseases using the PlantVillage dataset and a fine-tuned ResNet50 architecture.

The project is divided into five specialized stages:

### 1. Data Preprocessing : 1_preprocessing.ipynb
* Data Partitioning : Splits the original dataset into Training (70%), Validation (15%), and Testing (15%)
* Quality Audit: Verified that 100% of sampled images met the $256 \times 256$ resolution requirement.
* Standardization: Prepared the directory structure for TensorFlow/Keras data loaders.

### 2. Exploratory Data Analysis (EDA) : 2_visualizations.ipynb 
* Distribution Analysis: Identified class imbalances (e.g., Orange Haunglongbing at 5,507 images vs. Potato healthy at 152 images).
* Visual Validation: Generated image grids to verify label accuracy and visual symptoms.
* Metadata Audit: Confirmed uniform aspect ratios and widths before training.

### 3. Model Training & Fine-Tuning : 3_trainings.iypnb
* Transfer Learning: Utilized a ResNet50 base pre-trained on ImageNet.
* Data Augmentation: Implemented random flips, rotations, zooms, and contrast adjustments.
* Hyperparameter Optimization: Used Keras Tuner to find the best configuration:
    * Dense Units: 128 | Dropout Rate: 0.4
    * Fine-tuning Depth: Unfreezing the top 40 layers.
* Results: Achieved a 99.22% Test Accuracy.

### 4. Model Evaluation : 4_evaluation.ipynb
* Metrics: Detailed classification report showing a 0.99 Weighted Average F1-Score.
* Confusion Matrix: Generated to pinpoint specific inter-class misclassifications.

### 5. Real-World Inference : 5_inference_real_leaf.ipynb
* Deployment: A script to upload local images for disease prediction.
* Top-K Prediction: Displays the top 3 most likely diagnoses with confidence percentages.



## Key Findings
* Exceptional Dataset Performance: The model achieved near-perfect precision (1.00) for major classes like Orange Haunglongbing and Soybean Healthy when tested on the PlantVillage dataset.
* Specific Confusion Points: Minor performance dips were noted in Corn Cercospora Leaf Spot (0.91 recall), showing confusion with Northern Leaf Blight due to similar lesion patterns.
* The "Domain Shift" Challenge: While the model is highly accurate on studio-captured images (uniform backgrounds/lighting), performance degrades on real-world photos. Natural lighting, shadows, and cluttered backgrounds introduce noise the model was not fully exposed to during training.


## Future Work
To evolve this project from a laboratory model to a robust field tool, the following steps are proposed:

1.  Background-Aware Training: Incorporate "Background" or "Noise" classes containing non-leaf images to reduce false positives in the field.
2.  Diverse Data Acquisition: Supplement the training set with images captured in natural environments (different weather conditions and growth stages) to combat Domain Shift.


### Performance & Optimization Notes

During the development significant training bottlenecks were identified and resolved through data pipeline restructuring and hardware scaling.

### Data Pipeline Optimization & I/O Efficiency
* Initial Bottleneck: Storing the data on Google Drive and reading images during training caused extreme I/O latency. This resulted in training times of nearly 2 hours per epoch and left the CPU significantly underutilized.
* Latency Resolution: The dataset was compressed into a .zip file, transferred to Colab run time memory, and extracted before the training loop began.
* Performance Gain: This structural change improved data throughput drastically, reducing the training time from 120 minutes per epoch to approximately 20 minutes.
* Key Insight: Efficient data access and minimizing "small file" I/O over network drives can have a greater impact on performance than simply upgrading raw computational resources.

### Hardware Acceleration & Training Stability
* Compute Constraints: Initial attempts to run Keras Tuner for hyperparameter optimization using standard CPUs resulted in a system crash after 5 hours of execution (at roughly 70% completion).
* Strategic Upgrade: To ensure session stability and handle the 38-class classification complexity, the environment was switched to high-performance A100 GPUs via Colab compute credits.
* Result: With hardware acceleration, the entire fine-tuning process—which previously failed after hours on a CPU—completed successfully in just 60 minutes.
* Outcome: This allowed for the successful generation of the final optimized model, with a 99.22% Test Accuracy.

### Link to Google Colab Notebooks. 
1. [1_preprocessing.ipynb](https://colab.research.google.com/drive/1AcfjIR24M1xhrg_bjjfrYb0TyzNbb8Iv)
2. [2_visualizations.ipynb](https://colab.research.google.com/drive/1ylmBTFvj0P2JePw1H-DLsuSv7nTiCPMs)
3. [3_training.ipynb](https://colab.research.google.com/drive/1892Kg6_uzkmJludGy4-wTtImHYczwPUk)
4. [4_evaluation.ipynb](https://colab.research.google.com/drive/1FNqPBTSkM4Z0vufG6STjX2Ugi1lpOnM0)
5. [5_inference_real_leaf.ipynb](https://colab.research.google.com/drive/1nD3Rnz5gv8vQNcSoEieEuh-7StOZ_z3o)
