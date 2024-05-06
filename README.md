# Video Reconstruction from Randomized Video Frames

## Introduction
This project is inspired by Ned Danyliw's paper on [Video Reconstruction from Randomized Video Frames](https://web.stanford.edu/class/ee368/Project_Autumn_1617/Reports/report_danyliw.pdf).

The focus of this project is to reorder the randomized video frame sequences with a comprehensive exploration of diverse methods at each procedural step.

I made this project with my teammates Dinesh, Rahulrajan and Vaishnavi.

## Dependencies
Python, OpenCV, Numpy, Matplotlib, Random, and Scipy.

## Proposed Solution
Our proposed solution utilizes preprocessing, feature matching and sorting algorithms.

![](photos/video%20reconstruction.png)

In the initial preprocessing stage, we employ downsampling and greyscaling on the original video and streamlining the subsequent operations. Preprocessing allows us to remove the small changes in the video and also reduces computational load. The video is then shuffled and features are extracted using two feature detectors: ORB and SIFT Feature detectors. We compared and evaluated these feature detectors: SIFT for its capacity to maintain consistency in detecting key points and ORB for its efficiency.

After the features of each frame have been extracted, they are sent to a distance estimation algorithm that calculates
various distance metrics between the matched features such as: L2 norm and L1 norm.
Later a cost matrix is generated for each permutation of the frame sequence as there is a “cost” for transforming a
given image to another image. Similar image frames have a lower cost whereas different image frames have a higher
cost. We generated the cost matrix with the number of matches, L1 and L2 distance between matched pairs.
After generating the cost matrix, we proceed with different sorting algorithms. We evaluated three sorting
algorithms: Growing approach, Hierarchical clustering and Travelling Salesman solution. In the Growing approach,
we start with a random frame and find the nearest neighbor frame based on cost. This frame is appended to the
sequence depending on its proximity to the start or end. The process repeats until all frames are sorted. Hierarchical
Clustering is a method where each frame is initially treated as a separate cluster. The algorithm then progressively
merges these clusters based on similarity measures, often derived from a predefined cost matrix. This approach
organizes the video frames into a hierarchy, from individual frames to increasingly larger clusters, until a single,
sorted sequence is achieved.

On close observation, we can map our original problem of sorting a video, to a variant of the Travelling Salesman
Problem. Using the Travelling Salesman Problem (TSP) approach to sort frames in video sorting involves modeling
the sequence of frames as a fully connected graph. Each frame represents a node, and the edges between them are
weighted according to the cost of transitioning from one frame to another, as defined in a cost matrix. The goal is to
find the shortest path that visits each frame once, creating an optimal sequence. This approach, especially when
employing a greedy algorithm, efficiently organizes frames in a manner that minimizes the overall transition cost,
thus ensuring a smooth and logical progression of the video content. Unlike the traditional TSP, this variant does not
require returning to the starting frame, making it more suitable for linear video sequences.

## Performance Evaluation
To measure the performance of the algorithm, the paper recommends a Logarithmic Error Function as below:

We propose a new Sequential error function. This error function rewards longer continuous sequences within the sorted array. It is less sensitive to the overall order and more to the length of sequentially ordered sub-sequences. This approach is particularly useful when the relative ordering of the elements is more important than their absolute positioning.

## Observation
1. After shuffling, we're unaware about the flow of time.
2. The algorithm does not perform well when there are scene jumps due to abrupt changes in the feature matches.
3. The shuffling order can determine the output of the reconstructed video. If the first few frames and the last few frames are kept constant, the qualitative performance improves significantly.
4. Complex brightness changes can affect the feature matches generated.

## Conclusion
The randomized video frames were accurately reordered in short bursts using preprocessing, feature-based matching and finally sorting the frames according to the number of feature matches. On evaluation, we can come to the conclusion that the TSP solution gives the best result. Hence, we were able to provide a better alternative working algorithm to reconstruct the randomized sequences and a better performance metric to measure the error rate of the algorithm.
