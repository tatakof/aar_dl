# Improvement Suggestions for Methodology Section

## 1. Data Section Enhancements

### 1.1 Data Leakage Prevention Details
**Current Issue**: The data splitting section lacks sufficient detail about preventing data leakage.
**Improvement**: Add a dedicated subsection explaining:
- Temporal stratification ensures no future data is used to predict past behaviors
- Individual animal separation during train/validation splits to prevent cross-contamination
- Specific protocols for handling overlapping sequences from the same time periods

### 1.2 Sequence of Segments Creation
**Current Issue**: The complex sequence creation process is under-explained.
**Improvement**: Expand explanation of:
- Why sequences of segments are necessary vs. single segments with CNN only
- How the transformer component specifically leverages temporal dynamics across segments
- The sliding window approach with configurable overlap (fraction vs. segment vs. row movement)
- Trade-offs between computational efficiency and temporal resolution

### 1.3 Dataset Scale and Characteristics
**Current Issue**: Missing quantitative details about dataset size and characteristics.
**Improvement**: Add:
- Total number of animals, recording hours, and data volume (leave this as an input section with <requires_input>) tag, and my colleague will fill it later. 
- Distribution of behaviors across the dataset

## 2. Model Architecture Section Enhancements

### 2.1 Architectural Justification
**Current Issue**: Limited explanation of why this specific hybrid architecture was chosen.
**Improvement**: Add:
- Comparison with CNN-only baseline to justify transformer addition (We don't have this data yet, better to just say the theoretical reason of why we added the transformer)
- Explanation of how the architecture captures both local (within-segment) and global (across-segment) patterns
- Discussion of computational trade-offs vs. performance gains (we don't have data here, but let's assume that deep learning approach is way faster than bayesian methods, and add a <citation> tag and I will fill it later)

### 2.2 Feature Engineering Details
**Current Issue**: Incomplete description of feature processing.
**Improvement**: Elaborate on:
- Derivation of pitch and roll angles from magnetometer data
- Feature normalization and scaling strategies


### 2.3 Adaptive Pooling Strategy
**Current Issue**: Adaptive pooling is mentioned but not fully explained.
**Improvement**: Add:
- Detailed explanation of pool size selection for different segment sizes
- Mathematical formulation ensuring consistent output dimensions
- Impact on receptive field and temporal resolution






