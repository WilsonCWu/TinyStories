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
