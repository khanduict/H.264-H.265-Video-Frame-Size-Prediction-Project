# H.264-H.265-Video-frame-size-prediction-project
As video has become the dominant type of traffic over wired and wireless networks, the efficient transmission of video streams is of paramount importance. Hence, especially for wireless networks, the optimum utilisation of the available bandwidth while preserving the users’ Quality of Service and Quality of Experience requirements is crucial. Towards this goal, the accurate prediction of upcoming video frame sizes can play a significant role. This work focuses on achieving such an accurate prediction for videos encoded with H.264 and H.265, which are the major state-of-the-art standards based on their current market share. Unlike previous studies, we use single-step and multi-step approaches to capture the long-range dependence and short-range dependence properties of variable bit rate video traces through neural networks-based modeling. We evaluate the accuracy of Long Short Term Memory, Convolutional Neural Networks and Sequence-to-Sequence models and compare them with existing approaches. Our models show significantly higher accuracy for a variety of videos. 
# Contribution
•	We analyze the ACF properties of the traces for various quantization levels to understand the interdependencies of frames within the VBR video sequences.
•	We propose and evaluate the use of Long Short Term Memory (LSTM), Convolutional Neural Networks (CNN) and Sequence-to-Sequence (seq2seq) models to exploit the capabilities of such models to capture long- range dependence and non-linearity. The accuracy of the proposed models is compared with several well-known approaches from the literature in order to select the most accurate model (i.e., the one with the lowest prediction error rate). Our models are shown to excel in the comparison.
•	We also focus specifically on B frame size prediction and discuss the possibility of selective dropping of B frames when necessary to reduce bandwidth requirements and smooth the encoded video stream, in cases when the model predicts larger upcoming B frame traffic than the network can handle.

# Dataset
In this study we focus on videos encoded with H.264 and H.265 high efficiency video coding (HEVC) compression standards. The video traces corresponding to all H.264  and H.265 video traffic and quality characteristics presented in this section are available from the video trace library http://trace.eas.asu.edu/h264/ and http://trace.eas.asu.edu/videotraces2/h265/ to facilitate their usage in communication and networking studies.  H.264 is currently one of the most used video coding standards as it covers a huge range of applications like video conferencing, mobile services, and HD video storage. In addition, H.265 aim to achieve the same video quality at lower bit rates. The two basic structural features that H.264 and H.265 encoders share in common are the three types of frames (I, P B), the periodic structure in which video frames are encoded and the pattern in which these frames are generated. The periodic structure is called a Group of Pictures (GoP). Each video frame is either intra-coded (I-frame), forward predictive coded (P-frame) with motion compensated prediction form the preceding I or P frame, or bi-directionally predictive coded (B-frame) according to its position in a group of pictures (GoP) structure.

Figure 1 shows an encoded sequence IBBBPBBBPBBBPBBBI of the GoP comprising of 16 frames (G16B3) with 1 I-frame, 3 P-frames and 12 B-frames. The first frame is encoded as an I-frame. Frames B-1, B-2 and B-3 are B-frames and cannot be encoded right away because they are waiting for the future P-frame. Therefore, they are put in the buffer. When frame P-4 arrives at the encoder, it is inter-encoded (unidirectional) with the first frame (I-frame) as a reference. Now that the P-frame has been encoded, the frames B-1, B-2 and B-3 which are bidirectional need references in the past and in the future, can be encoded. Thus, B-frames are constructed based on the reference frames namely, the P and I-frames.
<img width="417" alt="image" src="https://user-images.githubusercontent.com/38637722/179394532-63ab150e-1d8b-403a-8597-d8d7ae87e605.png">

B-frames occupy the largest number of frames in H.264 group of pictures (GoP) as shown in Figure 2. In addition, the trace of B-frames is observed to be burstier than other frames which implies that the bandwidth allocation of B-frames is more challenging than the other frame types [3]. On the other hand, B-frames are considered least important as compared to I-frame and P-frames and dropping the same will not have much impact on the performance as the frames transmitted following a B-frame is not dependent on that frame. In addition, B-frames contain only temporal information and their loss can only cause motion artifacts which may be difficult to notice unless the loss rates are high.
<img width="293" alt="image" src="https://user-images.githubusercontent.com/38637722/179394869-cc44d6f6-fe8e-4e02-9f78-5ac39fe2e327.png">

Figure 3 illustrates the delay analysis and transmitted sequence of classical G16B3 frame with IBBBPBBBPBBBPBBBI GoP pattern. The decoder needs both the preceding I and P-frame and the succeeding P and I-frame for decoding a B-frame. The encoder emits the frames in the order IPBBBPBBBPBBBI and, the display sequence is displayed in the order IBBBPBBBPBBBPBBB.
<img width="429" alt="image" src="https://user-images.githubusercontent.com/38637722/179394884-2997039e-dfe0-45c5-ac47-59be4721acc8.png">

