## Introduction

This pilot study and research project aims to define a model to differentiate between diverse fabric textures using computer vision with AI and deep learning. In the fashion and interior design industries, making the right fabric choice is an imperative part of the design process. Accurate fabric classification ensures that designers select materials that meet aesthetic and functional requirements. For instance, a fashion designer might need to distinguish between silk and satin to achieve a garment's desired drape and feel. In another context, an interior designer might choose velvet and linen for upholstery based on texture and durability.

Much of these industries' complex and repetitive work is already automated, thanks to the diverse implications of the AI Automated Visual Inspection System. Automated systems can identify defects and ensure quality control, significantly enhancing efficiency and consistency. However, there are still areas where automation is needed, specifically where classifying fabrics in large quantities is an integral part of an everyday task. For example, in the e-commerce sector, accurately classifying fabric textures helps provide detailed product descriptions, enhancing customer satisfaction and reducing return rates. This project aims to fill these gaps by leveraging AI and deep learning, providing a robust solution for fabric texture classification.




## Research Question

Can a visual AI model differentiate fabric types based on weaving patterns visible in a photograph?



## Application Overview
This project has four stages, as shown in the diagram. Initially, the data was supposed to be collected from three primary sources: First, manually using a mobile camera as the primary sensing device, and second, from diverse image databases online. Third, the images of textiles are scanned using a fingerprint scanner. 

![](C:\Users\Tina\OneDrive - University College London\CASA\CE\CASA0018-DeepLearning\Report\Img\Model diagram.jpg)

The second stage is the data processing stage, where the data is configured, labeled and normalised. The clean data is then fed into the Edge Impulse, where 80% of the dataset is used to train the model, and the other 20% is set aside for testing the trained model. When the model is trained and tested, a smartphone camera is deployed at inference time to categorise the fabric textile detection. The model will ideally be developed and used as an application for textile recognition, developing the automation process in labelling and storing large quantities of fabric products in commercial and design industries. 

The original model was supposed to use a fingerprint scanner to scan the textile details of a fabric, just like a fingerprint. However, after many trials with the sensor, it was discovered that it is internally programmed to pick up the minutia patterns of the finger and skin; therefore, it will not store the image if it does not sense a fingerprint pattern. Thus, this idea was abandoned due to time limitations and the impediments to reprogramming the fingerprint scanner sensor. The source code for the experiments with the fingerprint sensors can be found in the GitHub repository for further study.



<img src="C:\Users\Tina\OneDrive - University College London\CASA\CE\CASA0018-DeepLearning\Report\Img\photo_2025-04-23_13-09-30.jpg" style="zoom: 40%;" /><img src="C:\Users\Tina\Documents\GitHub\casa0018\Assessment\Projects\Final Project\FingerPrint_Scanner\Screenshot 2025-04-23 131427.png" style="zoom: 32%;" />

## Data
The data is divided into four fabric textile categories: **Cotton**, **Linen**, **Silk**, and **Wool**. However, in the training process, the two categories of Linen and Cotton were merged as there was very little difference, and the model struggled to pick this up with the limited data collected. The model is trained with two different data sources: 

1. Data collected through a mobile camera: This batch is collected from diverse everyday objects and clothes using an iPhone camera under adequate lighting conditions, mainly normal daylight. Photos taken in fabric shops and markets of diverse fabrics with and without patterns were another source for less common fabric types, like silk. A total of 183 different types of fabric photos were collected with this method. 

2. Data collected from online resources and textile databases: The images are chosen manually from diverse sources online to maintain the quality of the photos and match the zooming scale for the weaving details to be recognisable by the learning block algorithm. Furthermore, assigned labels and tags are verified with human supervision for more accurate texture detail recognition. To create a good balance for comparison, 190 fabric textures were collected from various online sources. 

   

   ![image-20250412193542872](C:\Users\Tina\AppData\Roaming\Typora\typora-user-images\image-20250412193542872.png)

   

These two data sources were chosen to discover which source gives the most accurate output prediction, or whether they yield more accurate results when combined. Furthermore, the fabrics with patterns were also included to test whether the model could differentiate between the printed patterns and the actual weaving features of the fabric. 



## Model
The central part of this project is the processing architecture of the model and learning blocks, which are used to train the model to recognise patterns effectively when presented with new data. For the purpose of this project, an impulse is defined that takes raw data, uses signal processing to extract features, and then uses a Learning block to classify new data. 

For this project, the processing block was fixed on the images as the input data is pixel-based (96x96) and requires a model that can effectively process image data. However, two different learning blocks were used for this data: Classification and Transfer Learning blocks. Although the classifier block is more commonly used in audio and speech processing, it was chosen in this project because we wanted our model to pick up the detailed weaving patterns(similar to sound image patterns) and categorise what type of textile the fabric belongs to. With this block, the neural networks will take the input data and give a probability score that shows how likely it is that the input data belongs to a particular class; This is while with the Transfer learning block, the model will learn from an existing problem and apply that progressively to the next. However, in the end, the Transfer learning block still yielded more accurate results and so was chosen as the primary learning block.

It was decided to change the DSP from RGB to Grayscale when analysing the fabric texture to optimise the processing time and remove the colour factor from the decisive parameters. This would ensure the fabric is solely categorised by the weaving and texture features, not by colour. However, the patterns of the fabrics still remain an issue, which is deliberately left for the model to decide whether or not to use it as a factor. 

![image-20250414174944919](C:\Users\Tina\AppData\Roaming\Typora\typora-user-images\image-20250414174944919.png)

![image-20250414175031040](C:\Users\Tina\AppData\Roaming\Typora\typora-user-images\image-20250414175031040.png)

