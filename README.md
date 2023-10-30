# J-peak-detection-Using-Signal-processing-and-Bi-LSTM-Deep-Learning-on-BCG-signals
*************************
# Project Summary
This project presents a novel approach for J peak detection in Heart-Rate monitoring. The proposed approach employs accelerometers to collect Ballistocardiogram (BCG) data for short-term HRV monitoring during sleep in a cohort of 14 patients. The bidirectional Long Short-Term Memory (Bi-LSTM), a deep learning model, is then used to detect J-peak.
*******************
# Problem Statement
Heart-Rate Variability (HRV) is typically measured by analyzing the changes in the R-R interval (RRI) of an electrocardiogram (ECG), where the R peak corresponds to the prominent peak in a cardiac cycle. Ballistocardiogram (BCG) is a recording of the body's mechanical movement caused by the rhythmic pumping of the heart as blood is ejected into the major blood vessels. The BCG signals can be obtained using simple pressure or vibration sensors. The J peak in BCG signal aligns with the R peak in ECG and offers a promising alternative for long-term monitoring in the comfort of one's home. Detecting individual heartbeats from BCG signals is more challenging compared to ECG due to the complex nature of multiple waves present in BCG. Deep learning has emerged as an effective approach for extracting heartbeats from BCG signals. The aim of this work is to provide and implement an effective solution for HRV estimation, demonstrated by using signal processing techniques for accurate and steady BCG signal detection
*************************
# I.  DataSet Acquisition and Decription
These BCG signals are collected using ADXL 357 biaxial 3-axis (x-y-z) accelerometer recorded alongside ECG measurements at 256Hz using 12-bit analog-to-digital conversion. The sample frequency is essential for precise HR signal computation. Accelerometer readings were first recorded using biaxial accelerometer, then transferred over SPI protocol into a Raspberry Pi 4B computing device. The data encompassed 14 subjects adopting various body postures (supine, left lateral, and right lateral) during 3-minute  intervals, generating corresponding BCG data. 
*************************
# II. Methods
The methodology will be implemented with Python Programming Languages. Also, R is well built to perform a huge variety of run arithmetic calculations and scientific implementation of machine learning algorithms. Methods used for this projects are outlined below
1.  BCG Data Preprocessing: This invoves using 3rd order butterworth bandpass at low and high cutoff (0.5 and 10hz) to filter noise, body movements, and motion artifacts. Next, application of a median filter to mitigate baseline drift and a cross-correlation function to enhance the visibility of signal peaks.
2.  BCG Data Decomposition: This stage involves application of two signal processing techniques — DWT (Discrete Wavelet Transform) and EEMD (Emperical Ensemble Mode Decomposition) to extract respiratory and cardiac signals. For this project, a DWT decomposition at J = 8 levels is used. While the biorthogonal (bior3.9) wavelet was used as wavelet basis function with detail coefficient 3 and 4 derived as cardiac signal. For EEMD decomposition, decomposition was set to 13 IMFs (Intrisic Mode Functions) with IMF4, IMF5, IMF6 derived as cardiac signals
3.  J-peak detection: To implement this J-peak detection method, a  local peak detector proposed by Azzini et al. is employed to analyze a derivative computed at each datapoint in the DWT and EEMD cardiac signal. This peak detector specifically identifies local peaks in a subject’s cardiac signal which satisfy the threshold of the optimum minimum peak distance between 500 - 600ms.
4.  Data Labelling: A Signal of Interest (SOI) is introduced as a method for data labeling. This is bevause there is no direct one-to-one correspondence/synchronization between the R-peak and the J-peak in the time domain and This will present challenges when making J-peak prediction. The sampling points within the range of -0.20s to +0.20s surrounding the reference R-peaks are labelled as J-peak. Since reference ECG has been observed to have an average variation of -0.07s to +0.07s, a label of ‘0’ is set as R-peak, while width exceeding the average variation in ECG is labelled as Jpeak (label = 1)
5.  Feature Extraction: In modelling the Bi-LSTM network, the forward and backward features of a moving 3s sliding period the BCG signals are fed as input data into the model. The forward and backward features are extracted from a signal length of 13s and 30s using the DWT and EEMD cardiac signal respectively.
6.  Target Definition: The target data is the variation (seconds) between consecutive JJI within the entire duration of the cardiac signal. The target data is stored as two binary classes; ‘1’ and ‘0’. The label '0' is assigned when the variation between consecutive JJI falls within the width of the SOI
7.  Bi-LSTM model architecture:  The Bi-LSTM architecture employs a single layer of Bi-LSTM units, each housing 128 neurons. The ReLU activation function is added to the network to introduce non-linearity into the model. A learning rate of 0.0003 facilitates efficient convergence and accurate prediction 
8.  Train and Testing: To train the Bi-LSTM model, 10 kfold cross-validation strategy is adopted because it provides a balance between the bias of a single train-test split (which might not be representative of the entire dataset) and the high computational cost of leave-one-out cross-validation (where each data point is treated as a test set). The Bi-LSTM model is trained with 70% of the labelled DWT and EEMD datasets associated with subjects 1-7. The efficiency of the trained model is evaluated using the 30% validation data of subject 1-7. Thereafter, the model's predictive capabilities are tested on subject 8, 9 and 10 datasets. The epoch pass is set to 150.
***********
# III. Analysis and Results
A. HRV Time Domain Analysis: 

In general, the bpm estimates using DWT technique show a good degree of consistency across the 3 different sleeping positions for each subject. In addition, the bpm values tend to follow a similar increasing or decreasing trend as the ground truth ECG- average bpm values.  On the other hand The bpm estimates with EEMD are sensitive to sleeping positions. Several subjects show variations in bpm when transitioning between sleeping positions. Although the DWT values also exhibit variability, similar to EEMD, however, DWT tends to exhibit greater consistency and agreement with reference ECG bpm across subjects and positions

![image](https://github.com/oawonuga92/J-peak-detection-Using-Signal-processing-and-Deep-Learning-on-BCG-signals/assets/61459286/7c04b731-19d4-4ab5-b66a-e8e3200bb327)

#
B. Coverage Analysis

Coverage is a crucial metric for evaluating the precision of the DWT and EEMD methods in deriving JJI. It offers insights into how well the alignment between derived intervals and ground truth data fit. 
DWT tends to have higher CR for subjects 1, 4, and 8, while EEMD performs slightly better than DWT for subject 10. Generally, DWT shows higher CR for most subjects in this analysis, suggesting it may be a more robust choice across different subjects with different heart conditions

![image](https://github.com/oawonuga92/J-peak-detection-Using-Signal-processing-and-Deep-Learning-on-BCG-signals/assets/61459286/6f95e8af-a254-438d-88fc-799f64a88c44)


