# **Single-Cell Perturbation Transformer Model**

Transformer-based generative model to predict genome-wide expression changes, following unseen genetic perturbations in single-cell data.

## Model Construction

### Input Encoders and Data
 - UMI Encoder
   - Binned UMI (log1p) converted to dense vector
 - GeneID Embedding
   - Static seqeunce of geneids converted into a matrix of embeddings
 - Expression Embedding
   - Binned exppression (log1p) values are converted into a matrix of embeddings
 - Summed Sequence Embedding
   - GeneID and Expression emebdding are summed
 - Perturbation Embedding (semi-shared)
   - Initial embeding is looked up from shared matrix (GeneID Embedding) and adapted via a linear transformation
  
### Transformer Encoder Forward Pass
- Standard attention score calculation after input sequence projected into three matrices, using separate learnable weight matrices
- Updated emebddings sequence then sliced and passes through a linear layer prediction head to generate a final prediction

### Updating Parameters
- 15% of gene expresison bins are masked
- MSE used to calculate loss on masked outputs
- Adam optimizer takes gradients to make updates

## Model Architecture

1. Input Embeddings & Assembly
2. Transformer Encoder Block (x6)
   - **Sub-Layer1: Attention**
     - LayerNorm(norm1)
     - Multi-head Self-Attention(self_attn)
       - 8 heads
     - Dropout(dropout1) + Residual Connection
   - **Sub-Layer2: Feed-Forward**
     - LayernNorm(norm2)
     - Feed-Forward Network (linear1 -> dropout -> linear2)
     - Dropout(dropout2) + Residual Connection
3. Prediction Head
 
   
   


