# Object-Detection with robotic hand grasping images
    This obeject-detection models determine the x, y coordinates of a bounding box for an object in an image

**1. Architecture description**        
   - One of the image classfication models is deployed, which was used to classify if the robotic hand successfully grapsed an obeject or not. This model used Inception Resnet V2 and its pre-trained weights with Imagenet. Top layers were replaced with fully connected layers. Then, it was trained with grasp images. Details are described in https://github.com/u0953009/Binary-Classifier.  
   
   - For object detection, classifier layers (fully connected layers) of the classification model is taken off and four regression layers (4 ouputs) are put on the top of the model.  
   - Outputs return a location of a bounding box.  
     - **architecture 1, 2** :  
       x, y coordinates of top left vertex (Xmin, Ymin) and x, y coordinates of bottom right vertex (Xmax, Ymax).  
     - **architecture 3**    :  
       x, y coordinates of the center of a box, and the width and height.   
   - Architectures have different regression layers.  
     
     - **Architecture 1**  
     
       1. 1024 hidden units      
       2. output 1 : Xmin coordinate of a box, output 2 : Ymin coordinate of a box, output 3 : Xmax coordinate of a box, output 4 : Ymax coordinate of a box    
 
     
     - **Architecture 2**    
     
       1. 1024 hidden units + 512 hidden units  
       2. output 1 : Xmin coordinate of a box, output 2 : Ymin coordinate of a box, output 3 : Xmax coordinate of a box, output 4 : Ymax coordinate of a box  

       
      - **Architecture 3**    
     
        1. 1024 hidden units + 512 hidden units  
        2. output 1 : X coordinate of the center of a box, output 2: Y coordinate of the center of a box, output 3 : the width of a box, output 4 : the height of a box   

        
&nbsp;  

**2. Data description**  
   - Images used to train are photos, images extracted from videos of an Allegro robotic hand trying to grasp an object.  
    
     <img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/data_sample/2018-09-04-2036432018ral_img953.jpg" width="450" height="240"><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/data_sample/frame7954.jpg" width="450" height="240">  
     &nbsp;  
     
   - Using labelImg, https://github.com/tzutalin/labelImg, I labeled images.  
   
     <img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/data_sample/2018-09-04-2036432018ral_img953_boxed.jpg" width="450" height="240"><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/data_sample/frame7954_boxed.jpg" width="450" height="240">  
     &nbsp;  
     
   - Information about ground truth box is stored in xml files.  
   
     <img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/data_sample/2018-09-04-2036432018ral_img953_xml.jpg" width="650" height="400">  
     &nbsp;  
     <b></b>  
     
   - Data was augmented using imgaug, https://github.com/aleju/imgaug, to increase the number of data.  
   
     <img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/data_sample/image_augementation.png" width="350" height="100">  
     

&nbsp;  

