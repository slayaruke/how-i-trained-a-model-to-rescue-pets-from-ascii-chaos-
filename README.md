# pet segmentation from ascii images

This repository contains my baseline for the NEOAI 2026 Terminal Cuties Segmentation competition.

## a small clarification

The organizers provided the task, dataset, evaluation setup, statistical context, and a starter baseline. I used that baseline as a starting point instead of pretending this appeared from a completely blank notebook.

I wrote and adapted the Kaggle code myself, including the preprocessing, compact segmentation model, training loop, validation, inference, and submission pipeline.

I am currently studying statistics independently. I use the statistical terms that the problem actually needs, but I am not pretending I have already mastered the whole subject. learning in public, basically.

Public Dice: **0.3048**

Result: **68th overall and around top 2-3 in the team selection**

## approach

The original images are 1280 by 1280 pixels, but each ASCII character occupies a fixed 10 by 10 block. I resize the images and masks to a 128 by 128 character grid before training.

The model is a small fully convolutional encoder-decoder trained from random initialization. I kept it simple cuz the goal was to build a clear baseline before testing more complex architectures. It is tiny, but at least it trains fast. hohoho.

## training

- RGB values are normalized to the 0 to 1 range
- masks are resized with nearest-neighbor interpolation
- random horizontal flips are used for augmentation
- the loss function is `BCEWithLogitsLoss`
- the optimizer is Adam
- training runs for four epochs

No pretrained weights or external data are used.

## submission

Predictions are thresholded at 0.5, resized back to 1280 by 1280, and converted to column-major run-length encoding. The notebook saves the final result as `submission.csv.gz`.

## limitations

The model has no skip connections, so some spatial detail is lost in the encoder. The next improvements I would test are skip connections, Dice loss, stronger augmentation, and threshold tuning.

## files

- `pixelpaws-lite-baseline.ipynb`: training, validation, inference, and submission code
- `README.md`: project overview

## running the notebook

Add the official competition dataset to Kaggle, enable a GPU, and run all cells in order.
