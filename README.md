
# Dog Behavior Classifier – Full Project Guide

This system helps you automatically detect common dog behaviors like walking, sniffing, eating, shaking and so on, using only data from a neck-mounted motion sensor.
It works by analyzing accelerometer and gyroscope data collected from a collar. The system learns patterns in how dogs move and uses this to automatically identify behaviors.


## What This Project Does

It takes two input files:

1. Sensor data — from a device on the dog’s neck
2. Dog info — the dog's breed, age, weight, gender, etc.

Then it:

* Trains a smart model to **recognize behavior patterns**
* Makes accurate predictions from new data
* Gives you a simple report showing how well it performed



## What Behaviors Can It Detect?
* Walking – normal, rhythmic movement
* Eating – jaw/head-focused movement
* Sniffing – subtle, low-intensity movement
* Shaking – fast, intense body motion
* Other movements such as Lying chest, Playing, Panting, Trotting, Sitting, Standing, Pacing, Drinking, Carrying object, Tugging, Galloping,  Jumping, Bowing              


## How The Classifier Works

### Used Smart Learning with a Hybrid Model

We used a hybrid model that combines:

| Component       | What It Sees                    | Why It Matters                                                             |
| --------------- | ------------------------------- | -------------------------------------------------------------------------- |
| CNN + LSTM   | Time-based neck motion patterns | Captures short bursts (shaking), trends (walking)                          |
|  Dog Metadata | Age, breed, weight, etc.        | Adds important context (e.g., a puppy moves differently than a senior dog) |

Combining these gives the model a full picture, using both shows how the dog is moving, and who the dog is.
Because:

- Small dogs shake differently than large ones
- Breed, age, and weight affect motion
- Adding these boosts accuracy, especially for rare behaviors



## Step-by-Step Process of the Project

### STEP 1 – Preprocess Your Data

- Keeps only the neck sensor columns  
- Removes undefined or junk labels such as 'undefined', 'synchronization' and 'Extra_synchronization'
- Merge sensor-data row with dog info data
- Converts everything into numeric arrays  
- Window the data into squence. Slices the data into 1-second 
- Encode labels
- Saves the result for training



### STEP 2 – Train the Model

- Split data into train and test
- Stanadardize data
- Handle imbalanced data (since lying chest and sniffing dominates the dataset)
- Builds a hybrid model  
- Uses CNN-LSTM to understand movement  
- Uses breed/age/weight as added context  
- Trains the model and saves it in `models/`
- Visualize the learning curve for the train and validation accuracy and loss
- Saves model for prediction



### STEP 3 – Predict and Evaluate Test Data


- Loads your model and encoder  
- Makes predictions (like: Walking, Sniffing, etc)  
- Get Accuracy score of the predicted classes compared to the true classes
- Show the classification report for which classes. It shows the Precision, Recall and F1-score
- Plot a confusion matrix. It shows the true positives, True negatives



### STEP 4 – Prediction Script for New Data

Run:

```bash
python Predict_Movement_Script.py
```

Files needed:
1. sensor_path (The path where the sensor data is stored in your computer or dirve)

This contains readings from a neck sensor:

| Column       | Meaning                     |
| ------------ | --------------------------- |
| ANeck_x/y/z | Accelerometer (motion axes) |
| GNeck_x/y/z | Gyroscope (rotation axes)   |
| Behavior_1  | The labeled behavior        |

2. doginfo_path

This contains extra info about each dog:

| Column          | Example              |
| --------------- | -------------------- |
| DogID           | 16                   |
| Breed           | Labrador             |
| Age months      | 24                   |
| Weight          | 20 (kg)              |
| Gender          | 1 = Male, 2 = Female |
| NeuteringStatus | 0 = No, 1 = Yes      |

3. model_path
4. label_encoder_path 
5. scaler_seq_path
6.  scaler_meta_path

The script follow the same steps as the trained data; the loading, cleaning, window creation, prediction and evaluation

### STEP 5 – Read the Report of the results

Open:

```
report.txt
```
You’ll see:

- Model accuracy  
- How well it predicted each behavior in the classification report 
- A confusion matrix showing what it got right vs wrong


