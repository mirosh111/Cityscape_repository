
## City scape 2D images segmentation in PyTorch

 

### Requirements

* pytorch >=0.4.0
* torchvision ==0.2.0
* scipy
* tqdm
* tensorboardX

#### One-line installation
    
`pip install -r requirements.txt`

### Data

* [Cityscapes](https://www.cityscapes-dataset.com/)


### Usage

**Setup config file**

```yaml
# Model Configuration
model:
    arch: <name> [options: 'fcn[8,16,32]s, unet, segnet, pspnet, icnet, icnetBN, linknet, frrn[A,B]'
    <model_keyarg_1>:<value>

# Data Configuration
data:
    train_split: <split_to_train_on>
    val_split: <spit_to_validate_on>
    img_rows: 512
    img_cols: 1024
    path: <path/to/data>
    <dataset_keyarg1>:<value>

# Training Configuration
training:
    n_workers: 64
    train_iters: 35000
    batch_size: 16
    val_interval: 500
    print_interval: 25
    loss:
        name: <loss_type> [options: 'cross_entropy, bootstrapped_cross_entropy, multi_scale_crossentropy']
        <loss_keyarg1>:<value>

    # Optmizer Configuration
    optimizer:
        name: <optimizer_name> [options: 'sgd, adam, adamax, asgd, adadelta, adagrad, rmsprop']
        lr: 1.0e-3
        <optimizer_keyarg1>:<value>

        # Warmup LR Configuration
        warmup_iters: <iters for lr warmup>
        mode: <'constant' or 'linear' for warmup'>
        gamma: <gamma for warm up>
       
    # Augmentations Configuration
    augmentations:
        gamma: x                                     #[gamma varied in 1 to 1+x]
        hue: x                                       #[hue varied in -x to x]
        brightness: x                                #[brightness varied in 1-x to 1+x]
        saturation: x                                #[saturation varied in 1-x to 1+x]
        contrast: x                                  #[contrast varied in 1-x to 1+x]
        rcrop: [h, w]                                #[crop of size (h,w)]
        translate: [dh, dw]                          #[reflective translation by (dh, dw)]
        rotate: d                                    #[rotate -d to d degrees]
        scale: [h,w]                                 #[scale to size (h,w)]
        ccrop: [h,w]                                 #[center crop of (h,w)]
        hflip: p                                     #[flip horizontally with chance p]
        vflip: p                                     #[flip vertically with chance p]

    # LR Schedule Configuration
    lr_schedule:
        name: <schedule_type> [options: 'constant_lr, poly_lr, multi_step, cosine_annealing, exp_lr']
        <scheduler_keyarg1>:<value>

    # Resume from checkpoint  
    resume: <path_to_checkpoint>
```

**To train the model :**

```
python train.py [-h] [--config [CONFIG]] 

--config                Configuration file to use
```

**To validate the model :**

```
usage: validate.py [-h] [--config [CONFIG]] [--model_path [MODEL_PATH]]
                       [--eval_flip] [--measure_time]

  --config              Config file to be used
  --model_path          Path to the saved model
  --eval_flip           Enable evaluation with flipped image | True by default
  --measure_time        Enable evaluation with time (fps) measurement | True
                        by default
```

**To test the model w.r.t. a dataset on custom images(s):**

```
python test.py [-h] [--model_path [MODEL_PATH]]
               [--dcrf [DCRF]] [--img_path [IMG_PATH]] [--out_path [OUT_PATH]]
 
  --model_path          Path to the saved model
  --dcrf                Enable DenseCRF based post-processing
  --img_path            Path of the input image
  --out_path            Path of the output segmap
```


