# Understanding LLMs Mathematically: Expanded Examples

> **Note on Example Values:** Throughout this document, I've used specific numerical values to make the examples concrete. These values are simplified representations chosen to demonstrate the mathematical operations clearly, not actual values from real LLMs. In practice, real LLMs would have much larger dimensions (768 to 4096 or more), and the actual values would be learned during training rather than manually set. The token IDs (like 7, 102, 408) are arbitrary examples that represent how words get mapped to unique numbers in a vocabulary. In real systems, these mappings are created based on frequency in the training data.

## Foundation Mathematical Concepts with Detailed Examples

### 1. Vectors: Detailed Operations

A vector is a list of numbers. In LLMs, vectors represent words, sentences, or features.

**Vector Addition:**
```
[a, b, c] + [d, e, f] = [a+d, b+e, c+f]

Example:
[2, 3, 1] + [1, 5, 2] = [3, 8, 3]
```

**Vector Dot Product:**
```
[a, b, c] · [d, e, f] = a×d + b×e + c×f

Example:
[2, 3, 1] · [1, 5, 2] = 2×1 + 3×5 + 1×2 = 2 + 15 + 2 = 19
```

**Vector Magnitude (Length):**
```
|[a, b, c]| = √(a² + b² + c²)

Example:
|[3, 4, 0]| = √(3² + 4² + 0²) = √(9 + 16 + 0) = √25 = 5
```

**Vector Normalization (creating a unit vector):**
```
unit([a, b, c]) = [a, b, c] / |[a, b, c]|

Example:
unit([3, 4, 0]) = [3, 4, 0] / 5 = [0.6, 0.8, 0]
```

### 2. Matrices: Expanded Operations

**Matrix Addition:**
```
[a b]   [e f]   [a+e b+f]
[c d] + [g h] = [c+g d+h]

Example:
[1 2]   [5 2]   [6  4]
[3 4] + [1 3] = [4  7]
```

**Matrix Multiplication (Matrix × Matrix):**
```
[a b]   [w x]   [a×w+b×y a×x+b×z]
[c d] × [y z] = [c×w+d×y c×x+d×z]

Example:
[2 3]   [1 4]   [2×1+3×2 2×4+3×5]   [8  23]
[1 4] × [2 5] = [1×1+4×2 1×4+4×5] = [9  24]
```

**Matrix Transpose:**
```
[a b c]ᵀ   [a d]
[d e f]   = [b e]
           [c f]

Example:
[1 2 3]ᵀ   [1 4]
[4 5 6]   = [2 5]
           [3 6]
```

### 3. Probability Functions

**Softmax Function (Converting Numbers to Probabilities):**
```
softmax(x₁, x₂, ..., xₙ) = [e^x₁/sum, e^x₂/sum, ..., e^xₙ/sum]
where sum = e^x₁ + e^x₂ + ... + e^xₙ

Example:
softmax([2, 1, 0]) = [e²/(e²+e¹+e⁰), e¹/(e²+e¹+e⁰), e⁰/(e²+e¹+e⁰)]
= [7.39/12.37, 2.72/12.37, 1/12.37]
= [0.60, 0.22, 0.08]
```

**Cross-Entropy Loss (Measuring Prediction Error):**
```
Loss = -∑ y_true × log(y_pred)

Example:
If true word is "cat" (index 0) from ["cat", "dog", "fish"]:
y_true = [1, 0, 0]
If model predicts probabilities y_pred = [0.7, 0.2, 0.1]:
Loss = -(1×log(0.7) + 0×log(0.2) + 0×log(0.1))
     = -(log(0.7))
     = -(-0.36)
     = 0.36
```

## LLM Mathematical Examples in Detail

### Tokenization Example

For the sentence "The cat sits":

1. Split into tokens: ["The", "cat", "sits"]
2. Convert to token IDs: [7, 102, 408] 
   > *Note: These token IDs are arbitrary examples. In real tokenizers, IDs are assigned based on the frequency of tokens in training data. Common words usually get smaller IDs. The values 7, 102, and 408 were chosen simply to illustrate that words get converted to unique numbers.*
3. Convert to one-hot vectors:
   - "The" → [0,0,0,0,0,0,0,1,0,0,0,...] (position 7 is 1)
   - "cat" → [0,0,...,1,0,...] (position 102 is 1)
   - "sits" → [0,0,...,1,...] (position 408 is 1)
   > *Note: One-hot vectors are vectors with a 1 in just one position (corresponding to the token ID) and 0s everywhere else. They're a way to represent categorical data (like words) as numbers. The length of these vectors would equal the vocabulary size (often 30,000 to 50,000 in real models).*

### Embedding Layer Example

