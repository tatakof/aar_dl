other stuff: 
- speed of inference vs other methods like bayesian methods? citate sofis paper. 
- how did we eval the results of the model?
- trained model was stored and used for classification of X Tbs of data
- add data prep details used to avoid data leakage (cite reproducibility crisis paper)
- sequence of segments creation details? that was kind of complicated. note we could have done only segments and just use a CNN, but the transformer incorporated time series aspect of the data (or time dimension? or time dynamics?)


# Methodology

## Data

Raw accelerometer and magnetometer data from livestock monitoring sensors were collected at 40 Hz sampling frequency, capturing tri-axial accelerometer measurements (acc_x, acc_y, acc_z) and tri-axial magnetometer measurements (mag_x, mag_y, mag_z). The dataset was collected from multiple sheep over several months in 2019, with each animal tagged with an individual identifier. Ground truth behavioral annotations were provided by trained observers, classifying animal activities into fine-grained categories including resting, vigilance, eating, walking, fast walking, and foraging.

The raw behavioral labels were consolidated into three primary behavioral classes: (1) Inactive (comprising resting and vigilance behaviors), (2) Walking (combining walk and fast walk activities), and (3) Foraging (including eating and search behaviors). This consolidation reduces classification complexity while maintaining biologically meaningful behavioral distinctions relevant for livestock monitoring applications.

Data preprocessing involved several steps to ensure data quality and consistency. Outliers in pitch and roll angle measurements were filtered using the interquartile range (IQR) method, removing values beyond 1.5 × IQR from the first and third quartiles. Angular measurements were converted from degrees to radians for mathematical consistency. Missing values in behavioral annotations were handled by either interpolation or removal, depending on the extent of missing data.

The preprocessed continuous time series data was segmented into fixed-length temporal segments to create training samples suitable for deep learning. Each segment contained a fixed number of consecutive sensor readings at 40 Hz, with segment sizes ranging from 32 to 256 data points (corresponding to 0.8 to 6.4 seconds of data). To ensure behavioral consistency within segments, a behavior threshold was applied requiring at least 51% of observations within each segment to belong to the same behavioral class.

Sequences of consecutive segments were then constructed, with sequence lengths varying from 5 to 20 segments. This sequential structure allows the model to learn temporal dependencies across multiple behavioral periods. Only sequences where all constituent segments met the behavioral threshold criteria were retained for training, ensuring high-quality labeled data.

## Model Architecture

The proposed model combines Convolutional Neural Networks (CNN) for local feature extraction with Transformer attention mechanisms for modeling long-range temporal dependencies. This hybrid architecture leverages the strengths of both approaches: CNNs excel at capturing local patterns in sensor data, while Transformers can model complex temporal relationships across extended sequences.

The input to the model consists of sequences with shape (sequence_length × segment_size, n_features), where sequences are flattened into continuous time series spanning multiple behavioral periods. For a typical configuration, this results in input dimensions of (1280, 5) for sequences of 10 segments with 128 data points each and 5 sensor features (3 accelerometer axes plus pitch and roll angles derived from magnetometer data).

The CNN component begins with a 1D convolutional layer using 64 filters with kernel size 3, followed by max pooling with stride 2. This is followed by seven additional convolutional layers, each with 64 filters and kernel size 3, interspersed with max pooling operations. The pooling stride is adaptive based on segment size to ensure consistent dimensionality reduction across different temporal resolutions. Dropout layers with configurable rates are included after each pooling operation to prevent overfitting.

The Transformer component implements self-attention mechanisms to capture temporal dependencies in the CNN-extracted features. A multi-head attention layer with 2 attention heads and key dimension of 2 computes attention weights across the temporal sequence. Residual connections combine the attention output with the original CNN features, followed by layer normalization. A position-wise feed-forward network, implemented as two 1D convolutional layers with 128 and 64 filters respectively, provides additional feature transformation. Another residual connection and layer normalization complete the Transformer block.

The output layers consist of time-distributed dense layers that produce predictions for each temporal position in the sequence. The first dense layer has sequence_length units with ReLU activation, followed by a final dense layer with n_classes units and softmax activation for multi-class probability output. This architecture produces predictions with shape (sequence_length, n_classes), providing behavior classification for each segment in the input sequence.

## Training

The model was trained using categorical crossentropy loss with the Adam optimizer. Given the inherent class imbalance in behavioral data (animals spend varying amounts of time in different activities), class weights were computed using scikit-learn's balanced weighting strategy. These weights inversely proportional to class frequencies ensure that minority behavioral classes receive appropriate attention during training.

Training data was split 80/20 for training and validation using temporal stratification to maintain chronological order within individual animal sequences. This prevents data leakage while preserving the temporal structure essential for behavior recognition. The model was trained for up to 300 epochs with early stopping monitoring validation loss to prevent overfitting, with patience set to 50 epochs.

Hyperparameter configuration included a learning rate of 1×10⁻⁶ for the Adam optimizer, batch size of 32 sequences, and dropout rates between 0 and 0.05 depending on model variant. Multiple model configurations were evaluated with different segment sizes (32, 64, 128, 256 data points) and sequence lengths (5, 10, 15, 20 segments) to determine optimal temporal resolution for behavior recognition.

The training procedure incorporated data augmentation through overlapping window extraction during segment creation. Windows were moved by fractions of the segment size rather than full segments, increasing the diversity of training samples while maintaining temporal coherence. This approach helps the model generalize across different temporal alignments of behavioral patterns.

Model performance was evaluated using accuracy metrics and confusion matrices on the validation set. Training and validation loss curves were monitored to detect overfitting, with the best model selected based on validation accuracy before performance degradation occurred. The final model achieved validation accuracies exceeding 95% on the three-class behavioral classification task, demonstrating effective learning of temporal behavioral patterns from multimodal sensor data.