---
layout: post
title: Binary Classification of Rip Current Images Using Artificial Intelligence / 使用人工智能对离岸流图像进行二分类
date: 2024-06-30
tags: [PBL, Rip Currents, Visual Perception, Artificial Intelligence, dev]
toc:  true
author: Mickey Yang · Ethan Meng · Connor Wu · Leo Qiu · Jocelyn Cai
math: true
---

An effective method for detecting rip currents, which pose significant hazards to beachgoers, using artificial intelligence. A binary classification model on aerial images of the ocean is trained, utilizing Multi-Layer Perceptron classifiers and achieving a testing accuracy of 73.9%, and an accuracy of 92.5% after applying a confidence threshold.
{: .message }

## Abstract 

- _Context._ Rip currents pose significant hazards to beachgoers, often catching untrained individuals unaware and leading to dangerous situations. 
- _Aims._ The research aims to develop an effective method for detecting rip currents using artificial intelligence, thereby enhancing beach safety. 
- _Methods._ The method involves training a binary classification model on aerial images of the ocean, categorizing them into those with rip currents and without rip currents. Data augmentations such as cutting, rotating, and flipping the image are employed so that the generalization ability of our model could be enhanced. 
- _Results._ We utilized Multi-Layer Perceptron classifiers, achieving a testing accuracy of 73.9%, and an accuracy of 92.5% after applying a confidence threshold. The integration of the model with a Tello Talent Robomaster TT drone introduced significant flexibility and efficiency, with an average processing time of 3.09 milliseconds per 1000 images. The model also demonstrated relatively high performance, with a True Positive Rate (TNR) of 93.06% and True Negative Rate (TNR) of 91.10%. 
- _Conclusions._ This innovative approach of integrating AI with rip current proves to be flexible, efficient, and effective making a significant contribution to beach safety.

**Key words:** Rip current; Artificial intelligence; Binary classification; Beach safety; Artificial neural network; Marine environmental monitoring 

## Introduction 

Rip currents are powerful, narrow channels of fast-moving water running from a beach back to the open ocean, sea, or lake, usually sustains for several minutes. Rip current can move up to 8 feet per second (more than 2 meters per second), which is much faster than even Olympic swimmers. They are usually caused by the fragmented tides created by radiation stress in water waves.