If our embedding matrix has dimensions 10,000 × 256 (vocabulary size × embedding size):

For the token ID 102 ("cat"):
```
embedding_vector = embedding_matrix[102]
```

This selects the 102nd row from the matrix, giving us a 256-dimensional vector like:
```
[0.021, -0.412, 0.731, ..., 0.182]
```

> *Note: The embedding values (0.021, -0.412, etc.) are representative examples. In real LLMs, these values are learned during training and would capture semantic properties of words. Similar words end up with similar embedding vectors. The dimension 256 is smaller than real models (which might use 768 or larger) but was chosen to be manageable for examples while still being realistic.*

### Self-Attention Calculation Example

Let's work through a simple self-attention calculation with 3 words, each with 4-dimensional embeddings:

Word embeddings:
- "The": [0.1, 0.2, 0.3, 0.4]
- "cat": [0.5, 0.6, 0.7, 0.8]
- "sits": [0.9, 1.0, 1.1, 1.2]

> *Note: These embedding values are simplified for the example. I chose a pattern where each word's embedding increases by 0.1 for each dimension, and each subsequent word starts 0.4 higher than the previous word. This makes the calculations easier to follow. In real LLMs, embeddings would have hundreds of dimensions with values learned during training to capture semantic relationships between words.*

1. Create Query (Q), Key (K), and Value (V) matrices (simplified):
   - W_Q = [[0.1, 0.2], [0.3, 0.4], [0.5, 0.6], [0.7, 0.8]]
   - W_K = [[0.1, 0.3], [0.2, 0.4], [0.5, 0.7], [0.6, 0.8]]
   - W_V = [[0.2, 0.4], [0.6, 0.8], [0.1, 0.3], [0.5, 0.7]]
   
   > *Note: These weight matrices contain values that follow simple patterns to make calculations traceable. For W_Q, each row increases by 0.2 horizontally and 0.2 vertically. The other matrices follow similar patterns. In real LLMs, these values would be learned during training and would be much larger matrices (e.g., 768×768). The output dimension is reduced to 2 here (instead of 4) to simplify calculations while still showing the dimensionality reduction that often happens in attention mechanisms.*

2. Calculate Q, K, V vectors for each word:
   - Q_the = [0.1, 0.2, 0.3, 0.4] × W_Q = [0.30, 0.70]
   - K_the = [0.1, 0.2, 0.3, 0.4] × W_K = [0.33, 0.75]
   - V_the = [0.1, 0.2, 0.3, 0.4] × W_V = [0.42, 0.82]

   Similarly for other words...

3. Calculate attention scores (for "cat" attending to all words):
   - Score(cat→the) = Q_cat · K_the / √2 = [0.70, 1.50] · [0.33, 0.75] / 1.414 = (0.231 + 1.125) / 1.414 = 0.96
   - Score(cat→cat) = Q_cat · K_cat / √2 = 1.60
   - Score(cat→sits) = Q_cat · K_sits / √2 = 2.24

4. Apply softmax to get attention weights:
   - softmax([0.96, 1.60, 2.24]) = [0.12, 0.23, 0.65]

5. Calculate weighted sum of values:
   - Output_cat = 0.12 × V_the + 0.23 × V_cat + 0.65 × V_sits
   - Output_cat = 0.12 × [0.42, 0.82] + 0.23 × [0.90, 1.70] + 0.65 × [1.38, 2.58]
   - Output_cat = [0.05, 0.10] + [0.21, 0.39] + [0.90, 1.68]
   - Output_cat = [1.16, 2.17]

### Layer Normalization Example

For a hidden state vector h = [1.2, -0.5, 3.7, 0.8]:

> *Note: This example vector was chosen to include both positive and negative values with different magnitudes to demonstrate how normalization handles diverse inputs. The values represent activation outputs that might occur at some layer in the network. Layer normalization is important because it keeps values in a consistent range throughout the network, preventing them from growing too large or too small as they pass through many layers.*

1. Calculate mean: (1.2 + (-0.5) + 3.7 + 0.8) / 4 = 1.3
2. Calculate variance: ((1.2-1.3)² + (-0.5-1.3)² + (3.7-1.3)² + (0.8-1.3)²) / 4 = 2.92
3. Normalize: 
   - (1.2 - 1.3) / √(2.92 + 0.00001) = -0.1 / 1.71 = -0.06
   - (-0.5 - 1.3) / √(2.92 + 0.00001) = -1.8 / 1.71 = -1.05
   - (3.7 - 1.3) / √(2.92 + 0.00001) = 2.4 / 1.71 = 1.40
   - (0.8 - 1.3) / √(2.92 + 0.00001) = -0.5 / 1.71 = -0.29

