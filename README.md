# Fourier-Wavelet-and-Feature-extraction
## 1. Frequency domain processing
One can see that the input image has lines flowing across it and these are due to the presence of periodic noise in the image. These periodic noises can be removed using a band reject or notch filters to filter out the specific range of frequencies that can filter out these periodic noises. 

\
The image is first converted into the frequency domain using FFT and then shifted to center the DC component of the image. Various band reject filters and notch filters were implemented as per their formulae and various values of the input parameters were experimented with. A desired kernel can be applied in the frequency domain by simply multiplying with the frequency transformed image. Once this is done, the image is converted back to time domain by performing inverse FFT and Inverse shifting. The image is then displayed. 

\
Since the two images provided have different kind of periodic noises, various kernels with different parameters were tried. For both of them, gaussian kernel with the parameters specified in the code yielded he best results. 

## 2. Wavelet Domain Processing
The input image is first applied with a 2-level DWT using the pywt module. The coeffecients are obtained after this process. If we are dealing with a salt and pepper noise, a median filter is applied by basic convolution on each and every wavelet. This process is one of the most effective ways to remove salt and pepper noise from the image based on the paper: https://arxiv.org/ftp/arxiv/papers/1703/1703.06499.pdf
When a median blur is used, it removed the salt and pepper nooise, but the reusltinng image is blurred and also lost all its contrast. However, this can be corrected by histogram eqalization which is not the scope of this assignment. 

\
The remaining steps are the same for gaussian noise and the salt and pepper noise. Sigma_x is obtained by taking the median of the HH1 wavelet. After this, sigma_w is taken for each and every wavelet exccept LL2 based on the provided formula. Once this is done, the thresholding function provied by pywt is used to threshold each and every wavelet. Once this is done, the wavelets can again be combined to get the reconstructed image. 

## 3. 3. Feature Extraction, Description and image stitching
OpenCv helps in providiing the feature detection and descriptions using SIFT algorithm. Keypoints are the important feature points found out by SIFT and the descriptors are the 128 length feature vectors given for each corresponding points.

\
A KNN algorithm was used to find the corresponding points between both the images and to find a distance was set as threshold to compare the differences in features. Two neighbrs were used in order to comapre the two feature with the lowest and second lowest distance from one another. These matches are called good matches and are stored for computing the homography matrices in the next step

\
The homography matrix is found out based on the good matches from the previous steps. This matrix is essential to convert the image from one reference frame to another. This homography matrix is used to convert the input image into the reference frame of the reference image.

\
Once the input image is warped into the reference frame, the images are then alligned and overlayed on one another. We can see from the output image that when they are overlayed on one another, the corresponding points overlap and hence an image mosaic is created which covers all the parts of both images.
