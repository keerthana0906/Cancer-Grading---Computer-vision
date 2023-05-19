# Cancer-Grading---Computer-vision
Done as part of Design Project 

## Problem Description:



All the images attached along with this show cells identifiable by their well-stained nuclei. The nuclei are stained either blue or brown.



The diagnosis report is defined as follows:  

**percentage positivity =  percentage of Total nuclei that are brown in color / Total nuclei in the picture (blue & brown inclusive)**

 

Write a program to find the percentage of positivity in the image given below and classify into grade the level of cancer, using the thresholding scheme followed in the convention method, which is detailed below.

 

The Convention Approach:

Currently, we see the image in the microscope, count and calculate the %, manually. This is not reproducible always and is subjective. Idea is to make it objective. 



The gold standard is to take prints and manually count on the images and many studies show that this is objective. To save on paper and time, we want the software to do the job. Brown nuclei indicate that they express the immune marker called Ki67 and the higher the percentage positivity, the more aggressive it is considered to be. In many cases, a cut-off is used to grade cancer. For example <15% is considered low grade and >15% as high grade. Treatment and follow-up actions differ in these 2 situations. Hence, the real problem is with borderline percentages (between 20 & 30% on manual counts). We expect the software to give more accurate and reproducible results.



Here the software must do 2 things

1) Identify percentage positivity (similar to that described above)

2) Classify the intensity of staining as low and high grade.

A score is derived based on the information from the above 2 results.

The score guides therapy and has prognostic implications too.

## Dataset :  


The SHIDC-B-Ki-67 dataset has been downloaded by sending the request at this [link](https://shiraz-hidc.com/service/ki-67-dataset/)     


## Approaches :       


The problem has been solved in four different methods.

### Method 1     


1. The given input slide image was converted into HSV format     
2. Thresholds for S and H channels have been calculated using Otsu’s algorithm        
3. Masks for Brown cells and Blue cells were generated using the thresholds generated in previous steps.      
4. Erosion operation was performed on the generated masks in order to make the cell boundaries clearer.
5. Contours were computed for each of the generated masks and contours having area greater than 0 were filtered out .
6. The resulting contours are then divided into two categories based on their areas ,outliers and non outliers. All the cells within the non-outliers category are considered to be single cells and cells in outliers category are considered as a cluster of cells i.e more than one cell is present in that area and are assumed to be overlapping.
7. Inorder to count the number of cells in clusters (cells present in outliers category) ,the mean area of cells present in non-outlier category is calculated. For each cluster in the outlier category , its area is divided by this mean area thus giving an approximation of the number of cells lying in the cluster.It is further observed that the results were better when this value is further divided by 2. This might be due to the fact that half of the cluster area mostly contains the background cytoplasm.
8. In this way number of Immunopositive and Immunonegative cells were counted from their respective masks and the Ki67 index was calculated using the formula mentioned previously.         

### Method 2       



The masks computed in Method-2 were same as that of masks computed in Method-1.       
1. Erosion operation was performed on the generated masks in order to make the cell boundaries clearer
2. Euclidean distance transform is computed on these masks. This will lead to boundary foreground pixels having low values and centre/internal foreground pixels having high values.
3. The local maxima are calculated for the previous distance transforms. This will highlight the centres of each component present in the distance transform.
4. Finally labels are calculated and number of labels generated will be equal to the number of cells present in the image
5. In this way, the number of Immunopositive and Immunonegative cells were counted from their respective masks and the Ki67 index was calculated using the formula
mentioned previously.      

### Method 3     


1. For computing the mask in this case I have made use of Unet architecture which is widely known for semantic segmentation.
2. Unet architecture has been trained on 1656 training images with validation split of 20 percent.
3. Initially it has been trained with a batch size of 16 for 40 epochs. But it has been observed that the validation loss has been wildly increasing after the 14th epoch.So the model checkpoint saved at 14th epoch has been used for computing the mask.
4. Using this saved model checkpoint , the mask for the unseen images are computed
5. Now the 0th and 1st channels of the generated mask from the model are taken and converted into binary masks with thresholds 127 , 127 respectively. The generated binary images act as binary masks for Immunopositive and Immunonegative cells respectively.
6. The postprocessing to count the number of Immunopositive and Immnonegative cellsis same as that of mentioned in the post processing of Method 2, except for the fact that erosion operation was not performed on Immunopositive cells mask as it was shown to be giving better results without erosion operation.       


### Method 4    


1. For computing the mask in this case I have made use of the latest DeepLabv3 plus architecture which uses xception network as backbone [6].
2. The Deeplabv3+ architecture has been trained on 1656 training images with a validation split of 20 percent.
3. It has been trained with a batch size of 16 for 45 epochs. After verifying to great extent it was found that models generated in epoch 23 and epoch 39 were giving better results
4. Using these two saved model checkpoints , the masks for the unseen images are computed
5. For generating binary masks while using model at epoch 39 , threshold was taken as 80 for both channels.
6. The post processing to count the number of Immunopositive and Immnonegative cells is same as that of mentioned in the post processing of Method 2, except for the fact that erosion operation was not performed on both the masks as it was shown to be giving better results without erosion operation.















