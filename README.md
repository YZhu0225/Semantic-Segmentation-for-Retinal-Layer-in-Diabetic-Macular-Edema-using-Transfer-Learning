# Transfer Learning for Domain Adaptation
Adapting Semantic Segmentation Models to Medical Imaging for Retinal Layer Analysis in Diabetic Macular Edema

Team Member: Xiaoquan Liu, Yuanjing Zhu

## Project Overview

This project focuses on optimizing pre-trained semantic segmentation models for fluid segmentation in optical coherence tomography (OCT) images, specifically focusing on diabetic macular edema (DME). Utilizing a public OCT dataset, we first demonstrate that the improvements in segmentation accuracy are highly task-dependent in transfer learning. Initial results without fine-tuning show low mean intersection over Union(mIoU) scores, indicating the need for further model adjustments. Then, we explore various fine-tuning techniques and employ data augmentation strategies and loss functions to enhance model performance. The results demonstrate significant improvements in segmentation accuracy, as indicated by substantial increases in both mIoU and mean Dice Coefficient (mDice) scores. These experiments underscore the potential of domain-adapted network tuning to achieve high accuracy and reliability in medical image segmentation, even with limited representative training data.

## Methodology

### Data

We utilized a public medical [OCT dataset](https://www.kaggle.com/datasets/paultimothymooney/chiu-2015/code) focused on diabetic macular edema, comprising 610 images, including 110 manually annotated ones for precise pixel-class segmentation. Key attention was given to these annotated images for accurate analysis. The dataset featured examples with and without diabetic macular edema, showcasing varied retinal layer conditions. We simplified the dataset by merging 13 fluid labels into a single category and resized the images from 468x796 to 256x256 pixels, converting them to .png format. This was done for compatibility with the MMSegmentation library. The dataset was then divided into training and testing sets with an 80:20 ratio for effective model training and evaluation.

### Workflow
![image](https://github.com/nogibjj/yj_xq_661_final_project/assets/110933007/5dbb3fbb-c7a2-434a-9233-e0b1a837772e)


In our research, we preprocessed a medical OCT dataset by resizing images and splitting them into training and testing sets. We largely leveraged the [MMSegmentation library](https://github.com/open-mmlab/mmsegmentation/tree/main), a versatile toolkit for semantic segmentation that contains high-quality implementations of popular semantic segmentation methods and datasets. The Unet architecture was chosen for its effectiveness in segmentation tasks, and we experimented with different decoder heads (FCN, PSPNet, DeepLabV3), selecting the one with the best performance on our dataset. To enhance the model's generalization ability, we implemented data augmentation techniques like random cropping, horizontal flipping, and random rotation. We also explored various loss functions (cross-entropy, Dice, and focal loss) and experimented with SGD and Adam optimizers, adjusting regularization parameters to optimize the model for precise segmentation of OCT images. Finally, the fine-tuned model was evaluated by mIoU and mDICE scores. 

## Results

To improve our model's performance, we systematically tuned training parameters, increasing the number of iterations for better convergence and data comprehension. We employed data augmentation techniques like random horizontal flipping and variable crop sizes to enhance robustness. To address class imbalances in the OCT dataset, especially in fluid accumulation areas, we incorporated Dice and focal loss functions, known for their effectiveness in such scenarios. We also experimented with various optimizers and implemented L2-regularization to prevent overfitting. The optimal configuration, involving cropping images to 320x320 without flipping, using cross-entropy loss for 400 iterations, the SGD optimizer, and 0.0001 L2 regularization, resulted in an mIoU score of 0.7600 and an mDice score of 0.8430. The figure below shows the comparison between the initial and optimized models, highlighting the advancements achieved through this fine-tuning process.

|   # of Iter  |    Crop Size     |   Random Flip  | Loss Function  |    Optimizer   |   $L_2$ reg.   |     mDice    |     mIoU     |
| :---         |     :---         |     :---       |:---            |     :---       |     :---       |     :---     |     :---     |
| 200          | False            | False          |Cross Entropy   | SGD            | 0.0005         | 0.7721       |   0.6855     |
| 400          | False            | False          |Cross Entropy   | SGD            | 0.0005         | 0.7173       |   0.6371     |
| 400          | False            | Horizontal     |Cross Entropy   | SGD            | 0.0005         | 0.7040       |   0.6263     |
| 400          | 192 $\times$ 192 | False          |Cross Entropy   | SGD            | 0.0005         | 0.6951       |   0.6190     |
| 400          | 224 $\times$ 224 | False          |Cross Entropy   | SGD            | 0.0005         | 0.7106       |   0.6299     |
| 400          | 320 $\times$ 320 | False          |Cross Entropy   | SGD            | 0.0005         | 0.8401       |   0.7567     |
| 400          | 320 $\times$ 320 | False          |Dice Loss       | SGD            | 0.0005         | 0.8395       |   0.7560     |
| 400          | 320 $\times$ 320 | False          |Focal Loss      | SGD            | 0.0005         | 0.8280       |   0.7429     |
| 400          | 320 $\times$ 320 | False          |Cross Entropy   | Adam           | 0.0005         | 0.4981       |   0.4963     |
| **400**      | **320 $\times$ 320** | **False**  |**Cross Entropy** | **SGD**      | **0.00001**    | **0.8430**   |   **0.7600** |
| 400          | 320 $\times$ 320 | False          |Cross Entropy   | SGD            | 0.001          | 0.8312       |   0.7464     |

![image](https://github.com/nogibjj/yj_xq_661_final_project/assets/110933007/e43f2252-0e46-441a-9b9a-fcbff5141c47)
*Comparative model inference on a OCT Image employing the preliminary and fine-tuned models.(Left) Preliminary inference result. (Middle) Fine-tuned inference result. (Right) Ground truth.*


## How to reproduce

1. Copy the repo: ```git clone https://github.com/nogibjj/yj_xq_661_final_project.git ```
2. Setup environment following mm-segmentation’s documentation: https://mmsegmentation.readthedocs.io/en/latest/get_started.html
3. Preprocess the data: click "Run All" on the [src/0_prepare_data.ipynb](https://github.com/nogibjj/yj_xq_661_final_project/blob/main/src/0_prepare_data.ipynb) Jupyter Notebook.
4. Make initial inference without fine tuning: click "Run All" on the [src/1_inferencer.ipynb](https://github.com/nogibjj/yj_xq_661_final_project/blob/main/src/1_inferencer.ipynb) Jupyter Notebook.
5. Make inference with fine-tuned model: click "Run All" on the [src/2_4_fine_tune_unet_sgd_l2small.ipynb](https://github.com/nogibjj/yj_xq_661_final_project/blob/main/src/2_4_fine_tune_unet_sgd_l2small.ipynb) Jupyter Notebook.


## Reference

[1] Navab, N., Hornegger, J., Wells, W. M., & Frangi, A. (Eds.). (2015). Medical Image Computing and Computer-Assisted Intervention–MICCAI 2015: 18th International Conference, Munich, Germany, October 5-9, 2015, Proceedings, Part III (Vol. 9351). Springer.

[2] Amin, J., Sharif, M., Yasmin, M., Saba, T., Anjum, M. A., & Fernandes, S. L. (2019). A new approach for brain tumor segmentation and classification based on score level fusion using transfer learning. Journal of medical systems, 43, 1-16.


[3] Ghafoorian, M., Mehrtash, A., Kapur, T., Karssemeijer, N., Marchiori, E., Pesteie, M., ... & Wells, W. M. (2017). Transfer learning for domain adaptation in MRI: Application in brain lesion segmentation. In Medical Image Computing and Computer Assisted Intervention− MICCAI 2017: 20th International Conference, Quebec City, QC, Canada, September 11-13, 2017, Proceedings, Part III 20 (pp. 516-524). Springer International Publishing.
