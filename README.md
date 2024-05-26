# TinyStories
https://arxiv.org/pdf/2305.07759

##  20240525
prelim: 
* good data for training new model. don't see much about the model architecture other than hyperparameters, i guess this means i should use standard architecture
* Two datasets: tinystories and tinystories-instruct. looks like they're each meant to train a different model.
* tinystories easier to start with
* meant to run on a single machine.
trying it out
https://github.com/WilsonCWu/TinyStories/blob/main/explore.ipynb

##  20240526
model sizing
* `We use GPT-Neo architecture with window size 256 and context length 512.` ??? arent those the same thing
* `Our results, shown in Figure 24, suggest that in the regime where the number of heads is small, increasing it improves the performance of the model across all metrics.` `To understand the model’s attention pattern after training, we use a 1-layer model with hidden dimension 1024 and 16 attention heads that was trained on TinyStories` ok let's start with head size 64

Try training
* https://github.com/WilsonCWu/nanoGPT/tree/tinystories using this because it's relatively simple and we're training a small thing
```
# total batch size ~0.5M??? assuming that's a good number based on train_gpt2
gradient_accumulation_steps = 30 
batch_size = 64
block_size = 256 # context of up to 256 previous characters

# baby GPT model :)
n_layer = 6
n_head = 6
n_embd = 384
dropout = 0.2

learning_rate = 1e-3 # with baby networks can afford to go a bit higher
max_iters = 5000
lr_decay_iters = 5000 # make equal to max_iters usually
min_lr = 1e-4 # learning_rate / 10 usually
beta2 = 0.99 # make a bit bigger because number of tokens per iter is small

warmup_iters = 100 # not super necessary potentially
```
```
tokens per iteration will be: 491,520
Initializing a new model from scratch
defaulting to vocab_size of GPT-2 to 50304 (50257 rounded up for efficiency)
number of parameters: 29.94M
num decayed parameter tensors: 26, with 30,031,872 parameters
num non-decayed parameter tensors: 13, with 4,992 parameters
using fused AdamW: True
```
oops this is 30mil, not 10mil params
