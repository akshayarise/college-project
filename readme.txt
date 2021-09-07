//fist Recognition using CvHaarClassifierCascade in opencv-2.4.8 in ubuntu-12.04 using c++ language.

// The data for this project is present at https://cloudinary.com/console/c-c4a0ebbf064643e73cc618faa49550/media_library/folders/home
// data on cloudinary is present in folders such as positive, negative, Samples, rawdata.

Cascade Classification
Haar Feature-based Cascade Classifier for Object Detection

The object detector described below has been initially proposed by Paul Viola Viola01 and improved by Rainer Lienhart Lienhart02 . First, a classifier (namely a cascade of boosted classifiers working with haar-like features ) is trained with a few hundred sample views of a particular object (i.e., a face or a car), called positive examples, that are scaled to the same size (say, 20x20), and negative examples - arbitrary images of the same size.

After a classifier is trained, it can be applied to a region of interest (of the same size as used during the training) in an input image. The classifier outputs a “1” if the region is likely to show the object (i.e., face/car), and “0” otherwise. To search for the object in the whole image one can move the search window across the image and check every location using the classifier. The classifier is designed so that it can be easily “resized” in order to be able to find the objects of interest at different sizes, which is more efficient than resizing the image itself. So, to find an object of an unknown size in the image the scan procedure should be done several times at different scales.

The word “cascade” in the classifier name means that the resultant classifier consists of several simpler classifiers ( stages ) that are applied subsequently to a region of interest until at some stage the candidate is rejected or all the stages are passed. The word “boosted” means that the classifiers at every stage of the cascade are complex themselves and they are built out of basic classifiers using one of four different boosting techniques (weighted voting). Currently Discrete Adaboost, Real Adaboost, Gentle Adaboost and Logitboost are supported. The basic classifiers are decision-tree classifiers with at least 2 leaves. Haar-like features are the input to the basic classifers, and are calculated as described below. The current algorithm uses the following Haar-like features:
_images/haarfeatures.png

The feature used in a particular classifier is specified by its shape (1a, 2b etc.), position within the region of interest and the scale (this scale is not the same as the scale used at the detection stage, though these two scales are multiplied). For example, in the case of the third line feature (2c) the response is calculated as the difference between the sum of image pixels under the rectangle covering the whole feature (including the two white stripes and the black stripe in the middle) and the sum of the image pixels under the black stripe multiplied by 3 in order to compensate for the differences in the size of areas. The sums of pixel values over a rectangular regions are calculated rapidly using integral images (see below and the Integral description).

To see the object detector at work, have a look at the HaarFaceDetect demo.

The following reference is for the detection part only. There is a separate application called haartraining that can train a cascade of boosted classifiers from a set of samples. See opencv/apps/haartraining for details.

files required in my project are:-
1)ObjectMarker.cpp
2)convert_cascade.c
3)opencv_haartraining //folder present in opencv
4)opencv_createsamples //folder present in opencv
5)final.cpp

proper steps to be followed for making your own data of any gesture :-

1)take positive photos that contains your gesture and collect it in the positive name folder
2)take negative photos that do not contain your gesture and collect it in the negative name folder
3)follow the command in the terminal :- 
   a)find ./negative -name '*.bmp'> negative.txt     //it will make ur negative txt file
   b)compile the ObjectMarker.cpp with this command 
     g++ -ggdb `pkg-config --cflags opencv` -o `basename ObjectMarker.cpp .cpp` ObjectMarker.cpp `pkg-config --libs opencv`

   c)type this command in the terminal
     ./ObjectMarker positive.txt ./positive/

    it will pop up the window in which u have to select the region of ur gesture in images then press space and then enter for ur next image
4)type the command in the terminal 
  opencv_createsamples -info positive.txt -vec positive.vec -w 30 -h 32
5)above command will make the positive.vec file

6)type the command in the terminal
   opencv_haartraining -data haar -vec positive.vec  -bg negative.txt -nstages 20 -npos 77 -nneg 80  -mem 2024  -mode ALL -w 30 -h 32

7)above command will make the haar folder in which the training of the classifier is started automatically. u can adjust the nstages upto ur choice for more accuracy. the training of classifier can take a 7 days or more,,so take patience.

8)make xml file for storing the data with this command in the terminal:-
  ./convert_cascade --size="30*32" haar fist.xml

9)now compile the program final.cpp with this command
  g++ -ggdb `pkg-config --cflags opencv` -o `basename final.cpp .cpp` final.cpp `pkg-config --libs opencv`

10)now run the program with this command
  ./final
