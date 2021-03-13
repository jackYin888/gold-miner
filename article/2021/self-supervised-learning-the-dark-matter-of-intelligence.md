> * 原文地址：[Self-supervised learning: The dark matter of intelligence](https://ai.facebook.com/blog/self-supervised-learning-the-dark-matter-of-intelligence/)
> * 原文作者：Yann LeCun， Ishan Misra
> * 译文出自：[掘金翻译计划](https://github.com/xitu/gold-miner)
> * 本文永久链接：[https://github.com/xitu/gold-miner/blob/master/article/2021/self-supervised-learning-the-dark-matter-of-intelligence.md](https://github.com/xitu/gold-miner/blob/master/article/2021/self-supervised-learning-the-dark-matter-of-intelligence.md)
> * 译者：
> * 校对者：

# Self-supervised learning: The dark matter of intelligence

In recent years, the AI field has made tremendous progress in developing AI systems that can learn from massive amounts of carefully labeled data. This paradigm of supervised learning has a proven track record for training specialist models that perform extremely well on the task they were trained to do. Unfortunately, there’s a limit to how far the field of AI can go with supervised learning alone.

Supervised learning is a bottleneck for building more intelligent generalist models that can do multiple tasks and acquire new skills without massive amounts of labeled data. Practically speaking, it’s impossible to label everything in the world. There are also some tasks for which there’s simply not enough labeled data, such as training translation systems for low-resource languages. If AI systems can glean a deeper, more nuanced understanding of reality beyond what’s specified in the training data set, they’ll be more useful and ultimately bring AI closer to human-level intelligence.

As babies, we learn how the world works largely by observation. We form generalized predictive models about objects in the world by learning concepts such as object permanence and gravity. Later in life, we observe the world, act on it, observe again, and build hypotheses to explain how our actions change our environment by trial and error.

A working hypothesis is that generalized knowledge about the world, or common sense, forms the bulk of biological intelligence in both humans and animals. This common sense ability is taken for granted in humans and animals, but has remained an open challenge in AI research since its inception. In a way, common sense is the dark matter of artificial intelligence.

Common sense helps people learn new skills without requiring massive amounts of teaching for every single task. For example, if we show just a few drawings of cows to small children, they’ll eventually be able to recognize any cow they see. By contrast, AI systems trained with supervised learning require many examples of cow images and might still fail to classify cows in unusual situations, such as lying on a beach. How is it that humans can learn to drive a car in about 20 hours of practice with very little supervision, while fully autonomous driving still eludes our best AI systems trained with thousands of hours of data from human drivers? The short answer is that humans rely on their previously acquired background knowledge of how the world works.

How do we get machines to do the same?

![https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/152694956_207655794383548_8489544025025665441_n.png?_nc_cat=102&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=vTr8Ey9RqxEAX_hYt0f&_nc_ht=scontent-nrt1-1.xx&oh=f1466ddb3a53501d68d6ca58e1574ef1&oe=6071B6F1](https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/152694956_207655794383548_8489544025025665441_n.png?_nc_cat=102&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=vTr8Ey9RqxEAX_hYt0f&_nc_ht=scontent-nrt1-1.xx&oh=f1466ddb3a53501d68d6ca58e1574ef1&oe=6071B6F1)

We believe that self-supervised learning (SSL) is one of the most promising ways to build such background knowledge and approximate a form of common sense in AI systems.

Self-supervised learning enables AI systems to learn from orders of magnitude more data, which is important to recognize and understand patterns of more subtle, less common representations of the world. Self-supervised learning has long had great success in advancing the field of natural language processing (NLP), including the [Collobert-Weston 2008 model](https://l.facebook.com/l.php?u=https%3A%2F%2Fronan.collobert.com%2Fpub%2Fmatos%2F2008_nlp_icml.pdf&h=AT1yOAr331_AUdt65mqISQy-_ejoxQLKDRouxlGfpR0q1stomp2cdKkfrg3NRvm6otV7B__bvAjftxzZXPXnFtR6NzneYOqwourNxEXYoCOhHof2I-pykUiTPcRItQ8Ps5fWLN_IDgHLS9-ctSRLf9AOKoI), [Word2Vec](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fpdf%2F1301.3781.pdf&h=AT2jLU4yk75SQC_TRhoKW7VxXnrxpuMIJ0MbVQKoz7rFprDLqddCm2ZkVDVpHlbNxgTOQZLBQsGbpexUSy7_3SlFjYCrsCnmvosxw7F8QOSnIMDk-agewDgwzU_zIcjTN7sb88yjBY7ofrw4qf8gdBXzvPU), [GloVE](https://l.facebook.com/l.php?u=https%3A%2F%2Fnlp.stanford.edu%2Fpubs%2Fglove.pdf&h=AT3xNxZzYY6jjwV0QuqQIpORfF_cDnZa2_ybdVMuy79Bbxfinvhym3GBbwR_s9mVdWdd7hDy1PfaF6INbC-IW71Su1ep6_R5tlMouzyVgif_pWHxyAaeoVm-MyCxZ7tJm32ClcyIlU_Z1bbVPPEiIKkbnGw), [fastText](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fpdf%2F1607.01759.pdf&h=AT1fFLk32dWLmQgrfPgFVh-nnuu3Gs_uYn2BnFmyJ6EzQWVNU_R5jUOM096qjHjBjwbmO0oUhllkkguDJduIe44so0ViXJrTDPcI7uBhHlYsNb9ARXnGpuwrvZ8hIljPmTU5pFhueOkkLAo-9wMcA4xrARY), and, more recently, [BERT](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fpdf%2F1810.04805.pdf&h=AT3FYFWnWwZoAFAmqqdB0iCSoI1GDYIH9IfcL6YOAf9uPh5xNl5A0G4DboF3taMf7mpPfaQfI8bZduWQIUlh1cdRzboolktPLjNh3lUz76SPzfoWgGCfHmCntbyhmEgP1WJn7-tEKbSaH4g3lHiuv25VeyA), [RoBERTa](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fpdf%2F1907.11692.pdf&h=AT2CQnffCaZC6L5Y5CKZeea9Dp1EVhdCPb86jEDCTAxoCozK45ePOxDD-xI-Pd8cdX6_IXk4z19c6ljVw3AeA8KpK0SgRd95mXmyej8Bb4OUgddGs_TCjHMBVg-0RFXUgV4jm9FZivJHW4Dydiss5i-rjwg), [XLM-R](https://ai.facebook.com/blog/-xlm-r-state-of-the-art-cross-lingual-understanding-through-self-supervision/), and others. Systems pretrained this way yield considerably higher performance than when solely trained in a supervised manner.

Our latest research project [SEER](https://ai.facebook.com/blog/seer-the-start-of-a-more-powerful-flexible-and-accessible-era-for-computer-vision) leverages SwAV and other methods to pretrain a large network on a billion random unlabeled images, yielding top accuracy on a diverse set of vision tasks. This progress demonstrates that [self-supervised learning can excel at CV tasks in complex, real-world settings as well](https://arxiv.org/pdf/2103.01988.pdf?fbclid=IwAR2pqhYda6MV9r2b3Afx_0eKUiZhX-Es6Pa_FbLOqH8fglQzO2kY3yKxZE8).

Today, we’re sharing details on why self-supervised learning may be helpful in unlocking the dark matter of intelligence — and the next frontier of AI. We’re also highlighting what we believe are some of the most promising new directions of energy-based models for prediction in the presence of uncertainty, joint embedding methods and latent-variable architectures for self-supervised learning and reasoning in AI systems.

## Self-supervised learning is predictive learning

Self-supervised learning obtains supervisory signals from the data itself, often leveraging the underlying structure in the data. The general technique of self-supervised learning is to predict any unobserved or hidden part (or property) of the input from any observed or unhidden part of the input. For example, as is common in NLP, we can hide part of a sentence and predict the hidden words from the remaining words. We can also predict past or future frames in a video (hidden data) from current ones (observed data). Since self-supervised learning uses the structure of the data itself, it can make use of a variety of supervisory signals across co-occurring modalities (e.g., video and audio) and across large data sets — all without relying on labels.

![https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/148954125_461761118405979_2035914075893596810_n.png?_nc_cat=107&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=a7mBQyPFw24AX_f0OWL&_nc_ht=scontent-nrt1-1.xx&oh=0124866062146d753e181cb9f12b0c98&oe=60720D19](https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/148954125_461761118405979_2035914075893596810_n.png?_nc_cat=107&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=a7mBQyPFw24AX_f0OWL&_nc_ht=scontent-nrt1-1.xx&oh=0124866062146d753e181cb9f12b0c98&oe=60720D19)

In self-supervised learning, the system is trained to predict hidden parts of the input (in gray) from visible parts of the input (in green).

As a result of the supervisory signals that inform self-supervised learning, the term “self-supervised learning” is more accepted than the previously used term “unsupervised learning.” Unsupervised learning is an ill-defined and misleading term that suggests that the learning uses no supervision at all. In fact, self-supervised learning is not unsupervised, as it uses far more feedback signals than standard supervised and reinforcement learning methods do.

## Self-supervised learning for language versus vision

Self-supervised learning has had a particularly profound impact on NLP, allowing us to train models such as BERT, RoBERTa, XLM-R, and others on large unlabeled text data sets and then use these models for downstream tasks. These models are pretrained in a self-supervised phase and then fine-tuned for a particular task, such as classifying the topic of a text. In the self-supervised pretraining phase, the system is shown a short text (typically 1,000 words) in which some of the words have been masked or replaced. The system is trained to predict the words that were masked or replaced. In doing so, the system learns to represent the meaning of the text so that it can do a good job at filling in “correct” words, or those that make sense in the context.

Predicting missing parts of the input is one of the more standard tasks for SSL pretraining. To complete a sentence such as “The (blank) chases the (blank) in the savanna,” the system must learn that lions or cheetahs can chase antelope or wildebeests, but that cats chase mice in the kitchen, not the savanna. As a consequence of the training, the system learns to represent the meaning of words, the syntactic role of words, and the meaning of entire texts.

These techniques, however, can’t be easily extended to new domains, such as CV. Despite promising early results, SSL has not yet brought about the same improvements in computer vision that we have seen in NLP (though this will change).

The main reason is that it is considerably more difficult to represent uncertainty in the prediction for images than it is for words. When the missing word cannot be predicted exactly (is it “lion” or “cheetah”?), the system can associate a score or a probability to all possible words in the vocabulary: high score for “lion,” “cheetah,” and a few other predators, and low scores for all other words in the vocabulary.

Training models at this scale also required a model architecture that was efficient in terms of both runtime and memory, without compromising on accuracy. Fortunately, a recent innovation by FAIR in the realm of architecture design led to a new model family called [RegNets](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fabs%2F2003.13678&h=AT0R7AOGM3W0HxnXE94rVR33vdmjyoOhjALclG00EUTQuel2P09LSzyaDzDrvM8pjvx9TLPtGRJT94F8pmX4NxFA6VsvSJBotumq0JlaPNi6zTnH0jAsgwv8kxn2r7F3_Hi-UapmqfPtbr8yVvS-ZZxoSb8) that perfectly fit these needs. RegNet models are ConvNets capable of scaling to billions or potentially even trillions of parameters, and can be optimized to fit different runtime and memory limitations.

But we do not know how to efficiently represent uncertainty when we predict missing frames in a video or missing patches in an image. We cannot list all possible video frames and associate a score to each of them, because there is an infinite number of them. While this problem has limited the performance improvement from SSL in vision, new techniques SSL techniques such as SwAV are starting to beat accuracy records in vision tasks. This is best demonstrated by the SEER system that uses a large convolutional network trained with billions of examples.

## Modeling the uncertainty in prediction

![https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/148662482_469317657771087_6509708649537324681_n.png?_nc_cat=110&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=oZ2iEBMofskAX-Br_0U&_nc_ht=scontent-nrt1-1.xx&oh=4e685702e8fdb5b3ea65e3e14b71c7b8&oe=60707C09](https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/148662482_469317657771087_6509708649537324681_n.png?_nc_cat=110&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=oZ2iEBMofskAX-Br_0U&_nc_ht=scontent-nrt1-1.xx&oh=4e685702e8fdb5b3ea65e3e14b71c7b8&oe=60707C09)

To better understand this challenge, we first need to understand the prediction uncertainty and the way it’s modeled in NLP compared with CV. In NLP, predicting the missing words involves computing a prediction score for every possible word in the vocabulary. While the vocabulary itself is large and predicting a missing word involves some uncertainty, it’s possible to produce a list of all the possible words in the vocabulary together with a probability estimate of the words’ appearance at that location. Typical machine learning systems do so by treating the prediction problem as a classification problem and computing scores for each outcome using a giant so-called softmax layer, which transforms raw scores into a probability distribution over words. With this technique, the uncertainty of the prediction is represented by a probability distribution over all possible outcomes, provided that there is a finite number of possible outcomes.

In CV, on the other hand, the analogous task of predicting “missing” frames in a video, missing patches in an image, or missing segment in a speech signal involves a prediction of high-dimensional continuous objects rather than discrete outcomes. There are an infinite number of possible video frames that can plausibly follow a given video clip. It is not possible to explicitly represent all the possible video frames and associate a prediction score to them. In fact, we may never have techniques to represent suitable probability distributions over high-dimensional continuous spaces, such as the set of all possible video frames.

This seems like an intractable problem.

## A unified view of self-supervised methods

There is a way to think about SSL within the unified framework of an energy-based model (EBM). An EBM is a trainable system that, given two inputs, x and y, tells us how incompatible they are with each other. For example, x could be a short video clip, and y another proposed video clip. The machine would tell us to what extent y is a good continuation for x. To indicate the incompatibility between x and y, the machine produces a single number, called an energy. If the energy is low, x and y are deemed compatible; if it is high, they are deemed incompatible.

![https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/157272588_1389905731371138_8776386318723848066_n.png?_nc_cat=103&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=K0iuVzQHrTQAX8qFvra&_nc_ht=scontent-nrt1-1.xx&oh=d3d4572253e8c0099f0e462fdd901e31&oe=606FA02E](https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/157272588_1389905731371138_8776386318723848066_n.png?_nc_cat=103&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=K0iuVzQHrTQAX8qFvra&_nc_ht=scontent-nrt1-1.xx&oh=d3d4572253e8c0099f0e462fdd901e31&oe=606FA02E)

An energy-based model (EBM) measures the compatibility between an observation x and a proposed prediction y. If x and y are compatible, the energy is a small number; if they are incompatible, the energy is a larger number.

Training an EBM consists of two parts: (1) showing it examples of x and y that are compatible and training it to produce a low energy, and (2) finding a way to ensure that for a particular x, the y values that are incompatible with x produce a higher energy than the y values that are compatible with x. Part one is simple, but part two is where the difficulty lies.

For image recognition, our model takes two images, x and y, as inputs. If x and y are slightly distorted versions of the same image, the model is trained to produce a low energy on its output. For example, x could be a photo of a car, and y a photo of the same car that was taken from a slightly different location at a different time of day, so that the car in y is shifted, rotated, larger, smaller, and displaying slightly different colors and shadows than the car in x.

## Joint embedding, Siamese networks

A particular well-suited deep learning architecture to do so is the so-called Siamese networks or joint embedding architecture. The idea goes back to papers from Geoff Hinton’s lab and Yann LeCun’s group in the early 1990s ([here](https://l.facebook.com/l.php?u=https%3A%2F%2Fwww.nature.com%2Farticles%2F355161a0&h=AT05vV0wl8-ymnIP9tLB7exqY1dYgCoJmyhkNKYZ_5uffyRaqZef2-5zDqlpmg214RjiiLfEdKtagMbXieCfTpqTdCyTPwO_Ai1bNAg4bIG8iVBCQ7SPKT-AMhkfBD1DV0UE13rBfUHFlVxJzLMKEXXZCsc) and [here](https://l.facebook.com/l.php?u=https%3A%2F%2Fwww.worldscientific.com%2Fdoi%2Fabs%2F10.1142%2FS0218001493000339&h=AT3cUfJu8IEmA9GoTqNdguxuQfr87tRTCHAOU7r1FLo1C_URCXx0op_TL85oOVCMM3d_JQZKeHImkiS58JImAXA2PhOpZYACuYOj6MBqJE5VIiMAnBRIKbplhH-ccSLmyniSIzlcNIEpqgU50-sF2FxccaM)) and mid-2000s ([here](https://l.facebook.com/l.php?u=https%3A%2F%2Fproceedings.neurips.cc%2Fpaper%2F2004%2Fhash%2F42fe880812925e520249e808937738d2-Abstract.html&h=AT0Jzqnsltx0bAHsyUqrGaAKY21eoFBj7vXrxY7XFXn38ykexxvifvX_M8axzA_mq_syOn4LVFJCqFdGySG7C-XAQRGAHPmxWkfI1ArTzvP0sP-TYJpgQWy6VVrcXFQf1F9mIq3flg06L6TdDw3e8Z4aDcs), [here](https://l.facebook.com/l.php?u=https%3A%2F%2Fieeexplore.ieee.org%2Fabstract%2Fdocument%2F1467314&h=AT3iw6_7KTUwwBPaC5Y2F84lpYOqU-zKG5axbft_xI981ZFgt1l8RRc1Ef-KcFUFC92ZXy6sWqbE6rlOqGwzw0eU49S3QbeZTPd-Jood14Or6JwvPKCmZvOxP4yvfXcIX3AlBrQtXNCY31X_mmP6h-AO7QM), and [here](https://l.facebook.com/l.php?u=https%3A%2F%2Fieeexplore.ieee.org%2Fabstract%2Fdocument%2F1640964&h=AT0VVtlgl6tkyUvpX-NAgRhibMi8lMnL6bsYI7n_5NxZSMnABpV2lEfk0RTMvcIipebgDswSNtd2yeIWSPePDOoQp0m5WiTddMx5FxfkRfIDqG6MNJzVNPhse3Se_cLgY6ZheO2LmK5sDQx2v6DPthWZqlc)). It was relatively ignored for a long time but has enjoyed a revival since late 2019. A joint embedding architecture is composed of two identical (or almost identical) copies of the same network. One network is fed with x and the other with y. The networks produce output vectors called embeddings, which represent x and y. A third module, joining the networks at the head, computes the energy as the distance between the two embedding vectors. When the model is shown distorted versions of the same image, the parameters of the networks can easily be adjusted so that their outputs move closer together. This will ensure that the network will produce nearly identical representations (or embedding) of an object, regardless of the particular view of that object.

![https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/149794655_780907256142255_4794526832594825319_n.jpg?_nc_cat=105&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=EiHfJJU682kAX_OTfbJ&_nc_ht=scontent-nrt1-1.xx&oh=e62f54136abfae75858098037ecbbc4f&oe=60719435](https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/149794655_780907256142255_4794526832594825319_n.jpg?_nc_cat=105&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=EiHfJJU682kAX_OTfbJ&_nc_ht=scontent-nrt1-1.xx&oh=e62f54136abfae75858098037ecbbc4f&oe=60719435)

Joint embedding architecture. The function C at the top produces a scalar energy that measures the distance between the representation vectors (embeddings) produced by two identical twin networks sharing the same parameters (w). When x and y are slightly different versions of the same image, the system is trained to produce a low energy, which forces the model to produce similar embedding vectors for the two images. The difficult part is to train the model so that it produces high energy (i.e., different embeddings) for images that are different.

The difficulty is to make sure that the networks produce high energy, i.e. different embedding vectors, when x and y are different images. Without a specific way to do so, the two networks could happily ignore their inputs and always produce identical output embeddings. This phenomenon is called a collapse. When a collapse occurs, the energy is not higher for nonmatching x and y than it is for matching x and y.

There are two categories of techniques to avoid collapse: contrastive methods and regularization methods.

## Contrastive energy-based SSL

Contrastive methods are based on the simple idea of constructing pairs of x and y that are not compatible, and adjusting the parameters of the model so that the corresponding output energy is large.

![https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/127784209_262471582050413_7179678990272222350_n.gif?_nc_cat=104&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=Qw2ydPy1j7MAX-uPZLK&_nc_ht=scontent-nrt1-1.xx&oh=a720c6baeadc92181bacdb2fe8c3b850&oe=606F9307](https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/127784209_262471582050413_7179678990272222350_n.gif?_nc_cat=104&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=Qw2ydPy1j7MAX-uPZLK&_nc_ht=scontent-nrt1-1.xx&oh=a720c6baeadc92181bacdb2fe8c3b850&oe=606F9307)

Training an EBM with a contrastive method consists in simultaneously pushing down on the energy of compatible (x,y) pairs from the training set, indicated by the blue dots, and pushing up on the energy of well chosen (x,y) pairs that are incompatible, symbolized by the green dots. In this simple example, x and y are both scalars, but in real situations, x and y could be an image or a video with millions of dimensions. Coming up with incompatible pairs that will shape the energy in suitable ways is challenging and expensive computationally.

The method used to train NLP systems by masking or substituting some input words belongs to the category of contrastive methods. But they don’t use the joint embedding architecture. Instead, they use a predictive architecture in which the model directly produces a prediction for y. One starts for a complete segment of text y, then corrupts it, e.g., by masking some words to produce the observation x. The corrupted input is fed to a large neural network that is trained to reproduce the original text y. An uncorrupted text will be reconstructed as itself (low reconstruction error), while a corrupted text will be reconstructed as an uncorrupted version of itself (large reconstruction error). If one interprets the reconstruction error as an energy, it will have the desired property: low energy for “clean” text and higher energy for “corrupted” text.

The general technique of training a model to restore a corrupted version of an input is called denoising auto-encoder. While early forms of this idea go back to the 1980s, it was [revived in 2008](https://l.facebook.com/l.php?u=https%3A%2F%2Fdl.acm.org%2Fdoi%2Fabs%2F10.1145%2F1390156.1390294&h=AT0iy2KZaDRdop2QuB0FNn-TuQDr_PY8D194VrfRJZ659gzmCtZHXTi2ie5CtfRGlMd_HrBfRZgvjlv8LcrhKrSI9dljoDsZFdoCigXlXGnqYltf9nCSe5T1GOdD071b6mWffNirylURp3-MyYZBlcOmqIM) by Pascal Vincent and colleagues at the University of Montréal, introduced in the context of NLP by [Collobert and Weston](https://l.facebook.com/l.php?u=https%3A%2F%2Fwww.jmlr.org%2Fpapers%2Fv12%2Fcollobert11a.html&h=AT2ijGuIKPJkvG6A4SE_ZE5IsRcFqoxMbzGTw4_YZYHTpnjMgvsKM5IELfhc8EjBZhqqSVqkXtQj7SUKHMPHgQ_wQft6_mnv-wJm1P82ovLPz1Y0_VI8ezEAtxo2LRkPvdHnM8bk99u7luL1Hu12udy3uc8), and popularized by the [BERT paper](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fabs%2F1810.04805&h=AT31I_xZIh8igomUwEa56DrfuzVLvtwTeTEiaRm2i1LQXDYXzC41rQS3bp_ywXbRdrndEMcIvDFrI5boPnOnXtNqU77B8GVZlLQDd-zvqnhPFrzcGwy_vVtNKVU9j3wWPKuCCC8LAIXpHIqcgOuIoGfdhVE) from our friends at Google.

![https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/155526762_897797787644739_8022451761586606565_n.png?_nc_cat=108&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=v1xIgMFEa-sAX_NR5C9&_nc_ht=scontent-nrt1-1.xx&oh=a8dfd2ba7a9f2ddbe296f0df7eba3d4f&oe=60705F16](https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/155526762_897797787644739_8022451761586606565_n.png?_nc_cat=108&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=v1xIgMFEa-sAX_NR5C9&_nc_ht=scontent-nrt1-1.xx&oh=a8dfd2ba7a9f2ddbe296f0df7eba3d4f&oe=60705F16)

A masked language model, which is an instance of denoising auto-encoder, itself an instance of contrastive self-supervised learning. Variable y is a text segment; x is a version of the text in which some words have been masked. The network is trained to reconstruct the uncorrupted text.

As we pointed out earlier, a predictive architecture of this type can produce only a single prediction for a given input. Since the model must be able to predict multiple possible outcomes, the prediction is not a single set of words but a series of scores for every word in the vocabulary for each missing word location.

But we cannot use this trick for images because we cannot enumerate all possible images. Is there a solution to this problem? The short answer is no. There are interesting ideas in this direction, but they have not yet led to results that are as good as joint embedding architectures. One interesting avenue is latent-variable predictive architectures.

![https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/151779454_861787841053485_9032472471565149785_n.jpg?_nc_cat=106&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=cc4pkqqvFtsAX_GIs3Z&_nc_ht=scontent-nrt1-1.xx&oh=b5891bcaaf377ef489d8f486030ae8a7&oe=606F3303](https://scontent-nrt1-1.xx.fbcdn.net/v/t39.2365-6/151779454_861787841053485_9032472471565149785_n.jpg?_nc_cat=106&ccb=1-3&_nc_sid=ad8a9d&_nc_ohc=cc4pkqqvFtsAX_GIs3Z&_nc_ht=scontent-nrt1-1.xx&oh=b5891bcaaf377ef489d8f486030ae8a7&oe=606F3303)

A latent-variable predictive architecture. Given an observation x, the model must be able to produce a set of multiple compatible predictions symbolized by an S-shaped ribbon in the diagram. As the latent variable z varies within a set, symbolized by a gray square, the output varies over the set of plausible predictions.

Latent-variable predictive models contain an extra input variable (z). It is called latent because its value is never observed. With a properly trained model, as the latent variable varies over a given set, the output prediction varies over the set of plausible predictions compatible with the input x.

Latent-variable models can be trained with contrastive methods. A good example of this is a generative adversarial network (GAN). The critic (or discriminator) can be seen as computing an energy indicating whether the input y looks good. The generator network is trained to produce contrastive samples to which the critic is trained to associate high energy.

But contrastive methods have a major issue: They are very inefficient to train. In high-dimensional spaces such as images, there are many ways one image can be different from another. Finding a set of contrastive images that cover all the ways they can differ from a given image is a nearly impossible task. To paraphrase Leo Tolstoy’s Anna Karenina: “Happy families are all alike; every unhappy family is unhappy in its own way.” This applies to any family of high-dimensional objects, it seems.

What if it were possible to make sure the energy of incompatible pairs is higher than that of compatible pairs without explicitly pushing up on the energy of many incompatible pairs?

## Non-contrastive energy-based SSL

Non-contrastive methods applied to joint embedding architectures is possibly the hottest topic in SSL for vision at the moment. The domain is still largely unexplored, but it seems very promising.

Non-contrastive methods for joint-embedding include [DeeperCluster](https://l.facebook.com/l.php?u=https%3A%2F%2Fopenaccess.thecvf.com%2Fcontent_ICCV_2019%2Fhtml%2FCaron_Unsupervised_Pre-Training_of_Image_Features_on_Non-Curated_Data_ICCV_2019_paper.html&h=AT0lGsUnkJuqLTGwf4IilXCQSdkXDxkc-1tIlVMywb1TiAOI7QgaGHpt-LgYZaLGn6Z0JdGe6cklqw-gugrOTjTI0U8mX5DFVGUiITHDjojotorDRkA2BX5GxTtN0qzXdpOnpidbEFy9d7-6EqTgDRosBe8), [ClusterFit](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fabs%2F1912.03330&h=AT2gErfIVKIz0AQJmisT_3vzLwebVKnwEf7Jmb4BSdXt8oQCNlL6JseM3BAWk8G-oawbQQcddUvVcpGpAFJ2xBLSlpgeb8QsbuQFlQ1JRJBvXNTUjbiB7F1kJGLbvfZpE62uGKXvKR9MmWbhGTGJIMmg_zk), [MoCo-v2](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fabs%2F2003.04297&h=AT13BN44Ili715bIZ-9ERSdFXfElNdexuWjDCQCtNHFjX83ViV0ktfsmVBLJ4_XkKDgGxa7RxVg5crNtQwYOzcT5ImrkCTGxcb6cp532z0cZ1K2-0gO4kybzXNrVzlA7ntRT2ue0oJx4LXWw24nJgEm9o6k), [SwAV](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fabs%2F2006.09882&h=AT34EtJlZPzlYoVFNt3HGYT0L7i4P2er5UNiuS7vW1J9s4nRsU-Ly5EEtd6scF5DMI2rgqzdxsFzcXR71mifQHgL3VuKGNZjQ_oLoy9h8cjcm3WKVzUynb6Rf1rVsOYhuC1K7w9OVILn1IIqbo3AaduhJNGgbrY714pzCFCF), [SimSiam](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fabs%2F2011.10566&h=AT1EbBeittVqlUaApLvyl-pgmbHZO46cMCbSlKw2bPwqI1My4L5cj401n8A9NrIo5-0o8rRLosPZV2b4qexouK7xrjGeKkVrYYnLoihnaVURkx0i_kyixZWaVYNrhp1ItNjXjUizgfC73NbIR4bXFNeIuqM), Barlow Twins, [BYOL](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fabs%2F2006.07733&h=AT3bB9uJ-JmvSJl0lumFLkSdhJFmnbM7IlIFd5Aufs1U8H0MIQqPjgGJtPqUPfYYVh2WB6p5xkRASQpWcrLQicLAvFykWvcu_25bkTNYw6pJ4nj2Qx-6yXTQ1WujDha060x0moKHhb34cqarvQusxKGSxE4) from DeepMind, and a few others. They use various tricks, such as computing virtual target embeddings for groups of similar images (DeeperCluster, SwAV, SimSiam) or making the two joint embedding architectures slightly different through the architecture or the parameter vector (BYOL, MoCo). Barlow Twins tries to minimize the redundancy between the individual components of the embedding vectors.

Perhaps a better alternative in the long run will be to devise non-contrastive methods with latent-variable predictive models. The main obstacle is that they require a way to minimize the capacity of the latent variable. The volume of the set over which the latent variable can vary limits the volume of outputs that take low energy. By minimizing this volume, one automatically shapes the energy in the right way.

A successful example of such a method is the [Variational Auto-Encoder](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fabs%2F1312.6114&h=AT0LSo74Ry4dVnWT2ib7xuKHKfBG3vK4p02VWz4HwBBVlrE054Cd-pQNiVseCTqrtZH-6-35ystyZ83lX60u5x_l0WwluR6SN2SqtwULMnog76bHo0vbVUGUbldzTD1d0dNqRKLt5mLw2bbmkYsDqgOrWRw) (VAE), in which the latent variable is made “fuzzy”, which limits its capacity. But VAE have not yet been shown to produce good representations for downstream visual tasks. Another successful example is [sparse modeling](https://l.facebook.com/l.php?u=https%3A%2F%2Fwww.nature.com%2Farticles%2F381607a0&h=AT3bvD2Vlvu0QQIkeY1fbGP4hURuEYRC4g59AjajqLOOiMeBz9GPfbx1z-rTvqSUJSDANOozRNa3JG2wFNrYPGf2dbdq3c9ixxqJiJxxehwUqnCh3MlKSVWzM9Gjj2XZshO0au6qN7ZGgASzNwJ_9b9osuw), but its use has been limited to simple architectures. No perfect recipe seems to exist to limit the capacity of latent variables.

The challenge of the next few years may be to devise non-contrastive methods for latent-variable energy-based model that successfully produce good representations of image, video, speech, and other signals and yield top performance in downstream supervised tasks without requiring large amounts of labeled data.

## Advancing self-supervised learning for vision

Most recently, we’ve [created and open sourced](https://ai.facebook.com/blog/seer-the-start-of-a-more-powerful-flexible-and-accessible-era-for-computer-vision) a new billion-parameter self-supervised CV model called SEER that’s proven to work efficiently with complex, high-dimensional image data. It is based on the SwAV method applied to a convolutional network architecture (ConvNet) and can be trained from a vast number of random images without any metadata or annotations. The ConvNet is large enough to capture and learn every visual concept from this large and complex data. After pretraining on a billion random, unlabeled and uncurated public Instagram images, and supervised fine-tuning on ImageNet, [SEER outperformed the most advanced, state-of-the-art self-supervised systems, reaching 84.2 percent top-1 accuracy on ImageNet](https://l.facebook.com/l.php?u=https%3A%2F%2Farxiv.org%2Fpdf%2F2103.01988.pdf%3Ffbclid%3DIwAR2pqhYda6MV9r2b3Afx_0eKUiZhX-Es6Pa_FbLOqH8fglQzO2kY3yKxZE8&h=AT3oeBOnOiEqqnEhqdnng6gu8m4PZCAqD8mJmSaAt5NAzKWqb25TUfoKCWxSHENUx8xHNVOBsQ2UO1Rz-L_Bc5tVudMuhjoWVWbRUL4ccNvlY9t5Bx_hRmw_M55y79apxax_j8yoTXA2GFqQDx1zFnp1pFg).

These results show that we can bring the self-supervised learning paradigm shift to computer vision.

## Using self-supervised learning at Facebook

At Facebook, we’re not just advancing self-supervised learning techniques [across many domains](https://ai.facebook.com/blog/advances-in-content-understanding-self-supervision-to-protect-people/) through fundamental, [open scientific research](https://ai.facebook.com/blog/advances-in-content-understanding-self-supervision-to-protect-people/), but we’re also applying this leading-edge work in production to quickly improve the accuracy of content understanding systems in our products that keep people safe on our platforms.

Self-supervision research, like our pretrained language model [XLM](https://l.facebook.com/l.php?u=https%3A%2F%2Fgithub.com%2Ffacebookresearch%2FXLM%3Ffbclid%3DIwAR2Gqz_1SBcEXAVowtEOqRvN9Iveaci6Jwdvdy8yHDnlyjnfm91ZDREK6Rs&h=AT0eYnZVx3prg4YHeL22pV7-dw9V2CFxQ08Es75A1aWt8aCDBuMjYbWLUs0j04C6Teizxz5z44yBS9O-JhKCtLkvwIlm4rOWE9NKNV75w5y46BYgpFn3VYNO0NbMlOh6tFUIgbDkjbufFeXjbmlV51ZZUEI), is accelerating important applications at Facebook today — including [proactive detection of hate speech](https://ai.facebook.com/blog/how-ai-is-getting-better-at-detecting-hate-speech/). And we’ve deployed [XLM-R](https://ai.facebook.com/blog/-xlm-r-state-of-the-art-cross-lingual-understanding-through-self-supervision/), a model that leverages our [RoBERTa](https://ai.facebook.com/blog/roberta-an-optimized-method-for-pretraining-self-supervised-nlp-systems/) architecture, to improve our hate speech classifiers in multiple languages across Facebook and Instagram.This will enable hate speech detection even in languages for which there is very little training data.

We’re encouraged by the progress of self-supervision in recent years, though there’s still a long way to go until this method can help us uncover the dark matter of AI intelligence. Self-supervision is one step on the path to human-level intelligence, but there are surely many steps that lie behind this one. Long-term progress will be cumulative. That’s why we’re committed to working collaboratively with the broader AI community to achieve our goal of, one day, building machines with human-level intelligence. Our research has been made publicly available and published at top conferences. And we’ve organized workshops and released libraries to help accelerate the research in this area.

> 如果发现译文存在错误或其他需要改进的地方，欢迎到 [掘金翻译计划](https://github.com/xitu/gold-miner) 对译文进行修改并 PR，也可获得相应奖励积分。文章开头的 **本文永久链接** 即为本文在 GitHub 上的 MarkDown 链接。

---

> [掘金翻译计划](https://github.com/xitu/gold-miner) 是一个翻译优质互联网技术文章的社区，文章来源为 [掘金](https://juejin.im) 上的英文分享文章。内容覆盖 [Android](https://github.com/xitu/gold-miner#android)、[iOS](https://github.com/xitu/gold-miner#ios)、[前端](https://github.com/xitu/gold-miner#前端)、[后端](https://github.com/xitu/gold-miner#后端)、[区块链](https://github.com/xitu/gold-miner#区块链)、[产品](https://github.com/xitu/gold-miner#产品)、[设计](https://github.com/xitu/gold-miner#设计)、[人工智能](https://github.com/xitu/gold-miner#人工智能)等领域，想要查看更多优质译文请持续关注 [掘金翻译计划](https://github.com/xitu/gold-miner)、[官方微博](http://weibo.com/juejinfanyi)、[知乎专栏](https://zhuanlan.zhihu.com/juejinfanyi)。