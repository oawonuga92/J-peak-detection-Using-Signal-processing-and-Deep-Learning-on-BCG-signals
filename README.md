# J-peak-detection-Using-Signal-processing-and-Deep-Learning-on-BCG-signals
*************************
# Project Summary
This project presents a novel approach for J peak detection in Heart-Rate monitoring. The proposed approach employs accelerometers to collect Ballistocardiogram (BCG) data for short-term HRV monitoring during sleep in a cohort of 14 patients. The bidirectional Long Short-Term Memory (Bi-LSTM), a deep learning model, is then used to detect J-peak.
*************************
# I DataSet Acquisition and Decription
These BCG signals are collected using ADXL 357 biaxial 3-axis (x-y-z) accelerometer recorded alongside ECG measurements at 256Hz using 12-bit analog-to-digital conversion. The sample frequency is essential for precise HR signal computation. Accelerometer readings were first recorded using biaxial accelerometer, then transferred over SPI protocol into a Raspberry Pi 4B computing device. The data encompassed 14 subjects adopting various body postures (supine, left lateral, and right lateral) during 3-minute  intervals, generating corresponding BCG data.
