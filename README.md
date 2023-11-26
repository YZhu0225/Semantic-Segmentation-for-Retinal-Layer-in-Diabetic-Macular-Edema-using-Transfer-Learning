# YJ_661_final_project

Set up envrionment: https://mmsegmentation.readthedocs.io/en/latest/get_started.html

Basic Requirement
● Setup environment following mm-segmentation’s documentation, which is the library that will provide you access to many pre-trained segmentation models (to be used as the backbone), as well as the functionalities needed for transfer learning.
● Download and process the DME dataset using online code, and follow the example to visualize some of the images as an initial examination of the dataset, as well as document the core statistics (e.g., how many images in total, and what are the image resolution). Then, process them in the format that can be read in by mm-segmentation, following the corresponding documentation.
● Choose at least two semantic segmentation models as provided by mm-segmentation’s documentation, and evaluate their performance on the ophthalmic dataset, without fine-tuning.
● Based on your observation above, choose the appropriate model to be fine-tuned specifically toward achieving better segmentation performance. Unlike typical classification where the main metrics are accuracy, recall, precision etc. Segmentation outcomes are usually evaluated by metrics like intersection-of-union (IoU) score etc. to provide a holistic view of how accurate the pixel-wise classification would be. Refer to documentation and choose a few metrics that you target to improve during fine-tuning.