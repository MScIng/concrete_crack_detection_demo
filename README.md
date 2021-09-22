# Concrete Crack Detection
Concrete crack detection for bridge inspections

<p align="center">
    <img src="/figures/congl_pred.jpg" | width=500 />
    <img src="/figures/lcw.png" | width=500 />
</p>

The two semantic classes in the dataset are:
```
Non concrete crack (Background)
Concrete crack
```
***Coming soon in November***

:red_circle:\[[Paper](/access/not_ready.png)\] :red_circle:\[[Dataset (LCW)](/access/not_ready.png)\] :red_circle:\[[Dataset (Conglomerate)](/access/not_ready.png)\] :red_circle:\[[Trained models (LCW)](/access/not_ready.png)\] :red_circle:\[[Trained models (Conglomerate)](/access/not_ready.png)\]

The conglomerate concrete crack segmentation dataset is comprised of many existing concrete crack datasets from literature. The Labeled Cracks in the Wild dataset is comprised of more global scenes gathered from bridge inspection reports. These two datasets aid with the detection and localization on concrete cracks. In particular, the LCW dataset is useful to the field becuase it offers a global scene perspective, which is more of what is found during inspection.  

## Results
While f1-scores may be useful when classes are well-balanced, they can be misleading in situations such as this, where the classes are unbalanced. The percentage of labeled crack pixels in the conglomerate and the original LCW dataset was 2.8% and 0.3% respectfully. The f1-score for the model trained on the conglomerate dataset was 98.5%, however it was only able to detect 71% of the crack pixels. Therefore we have used the percentage of correctly identified cracks as our metric for success. With the conglomerate dataset predicting approximately 71% of the crack pixels, and the LCW with no blank images predicting approximately 40% of the cracks.  

<p align="center">
    <img src="/figures/lcw_table.jpg"  | width=600/>
</p>

<p align="center">
    <img src="/figures/percent cracks.png"  | width=400/>
</p>
    

## Requirements
The most important environment configurations are the following:
- Pytorch >= 1.4
- Python >= 3.6
- tqdm
- matplotlib
- sklearn

## Evaluating the Trained DeeplabV3+ Model
- Download the DeeplabV3+ :red_circle:[trained model weights](/access/not_ready.png)
- Configure ***run_metrics_evaluation.py***

You will get the f1 score, the jaccard index, and the confusion matrix. We suggest running this in an IDE. 
  
## Visualizing the results from the Trained DeeplabV3+ Model
Once training has converged or when it has stopped, we can used the best checkpoint based on the validation data results. This checkpoint is loaded and our test data is evaluated. 

***run_show_results__.py***
- gets predicted masks
- gets combined mask and image overaly
- gets one-hot-encoded vector images of predictions

## Training with the concrete crack dataset

1. Clone the repository
2. Download the Labeled Cracks in the Wild :red_circle:[dataset](/access/not_ready.png) or the Conglomerate Concrete Crack :red_circle:[dataset](/access/not_ready.png)
3. Go into the Training folder
4. Create a DATA folder
5. Copy and paste the Train and Test folders for 512x512 images from the dataset you downloaded into the DATA folder
6. The DATA folder should have a folder called 'Train' and a folder called 'Test'. Inside each of those folders include the mask and image pairs in their respective folders (Masks, Images). 
7. If you have set this up correctly then you are now ready to begin.

Neccesary and optional inputs to the ***main_plus.py*** file:
('-' means it is neccessary, '--' means that these are optional inputs)
```
 -data_directory = dataset directory path (expects there to be a 'Test' and a 'Train' folder, with folders 'Masks' and 'Images')
 -exp_directory = where the stored metrics and checkpoint weights will be stored
 --epochs = number of epochs
 --batchsize = batch size
 --output_stride = deeplab hyperparameter for output stride
 --channels = number of classes (we have four, the default has been set to four). 
 --class_weights = weights for the cross entropy loss function
 --folder_structure = 'sep' or 'single' (sep = separate (Test, Train), single = only looks at one folder (Train). If you want to get validation results instead of getting back your test dataset results then you should use 'single'. If you want to test directly on the Test dataset then you should use 'sep'.
 --pretrained = if there is a pretrained model to start with then include the path to the model weights here. 
```

Run the following command:
```
python main_plus.py -data_directory '/PATH TO DATA DIRECTORY/' -exp_directory '/PATH TO SAVE CHECKPOINTS/' \
--epochs 40 --batch 2
```

During training there are model checkpoints saved every epoch. At these checkpoints the model is compared against the test or validation data. If the test or validation scores are better than the best score, then it is saved. 

## Training with a custom dataset
1. Clone the repository
2. Ensure your image and mask data is 512x512 pixels. *(can use the ***rescale_image.py*** in Pre-processing)*
3. Ensure that if you resized your masks to 512x512 that they did not interpolate the colors into more color classes than you have. The expected format is BGR. *(can use the ***rescale_segmentation.py*** in Pre-processing)*
4. You now need to go into the ***datahandler_plus.py*** file and edit the colors as necessary. For example, the Structural Materials dataset used the following format, which is in the ***datahandler_plus.py*** in this repository.
```
# color mapping corresponding to classes
# ---------------------------------------------------------------------
# 0 = background (Black)
# 1 = crack (white)
# ---------------------------------------------------------------------
self.mapping = {(0,0,0): 0, (255,255,255): 1}
```
6. Adjust the number of 'channels' in the training command to match the number of channels that you have. ***(In this case you should only have two channels since we only have a binary classification)***. 
7. Ensure that your DATA folder has a folder called 'Train' and a folder called 'Test'. Inside each of those folders include the mask and image pairs in their respective folders (Masks, Images). 
8. If you have set this up correctly then you are now ready to begin.

## Building a Custom Dataset
(The images in the dataset were annotated using [GIMP](https://www.gimp.org/). We suggest that you use this tool)

1. Before beginning to annotate, we suggest that you use jpeg for the RGB image files. We advised against beginning with images which are already resized. If the images have a high-pixel you want to capture this in your original annotation. This lets whoever is using the dataset to decide if they want to break the original image into sub-images or resize it as they see fit. 

2. For assistance on how to annotate the images using the GIMP software, we have provided a [video tutorial]() to outline our process. 

3. If you followed the video, you will have matching GIMP, PNG, and jpeg files, indicating the GIMP, mask, and image trio respectfully. 

4. Here you must convert the color of the PNG to a black background, and a white labeled crack. This is done using the .

5. Re-scale these images and masks using the respective files in Pre-processing as you find appropiate. You can also use the random sort function we have created to randomly split the data into Training and Testing. 


## Citation
```
Conglomerate Concrete Crack Dataset: 
Conglomerate Concrete Crack Models:
LCW Dataset: 
LCW Models:
Paper:
```


