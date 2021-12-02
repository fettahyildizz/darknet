# *------------Nasıl eğitim yapılır---------------*
Darknet install edilir. (Yönergeler aşağıda var.)

cd darknet/data/obj (yoksa oluşturulur)
000001.jpg ve 000001.txt diye iki dosya olmalı. (darknet için gerekli format bu)
txt dosyası şöyle gözükmeli: . 0 0.5985 0.87 0.78547 (Class no, bounding boxes)

cd darknet/cfg
yolov3-custom.cfg diye yeni bir cfg yarat. yolov3.cfg mimarisini kopyala.

# ***YOLOV3 cfg dosyası hazırlamak***
Training
batch=64
subdivision= 8 - 16 - 32 - 64 (memorye göre sırasıyla dene)

change line max_batches to (classes*2000, but not less than number of training images and not less than 6000), f.e. max_batches=6000 if you train for 3 classes
change line steps to 80% and 90% of max_batches, f.e. steps=4800,5400
set network size width=416 height=416 or any value multiple of 32:
change line classes=80 to your number of objects in each of 3 [yolo]-layers:
      https://github.com/AlexeyAB/darknet/blob/0039fd26786ab5f71d5af725fc18b3f521e7acfd/cfg/yolov3.cfg#L610
      https://github.com/AlexeyAB/darknet/blob/0039fd26786ab5f71d5af725fc18b3f521e7acfd/cfg/yolov3.cfg#L696
      https://github.com/AlexeyAB/darknet/blob/0039fd26786ab5f71d5af725fc18b3f521e7acfd/cfg/yolov3.cfg#L783
      
change [filters=255] to filters=(classes + 5)x3 in the 3 [convolutional] before each [yolo] layer, keep in mind that it only has to be the last [convolutional] before each of the [yolo] layers.
      https://github.com/AlexeyAB/darknet/blob/0039fd26786ab5f71d5af725fc18b3f521e7acfd/cfg/yolov3.cfg#L603
      https://github.com/AlexeyAB/darknet/blob/0039fd26786ab5f71d5af725fc18b3f521e7acfd/cfg/yolov3.cfg#L689
      https://github.com/AlexeyAB/darknet/blob/0039fd26786ab5f71d5af725fc18b3f521e7acfd/cfg/yolov3.cfg#L776
      
(!Eğer memory yetmez ise random=1 leri random=0'a değiştirebilirsiniz. Böylece sadece 416x416 görüntüler sokarız. Random=1 iken daha farklı çözünürlüklerden görüntü sokabiliyoruz böylece overfittingi önlüyor.)

cd darknet/data/obj.names (yoksa oluştur)
Person
Car
Truck
...

class isimleri sırayla yazılır.

cd darknet/data/obj.data

classes = 2
train = data/train.txt
valid = data/test.txt
names = data/obj.names
backup = backup/

mkdir darknet/backup

https://github.com/theAIGuysCode/YoloGenerateTrainingFile

darknet klasöründe python 'generate_train.py' kodunu çalıştır. 
yukarıdaki script darknet/data/train.txt dosyasını oluşturur. Bu dosyada train klasöründeki her resmin path'i var.
 
 Download Pretrained Convolutional Weights
 wget https://pjreddie.com/media/files/darknet53.conv.74
 inen dosyayı darknet klasörüne kopyala.
 
 ./darknet detector train data/obj.data cfg/yolov3-custom.cfg darknet53.conv.74
 (1.argüman: obj.data 2.argüman: config - 3.argüman: conv ağırlık dosyası)
 
 # ***Average Loss Yorumları***
 loss 1000'lerden başlayabilir.
 
 200. iterasyonlarda loss 5'lerin altına inebilir.
 
 eğitim sonunda lossun 2'nin altına inmesi beklenir.
 

![Darknet Logo](http://pjreddie.com/media/files/darknet-black-small.png)

# Darknet #
Darknet is an open source neural network framework written in C and CUDA. It is fast, easy to install, and supports CPU and GPU computation.

**Discord** invite link for for communication and questions: https://discord.gg/zSq8rtW

## Scaled-YOLOv4: 

* **paper (CVPR 2021)**: https://openaccess.thecvf.com/content/CVPR2021/html/Wang_Scaled-YOLOv4_Scaling_Cross_Stage_Partial_Network_CVPR_2021_paper.html

* **source code - Pytorch (use to reproduce results):** https://github.com/WongKinYiu/ScaledYOLOv4

* **source code - Darknet:** https://github.com/AlexeyAB/darknet

* **Medium:** https://alexeyab84.medium.com/scaled-yolo-v4-is-the-best-neural-network-for-object-detection-on-ms-coco-dataset-39dfa22fa982?source=friends_link&sk=c8553bfed861b1a7932f739d26f487c8

## YOLOv4:

* **paper:** https://arxiv.org/abs/2004.10934

* **source code:** https://github.com/AlexeyAB/darknet

* **Wiki:** https://github.com/AlexeyAB/darknet/wiki

* **useful links:** https://medium.com/@alexeyab84/yolov4-the-most-accurate-real-time-neural-network-on-ms-coco-dataset-73adfd3602fe?source=friends_link&sk=6039748846bbcf1d960c3061542591d7

For more information see the [Darknet project website](http://pjreddie.com/darknet).

For questions or issues please use the [Google Group](https://groups.google.com/forum/#!forum/darknet).

![scaled_yolov4](https://user-images.githubusercontent.com/4096485/112776361-281d8380-9048-11eb-8083-8728b12dcd55.png) AP50:95 - FPS (Tesla V100) Paper: https://arxiv.org/abs/2011.08036

----

![YOLOv4Tiny](https://user-images.githubusercontent.com/4096485/101363015-e5c21200-38b1-11eb-986f-b3e516e05977.png)

----

![YOLOv4](https://user-images.githubusercontent.com/4096485/90338826-06114c80-dff5-11ea-9ba2-8eb63a7409b3.png)


----

![OpenCV_TRT](https://user-images.githubusercontent.com/4096485/90338805-e5e18d80-dff4-11ea-8a68-5710956256ff.png)


## Citation

```
@misc{bochkovskiy2020yolov4,
      title={YOLOv4: Optimal Speed and Accuracy of Object Detection}, 
      author={Alexey Bochkovskiy and Chien-Yao Wang and Hong-Yuan Mark Liao},
      year={2020},
      eprint={2004.10934},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```

```
@InProceedings{Wang_2021_CVPR,
    author    = {Wang, Chien-Yao and Bochkovskiy, Alexey and Liao, Hong-Yuan Mark},
    title     = {{Scaled-YOLOv4}: Scaling Cross Stage Partial Network},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
    month     = {June},
    year      = {2021},
    pages     = {13029-13038}
}
```
