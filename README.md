# System-Development-CNN-Master-Thesis

## Description
This is my repository for the final project (or thesis) I did in my Master's degree of Computer Science based on Convolutional Neural Networks (also known as CNNs).
Here I am going to describe the different phases I passed through as well as the different notebooks used in the project. For more detailed information, the memory I wrote down is here https://academica-e.unavarra.es/handle/2454/39829 (only in Spanish right now).

The main goal of this final project was to create an embedding system combined with a real time industry application in C++ alongside the powerful architectures designed for computer vision, the CNNs. All of that would be deployed in a real environment for an omelette producer in Navarre, Spain.

This was designed and deployed as a need of the omelette producer to improve their quality process. In particular, they aimed for a system that was able to classify incorrect printing labels from their line of production. So that was clearly a binary classification task with a huge variety of labels (types of omelettes, different printing mistakes, different printing languages and so on). 

Thus the main problem was divided in two different ones: a ML problem with an omelette images dataset looking for the best model to classify correctly all the different labels printing and other task embedding all the hardware and software involved into a system to make it work in a production line.

For the Machine Learning task, I used Tensorflow and Keras to develop the neural networks. Specifically, I tried a Basic CNN, VGG16 and VGG16 with Fine-Tuning to compare their results. Besides that, my first intuition was to develop a model to do well on a single omelette reference. If that was the case, I would try to develop a more generalist model to do it well on all the printing labels. For this ML task, I also had to preprocess the data, label the data manually (hard stuff, trust me) and reduce the size of each image before starting off.

For the embedding task, I created a C++ application to integrate the libraries for the camera used (Basler camera with Pylon SDK) and the Python funcionalities by means of the Python/C API to be able to get the inference result from the model already trained and put all together into a single application. This main application has several functionalities:
+ To take pictures from the camera to create a dataset of omelette images
+ To set up the parameters of the own camera
+ To return a result from the model trained to distinguish a correct from an incorrect printing label from the current image in the system


## Code
In the repository there are a few notebooks I was developing throughout the project in the ML task. Specifically:
+ _Preprocessing.ipynb_: Notebook to renaming the images, converting to .jpg, reducing dimensions and splitting them into train, val and test.
+ _CNNs_Training_Inference.ipynb_: Main notebook of the project. It contains the steps for loading the images, building the models, training phase, plotting learning curves and results as well as the option to download the wrong predictions and visualizing the activations of the hidden layers. 

To keep in mind:
+ I also used another software to remove the background from the images within the preprocessing step but it is not shown here.
+ The main notebook (_CNNs_Training_Inference.ipynb_) contains the process and results for the global reference. The notebook for the most common omelette reference is quite similar, so I decided not to publish it here.
+ I truly believe that the ML part of this project is the spotlight, so I did not upload any C++ code of the main application. If someone is really interested, feel free to talk to me!

## Results, Conclusions and Improvements
The results obtanied were reasonable good. For a single omelette reference (the most common) I had just 3255 images to play with and the metrics were quite awesome. In the test set (around 488 images from the 3255 images) I got the following metrics:

![image](https://user-images.githubusercontent.com/18461107/135482957-b78f9bf9-152a-4674-8c6b-e8cde18ccc89.png)

So I decided to go over all references (7559 images) and the best model (VGG16 with Fine-Tuning) achieved the following metrics:

![image](https://user-images.githubusercontent.com/18461107/135483014-e324df3e-d280-40eb-b610-db2f80b6a551.png)


So glad to see that after hard work!

In conclusion:
+ I ended up completing the project for the deadline (not always trivial) and with some great results.
+ I want to highlight the lack of variety and quantity of data which made the main goal harder to achieve.
+ The installation and configuration of the drivers, libraries and so on was one of the most consuming tasks.
+ The Fine Tuning process is incredible useful specially in computer vision tasks and when you are limited by the data.

Future improvements (or things to keep in mind):
+ The size of the details from the printing labels forced the dimensions of the images to be quite big and using Google Collab (once again 😅) limited the iteration process. Each epoch was timeless. Trying a bigger (or different) model or training longer should help the performance. Using more computational resources could have improved this point.
+ The unbalanced dataset presented surely impacts the results. Oversampling or undersampling techniques are always a good try.
+ I could not load the model beforehand in the inference step, so this was too slow to test in a real environment. Changing to different API may speed it up.
