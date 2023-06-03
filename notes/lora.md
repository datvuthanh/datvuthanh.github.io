# [LoRA: Low-rank adaptation of large language models](https://arxiv.org/pdf/2106.09685.pdf)

_June 2023_

tl;dr: An Efficient Training Speed Improvement for Large Language Models (LLMs)

#### Overall impression

- LoRA can reduce the number of trainable parameters of GPT-3 175B by 10,000 times and GPU memory requirement by 3 times.
- LoRA can even outperform full finetuning training only 2% of the parameters.
- Instead of taking a few months to train LLMs models, the training process may decrease to 1-2 weeks.
- This study reminds me of dimension compression algorithms such as PCA and SVD.

#### Key ideas

- Freeze Pretrained Weights $W$ and $\deltaW$.
- Create new <!-- $W'=W+\Delta W$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=W'%3DW%2B%5CDelta%20W">. Where <!-- $\Delta W$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5CDelta%20W"> is the weight decomposition, so the network only needs to derive the gradients of <!-- $\Delta W$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5CDelta%20W">. 
- <!-- $\Delta W=\alpha * W_{A}W_{B}$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5CDelta%20W%3D%5Calpha%20*%20W_%7BA%7DW_%7BB%7D">. Where <!-- $W_{A}=A \times r$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=W_%7BA%7D%3DA%20%5Ctimes%20r">, <!-- $W_{B}=r \times B$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=W_%7BB%7D%3Dr%20%5Ctimes%20B">, <!-- $r$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=r"> is low-rank representation and <!-- $\alpha$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=%5Calpha"> is the scale factor. We keep the original weight <!-- $W$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=W"> frozen and **only train** the new matrices <!-- $W_{A}$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=W_%7BA%7D"> and <!-- $W_{B}$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=W_%7BB%7D">.
- However, choosing the rank <!-- $r$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=r"> has trade-off between model complexity, adaptation capacity, the risk of underfitting and overfitting. 
- It is necessary to perform various experiments to choose the right <!-- $r$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=r"> value to achieve the best possible performance.

### Code ideas

```python=
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