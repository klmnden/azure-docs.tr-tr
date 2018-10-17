---
title: 'Öğretici: Görüntü sınıflandırma projesi oluşturma - Özel Görüntü İşleme Hizmeti, Python'
titlesuffix: Azure Cognitive Services
description: Bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yükleyin, projenizi eğitin ve varsayılan uç noktayı kullanarak bir tahminde bulunun.
services: cognitive-services
author: areddish
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: 14b805a60637a889698132e169d5a41670a8bce0
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363386"
---
# <a name="tutorial-create-an-image-classification-project-using-the-custom-vision-service-with-python"></a>Öğretici: Python ile Özel Görüntü İşleme Hizmeti kullanarak görüntü sınıflandırma projesi oluşturma

Özel Görüntü İşleme Hizmeti ve temel Python betiği ile nasıl görüntü sınıflandırma projesi oluşturulacağını öğrenin. Oluşturulduktan sonra etiketler ekleyebilir, görüntüleri karşıya yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç nokta URL’sini alabilir ve bir görüntüyü programlama yoluyla test etmek için bunu kullanabilirsiniz. Özel Görüntü İşleme API’sini kullanarak kendi uygulamanızı derlemek için şablon olarak bu açık kaynak örneği kullanın.



## <a name="prerequisites"></a>Ön koşullar

- Python 2.7+ veya Python 3.5+.
- Pip aracı.

## <a name="get-the-training-and-prediction-keys"></a>Eğitim ve tahmin anahtarlarını alma

Bu örnekte kullanılan anahtarları almak için [Özel Görüntü İşleme web sayfasını](https://customvision.ai) ziyaret edin ve sağ üst kısımdaki __dişli simgesini__ seçin. __Hesaplar__ bölümünde, __Eğitim Anahtarı__ ve __Tahmin Anahtarı__ alanlarından değerleri kopyalayın.

![Anahtarlar kullanıcı arabiriminin görüntüsü](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Özel Görüntü İşleme Hizmeti SDK’sını yükleme

Özel Görüntü İşleme Hizmeti SDK’sını yüklemek için aşağıdaki komutu kullanın:

```
pip install azure-cognitiveservices-vision-customvision
```

## <a name="get-example-images"></a>Örnek görüntüleri alma

Bu örnekte, [https://github.com/Microsoft/Cognitive-CustomVision-Windows](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images) projesinin `Samples/Images` dizininde yer alan görüntüler kullanılmaktadır. Projeyi geliştirme ortamınıza kopyalayın veya indirin ve ayıklayın.

## <a name="create-a-custom-vision-service-project"></a>Özel Görüntü İşleme Hizmeti projesi oluşturma

Yeni bir Özel Görüntü İşleme Hizmeti projesi oluşturmak için `sample.py` adlı yeni dosya oluşturun. Dosya içerikleri olarak aşağıdaki kodu kullanın:

> [!IMPORTANT]
> `training_key` öğesini, daha önce aldığınız eğitim anahtarı değerine ayarlayın.
>
> `prediction_key` öğesini, daha önce aldığınız tahmin anahtarı değerine ayarlayın.

```python
from azure.cognitiveservices.vision.customvision.training import training_api
from azure.cognitiveservices.vision.customvision.training.models import ImageUrlCreateEntry

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"

trainer = training_api.TrainingApi(training_key)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My Project")
```

## <a name="add-tags-to-your-project"></a>Projenize etiketler ekleme

Projenize etiketler eklemek için, `sample.py` dosyasının sonuna aşağıdaki kodu ekleyin:

```python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-images-to-the-project"></a>Projeye görüntüleri karşıya yükleme

Projeye örnek görüntüleri eklemek için etiket oluşturduktan sonra aşağıdaki kodu ekleyin. Bu kod, karşılık gelen etiketle görüntüyü karşıya yükler:

> [!IMPORTANT]
>
> Daha önce Bilişsel CustomVision Windows projesini indirdiğiniz yere göre görüntülerin yolunu değiştirin.

```python
base_image_url = "https://raw.githubusercontent.com/Microsoft/Cognitive-CustomVision-Windows/master/Samples/"

print ("Adding images...")
for image_num in range(1,10):
    image_url = base_image_url + "Images/Hemlock/hemlock_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ hemlock_tag.id ] ) ])

for image_num in range(1,10):
    image_url = base_image_url + "Images/Japanese Cherry/japanese_cherry_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ cherry_tag.id ] ) ])


# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following:
#
#import os
#hemlock_dir = "Images\\Hemlock"
#for image in os.listdir(os.fsencode("Images\\Hemlock")):
#    with open(hemlock_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ hemlock_tag.id ])
#
#cherry_dir = "Images\\Japanese Cherry"
#for image in os.listdir(os.fsencode("Images\\Japanese Cherry")):
#    with open(cherry_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ cherry_tag.id ])
```

## <a name="train-the-project"></a>Projeyi eğitme

Sınıflandırıcıyı eğitmek için `sample.py` dosyasının sonuna aşağıdaki kodu ekleyin:

```python
import time

print ("Training...")
iteration = trainer.train_project(project.id)
while (iteration.status != "Completed"):
    iteration = trainer.get_iteration(project.id, iteration.id)
    print ("Training status: " + iteration.status)
    time.sleep(1)

# The iteration is now trained. Make it the default project endpoint
trainer.update_iteration(project.id, iteration.id, is_default=True)
print ("Done!")
```

## <a name="get-and-use-the-default-prediction-endpoint"></a>Varsayılan tahmin uç noktasını alma ve kullanma

Tahmin uç noktasına bir görüntü göndermek ve tahmini almak için `sample.py` dosyasının sonuna aşağıdaki kodu ekleyin:

```python
from azure.cognitiveservices.vision.customvision.prediction import prediction_endpoint
from azure.cognitiveservices.vision.customvision.prediction.prediction_endpoint import models

# Now there is a trained endpoint that can be used to make a prediction

predictor = prediction_endpoint.PredictionEndpoint(prediction_key)

test_img_url = base_image_url + "Images/Test/test_image.jpg"
results = predictor.predict_image_url(project.id, iteration.id, url=test_img_url)

# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following.
#
# Open the sample image and get back the prediction results.
# with open("Images\\test\\test_image.jpg", mode="rb") as test_data:
#     results = predictor.predict_image(project.id, test_data, iteration.id)

# Display the results.
for prediction in results.predictions:
    print ("\t" + prediction.tag_name + ": {0:.2f}%".format(prediction.probability * 100))
```

## <a name="run-the-example"></a>Örneği çalıştırma

Çözümü çalıştırın. Tahmin sonuçlarını konsolda görüntülenir.

```
python sample.py
```

Uygulamanın çıktısı aşağıdaki metne benzer:

```
Creating project...
Adding images...
Training...
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```