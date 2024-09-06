# Superconscious NeuralHash Collider Demo Idea
## Author: Arthur

In this demo, we explore how to find target [hash collisions] using NeuralHash. It's all in good fun and for the sake of learning about how big companies search through faces ... and sometimes make mistakes.
I hope to reuse this for Studienarbeit in the future.

### Quick Demo

Starting with a picture of [this cute cat](https://github.com/anishathalye/neural-hash-collider/raw/assets/cat.jpg), we can generate an adversarial image that somehow has the same hash as this [dog pic](https://user-images.githubusercontent.com/1328/129860794-e7eb0132-d929-4c9d-b92e-4e4faba9e849.png). Here's how you do it:

```bash
python collide.py --image cat.jpg --target 59a34eabe31910abfb06f308
```

You should get something like this:

![Cat image with NeuralHash](https://raw.githubusercontent.com/anishathalye/assets/master/neural-hash-collider/cat-adv.png) ![Dog image with NeuralHash](https://raw.githubusercontent.com/anishathalye/assets/master/neural-hash-collider/dog.png)

For the doubters: we can check the hash match using `nnhash.py` from [AppleNeuralHash2ONNX].

```bash
$ python nnhash.py dog.png
59a34eabe31910abfb06f308
$ python nnhash.py adv.png
59a34eabe31910abfb06f308
```

### What’s Actually Happening?

NeuralHash is a [perceptual hash](https://en.wikipedia.org/wiki/Perceptual_hashing) that uses neural networks to create a 96-bit hash for images. It works like magic, but there’s a trick: neural networks are sensitive to [adversarial examples](https://arxiv.org/abs/1312.6199), so with a little clever math (read: gradient descent), we can force two images to share the same hash.

TL;DR: we get computers to "think" two images are the same when they’re obviously not.

### How We Do It

We create a loss function that tells us how close our image is to the target hash, then we tweak the image with some math sorcery (gradient descent). It might take some fiddling with parameters, but we’re student researchers—fiddling is our forte. 

For the curious, the code in `collide.py` explains everything. We switch between optimizing for the target hash and making the image look as close to the original as possible. You can mess with parameters by running:

```bash
python collide.py --help
```

And if you like things to look a bit smoother, try adding the `--blur [sigma]` flag—though, fair warning, it might slow down the process.

### Examples

Want some cool case studies? Check out our recreation of the classic [Lena](https://raw.githubusercontent.com/anishathalye/assets/master/neural-hash-collider/lena.png) and [Barbara](https://raw.githubusercontent.com/anishathalye/assets/master/neural-hash-collider/barbara.png) hash collision. Here’s what the magic looks like:

<img width="200" src="https://raw.githubusercontent.com/anishathalye/assets/master/neural-hash-collider/lena.png"></img> <img width="200" src="https://raw.githubusercontent.com/anishathalye/assets/master/neural-hash-collider/lena-adv.png"></img> <img width="200" src="https://raw.githubusercontent.com/anishathalye/assets/master/neural-hash-collider/lena-adv-blur-1.0.png"></img> <img width="200" src="https://raw.githubusercontent.com/anishathalye/assets/master/neural-hash-collider/barbara.png"></img>

As you can see, Lena and Barbara now share a hash. Impressive? Creepy? You decide.

---

## Setup

1. Grab Apple's NeuralHash model following the instructions from [AppleNeuralHash2ONNX], and make sure the necessary files are in this directory or provided via `--model` / `--seed` arguments.
2. Install the required dependencies:

```bash
pip install -r requirements.txt
```

## Usage

To generate a hash collision, run the following:

```bash
python collide.py --image [path to image] --target [target hash]
```

There are plenty of options to tinker with (`python collide.py --help`), including learning rate, blurring, and more.

## Limitations (We’re Only Human)

This code is a proof of concept. You’ll probably want to tweak parameters or even rewrite bits to get the best results. Think of this as a starting point—there’s plenty more to explore!

## Citation

If you end up using this in your own work, feel free to cite us like this:

```bibtex
@misc{athalye2021neuralhashcollider,
  author = {Anish Athalye},
  title = {NeuralHash Collider},
  year = {2021},
  howpublished = {\url{https://github.com/anishathalye/neural-hash-collider}},
}
```
