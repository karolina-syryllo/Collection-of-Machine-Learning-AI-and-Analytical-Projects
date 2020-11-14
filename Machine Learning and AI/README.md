# Image Classification with Convolutional Neural Networks - ImageNet10 <h1> 

## 1. Testing the effect of the number of convolutional layers on training process and test performance. 

Assumptions: 

-Validation loss and training loss were used to monitor training process, while test accuracy was used to evaluate performance on test set. 

-While increasing the number of convolutional layers, number of outputs increased by 8 each, every time additional convolutional layer was added. 

  1.  Two Layers. 

![1st](https://user-images.githubusercontent.com/61549398/99155908-2d3eef00-26b4-11eb-8293-5d79fca9e668.jpg)
![1stconf](https://user-images.githubusercontent.com/61549398/99155953-b81fe980-26b4-11eb-98fc-f40281505938.png)

Fig.1: On the left: Training and validation loss, On the right: Confusion matrix.

 
With only two layers, we can notice that the network starts overfitting very quickly, just after first epoch. Overfitting means that the model performance of the training data set can be significantly higher as compared to the performance measured on examples the model had not seen before. In practice this phenomenon occurs when validation loss increases training loss. As we can see on the graph (see Fig. 1) training loss kept decreasing throughout the whole training while validation loss started increasing just after 1st epoch. Therefore, no further training was required. 

This further implies that the model’s generalization abilities on unseen examples are poor, what is confirmed by low test accuracy of 46%. 
In terms of confusion matrix, the easiest class to recognize was class 9 which represents an orange with 133 examples recognized correctly, while the most difficult class to recognize was class 7 (football). 

  2. Three layers

The training process looks better after adding third layer as overfitting is reduced. However, after 6th epoch validation loss increases and reached 1.626 after 10 epoch. The results suggest that no further training was required.
After adding additional layer, test accuracy increased from 46% to 49%.


![2nd](https://user-images.githubusercontent.com/61549398/99155995-2cf32380-26b5-11eb-8f21-640452edfda7.jpg)
![2ndconf](https://user-images.githubusercontent.com/61549398/99155997-2ebce700-26b5-11eb-927d-928627624f82.jpg)

Fig. 2: On the left: Training and validation loss, On the right: Confusion matrix.


By looking at the confusion matrix, we can see that again the easiest object to recognize was an orange (class 9), while the most difficult object was a desk (class 4) with only 20 images recognized correctly.

  3. Four layers

![3rd](https://user-images.githubusercontent.com/61549398/99156087-3466fc80-26b6-11eb-8076-dfbc0bc56f15.jpg)
![3rd conf](https://user-images.githubusercontent.com/61549398/99156086-3204a280-26b6-11eb-9dcc-78d62f18a345.jpg)

Fig. 3: On the left: Training and validation loss, On the right: Confusion matrix.

With four convolutional layers, validation loss is very similar to training loss throughout the entire training, therefore there are no overfitting issues.

The fourth layer allowed us to decrease validation loss to 1.441 which is the lowest validation loss obtained so far. Similarly, accuracy increased to 50%, what is also the best result as compared to two and three layers.

Again, we can see that the easiest recognizable item was and orange (class 9). The least recognizable object was this time class 6 (a drill).

  4. Five layers

After adding the fifth layer, it was advisable to increase the number of epochs as validation loss kept decreasing (see Fig. 4).

Test accuracy with 10 epochs was only 46%, however after longer training increased to 57%. It is lower than in case of 4 layers, but this might be a result of random weights initialization.

![4th](https://user-images.githubusercontent.com/61549398/99156100-4e084400-26b6-11eb-9b88-8471da4c8c53.jpg)
![4thconf](https://user-images.githubusercontent.com/61549398/99156102-4f397100-26b6-11eb-80c7-d404d6cbb653.jpg)

Fig.4: On the left: Training and validation loss, On the right: Confusion matrix

On the other hand, we can see that five layers and increased training time resulted in decreased validation loss of 1.253 at the end of the training what is the best result obtained so far. 

In terms of the confusion matrix, the easiest recognisable item was class 2 (canoe), although orange (class 9) was still easy to recognize for the network with 138 images recognized correctly. 

Summary: 

Performance of the network increases with each additional layer given appropriate training time is provided as the network starts overfitting at different points depending on the number of layers. We can conclude that additional number of convolutional layers increases the required number of epochs as validation loss keeps decreasing in line with training loss. Adding each additional layer reduces validation loss at the end of each training.
	
	
## 2. The effect of different convolutional layer’s kernel sizes. 
	
Five convolutional layers allowed us to obtain the lowest validation loss therefore such architecture was used in the second part to conduct further experiments with different kernel size. In this experiment, the same kernel size was used across all convolutional layers. 

	
  1. Kernel size 3x3 


![ker33](https://user-images.githubusercontent.com/61549398/99156298-c6bbd000-26b7-11eb-94c9-4a7822c9e061.jpg)
![cof33](https://user-images.githubusercontent.com/61549398/99156300-c91e2a00-26b7-11eb-842c-ca8d4ec0b416.jpg)


Fig.5: On the left: Training and validation loss, On the right: Confusion matrix.
	
Smaller kernel size is be able to capture fine details of the picture. It is also less computationally expensive as there are less weights to learn. However, in our case, reducing filter size to 3x3 resulted in an increase in validation loss to 1.396 (see Fig 5) in the last epoch and reduction in test accuracy to 53%. 

Orange remains the item predicted correctly the most.


  2. Kernel size 4x4 
	
Greater kernel size improved validation loss as compared to kernel size of 3x3 as validation loss was reduced from 1.396 to 1.306. However, test accuracy remained the same of 53%. 
Surprisingly, this time orange is not the easiest class to be recognized, instead the easiest object if class 4 and 2 (desk and canoe).

![ker44](https://user-images.githubusercontent.com/61549398/99156309-d9cea000-26b7-11eb-87f0-c276331c7b8d.jpg)
![conf44](https://user-images.githubusercontent.com/61549398/99156310-db986380-26b7-11eb-8efd-1cbf57a55e9e.jpg)

 
Fig.6: On the left: Training and validation loss, On the right: Confusion matrix

  3. Kernel size 5x5 
  
  
![ker55](https://user-images.githubusercontent.com/61549398/99156334-02569a00-26b8-11eb-9fe3-42f22bfc7bf8.png)
![conf55](https://user-images.githubusercontent.com/61549398/99156335-0387c700-26b8-11eb-9ca9-171e2c59bf85.png)

Fig.7: On the left: Training and validation loss, On the right: Confusion matrix 

Increasing kernel size was not beneficial in this case because this resulted in an increase in validation loss to 1.385 as compared to kernel size of 4x4 (see Fig. 7). However, test accuracy increased to 55%. 
Summary: 
In terms of overfitting, kernel size did not have any direct effect as in all cases validation loss decreases in line with training loss. However, there was an effect on test accuracy and the value of validation loss. 
After changing filter size in all layers to a different size, accuracy was lower in all cases as compared to the accuracy from the previous part which reached 57%. The same applies to validation loss which was higher after changing the filter size. 
While choosing the best architecture, I looked at the validation loss and tried to achieve the lowest possible validation loss. Therefore, I can conclude that the best architecture was the one obtained in the first part (see Fig. 4).



## 3. Filter visualisation

Filters before training

 
Filters during training
 
Filters after training
 
Fig.8: Visualisation of filters from

In convolutional neural networks, filters are learnable parameters and the goal of the training is to find appropriate set of weights to classify images correctly. The weights in neural networks are initialized randomly hence filters before training do not have any interpretation as they change every time the model is run. Throughout the training, the values of filters are adjusted using Stochastic Gradient Descent through Backpropagation. 
As we can see on the picture above (see Fig. 8), filters during training are similar to filters before training. We can easily notice that in many cases the general pattern of the filter remains the same with only few boxes (weights) changed slightly in terms of the brightness. This suggests that not all weights had to be updated. In few cases, weights are unchanged between and during training. 
Similarly, filters after training are similar to filters during and before training. However, the degree of similarity is greater between filters after training and during training as compared to filters after training and before training. This is caused by the fact that weights have been updated by smaller values within 10 epochs (during training) as compared to 20 epochs (after whole training).



## 4. Feature maps 


 


 

Fig.9: Feature maps.

In case of both pictures, feature maps resolution decreases with each additional layer as we can see on the picture above (see Fig.9). There is also a significant difference in the number of pixels in image from the first layer as compared to the fifth layer.



## 5. Best architecture 

Each dataset is unique and therefore there is no single answer to the question of how to improve network performance. Usually finding the best architecture is the result of conducting a number of experiments. Therefore, in this part I decided to conduct few additional experiments with several other parameters namely: number of outputs in convolutional layers, number of fully connected layers, number of outputs in fully connected layer and the number of epochs 
I started with the architecture obtained in part 1 (see Fig. 4) and I was changing each of the parameters one by one to analyse their effect on the network performance. 
Increasing and decreasing number of outputs in each convolutional layer did not result in higher accuracy. In most cases accuracy dropped and validation loss increased. 
The above statements also apply to increasing the number of fully connected layers. 
The best results I obtained by experimenting with different number of outputs in the first fully connected layer leaving the original number of fully connected layer as two. After decreasing number of outputs to 256, accuracy increased by 1% up to 59% which was the best results obtained up to that point.

 
Fig.10: On the left: Validation and training loss after decreasing number of outputs in fc1 to 256. 
On the right: Validation and training loss after decreasing number of outputs in fc1 to 256 and increasing number of epochs to 30.


We can see on the graph (see Fig. 10 left side) that there was no overfitting and validation loss kept decreasing what means that training can be extended. Therefore, in order to further increase the performance of the network, I increased the number of epochs from 20 to 30. This allowed me to obtain the lowest validation loss of 1.1672 and accuracy of 60%.