![placeholder](https://raw.githubusercontent.com/MickeyYQA/you7n-blog/master/img/20240630/wps_doc_1.jpg "img")

Figure 1. Anatomy of the rip current

_Note._ This image was acquired from the website of National Oceanic and Atmospheric Administration (NOAA)[^fn-1].

Rip currents present a significant danger due to the general public’s lack of awareness or inability to recognize these potentially lethal oceanic phenomena, often resulting in hazardous situations for unsuspecting swimmers. Typically, surfers often show themselves as calm areas in a sea of waves and this may deceive people into thinking that these are safe places. In essence, however, these tranquil zones are the most dangerous where strong currents create significant hazard. The untrained eye will not straightforwardly or hastily notice the subtle visual indicators of the presence of a rip current like watery tones, wave patterns or even foam moving outwards together with seaweed or debris.

![placeholder](https://raw.githubusercontent.com/MickeyYQA/you7n-blog/master/img/20240630/wps_doc_2.png "img")

Figure 2. Examples of rip current patterns

The prevalence of rip currents poses a substantial risk to public safety, as evidenced by data from the National Oceanic and Atmospheric Administration (NOAA). In 2023, rip currents were responsible for over 80% of the aquatic rescues executed by lifeguards across the United States, equating to approximately 90 out of 98 interventions. [^fn-1] Furthermore, the Florida Panhandle reports indicate that rip currents account for 60% of the fatalities in the vicinity of the Great Lakes, underscoring the lethal nature of these swift marine currents. [^fn-2] 

Rip currents are often unpredictable which magnifies the problem because it is challenging for individuals to spot them. It should be noted that they can form and dissipate quickly hence making it hard to give prompt warnings or even closely monitor all potential danger zones by beach safety personnel. Such unpredictability makes it necessary to install efficient alert systems aimed at protecting the public.

Given these challenges, there is a pressing need for advanced technological solutions to aid in the identification and monitoring of rip currents. AI-based models integrated with drones, like the system we propose, offer a promising approach to enhance beach safety. By providing real-time, accurate detection of rip currents, such systems can alert both the public and lifeguards to potential hazards, thereby reducing the risk of accidents and saving lives.

### Literature review 

#### Hazard and Influence: 
Rip currents have been reported as the most hazardous safety risk to beachgoers (Rampal et al., 2022, [^fn-6]). In China, accumulated Chinese surf zone fatalities were reported more than marine disaster casualties over the recent ten years. The surf zone accident has a high occurrence frequency in the southern regions and peaks from July to August (Zhang et al., 2021, [^fn-9]). In Australia, rip currents are responsible for more deaths than hurricanes, floods, and tornados combined (“Methodology for Prediction of RIP Currents Using a Three-Dimensional Numerical, Coupled, Wave Current Model,” 2011, [^fn-19]: ). Governments are concerned about the negative social and economic influence caused by the rip current. Many countries have established operational frameworks across various organizational levels on rip current prevention, including risk investigation, data-driven forecasting, beach safety improvement, and public awareness (Zhang et al., 2021, [^fn-9]). Despite warning signs and educational campaigns, many beachgoers and visitors still do not know how to reliably and efficiently identify and localize rip currents, and this coastal process still poses serious threats to beach safety (Pitman et al., 2021, [^fn-20]: ). This also extends to lifeguards, who, because of the oblique angle they observe the ocean, can struggle to identify certain rip currents, especially when the coastal morphology is complex.
#### Recent Study: 
As a public safety hotspot, the rip current has long been of great research interest in its mechanism, characteristics, and prediction. (Zhang et al., 2021b, [^fn-9]) Many practical approaches are adopted in the study of the rip current, such as scaled experiment, numerical simulation, remote image interpretation, onsite observation and measurement simulation, remote image interpretation, onsite observation and measurement (Castelle et al., 2016b, [^fn-23]: ). Owing to the intrinsic instability of the rip current, accurate prediction of its exact occurrence is not feasible at present (Zhang et al., 2021b, [^fn-9]). 
#### CNN and AI Prediction: 
Scientists introduced an interpretable AI to predict and localize rip currents. They also highlight some of the advantages of supervised learning through deep learning techniques such as CNN and their ability to learn from experience and learn complex dependencies and features to derive a set of model weights/parameters that produce maximum accuracy. (Rampal et al., 2022, [^fn-6])
#### Critical Analysis and Conclusion: 
In summary, rip currents represent a critical safety issue. They have a significant influence on beachgoers, coastal communities, and local governments. Recent researches mainly focus on the hazards and social influences, but only a few proposed plausible solutions to severe problems caused by rip currents. 
Although using interpretable AI and CNN is an effective and peculiar way to detect and predict the rip current, the authors didn’t provide any means related to the practical application of this technique.
### Artificial Intelligence 
To tackle the problem, our team came across Artificial intelligence (AI); by implementing AI in our classification system, we can improve the classification accuracy by a huge margin. 

#### Neural Network:
In this project, we use a neural network model implemented with the Multi-Layer Perceptron Classifier. The model features a single hidden layer with 60 neurons using the ReLU activation function, enabling it to learn complex patterns. For binary classification, the output layer employs the sigmoid activation function.

The model is trained using the 'Adam' optimizer with a learning rate of 0.001 to minimize binary cross-entropy loss. Random_state=1 is set to ensure reproducibility. Training runs for up to 150 iterations, which updates weights through backpropagation until convergence or the iteration limit is reached.

#### Real-Time Analysis: 
AI has fast classified times and short classifying intervals, which means that it can keep up with real-time footage. By combining AI with video-taking methods like drones, we can continuously analyze the footage and give real-time classification results. 
#### High Accuracy: 
At the end of our research, we have achieved a consistent classification accuracy of 74% on testing set, and 93% after adjusting confidence threshold. This level of precision makes it reliable for practical uses such as beachside alerts. 
#### Automated Alerts: 
By implementing AI classification, we no longer require, or requiring only a little, human intervention during the detection process. If rip currents are detected along the coastline, the system can make alerts automatically. 
### Drone Implementation 
In addition to the implementation of AI, out team has also decided to use drone for the coastline footage. With the usage of drone taking footage from the air, we can achieve a real-time detection along the coastline and cover much more area. By doing this research, we built a system that can actively and reliably detect rip currents along coastlines to prevent fatalities happening due to the rip currents. 
## Methodology 
To address the problem of effectively detecting rip currents and enhancing beach safety, we developed an AI-based model integrated with drone technology.
### Data Acquisition and Preprocessing Pipeline
#### Image Acquisition: 
The first step is to collect a dataset of images representing near-shore conditions for training. A total of 2484 images, including scenarios of both rip currents (1781 images) and non-rip currents (703 images), were acquired from the database provided by [^fn-7] to build a reliable model for classifying rip currents. These images are high-resolution aerial images from Google Earth. [^fn-6] The 2484 images were split into two parts. One part takes up 80% of the total dataset, and was used as training set. This part contains 1425 (contains rip currents) + 562 (no rip currents) = 1987 images. The other part, which contains 356 (contains rip currents) + 141 (no rip currents) = 497 takes up 20% and was used as testing set. No validation set was established due to the relatively small amount of data.
#### Data Augmentation: 
In this study, we implemented a data augmentation pipeline to improve the robustness and generalization of our neural network model. Data augmentation helps increase the diversity of the training dataset by applying various transformations to the existing images, thus generating new training examples without additional manual labeling. 

Our augmentation strategy involves several techniques. First, we apply random rotations to the images by multiples of 90 degrees. This helps the model to recognize objects regardless of their orientation. We also perform random cropping and resizing, which introduces variability in the scale and viewpoint of the images. Additionally, random horizontal flips are applied to the images to ensure that the model learns to identify features from both left and right perspectives. This augmentation process was automated using a custom script written in Python with OpenCV. A few examples of augmented images will be shown later.

Four augmentations on training set with no rip currents and four augmentations on image dataset with rip currents were done. A total number of 1986 (training set) × 4 = 7948 images. 7948 (training set after augmentation) + 497 (testing set) = 8445 images were actually used in the model training.


Table 1. Training, testing, and rip current existence dataset detail

![placeholder](https://raw.githubusercontent.com/MickeyYQA/you7n-blog/master/img/20240630/wps_doc_3.png "img")


#### Dataset Preparation: 
Since the dataset has images with resolution varying from 234 ×234 to 1094 × 1322, after augmenting the images, it is essential to standardize them in order to maintain uniformity concerning input format for the model. The resizing process was done using OpenCV library and images were adjusted to a fixed resolution of 20 pixels by 20 pixels. This has the additional advantage of reducing computational effort during training the model. Along with this, these images are turned into grayscale through OpenCV. Converting images into grayscale simplifies information content and reduces data into one channel. This sort of operation focuses on structural characteristics instead of color changes and makes learning more efficient and better model training possible. 

![placeholder](https://raw.githubusercontent.com/MickeyYQA/you7n-blog/master/img/20240630/wps_doc_0.png "img")

Figure 3. Original image (left) and three different images after augmentations and dataset preparation (right)

#### Preprocessing: 
After that, every image was translated into a Comma-Separated Value (CSV) file so that the model could be trained. This stage required converting 2D array of pixel values into one row of CSV files. Each cell sequentially records the grayscale value of one pixel thus resulting in a 1-row sequence having 400 columns. In every row’s first column, labels were added which show if rip currents are there (1) or not there (0). The binary classification (labeling) of the training dataset has already done when we acquired the images. The machine learning model was trained using this binary classification to enable it accurately differentiate between rip current and non-rip current. 
### Model Training
#### Model Selection: 
In this project, we utilize a neural network model implemented with the MLPClassifier (Multi-Layer Perceptron Classifier) from the Scikit-Learn Library. The Multi-Layer Perceptron Classifier is well-suited for this task due to its capacity to handle complex patterns and relationships within the data, leveraging artificial neural network principles. The choice of MLPClassifier was driven by its proven effectiveness in image recognition tasks and its flexibility in adjusting parameters to optimize performance. [3,8] 
#### Architecture and training parameters:  
The architecture of the model comprised with a single hidden layer containing 60 neurons. The hidden layer utilizes the ReLU activation function, which introduces non-linearity into the network and enables it to learn complex patterns within the data. The output layer employs the SoftMax activation function, which is suitable for classification tasks. [^fn-18]: 

The model is trained using the ‘Adam’ optimizer, an efficient variant of stochastic gradient descent that adapts the learning rate for each parameter. [3,4] The learning rate for the ‘Adam’ optimizer is set to 0.001. The optimization process aims to minimize the binary cross-entropy loss, and is suitable for binary classification tasks.

To ensure reproducibility, a random seed is set using random_state = 1. The model training is executed over a maximum of 150 iterations, allowing the weights to be updated through backpropagation until convergence or the iteration limit is reached.

![placeholder](https://raw.githubusercontent.com/MickeyYQA/you7n-blog/master/img/20240630/wps_doc_4.png "img")

Figure 4. Schematic diagram of the architecture of the neural network, not to scale



#### Training: 
Since we have done four augmentations on the testing image dataset containing both rip currents and no rip currents, a total number of 7948 augmented images was put into the training set. 
### Performance Metrics 
We have used several key metrics in order to evaluate the performance of our binary classification model: accuracy, true negative rate (TNR), and true positive rate (TPR). These metrics provide a comprehensive understanding of how well the model distinguishes between images with rip currents and those without.

Accuracy is the proportion of correctly classified instances out of the total instances in the dataset. Our model achieved an accuracy of 82.8% on the training set and 73.9% on the testing set. This indicates that the model is performing relatively well on unseen data, maintaining a high level of accuracy from training to testing.

True Negative Rate (TNR) measures the proportion of actual negatives that are correctly identified by the model. For our testing set, the model achieved a TNR of 0.62. This means that 62% of the images without rip currents were correctly classified. True Positive Rate (TPR) measures the proportion of actual positives that are correctly identified by the model. On the testing set, the TPR was 0.75, indicating that 75% of the images with rip currents were correctly classified.

![placeholder](https://raw.githubusercontent.com/MickeyYQA/you7n-blog/master/img/20240630/wps_doc_5.png "img")


Figure 5. Examples of FP, TN, TP and FN classifications

The model’s training time was also a key consideration. The total time taken to train the model was 6.48 seconds, highlighting the efficiency of the training process. This quick training time is particularly advantageous when dealing with large datasets or when frequent model updates are required. This part will also be further discussed in later parts. Also, it takes only 25.97 milliseconds to run both of the training and the testing set, which is 3.09 milliseconds only for 1000 images on average. This fast runtime ensures that the model can be used in real-time applications where rapid image classification is essential.
### Rip Current Classification System Integration with Drone 
#### Setting Confidence Threshold: 
When it comes to putting the rip current classification system into practice, we focused on optimizing the confidence threshold to maximize the product of the True Positive Rate (TPR) and the True Negative Rate (TNR) in the next stage of our research. Given the initial TPR of 0.75 and TNR of 0.62, we aimed to find a threshold that would improve the overall balance and effectiveness of the model.

To determine the optimal threshold, we calculated the TPR and TNR for various confidence levels and observed the resulting TPR × TNR values. The goal was to identify the threshold at which this product was maximized, indicating the best trade-off between true positive and true negative rates.

To set a confidence threshold, we have to calculate the chances of n true positives or n true negatives occurring out of α trials, then we add up all of the chances above n. With that, we get the TPR and TNR. To maximize both, we want compare the value of TPR × TNR. We found out that by having n set at approximately 0.69 × α, we get the highest value of TPR and TNR rates. 

![placeholder](https://raw.githubusercontent.com/MickeyYQA/you7n-blog/master/img/20240630/wps_doc_6.png "img")

Table 2. TPR, TNR and TPR time TNR values at different thresholds


_Note:_ Due to space limitation, only a part of the table was shown. Full table could be freely access at [https://github.com/MickeyYQA/rip_current-appendix](https://github.com/MickeyYQA/rip_current-appendix) (accessed on June 30, 2024).


![placeholder](https://raw.githubusercontent.com/MickeyYQA/you7n-blog/master/img/20240630/chart.png "img")

Figure 6. Line chart - TPR times TNR values at different confidence threshold


From the analysis presented in the table above, the TPR and TNR values were computed for different confidence levels. The product TPR × TNR was used as the metric to evaluate the performance at each threshold. As shown in the table, the threshold of 69 trials yielded the highest TPR × TNR value of 84.78 %, with a corresponding TPR of 93.06 % and TNR of 91.10 %. This indicates that at this threshold, the model achieves a near-optimal balance between correctly identifying images with and without rip currents.

Given that TPR is 93.06% while TNR is 91.10% under the 0.69 confidence threshold, the overall performance accuracy could be calculated. By referring to the formula  \\\(Accuracy = (TPR \times \frac{P}{N + P}) + (TNR \times \frac{N}{N + P})\\\), we will get the TPR = 0.9306, TNR = 0.9110, P (Positive dataset size) = 1781, N (Negative dataset size) = 703, we finally got the overall \\\(Accuracy = 0.9306 \times 0.7169 + 0.9110 \times 0.2830 = 0.9249\\\) under the confidence threshold of 0.69.

Furthermore, after comparing the result of different α values, we decided to set it to 100, which gives us a high classification accuracy and low classification interval of around 1.3 seconds. This means that the drone could classify 100 images in 1.3 seconds to get one result with accuracy of 92.49 %. This time interval was get by testing in real situations.


![placeholder](https://raw.githubusercontent.com/MickeyYQA/you7n-blog/master/img/20240630/wps_doc_7.png "img")

Figure 7. Sketch of the process of drone integration.


#### Real-time Image Transmission: 
In our research, the model was integrated with the drone to demonstrate the flexibility and adaptability of this approach. Since this rip current classification system was integrated with the “Tello Talent Robomaster TT” drone, real-time image transmission capabilities were established using the features of the Tello drone development kit. This was achieved by communicating using UDP (User Datagram Protocol) which allowed the drone’s camera to send live videos to a computer.[^fn-5] Low latency transmissions that are essential for real time processing and classification provided by UDP. 
#### Image Preprocessing: 
By keeping the pictures of the camera from the drone at 10x10 pixels and changing them into grayscale, it was made sure that those images were accorded with the training data. Any deviation in image dimension or format could have adversely affected model’s performance during testing in real world environment where accurate predictions had to be maintained. 
#### Model Inference: 
Processed images were passed through the pre-trained ANN model for identifying any presence of rip currents. The processed images are inputted to the model during inference and it outputs predictions showing whether there is likelihood of rip current events. With this integration, therefore, live video streams could be processed thereby giving immediate feedbacks on likely occurrences of dangerous situations, rip currents. 
#### Enhancing Prediction Accuracy: 
In an effort to make more accurate predictions of rip currents, a customized method was designed and implemented. The function predicts rip current presence multiple times for every image and then combines the answers to create better concept as well as more reliable results. Precisely, each image is subjected to 100 predictions by the model. Where 69 or more of these predictions indicate that there is a rip current, the system confirms this fact. That threshold of confidence is arrived at empirically and balances between sensitivity and specificity based on initial analysis. 
#### Result Display: 
To display immediate and clear feedback in relation to the classification outcomes, we created a graphical user interface using OpenCV. If it detects a rip current (which means it has made 100 predictions where the confidence levels are higher than 82%), then this interface will pop-up a window displaying "RIP CURRENT" with a red background if a rip current is being classified, and "SAFE" with green background color in case no rip current is being classified. 

![placeholder](https://raw.githubusercontent.com/MickeyYQA/you7n-blog/master/img/20240630/wps_doc_8.png "img")

Figure 8. Rip currents classification user interface demonstration

_Note._ Image above demonstrates the GUI where the camera data received from the drone is shown, and the true prediction of that rip current exists is shown on the left, while the true prediction of that rip current does not exist is shown on the right.
Testing and Validation Methodology: The testing and validation of the integrated system involved a comprehensive evaluation against simulated and real-world scenarios. Simulated testing used pre-recorded video feeds under controlled conditions to verify the system’s functionality. Due to some limitation, testing in real beach environments is unable for us. 
## Discussion and Conclusion
### Methods Comparison
Our team utilized the database from the previous research conducted by de Silva and his colleagues. This database contains numerous pictures and videos randomly featuring rip currents and provides sufficient data to train the artificial intelligence module. The previous study, [^fn-6], utilized Convolutional Neural Networks (CNN) and Interpretable AI to classify and localize rip currents. Both of these technologies have a positive impact on the overall prediction rate. The Interpretable AI can identify the location of waves within videos and images, making predictions more intuitive.

On the other hand, the CNN module is highly reliable when subjected to deep training with the available data. It can learn complex patterns and features with high accuracy. Another study utilized the Faster R-CNN and a customized temporal aggregation stage to accurately and efficiently detect rip currents within a framed region. The previous research papers utilized these technologies and achieved remarkable results. 

However, limitations still exist in these methods. Although the CNN method and the Interpretable AI method have complementary effects, the CNN method requires a high level of hardware and a large amount of data to train the AI. This complicates the process, making it difficult to proceed quickly and efficiently, which is essential for detecting rip currents. 

On the other hand, our program utilized the Adam module which is significantly more efficient than the previous method. In addition, our programs yield remarkable accuracy in determining rip currents. The previous study by Neelesh [^fn-6] produced an accuracy of 89% when using the test videos from their datasets, while the Adam module resulted in an accuracy of 74% on testing sets and 93% after adding a confidential threshold. 

The study led by De Silva [^fn-7], on the other hand, achieved an overall accuracy of 98%. However, its method is limited by the requirement of framing during detection, which reduces the flexibility of the detection. Moreover, the method cannot be operated in real-time and is often not mobile-friendly, whereas our programs can operate on drones timely. 

Our program has several advantages compared to the current methods presented in previous studies. However, we do have a few limitations regarding our method. Our programs have not been tested in real environments such as near seashores and beaches. This could be a significant contributing factor because the cameras' field of view might be influenced by adverse environmental conditions like wind and rain. Aside from that, our drones used in study have limited altitude and restricted flight distances which are crucial for determining the size of the detection area. 
### Conclusions
Our study utilizes visual images and videos from the “Tello Talent Robomaster TT” drone for rip current detection. The data acquisition and preprocessing of the algorithm obtains 2484 images of near-shore conditions. Then, we resized them to 100 pixels and converted them to grayscale. We utilized Multi-Layer Perceptron algorithms for module training with an architecture comprising 1 hidden layer of 60 neurons and ‘Adam’ solver for training. 

Several studies have similarly utilized artificial intelligence to detect rip currents near the coastline. However, they are limited either by the need to construct a bounding box in the images or by their low accuracy. The requirement of constructing a binding box had a significant impact on the variety of the videos, consequently influencing the overall flexibility of the method. 

The method presented addresses these limitations and provides a mobile way of detecting rip currents. After carefully weighing the results of the individual tests, we achieved an accuracy of 74% on testing data and 93% after adding a confidential threshold. Although this accuracy is lower than that of the study presented by [^fn-7], it’s relatively more flexible than other previous studies. Aside from that, drones are employed to further test the viability of the method. The outcome, although not tested in real scenes, is expected to be similar to the initial accuracy based on the videos in the database.  
### Future Prospects 
Our study aims to further enhance the practicality of the method presented in the future. Drones will be employed on the nearby coastline to test the feasibility of using a drone in coastal areas and to assess environmental factors for detection. This is crucial because wind and rain could greatly impact the overall quality of the videos filmed. 

An alert mechanism could be implemented to notify beachgoers and tourists by collaborating with the local government and sending the information to the lifeguards. To make real-time rip current information and safety findings more easily accessible to users through mobile applications and web platforms, the program's interface will be enhanced to make it more user-friendly. 

Aside from that, the investigation of advanced image processing techniques to extract more informative features from the live video feed could proceed to enhance the accuracy of rip current detection. These future works could be pursued in upcoming studies.
## Data Availability Statement
The data used to train the AI-based model is freely available at [https://sites.google.com/view/ripcurrentdetection/download-data](https://sites.google.com/view/ripcurrentdetection/download-data) (accessed on June 30, 2024), provided by [^fn-7]. Additionally, all the code referenced in this paper can be freely downloaded from [https://github.com/MickeyYQA/rip_current_detection](https://github.com/MickeyYQA/rip_current_detection) (accessed on June 30, 2024).






## Reference 
[^fn-1]: US Department of Commerce, N. (n.d.). Surf Zone Fatalities in the United States in 2023: 98. Www.weather.gov. https://www.weather.gov/safety/ripcurrent-fatalities 
[^fn-2]: “Florida PanHandle.” FloridaPanhandle.com, 30 June 2023, floridapanhandle.com/blog/rip-current-statistics/#:~:text=Florida%20is%20accountable%20for%20143%20of%20the%20total,the%20Great%20Lakes%20are%20caused%20by%20rip%20currents. 
[^fn-3]: Gong, Chao, Introduction to Visual Perception. 2022. (in Chinese)
[^fn-4]: “Adam: A Method for Stochastic Optimization.” arXiv (Cornell University), Dec. 2014, https://doi.org/10.48550/arxiv.1412.6980.
[^fn-5]: “RoboMaster TT - Specifications - DJI.” DJI Official, www.dji.com/cn/robomaster-tt/specs.
[^fn-6]: Rampal, N., Shand, T., Wooler, A., & Rautenbach, C. (2021). Interpretable Deep Learning Applied to Rip Current Detection and Localization. Remote Sensing, 14(23), 6048. https://doi.org/10.3390/rs14236048
[^fn-7]: De Silva, A., Mori, I., Dusek, G., Davis, J., & Pang, A. (2021). Automated rip current detection with region based Convolutional Neural Networks. Coastal Engineering, 166, 103859. https://doi.org/10.1016/j.coastaleng.2021.103859 
[^fn-8]: “MLPClassifier.” Scikit-learn, scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html# 
[^fn-9]: Zhang, Y., Huang, W., Liu, X., Zhang, C., Xu, G., & Wang, B. (2021). Rip current hazard at coastal recreational beaches in China. Ocean & Coastal Management, 210, 105734. https://doi.org/10.1016/j.ocecoaman.2021.105734 
[^fn-10]: Salama, N.M., Iskander, M.M., El-Gindy, A.A. et al. Hydrodynamic Simulation of Rip Currents Along Al-Nakheel Beach, Alexandria, Egypt: Case Study. J. Marine. Sci. Appl. 22, 137–145 (2023). https://doi.org/10.1007/s11804-023-00320-2 
[^fn-11]: Mucerino, L., Carpi, L., Schiaffino, C. F., Pranzini, E., Sessa, E., & Ferrari, M. (2020). Rip current hazard assessment on a sandy beach in Liguria, NW Mediterranean. Natural Hazards, 105(1), 137–156. https://doi.org/10.1007/s11069-020-04299-9 
[^fn-12]: Haller, M. C., Honegger, D., & Catalan, P. A. (2014). Rip current observations via marine radar. Journal of Waterway, Port, Coastal, and Ocean Engineering, 140(2), 115-124. 
[^fn-13]: Liu, B., Yang, B., Masoud-Ansari, S., Wang, H., & Gahegan, M. (2021). Coastal image classification and pattern recognition: Tairua Beach, New Zealand. Sensors, 21(21), 7352. 
[^fn-14]: Nelko V. The prediction of rip currents. ProQuest Dissertations Publishing; 2012. 
[^fn-15]: De Silva, A., Mori, I., Dusek, G., Davis, J., & Pang, A. (2021). Automated rip current detection with region based convolutional neural networks. Coastal Engineering, 166, 103859. https://doi.org/10.1016/j.coastaleng.2021.103859
[^fn-16]: P. Dérian and R. Almar, "Wavelet-Based Optical Flow Estimation of Instant Surface Currents From Shore-Based and UAV Videos," in IEEE Transactions on Geoscience and Remote Sensing, vol. 55, no. 10, pp. 5790-5797, Oct. 2017, doi: 10.1109/TGRS.2017.2714202. 
[^fn-17]: Brander, R. W., Dominey‐Howes, D., Champion, C., Del Vecchio, O., & Brighton, B. (2013). Brief Communication: A new perspective on the Australian rip current hazard. Natural Hazards and Earth System Sciences, 13(6), 1687–1690. https://doi.org/10.5194/nhess-13-1687-2013 
[^fn-18]: comp.ai.neural-nets FAQ, Part 2 of 7: LearningSection - What Is a Softmax Activation Function? www.faqs.org/faqs/ai-faq/neural-nets/part2/section-12.html.
[^fn-19]: Zhang, Y., Huang, W., Liu, X., Zhang, C., Xu, G., & Wang, B. (2021b). Rip current hazard at coastal recreational beaches in China. Ocean & Coastal Management, 210, 105734. https://doi.org/10.1016/j.ocecoaman.2021.105734
[^fn-20]: Voulgaris, G.; Kumar, N.; Warner, J.C. Methodology for Prediction of Rip Currents Using a Three-Dimensional Numerical, Coupled, Wave Current Model; CRC Press: Boca Raton, FL, USA, 2011. 
[^fn-21]: Pitman, S. J., Thompson, K., Hart, D. E., Moran, K., Gallop, S. L., Brander, R. W., & Wooler, A. (2021). Beachgoers’ ability to identify rip currents at a beach in situ. Natural Hazards and Earth System Sciences, 21(1), 115–128. https://doi.org/10.5194/nhess-21-115-2021
[^fn-22]: Brander, R.; Dominey-Howes, D.; Champion, C.; del Vecchio, O.; Brighton, B. Brief Communication: A new perspective on the Australian rip current hazard. Nat. Hazards Earth Syst. Sci. 2013, 13, 1687–1690. 
[^fn-23]: Castelle, B., Scott, T., Brander, R., & McCarroll, R. (2016). Rip current types, circulation and hazard. Earth-science Reviews, 163, 1–21. https://doi.org/10.1016/j.earscirev.2016.09.008

