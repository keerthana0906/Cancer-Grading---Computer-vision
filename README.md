# Cancer-Grading---Computer-vision
Done as part of CV course assignment  

Problem Description:



All the images attached along with this show cells identifiable by their well-stained nuclei. The nuclei are stained either blue or brown.



The diagnosis report is defined as follows:  

percentage positivity =  percentage of Total nuclei that are brown in color / Total nuclei in the picture (blue & brown inclusive)

 

Write a program to find the percentage of positivity in the image given below and classify into grade the level of cancer, using the thresholding scheme followed in the convention method, which is detailed below.

 

The Convention Approach:

Currently, we see the image in the microscope, count and calculate the %, manually. This is not reproducible always and is subjective. Idea is to make it objective. 



The gold standard is to take prints and manually count on the images and many studies show that this is objective. To save on paper and time, we want the software to do the job. Brown nuclei indicate that they express the immune marker called Ki67 and the higher the percentage positivity, the more aggressive it is considered to be. In many cases, a cut-off is used to grade cancer. For example <15% is considered low grade and >15% as high grade. Treatment and follow-up actions differ in these 2 situations. Hence, the real problem is with borderline percentages (between 20 & 30% on manual counts). We expect the software to give more accurate and reproducible results.


 

Here the software must do 2 things

1) Identify percentage positivity (similar to that described above)

2) Classify the intensity of staining as low and high grade.

A score is derived based on the information from the above 2 results.

The score guides therapy and has prognostic implications too.
