---
title: Bir özel görme hizmet Python Eğitmen - Azure Bilişsel hizmetler oluşturma | Microsoft Docs
description: Microsoft Bilişsel Hizmetleri'nde özel görme API'sini kullanan basit bir Python uygulaması keşfedin. Bir proje oluşturun, etiket ekleme, resimler yükleyin, projenizin eğitmek ve varsayılan uç nokta kullanarak tahminde bulunmak.
services: cognitive-services
author: areddish
manager: chbuehle
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/07/2018
ms.author: areddish
ms.openlocfilehash: 0359935bf266d4f2a5cf845dd0d23183f4f77b72
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353285"
---
# <a name="custom-vision-api-python-tutorial"></a>Özel görme API Python Eğitmen

Özel görme hizmeti ve temel bir Python betiği ile bir görüntü sınıflandırma projesi oluşturmayı öğrenin. Oluşturulduktan sonra etiket ekleme, resimler yükleyin, proje Eğitimi, projenin varsayılan tahmin uç noktasının URL'sini alın ve program aracılığıyla bir görüntü sınamak için kullanın. Bu açık kaynak örneği özel görme API'sini kullanarak kendi uygulamanızı oluşturmak için şablon olarak kullanın.



## <a name="prerequisites"></a>Önkoşullar

- Python 2.7 + veya Python 3.5 +.
- PIP aracı.

## <a name="get-the-training-and-prediction-keys"></a>Eğitim ve tahmin anahtarı alma

Bu örnekte kullanılan anahtarlarını almak için şurayı ziyaret edin [özel görme web sayfası](https://customvision.ai) seçip __dişli simgesi__ sağ üst. İçinde __hesapları__ bölümünde, değerleri kopyalamak __eğitim anahtar__ ve __tahmin anahtar__ alanları.

![UI anahtarları görüntüsü](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>Özel görme hizmeti SDK'sını yükleyin

Özel görme hizmeti SDK'sını yüklemek için aşağıdaki komutu kullanın:

```
pip install azure-cognitiveservices-vision-customvision
```

## <a name="get-example-images"></a>Örnek görüntüleri alma

Bu örnek görüntülerden kullanır `Samples/Images` dizininde [ https://github.com/Microsoft/Cognitive-CustomVision-Windows ](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images) projesi. Kopyalama veya karşıdan yükleyip geliştirme ortamınızı projeyi ayıklayın.

## <a name="create-a-custom-vision-service-project"></a>Bir özel görme hizmet projesi oluşturma

Yeni özel görme hizmet projesi oluşturmak için adlı yeni dosya oluşturun `sample.py`. Aşağıdaki kodu dosyanın içeriği kullanın:

> [!IMPORTANT]
> Ayarlama `training_key` daha önce eğitim anahtar değeri.
>
> Ayarlama `prediction_key` daha önce tahmin anahtar değeri.

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

## <a name="add-tags-to-your-project"></a>Projeniz için etiketler ekleme

Etiketler projenize eklemek için sonuna aşağıdaki kodu ekleyin `sample.py` dosyası:

```python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-images-to-the-project"></a>Görüntüleri projesine karşıya yükleme

Örnek görüntüler projeye eklemek için etiketi oluşturulduktan sonra aşağıdaki kodu ekleyin. Bu kod, karşılık gelen etiketi görüntüsüyle yükler:

> [!IMPORTANT]
>
> Cognitive CustomVision Windows projesi daha önce indirdiğiniz üzerinde temel görüntülerine yolu değiştirin.

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

## <a name="get-and-use-the-default-prediction-endpoint"></a>Alın ve varsayılan tahmin uç noktası kullan

Bir görüntü tahmin uç noktasına göndermek ve tahmin almak için sonuna aşağıdaki kodu ekleyin `sample.py` dosyası:

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

Uygulamanın çıkışı aşağıdaki metne benzer:

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