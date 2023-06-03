# [LoRA: Low-rank adaptation of large language models](https://arxiv.org/pdf/2106.09685.pdf)

_June 2023_

tl;dr: An Efficient Training Speed Improvement for Large Language Models (LLMs)

#### Overall impression

- LoRA can reduce the number of trainable parameters of GPT-3 175B by 10,000 times and GPU memory requirement by 3 times.
- LoRA can even outperform full finetuning training only 2% of the parameters.
- Instead of taking a few months to train LLMs models, the training process may decrease to 1-2 weeks.
- This study reminds me of dimension compression algorithms such as PCA and SVD.

#### Key ideas

- Freeze Pretrained Weights <!-- $W$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/xvUO0BuXo2.svg">.
- Create new <!-- $W'=W+\Delta W$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/SaDnGM6geH.svg">. Where <!-- $\Delta W$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/Lifrw6R5Va.svg"> is the weight decomposition, so the network only needs to derive the gradients of <!-- $\Delta W$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/5fQO6AqIlH.svg">. 
- <!-- $\Delta W=\alpha * W_{A}W_{B}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/29Pk0rtjix.svg">. Where <!-- $W_{A}=A \times r$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/lAnBxcOD7i.svg">, <!-- $W_{B}=r \times B$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/fNrGHGmMnT.svg">, <!-- $r$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/czrGKrnLfD.svg"> is low-rank representation and <!-- $alpha$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/nTEVJetWua.svg"> is the scale factor. We keep the original weight <!-- $W$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/RHLRw61b45.svg"> frozen and **only train** the new matrices <!-- $W_{A}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/qNtlP9YUQA.svg"> and <!-- $W_{B}$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/RcbT0NpEUc.svg">.
- However, choosing the rank <!-- $r$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/C43sGn5JC3.svg"> has trade-off between model complexity, adaptation capacity, the risk of underfitting and overfitting. 
- It is necessary to perform various experiments to choose the right <!-- $r$ --> <img style="transform: translateY(0.1em); background: white;" src="../svg/4QswqwBZdz.svg"> value to achieve the best possible performance.

### Code ideas

```python
import torch
import torch.nn

embed_dim = 128
output_dim = 512
rank = 4 # the rank 'r' 
scale_factor = 5

W = ... # Pretrained weight from network with shape (embed_dim x output_dim)

W_A = nn.Parameter(torch.randn(embed_dim, rank)) # LoRA weight A
W_B = nn.Parameter(torch.randn(rank, output_dim)) # LoRA weight B

def lora_forward(self, x, W, W):
    new_W = x @ W # the normal forward
    new_W += x @ scale_factor * (W_A @ W_B) 
    
    return new_W
```

#### Notes
- [Code on Github](https://github.com/microsoft/LoRA)