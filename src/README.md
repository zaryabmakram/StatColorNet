# Process Documentation 

## 1. Data Exploration
The project was started with exploring the dataset by plotting several samples and their respective labels. Following were a few of the key findings of this step: 

- type of data images (texture/ background images)
- multiple label values (both for **state** and **color**)
- missing state/ color (included json files may have missing keys)

Since there can be multiple values for color or state, respective to the input image, the problem has been framed as a `Multi-Label Classification` i.e. **Sigmoid()** output with **Binary Cross Entropy** loss function. 

## 2. Data Preprocessing 
To feed the data to a neural network, one of the possible approaches used is to convert the labels (color and state) as `One-Hot encodings` according to the different possible values of colors and states for the images included in the dataset. 

In order to achieve this step, the whole data has been processed into a single pandas-dataframe (since the size of the total dataset is quite small) with paths of the respective images included. 

During One-Hot encoding, the images with missing state or color have been set to 0 (not discarding those samples).

## 3. Model Experiments 
The first step was to form different utilities such as Data Loader, training, evaluation and plotting methods.  

Since the original image sizes were quite large, the images were resized to (224, 224) which were normalized using the standard mean and std used by COCO dataset.

The first model to be experimented with was designed inspired from **VGG-16** (not exactly similar). This model would not only act as a baseline but would also used to test and debug different training utility methods. Adam was used as a default optimizer. 

The training curves from this experiment suggested that minimum training loss achieved was in the range of `0.2xx` and had plateaued. In order to further decrease the training loss, following steps could be taken: 
- Train Longer (training loss has already plateaued) 
- Try a deeper network

So, as a next step, two deeper models were experimented with: 

- TransferNet (resnet18)
- DeepTransferNet (resnext50_32x4d) 

Pretrained models were used for the above-mentioned experiments with their fully-connected layers replaced according to the number of classes required for the problem at hand. 

Initially, there was a huge different between the reported training loss and validation loss. In order to tackle this issue, regularization was added to the models through **Dropout** with the probability of an element to be zeroed set to 0.3.

The following two experiments resulted better than the baseline approach as per the assumptions, with the DeepTransferNet resulting slightly better than the TransferNet.