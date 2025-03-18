# Dynamic Time Warpping

Dynamic Time Warping (DTW) is a method used to measure the similarity or distance between two time series or sequences of varying lengths. Although its name may suggest advanced or futuristic applications, its core purpose is grounded in time-series analysis.

Let's understand this topic further.

**Table of Contents**
- [Difference between Dynamic Time Warping distance and Euclidean distance](#difference-between-dynamic-time-warping-distance-and-euclidean-distance)
- [Understanding the concept](#understanding-the-concept)
- [Applications of DTW](#applications-of-dtw)
- [DTW Algorithm](#DTW-algorithm)
- [Use Cases](#Use-Cases)

### Difference between Dynamic Time Warping distance and Euclidean distance
<hr>

The key distinction between Dynamic Time Warping distance and Euclidean distance lies in their approach to comparison. DTW facilitates many-to-one comparisons, allowing for the optimal alignment of time sequences by accommodating temporal distortions. In contrast, Euclidean distance performs a one-to-one point comparison, making it less suitable for time-series data with varying lengths or misalignments. This flexibility in handling temporal variations makes DTW particularly effective for comparing sequences of different lengths. Originally developed for automatic speech recognition, DTW remains widely used in areas involving sequential data analysis.
![image](https://github.com/user-attachments/assets/d9ed9f98-f3ef-4ef0-a90e-c919aa4bceb0)


### Understanding the concept
<hr>

**Dynamic Time Warping** is an algorithm used for measuring the similarity between two temporal time series sequences. They can have variable speeds. It computes the distance from the matching similar elements between two series. It is used in dynamic programming to find the optimal path.

### Applications of DTW
<hr>

Dynamic Time Warping has a wide range of applications across multiple domains, including speech processing, bioinformatics, finance, and computer vision. Below are some key applications with real-world examples:

1. Speech and Audio Processing
DTW is extensively used in speech recognition and speaker verification. The algorithm enables robust matching of spoken words despite variations in speech speed or pronunciation.
  - Example: In automatic speech recognition (ASR) systems, DTW is used to align an input speech signal with a reference template, ensuring that words spoken at different speeds (e.g., “hellllooooooo” vs. “hello”) are correctly identified.
  - Example: In music information retrieval, DTW helps compare different versions of the same song, even if they are played at different tempos.

2. Handwriting and Signature Verification
DTW is applied in biometric authentication systems to verify handwritten signatures and detect forgery attempts.
  - Example: Banks and financial institutions use DTW in signature verification systems to compare a new signature against a stored reference, ensuring consistency despite minor variations in writing speed or pressure.

3. Gait Analysis and Motion Recognition
In healthcare and biomechanics, DTW is employed to analyze human movement patterns, such as gait cycles, for diagnosing medical conditions or monitoring rehabilitation progress.
  - Example: In Parkinson’s disease research, DTW is used to compare the gait patterns of patients with those of healthy   individuals, helping in early diagnosis and treatment monitoring.
  - Example: In sports science, DTW assists in comparing an athlete’s running style to an optimal reference model, facilitating performance optimization and injury prevention.

4. Financial Time-Series Analysis
DTW is widely used in finance to compare stock market trends, detect anomalies, and identify similar movement patterns in financial data.
  - Example: In algorithmic trading, DTW helps recognize similar stock price movements from historical data, enabling predictive modeling and risk assessment.

5. Gesture and Activity Recognition
In human-computer interaction and robotics, DTW is utilized for recognizing gestures and human activities from motion sensor data.
  - Example: In smart home systems, DTW enables gesture-based control of devices, such as turning on lights with a specific hand motion.
  - Example: In gaming and virtual reality (VR), DTW helps track player movements and match them to predefined actions for accurate gameplay interaction.
  
6. Medical and Biological Signal Processing
DTW is crucial in analyzing physiological signals such as electrocardiograms (ECGs), electromyograms (EMGs), and electroencephalograms (EEGs).
  - Example: In cardiology, DTW is used to compare ECG waveforms to detect abnormalities such as arrhythmias or ischemic events.
  - Example: In neuroscience, DTW helps analyze brainwave patterns for diagnosing neurological disorders like epilepsy.

7. Shape and Image Matching
DTW is also applied in computer vision for comparing shapes, handwriting, and object recognition.
  - Example: In optical character recognition (OCR), DTW aids in recognizing handwritten or cursive text by aligning distorted characters with reference templates.
  - Example: In medical imaging, DTW is used to compare tumor growth patterns in sequential scans, aiding in disease progression analysis.

By adapting to variations in time-dependent data, DTW remains a fundamental tool in pattern recognition, classification, and predictive modeling across diverse industries.

### DTW Algorithm
<hr>

This is what it looks like when you start doing it on paper.

![image](https://github.com/user-attachments/assets/e827c285-1cf1-4ed5-abe1-af32d34382de)

Let's say you have 2-time series. TS-A and TS-B.
TS-A = [1, 3, 4, 9, 8, 2, 1, 5, 7, 3]
TS-B = [1, 6, 2, 3, 0, 9, 4, 3, 6, 3]

![image](https://github.com/user-attachments/assets/ad35968e-af05-48db-a109-47b569286445)

Now they can be plotted like this:

![image](https://github.com/user-attachments/assets/2df7597e-0692-40ac-a14f-9c094393d32a)

Where a new cell is calculated from the computation of previous computations.

$$DTW_{q(x,x')} = min(\sum_{(i,j)\in \pi}^{}D(x_{i}, x'_{i})^q)^\frac{1}{q}$$

K is a set of indexed pairs of x and x'. π is a set of sequences of indexes of K. or, a more simple form of this equation can be written as:

$$DTW_{q(x,x')} = |A_{i}-B_{j}| + min(D[i-1,j-1], D[i-1,j], D[i,j-1])$$

Where A and B represent TS-A and TS-B respectively. i and j belong to π. 
For cell (0,0), the formula gives, D(0,0) = |1–1|. Hence (0,0) is 0. Similarly for cell (4,4), the formula gives, D(4,4) = |9–3| + min(D(3,3), D(3,4), D(4,3)). Therefore, D(4,4) = 11, and soo on for other cells.

Once the distance matrix 𝐷 has been constructed, the next step is to determine the optimal alignment path by backtracking through the matrix. This path represents the minimal cumulative cost required to align the two time-series sequences.
The backtracking process begins at the final cell 𝐷(𝑁,𝑀), where 𝑁 and 𝑀 are the lengths of the two sequences being compared. At each step, the path moves in the direction of the minimum cost among the three possible adjacent cells:
  - Diagonal Move: 𝐷(𝑖−1,𝑗−1) (Matching both sequences)
  - Left Move: 𝐷(𝑖,𝑗−1) (Inserting an element from the second sequence)
  - Upward Move: 𝐷(𝑖−1,𝑗) (Inserting an element from the first sequence)

where arg min selects the index corresponding to the minimum value among the three candidates.

For example, if the algorithm starts at 𝐷(10,10)=15, it checks the three adjacent cells and selects the one with the lowest cost. This process is repeated iteratively until the starting cell 𝐷(1,1) is reached.

The resulting path provides the optimal alignment between the two sequences, ensuring the lowest cumulative cost for their warping. This approach is fundamental in applications such as speech recognition, time-series classification, and gesture recognition.

### Use Cases
<hr>

Before delving into the detailed mechanics of DTW, it is essential to understand its significance and real-world applications. DTW serves as a fundamental tool in various domains where time-series alignment is critical. Some notable applications include:
- **Sound Pattern Recognition**: DTW is widely employed in speech processing and audio analysis to compare and classify sound patterns, enabling applications such as speech recognition, speaker identification, and music similarity detection.
- **Stock Market Prediction**: In financial analysis, DTW is utilized to identify similar trends in stock price movements, facilitating predictive modeling and market trend forecasting.
- **Correlation Power Analysis (CPA)**: DTW plays a crucial role in side-channel attack methodologies, where it aids in aligning and analyzing power consumption traces to extract cryptographic keys in embedded systems.
- **Finance and Econometrics**: DTW is applied in economic data analysis to measure similarities between financial indicators, detect anomalies, and compare time-series data from different economic periods.
These applications demonstrate the versatility of DTW in handling time-dependent data across diverse industries.

And that is it! I hope this was useful and knowledgeable for you and hopefully it makes you understanding about how certain functions and operations are done clearer.

Author:\
Sarthak Bhan\
JTP Co., Ltd.
