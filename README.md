# Accelerometer-Action-Dectection

## Overview

This project uses data from accelerometers and gyroscopes to correctly detect what activity 
In nearly every smartphone or smart watch, you can find either an accelerometer or a gyroscope that is used to collect health data. They are how a Fitbit or Apple’s health app track your steps and other met.

## Business Understanding
This project addresses the growing use of electronic means of tracking exercise.

## Data Understanding

The data is from a the WISDM (Wireless Sensor Data Mining) Lab at Fordham University and is collected from accelerometers and gyroscope sensors from a smartphones and a smartwatch equipped with both. There were 51 participants, which each participant performing 18 different activities for roughly 3 minutes each for a total of 54 minutes of data per participant. Each participant’s data was stored in .txt files, one for each sensor per device. Each device captures a numeric reading in the X, Y, and Z planes. Accelerometers measure acceleration, while gyroscopes measure angular velocity.

![Axes-Sensitivity-Orientation](https://user-images.githubusercontent.com/110851861/205399223-45b1cd6f-6cd8-4a0f-9d16-6017df8c188f.jpg)


Activities included both hand-oriented activities (typing) and non-hand-oriented activities (jogging ) from a diverse selection of actions. The accelerometers and gyroscopes were programmed to sample at a rate of 20Hz (20 readings per second), but upon looking at the data, there is much inconsistency in the individual sampling rates among the four sensors.

For instance, for a particular participant, there would be variation in the amount of individual data points among the samples from the same device (smartphone, smartwatch) and across decides (gyroscope in the smartwatch vs gyroscope in the smartphone). Additionally, the timestamps did not align between the phone and the watch data, although the experiment documentation clearly states that the data was collected synchronously between the two. Additionally, the time stamps were incorrectly encoded in Unix time, as conversion to DateTime format would have indicated them occurring in different years.

To address these inconsistencies, I looked at the Unix timestamps at which number indicated one second by referring to the 20Hz frequency rate, and averaged to a fraction of a second across each of the sensors. This method standardized the length of each participant’s data so that across each sensor (e.g. phone accelerometer and gyroscope), there were the same amount of data points, while between devices (phone vs watch) there were roughly the same amount of entries. 

At this point, as the project used two different model architectures, data preparation was split.

### Time Series Classification Model
The aim of this model is to predict which activity a time series demonstrates, one time series per action. Each participant’s data was subdivided into a separate time series for each distinct activity they performed, and then these were zero padded to the length of the max sequence, leading to a total of 907 unique sequences.


### State Prediction Model
The aim of this model was to predict which activity someone is doing at each point in time along a series. The key difference is that activities change within an individual time series entry.
After standardizing the length among each participant with zero padding, I divided each participant’s data into 9 equal length sequences, leading to a total of 459 individual sequences.

For both models, activity labels were One Hot Encoded, though the first model has one matrix for each sequence, while the second model has a matrix for each timestamp.

## Modeling
LSTM (Long Short Term Memory) models were used for both tasks, in slightly different forms. 


## Further Work
Look into resampling data in a different way