**3. Training reports**  
   - 1633, 461, and 236 files were used to train, validate, and test respectively.  
   - 30 epochs were trained and, at each epoch, randomly augmented data was used to train.
   - Architecture 1  
     - output 1 : Xmin , output 2 : Ymin , output 3 : Xmax , output 4 : Ymax   
     - Loss function :  (Xmin_groundtruth - Xmin_predict)<sup>2</sup> + (Ymin_goundtruth - Ymin_predict)<sup>2</sup> + (Xmax_groundtruth - Xmax_predict)<sup>2</sup> +                                    (Ymax_groundtruth - Ymax_predict)<sup>2</sup>  
     - Total loss  
     <p align=center><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch1/loss.png" width="400" height="300"></p>  

     - Loss of each output  
     <p align=center><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch1/loss_1.png" width="400" height="300"><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch1/loss_2.png" width="400" height="300">  </p>    

     
     <p align=center><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch1/loss_3.png" width="400" height="300"><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch1/loss_4.png" width="400" height="300">  </p>     

     
   
   - Architecture 2  
     - output 1 : Xmin coordinate of a box, output 2 : Ymin coordinate of a box, output 3 : Xmax coordinate of a box, output 4 : Ymax coordinate of a box  
     - Loss function :  (Xmin_groundtruth - Xmin_predict)<sup>2</sup> + (Ymin_goundtruth - Ymin_predict)<sup>2</sup> + (Xmax_groundtruth - Xmax_predict)<sup>2</sup> +                                   (Ymax_groundtruth - Ymax_predict)<sup>2</sup>  
     - Total loss  
     <p align=center><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch2/loss.png" width="400" height="300"></p>  

     - Loss of each output  
     <p align=center><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch2/loss_1.png" width="400" height="300"><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch2/loss_2.png" width="400" height="300">  </p>    

     
     <p align=center><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch2/loss_3.png" width="400" height="300"><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch2/loss_4.png" width="400" height="300">  </p>     

   - Architecture 3  
     - output 1 : X coordinate of the center of a box, output 2: Y coordinate of the center of a box, output 3 : the width of a box, output 4 : the height of a box   
     - Loss function :  (Xcenter_groundtruth - Xcenter_predict)<sup>2</sup> + (Ycenter_groundtruth - Ycenter_predict)<sup>2</sup> +                                                                       (Width_groundtruth - Width_predict)<sup>2</sup> + (Height_groundtruth - Height_predict)<sup>2</sup>   
     - Total loss  
     <p align=center><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch3/loss.png" width="400" height="300"></p>  

     - Loss of each output  
     <p align=center><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch3/loss_1.png" width="400" height="300"><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch3/loss_2.png" width="400" height="300">  </p>    

     
     <p align=center><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch3/loss_3.png" width="400" height="300"><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/result/arch3/loss_4.png" width="400" height="300">  </p>     


     
&nbsp;  

**4. Conclusion**
   - Models couldn't locate a bounding box around an object. Models always locate a bounding box at a similar location. Below are some example images of test.  
      <p align=center><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/test_ex/1/2018-09-08-2141122018ral_img2018ral_img1047.jpg" width="400" height="300"><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/test_ex/1/IMG_20180907_144531phoneral1183.jpg" width="230" height="300">  </p>    

     
     <p align=center><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/test_ex/1/frame10024.jpg" width="400" height="300"><img src="https://raw.githubusercontent.com/u0953009/images/master/object_detection/test_ex/1/frame17597.jpg" width="400" height="300">  </p>     


   
   - Loss didn't decrease, but only fluctuated during training.
   - A model always returns similar outputs. As trained more, outputs converges to specific numbers.  
   

&nbsp;  

**5. Discussion**   
   - First, I tried to use selective search and object classifier to detect an object. However, performing selective search on an image took too long. Sometimes, it didn't finish the job in 20 minutes. So, instead of using selective search, I switched to regression approach.    
   - As architecture 1 cannot predict a bounding box correctly, I tried to increase the number of parameters (architecture 2) and change the output form (architecture 3). However, there were no positive changes in the result.  
   - Loss functions provided in Keras didn't support correctly in this project, so I used a custom loss function. Details are explained in the code.
   - Referring to existing algorithms could help. There are a few structural differences between models used here and existing ones such as Fast R-CNN, Faster R-CNN. If I can correctly adopt a part of those R-CNN, it may help to improve on detecting an object.  
   
   
     &nbsp;  

**6. Instructions on how to start**
   - I used Google Colab in which I worked on ipynb files. Instructions to process data and modify, train, and test models are explained in the files.  
   
   
     
     &nbsp;  
     
     
## Files     
Training data, images and xml files - https://drive.google.com/file/d/1yC87SJnkowEPZ3XoBZ0uQTbGUGmKJbs2/view?usp=sharing  
Pretrained model, classifier - https://drive.google.com/file/d/1_rVIcU6sBRS321dwnTO3htf83C7p3jY3/view?usp=sharing  
Architecture 1 model - https://drive.google.com/file/d/1bfLuINSketF60b3w75SYLAc4z67l8tZE/view?usp=sharing  
Architecture 2 model - https://drive.google.com/file/d/1-04jScfW--7BJrme8cUBeHQg2cwvtfrx/view?usp=sharing  
Architecture 3 model - https://drive.google.com/file/d/1Wsf8SF1XobpRsfl2Gb4-n9F8dJ0NGy4w/view?usp=sharing  





     
     
     
     
     
      
     
     

                
                
    