4. Result: [-0.06, -1.05, 1.40, -0.29]

### Feed-Forward Network Example

For a normalized vector x = [-0.06, -1.05, 1.40, -0.29]:

1. First linear transformation with W₁ (4×8 matrix) and b₁:
   - W₁ = [[0.1, 0.2, ..., 0.8], [0.2, 0.3, ..., 0.9], ...]
   - b₁ = [0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1]
   - linear₁ = x × W₁ + b₁ = [0.27, 0.35, 0.43, 0.51, 0.59, 0.67, 0.75, 0.83]
   
   > *Note: The weight matrix W₁ follows a simple pattern where each value increases by 0.1 horizontally and 0.1 vertically. This pattern was chosen to make calculations easy to follow. The bias vector b₁ has constant values (0.1) as is common in initial setups. In real LLMs, the feed-forward networks typically expand the dimension (e.g., from 768 to 3072) before reducing it back, which is why this example goes from 4 to 8 dimensions. These values would be learned during training in real models.*

2. Apply ReLU activation (replace negatives with 0):
   - ReLU(linear₁) = [0.27, 0.35, 0.43, 0.51, 0.59, 0.67, 0.75, 0.83]
   
   > *Note: In this case, all values after the first linear transformation are positive, so the ReLU function doesn't change anything. This was intentional to simplify the example, but in real networks, ReLU would typically zero out some negative values. The ReLU activation function is used in neural networks to introduce non-linearity, which allows the network to learn more complex patterns.*

3. Second linear transformation with W₂ (8×4 matrix) and b₂:
   - Result = [0.32, 0.45, 0.58, 0.71]
   
   > *Note: The specific values for the result were chosen to show a pattern of increasing by approximately 0.13 between each position. This represents the output after transforming back to the original dimension (from 8 back to 4). In real LLMs, these values would reflect complex transformations of the input data, capturing higher-level features of the text.*

### Multi-Head Attention Example

With 2 attention heads, each with dimension 2:

1. For each head, compute separate Q, K, V projections
2. Calculate attention outputs for each head separately
3. Concatenate results: [head₁_output₁, head₁_output₂, head₂_output₁, head₂_output₂]
4. Apply a final projection matrix to get the final output

### Complete Forward Pass Example

For the input sequence "The cat sits":

1. Embed tokens:
   - "The" → [0.1, 0.2, 0.3, 0.4]
   - "cat" → [0.5, 0.6, 0.7, 0.8]
   - "sits" → [0.9, 1.0, 1.1, 1.2]
   
   > *Note: These embedding values continue the pattern from earlier examples, with simple increments of 0.1 within each word's embedding and 0.4 between words. This pattern makes it easier to trace calculations through the network. In real LLMs, embedding values would reflect semantic relationships between words and would be learned during training on large text datasets.*

2. Apply positional encoding (simplified as addition):
   - "The" → [0.11, 0.22, 0.32, 0.43]
   - "cat" → [0.52, 0.63, 0.74, 0.85]
   - "sits" → [0.93, 1.04, 1.15, 1.26]
   
   > *Note: Positional encoding adds position information to the embedding vectors. In this simplified example, I've added 0.01, 0.02, 0.02, and 0.03 to the first position, 0.02, 0.03, 0.04, and 0.05 to the second position, and so on. Real LLMs use more sophisticated positional encodings, often based on sine and cosine functions of different frequencies. The purpose is to let the model know the order of words, since attention mechanisms don't inherently capture sequence information.*

3. First self-attention layer:
   - Calculate attention scores (as shown earlier)
   - Apply attention weights to values
   - Results after attention:
     * "The" → [0.7, 0.9, 1.1, 1.3]
     * "cat" → [0.8, 1.0, 1.2, 1.4]
     * "sits" → [0.9, 1.1, 1.3, 1.5]
     
   > *Note: These values are simplified results of the attention calculation. They follow a pattern where each position increases by 0.2 within a word and by 0.1 between words. In a real attention mechanism, these values would reflect which other words each word is attending to. For example, in "The cat sits on the mat," the word "sits" might attend strongly to "cat" (its subject). The values here were chosen to be easy to follow in subsequent calculations while still showing how information flows through the network.*

4. Add residual connection and normalize:
   - "The" → [0.81, 1.12, 1.42, 1.73]
   - "cat" → [1.32, 1.63, 1.94, 2.25]
   - "sits" → [1.83, 2.14, 2.45, 2.76]
   
   > *Note: Residual connections add the original input to the output of a layer. Here, we're adding the vectors from step 2 to the attention outputs from step 3. For example, for "The": [0.11, 0.22, 0.32, 0.43] + [0.7, 0.9, 1.1, 1.3] = [0.81, 1.12, 1.42, 1.73]. This helps information flow through deep networks and prevents gradient vanishing during training. The specific values follow from the earlier calculations and maintain the established patterns.*

