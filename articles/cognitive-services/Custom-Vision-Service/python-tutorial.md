---
title: Azure Bilişsel hizmetler Custom Vision Service Python öğreticisi için - Oluştur | Microsoft Docs
description: Microsoft Bilişsel hizmetler özel görüntü işleme API'sini kullanan temel bir Python uygulaması keşfedin. Bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yüklemek, projenizi eğitmek ve varsayılan uç nokta kullanarak bir tahminde bulunmak.
services: cognitive-services
author: areddish
manager: chbuehle
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: df0bdc0bbd2768566336323851f366c9ae280a88
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301608"
---
# <a name="custom-vision-api-python-tutorial"></a>Özel görüntü işleme API'si Python Öğreticisi

Custom Vision Service'e ve temel bir Python betiği ile bir görüntü sınıflandırma projesi oluşturmayı öğrenin. Oluşturulduktan sonra etiketler ekleyin, görüntüleri karşıya yüklemek, proje Eğitimi, projenin varsayılan tahmin uç nokta URL'sini alma ve program aracılığıyla resim test etmek için kullanın. Bu açık kaynaklı örneği, özel görüntü işleme API'sini kullanarak kendi uygulamanızı oluşturmaya yönelik şablon olarak kullanın.



## <a name="prerequisites"></a>Önkoşullar

- Python 2.7 + veya Python 3.5 +.
- Pip aracı.

## <a name="get-the-training-and-prediction-keys"></a>Eğitim ve tahmin anahtarları alma

Bu örnekte kullanılan anahtarlarını almak için şurayı ziyaret edin [Custom Vision web sayfası](https://customvision.ai) seçip __dişli simgesini__ sağ üst köşedeki. İçinde __hesapları__ bölümünde, değerleri kopyalayın __eğitim anahtarı__ ve __tahmin anahtar__ alanları.

![UI anahtarları görüntüsü](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Özel görüntü işleme hizmeti SDK'sını yükleme

Custom Vision Service SDK'sını yüklemek için aşağıdaki komutu kullanın:

```
pip install azure-cognitiveservices-vision-customvision
```

## <a name="get-example-images"></a>Örnek görüntüleri alma

Bu örnekte görüntülerden `Samples/Images` dizininde [ https://github.com/Microsoft/Cognitive-CustomVision-Windows ](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images) proje. Kopyalayın veya indirin ve sonra da projenin geliştirme ortamınıza ayıklayın.

## <a name="create-a-custom-vision-service-project"></a>Custom Vision Service projesi oluşturma

Yeni bir özel görüntü işleme hizmeti projesi oluşturmak için adlı yeni bir dosya oluşturmak `sample.py`. Dosya içeriği aşağıdaki kodu kullanın:

> [!IMPORTANT]
> Ayarlama `training_key` daha önce aldığınız eğitim anahtar değeri.
>
> Ayarlama `prediction_key` daha önce aldığınız tahmin anahtar değeri.

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

## <a name="add-tags-to-your-project"></a>Etiket projenize ekleme

Etiket projenize eklemek için sonuna aşağıdaki kodu ekleyin `sample.py` dosyası:

```python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-images-to-the-project"></a>Projeye görüntüleri karşıya yükleme

Örnek görüntüleri projeye eklemek için etiketi oluşturulduktan sonra aşağıdaki kodu ekleyin. Bu kod, karşılık gelen etiket görüntüyü yükler:

> [!IMPORTANT]
>
> Bilişsel CustomVision Windows proje daha önce indirdiğiniz üzerinde temel görüntülerin yolunu değiştirin.

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

## <a name="train-the-project"></a>Proje eğitimi

Sınıflandırıcı eğitmek için sonuna aşağıdaki kodu ekleyin `sample.py` dosyası:

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

## <a name="get-and-use-the-default-prediction-endpoint"></a>Alma ve varsayılan tahmin uç noktası kullanma

Görüntü tahmin uç noktasına göndermesi ve öngörü almak için sonuna aşağıdaki kodu ekleyin `sample.py` dosyası:

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

## <a name="run-the-example"></a>Örneği çalıştırın

Çözümü çalıştırın. Tahmin sonuçlarını konsolda görünür.

```
python sample.py
```

Uygulama çıktısı aşağıdaki metne benzer:

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