We worked with 12 different H.264 AVC video traces. All H.264 videos have the same periodic structure in which video frames are encoded. This periodic structure is called a Group of Pictures (GoP) and is defined as GaBz, where a is the total number of frames in the GoP and z is the number of consecutive B frames in each GoP. More specifically, we used the video traces of “Tokyo Olympics”, “Silence of the Lambs”, “NBC-news” and “Star Wars IV” each of them in the G16B3 types of GOP (periodically repeated structure of 16 frames as follows: IBBBPBBBPBBBPBBBIBBBPBBBPBBBPBBBI) and each of them in Low Quality (LQ), with QP = 48, Medium Quality (MQ) with QP = 38 and High Quality (HQ) with QP = 28. QP stands for Quantization Parameter, with lower QP values depicting a finer quantization and higher QP values depicting a rougher quantization. 
Similarly, for H.265, we worked with 12 different single layer H.265 video traces. We used the video traces of “Big Buck Bunny”, “Elephants Dream”, “Blue Planet”, “Tears of Steel”, each of them in Low Quality (LQ), with QP = 40, Medium Quality (MQ) with QP = 25 and High Quality (HQ) with QP = 10. All the traces use the compression format G24B7. The words “trace” and “movie” are used interchangeably throughout the rest of the paper. 

# Video traffic characteristics
Due to the continuous and ordered nature of time series data, there is often some degree of correlation between the series observations. Therefore, past observations may contain predictive information about future events, which could be utilized to forecast future observations. In this section, we focus on identifying such relationships in the video traces of our study, with the use of correlation analysis.
<img width="315" alt="image" src="https://user-images.githubusercontent.com/38637722/179399238-c2591886-dae3-414f-833c-211f2888533d.png">
As shown indicatively in the above Figure, for the H.264 NBC News trace, the autocorrelation values were very close to 1 for the first lags, indicating a strong Short Range Dependence, however the autocorrelation of B frames quickly decreased, indicating an absence of Long Range Dependence. 

<img width="316" alt="image" src="https://user-images.githubusercontent.com/38637722/179399248-f0be8b01-c560-4f44-ae28-cdadb24f1f05.png">
As for the H.265 encoded video traces, the curves of the autocorrelation between B frames show a different behavior. The strongest correlations are observed for the vast majority of the H.265 video traces for lag=7 and lag=14, as shown indicatively for one trace in Figure 2. The high autocorrelations for lags that are multiples of 7 can be explained by the GoP structure (G24B7) of the traces. Also, the  autocorrelation of B frames for movies encoded in LQ is typically weaker than the autocorrelation of the movies encoded in MQ and HQ, for both H.264 and H.265 traces.

# Proposed Model
<img width="312" alt="image" src="https://user-images.githubusercontent.com/38637722/179399350-09acab2b-2988-494f-90be-70def39f193c.png">
The proposed model architecture for predicting the frame size. We present only the LSTM and seq2seq models in the figure (the approach is similar for the other models that we have used in this study). The input of our model consists of frame size, frame type and arrival time. Based on the encoding sequence of a GoP structure, the frame size arrives at an interval of every 33 milliseconds. Feature engineering is carried out by filling the missing values, rescaling the data into similar scale using MinMax scaler. Then we have used a label encoding, as explained above. We then partitioned the data into a training (80%) and test (20%) dataset and built the predictive models for each trace separately. Finally, the whole GoP structure regardless of the frame type is fitted to the model to predict the frame size. 

# Results
<img width="361" alt="image" src="https://user-images.githubusercontent.com/38637722/179399587-a6fc8efc-cfe5-4286-b76f-47124e91d9b4.png">
<img width="361" alt="image" src="https://user-images.githubusercontent.com/38637722/179399610-17ba3a3d-6ce9-4be8-a961-7687d44fe005.png">

# Conclusion
We have focused on predicting video frame sizes for H.264 and H.265 video traces based on neural network models. We have proposed and implemented Long Short Term Memory (LSTM), Convolutional Neural Networks (CNN) and Sequence-to-Sequence (seq2seq) models to exploit their capability to capture long- range dependence and non-linearity. Our models were shown to significantly outperform other models from the literature. The seq2seq model provided the best results overall. We also used a case study on traffic policing to show how an efficient model as the one proposed in this work can significantly help in network administration.
We also focused specifically on B frame size prediction, because of the possibility that B frames offer to be selectively dropped, thus reducing bandwidth requirements and smoothing the encoded video stream, when the network is congested. We compared the accuracy of the neural network models again by comparing against three efficient approaches from the literature and showing that our models excel.