5. Feed-forward network and second normalization:
   - Final representations:
     * "The" → [0.1, 0.3, 0.5, 0.7]
     * "cat" → [0.2, 0.4, 0.6, 0.8]
     * "sits" → [0.3, 0.5, 0.7, 0.9]
     
   > *Note: These values represent the output after passing through the feed-forward network and another layer normalization. The actual calculations would involve matrix multiplications, ReLU activations, and normalization as shown in earlier examples. I've simplified the results to show a clear pattern (increasing by 0.2 within each word vector and by 0.1 between words) that makes it easy to understand how information flows. In real LLMs, these values would contain complex representations of each word in context.*

6. Output projection to vocabulary (10,000 words):
   - For "sits", final logits for next word prediction:
     * [0.01, 0.02, ..., 0.5, ..., 0.01]
     * Highest values for words likely to follow "The cat sits"
     
   > *Note: These logit values represent scores for each possible next word in the vocabulary. The pattern shown here indicates that most words get low scores (0.01, 0.02) while words that commonly follow "The cat sits" (like "on", "by", "quietly") get higher scores (around 0.5). In a real LLM, these values would result from a matrix multiplication between the final representation of "sits" and a learned output matrix. The highest values would correspond to words that the model has learned are most likely to follow in this context.*

7. Apply softmax to get probabilities:
   - Probability distribution across all words
   - Highest probabilities for words like "on", "by", "quietly"

### Training Example: Backpropagation

For a simple case where the model predicted:
- "on" with probability 0.3 (correct next word)
- "by" with probability 0.2
- "quietly" with probability 0.1
- All other words with remaining probability

> *Note: These probability values were chosen to represent a realistic scenario where the model assigns the highest probability to the correct next word ("on") but also considers other plausible continuations ("by", "quietly"). The values 0.3, 0.2, and 0.1 are simplified but reasonable magnitudes - real LLMs often distribute probability across many plausible next words rather than being completely certain. These probabilities come from applying the softmax function to the logits from the previous step.*

1. Calculate loss:
   - Loss = -log(0.3) = 1.20

2. Compute gradients (simplified):
   - For output layer: gradient = (predicted - actual)
     * For "on": 0.3 - 1.0 = -0.7
     * For "by": 0.2 - 0.0 = 0.2
     * For "quietly": 0.1 - 0.0 = 0.1

3. Backpropagate through layers:
   - Feed-forward gradients
   - Attention gradients
   - Embedding gradients

4. Update weights using gradients:
   - W_new = W_old - learning_rate × gradient
   - For learning_rate = 0.01:
     * W_output_on = W_output_on - 0.01 × (-0.7) = W_output_on + 0.007
     * W_output_by = W_output_by - 0.01 × 0.2 = W_output_by - 0.002

### Decoding Example: Generating Text

Starting with "The cat":

1. Forward pass through model to get next-word probabilities:
   - "sits": 0.3
   - "sleeps": 0.2
   - "jumps": 0.15
   - etc.
   
   > *Note: These probability values represent a realistic distribution of words that might follow "The cat" based on what the model has learned from training data. The most likely completions are verbs describing what a cat might do. The values 0.3, 0.2, and 0.15 were chosen to show how probability is spread across multiple plausible options, with the most likely option ("sits") having the highest probability but not dominating completely. In real LLMs, these probabilities would be calculated from the model's internal representations after processing "The cat".*

2. Sampling methods:
   - Greedy: Always pick "sits" (highest probability)
   - Temperature sampling (T=0.8): Slightly randomizes selection
   - Top-k (k=3): Sample only from ["sits", "sleeps", "jumps"]
   - Nucleus (p=0.9): Sample from words covering 90% of probability mass

3. Let's say "sits" is chosen. Next prompt becomes "The cat sits"

4. Repeat process for next word:
   - "on": 0.25
   - "by": 0.20
   - "quietly": 0.15
   - etc.

5. Continue until end token or maximum length

## Connections to the Full LLM Architecture

These mathematical examples provide the building blocks of modern LLMs:

1. **Embedding Layer**: Converts token IDs to dense vectors
2. **Self-Attention**: Allows words to "look at" other words
3. **Feed-Forward Networks**: Process each position independently
4. **Layer Normalization**: Stabilizes training
5. **Multi-Head Attention**: Captures different relationship types
6. **Positional Encoding**: Adds position information
7. **Output Layer**: Converts final representations to word probabilities

