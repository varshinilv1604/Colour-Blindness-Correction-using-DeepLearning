# Deep Correct — Deep Learning Colour Correction for Colour Vision Deficiency

Deep Correct is a deep learning framework that improves functional colour distinguishability for individuals with Colour Vision Deficiency (CVD). Instead of optimising for pixel-level similarity to the original image, it uses a GAN-inspired Corrector–Referee architecture trained with task-driven supervision (object classification accuracy) to make images more interpretable for CVD viewers — even if the resulting colours look unconventional to normal-vision observers.

## Getting Started

Open the notebook in Google Colab or Jupyter and run the cells in order.

## Use your preferred IDE

If you want to work locally using your own IDE, you can clone this repo and run the notebook.

The only requirement is having Python 3 installed.

Follow these steps:

```sh
# Step 1: Clone the repository.
git clone https://github.com/varshinilv1604/Colour-Blindness-Correction-using-DeepLearning

# Step 2: Navigate to the project directory.
cd Colour-Blindness-Correction-using-DeepLearning

# Step 3: Install the necessary dependencies.
pip install tensorflow numpy matplotlib pillow jupyter

# Step 4: Launch the notebook.
jupyter notebook Another_copy_of_deep_learning_colour_blindness.ipynb
```

## Dataset

This project uses a simulated CVD version of the **CIFAR-10** dataset (60,000 32×32 colour images across 10 classes). The original dataset is publicly available at [cs.toronto.edu/~kriz/cifar.html](https://www.cs.toronto.edu/~kriz/cifar.html).

## What technologies are used for this project?

This project is built with:

- Python
- TensorFlow 2.x / Keras
- NumPy
- VGG16 (pre-trained, used as the frozen Referee network)
- NVIDIA V100 GPU (training environment)

## How it works

1. **Convert RGB → LMS** — transform the input image into LMS cone space to model human photoreceptor response
2. **Simulate CVD** — apply a protanopia/deuteranopia/tritanopia simulation filter to produce the CVD-perceived image
3. **Corrector network** — a shallow 3-layer CNN learns a non-linear colour transformation of the input image
4. **Referee network** — a frozen, pre-trained VGG16 evaluates object classification accuracy on the simulated-corrected output
5. **Task-driven training** — gradients from the Referee's classification loss are backpropagated through the simulation layer to train the Corrector, optimising for *functional distinguishability* rather than pixel reconstruction
6. **Evaluation** — Top-1/Top-5 classification accuracy is compared across uncorrected, linearly corrected, and Deep Correct outputs

## Results

| Condition | Referee Accuracy |
|---|---|
| Original images (upper bound) | ~70.4% |
| CVD-simulated, uncorrected (baseline) | ~66.4% |
| Deep Correct (ours) | ~67.8%–73%* |

\* Accuracy varies by run/configuration; see the paper for the full comparison against linear daltonization, GAN-based, and saliency-based correction baselines.

Deep Correct consistently improves colour distinguishability over uncorrected and traditional linear correction methods while preserving the structural integrity of the original image, particularly for red–green colour vision deficiency.

## Citation

This work is part of an ongoing research paper, *"Deep Correct: Deep Learning Colour Correction for Colour Vision Deficiency"*
