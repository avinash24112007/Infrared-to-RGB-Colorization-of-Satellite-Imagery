# Multi-Band Fusion for Infrared-to-RGB Colorization of Satellite Imagery

Remote sensing satellites frequently rely on Infrared wavelength spectrum (IR) sensors to collect environmental conditions over an area during night time  or under adverse weather conditions where Visible spectrum sensors fail to capture most of the information.

However Infrared images are inherently monochrome and suffer from low contrast and structural ambiguity relative to RGB imagery. Making the inference of the geographical features like forests, rivers, mountains, lakes, etc. by human analysts and computer vision models difficult. 

Therefore, translation of an Infrared image to Visible Spectrum is an important image translation task in satellite remote sensing.  Infrared image colorization is an ambiguous problem because it involves converting the individual pixels which represent a temperature, high or low to a realistic RGB pixel. The model needs to learn a special mapping from the source domain (IR band) to Target Domain (RGB band).

Generative Adversarial Networks (GANs) have received much attention in image to image translation tasks and have achieved impressive results. Generic approaches like the Pix2Pix and CycleGAN have achieved great results in translation of high level details in images in successfully, however they fail to produce satisfactory results in translating IR images failing to capture the low level details in RGB imagery. 

We propose a multi band input strategy using IR + NIR + SWIR1 + SWIR2 giving more semantic information to the generator model to disambiguate colors and capture the underlying low level details to convert them to RGB. 

## Related Work

Pix2Pix (Isola et al., 2017) and CycleGAN (Zhu et al., 2017) established conditional GAN approaches for general-purpose image-to-image translation, using paired and unpaired training respectively. While effective for tasks such as sketch-to-photo and style transfer, these architectures were designed for natural images and do not explicitly account for the spectral ambiguity of infrared-to-visible translation, where a single monochrome intensity value must be disambiguated into three correlated RGB channels.

## Dataset

We collected 20 different Landsat 8/9 scenes around the Gujarat, India region. Each scene covers an area of 110 km x 110 km.  IR, NIR, SWIR1, and SWIR2 bands were used as model input, while R, G, and B bands served as the target output for supervised training.

Each band was normalized using a 2nd–98th percentile stretch to mitigate the influence of sensor outliers and saturation, followed by clipping to [0, 1]. Each scene was then processed to extract patches of 256 x 256 pixels with stride of 256 which produced ~ 530 patches for each scene. 11262 pairs of patches forms the training dataset.

 _ scenes consisting of _ patches were excluded from testing for testing the final model. 

## Architecture

We have utilized a standard pix2pix architecture involving a U-Net as generator and PatchGAN as the discriminator trained with a combination of loss functions such as 

- L1 Loss - It computes the per pixel difference between the generated image and target image and pushes the generator to produce average plausible images which keeps it from generating wild, unrelated colors.
- Adversarial Loss (Binary Cross Entropy) — Following the standard Pix2Pix architecture (Isola et al.), the discriminator learns to distinguish real images from generated ones, providing the generator a signal to correct blurry or averaged-looking outputs that L1 loss alone tends to produce
- Cosine-Color loss - It pushes the generator to produce the images with spectral accuracy.

The losses adversarial loss, L1 and cosine color loss are weighted 1:100:5

## Results

Our model has achieved :

Mean PSNR of 24.690466 and mean SSIM of 0.8265621 over 11169 individual patches from 20 different scenes from the training dataset. 

Mean PSNR of _ and mean SSIM of _ over _ individual patches from _ different scenes from the test dataset.