## Experiments
The experiments were divided into two parts to differentiate between data collected from online resources and data gathered manually using an iPhone camera. Identical parameters were adjusted in each part to achieve comparable results. Besides configuring the learning blocks for each training dataset, the main features altered in each testing trial included the **Neural Network Architecture**, the number of **Epochs**, the number of **neurons**, and the **Learning rate**.

In the first couple of tests with the DSP Block, it was clear that the Grayscale images produced more accurate results than the RGB versions. As the Table below shows, with the same constant features, the accuracy of both validation and test results increases when the color factor is removed. 

![ONLINE_1-3](C:\Users\Tina\OneDrive - University College London\CASA\CE\CASA0018-DeepLearning\Report\IMPULSES\ONLINE_1-3.png)

A total of 12 trial experiments were conducted for this dataset input, with five additional trials implemented for reference. Three experiments were identified as the most successful, yielding the most accurate results. To assess the impact of each parameter on the outcome, all other parameters were kept constant while one specific parameter was varied.

![MOBILE_MIX_1-3](C:\Users\Tina\OneDrive - University College London\CASA\CE\CASA0018-DeepLearning\Report\IMPULSES\MOBILE_MIX_1-3.png)

![MOBILE_1-3](C:\Users\Tina\OneDrive - University College London\CASA\CE\CASA0018-DeepLearning\Report\IMPULSES\MOBILE_1-3.png)

![COMBINED_1-3](C:\Users\Tina\OneDrive - University College London\CASA\CE\CASA0018-DeepLearning\Report\IMPULSES\COMBINED_1-3.png)

To better monitor potential overfitting, the logs of the training and testing datasets were extracted, analysed separately, and processed into a loss graph for both training and validation.

<img src="C:\Users\Tina\AppData\Roaming\Typora\typora-user-images\image-20250414193851732.png" alt="image-20250414193851732" style="zoom:33%;" />

## Results and Observations

![image-20250415120437965](C:\Users\Tina\AppData\Roaming\Typora\typora-user-images\image-20250415120437965.png)

In the first three attempts for the data collected with the mobile, the model's accuracy remained under 70% with different variations of Epochs, number of neurons, and Learning rate. The validation and training loss graphs also showed an intense overfitting for these trials. However, the most problematic part of the experiment was testing data accuracy, which dropped dramatically, even with the validation accuracy relatively increasing. This probably meant that the data had trouble with the new data and had memorised the existing training data. 



<img src="C:\Users\Tina\OneDrive - University College London\CASA\CE\CASA0018-DeepLearning\Report\Logs\01\Figure_20epoch_16NN_mobile.png" alt="Figure_20epoch_16NN_mobile" style="zoom: 25%;" /><img src="C:\Users\Tina\OneDrive - University College London\CASA\CE\CASA0018-DeepLearning\Report\Logs\01\Loss_100 epoch_mobile.png" alt="Loss_100 epoch_mobile" style="zoom: 29%;" /><img src="C:\Users\Tina\OneDrive - University College London\CASA\CE\CASA0018-DeepLearning\Report\Logs\01\Figure_30epoch_8NN_mobile.png" alt="Figure_30epoch_8NN_mobile" style="zoom: 25%;" />



Looking at the confusion matrix and the data explorer graphs, the model struggles to differentiate between cotton and linen, with linen being 83.3% mistaken for cotton. Even with all the adjusted parameters, the issue persists, which could mean that the data collected is insufficient for the model to determine the differences between the two materials. 

<img src="C:\Users\Tina\OneDrive - University College London\CASA\CE\CASA0018-DeepLearning\Report\Results\Mobile_1-3.png" style="zoom:50%;" />

The cotton and linen datasets were merged to resolve this issue, as the model had already associated them with the same features. This choice dramatically improved the model's accuracy. Also, the model generally performed better, specifically on the recognizing silk with the combined datasets of the mobile and online resources. 

<img src="C:\Users\Tina\AppData\Roaming\Typora\typora-user-images\image-20250415172602720.png" alt="image-20250415172602720" style="zoom: 50%;" />





To conclude, the best-performing models had three effective features: 

1. Grayscale image input using the Transfer learning block
2.  30 Epochs with 0.0005 learning rate and 16 neurons 
3. Combined image data from the mobile and online data sources

Given sufficient time, the model could be more effectively trained to distinguish between linen and cotton. Consequently, future improvements should focus on gathering additional data to enhance the model's ability to detect the most subtle differences between these materials.

## Bibliography


1. Patil, M., Rawoorkar, P., Muley, P., Motade, S., Kukade, S., Deshpande, A., Nair, A., 2024. Edge Impulse: TinyML Language Classification Model, in: 2024 4th Asian Conference on Innovation in Technology (ASIANCON). Presented at the 2024 4th Asian Conference on Innovation in Technology (ASIANCON), pp. 1–6. https://doi.org/10.1109/ASIANCON62057.2024.10838127

2. Sikka, M.P., Sarkar, A., Garg, S., 2022. Artificial intelligence (AI) in textile industry operational modernization. Research Journal of Textile and Apparel 28, 67–83. https://doi.org/10.1108/RJTA-04-2021-0046

3. Classification of Fabrics | PDF | Textiles | Knitting [WWW Document], n.d. . Scribd. URL https://www.scribd.com/document/648389029/1-5-Classification-of-Fabrics (accessed 4.24.25).
4. Warden, P., Situnayake, d., 2019. TinyML. n.d. URL https://www.oreilly.com/library/view/tinyml/9781492052036/ (accessed 4.24.25).

## Declaration of Authorship

I, Tina Samie, confirm that the work presented in this assessment is my own. Where information has been derived from other sources, I confirm that this has been indicated in the work.


Tina Samie

ASSESSMENT DATE

Word count: 1473