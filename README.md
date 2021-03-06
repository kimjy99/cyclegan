CycleGAN: Horse to Zebra
=============
  
## Current update status  
- [X] Implement basic form of CycleGAN   
- [X] Train discriminators if their loss is bigger than criterion  
- [ ] Replay buffer for training discriminator  
- [ ] Different GAN loss (ex. WGAN-GP)  
  
## Model
### Generator: ResNet (9 Residual Blocks)
Encoder(Downsampling) → Transformer(Residual Block x9) → Decoder(Upsampling)  
  
Encoder:  
> Conv #1: ReflectionPad(3) → Conv(in_channels, 64, 7) → InstanceNorm → ReLU  
> Conv #2: Conv(64, 128, 3, 2, 1) → InstanceNorm → ReLU  
> Conv #3: Conv(128, 256, 3, 2, 1) → InstanceNorm → ReLU
  
Residual Block: 
> ReflectionPad(1) → Conv(256, 256, 3) → InstanceNorm → ReLU  
> → ReflectionPad(1) → Conv(256, 256, 3) → InstanceNorm → Add

Decoder:  
> Conv #1: ConvT(256, 128, 3, 2, 1, out_padding=1) → InstanceNorm → ReLU  
> Conv #2: ConvT(128, 64, 3, 2, 1, out_padding=1) → InstanceNorm → ReLU  
> Conv #3: ReflectionPad(3) → Conv(64, out_channels, 7) → Tanh
  
  

### Discriminator: PatchGAN (30x30)  
> Conv #1: Conv(in_channels, 64, 4, 2, 1) → LeakyReLU(0.2)  
> Conv #2: Conv(64, 128, 4, 2, 1) → BatchNorm → LeakyReLU(0.2)  
> Conv #3: Conv(128, 256, 4, 2, 1) → BatchNorm → LeakyReLU(0.2)  
> Conv #4: Conv(256, 512, 4, 1, 1) → BatchNorm → LeakyReLU(0.2)  
> Conv #5: Conv(512, 1, 4, 1, 1) → Sigmoid  

### Gan Loss: Mean Squared Error (= LSGAN)
  
------------------
## Output images during training
![output_img](./images/output_img.png)
  
------------------
## Loss vs. epoch
![loss_img](./images/loss_img.png)
  
------------------
## Test
![test_img](./images/test_img.png)
