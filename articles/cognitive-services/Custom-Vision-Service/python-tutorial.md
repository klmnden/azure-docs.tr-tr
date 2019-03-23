---
title: "Hızlı Başlangıç: Bir görüntü sınıflandırma proje Python için özel görüntü işleme SDK'sı ile oluşturma"
titlesuffix: Azure Cognitive Services
description: Python SDK'sını kullanarak bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yükleyin, projenizi eğitin ve bir tahminde bulunun.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: areddish
ms.openlocfilehash: 43c9e155a16ab5d1cc907ecd849ef109f2507782
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351703"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-python-sdk"></a>Hızlı Başlangıç: Özel görüntü işleme Python SDK ile birlikte bir görüntü sınıflandırma projesi oluşturun

Bu makalede, Özel Görüntü İşleme SDK'sini Python ile kullanarak görüntü sınıflandırma modeli oluşturmaya başlarken size yardımcı olacak bilgiler ve örnek kod sağlanır. Oluşturulduktan sonra etiketler ekleyebilir, görüntüleri karşıya yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç nokta URL’sini alabilir ve bir görüntüyü programlama yoluyla test etmek için uç noktayı kullanabilirsiniz. Kendi Python uygulamanızı oluştururken bu örneği şablon olarak kullanın. Kod _içermeyen_ bir sınıflandırma modeli oluşturma ve kullama işlemi yapmak istiyorsanız, [tarayıcı tabanlı kılavuz](getting-started-build-a-classifier.md) konusuna bakın.

## <a name="prerequisites"></a>Önkoşullar

- [Python 2.7+ veya 3.5+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/) aracı

## <a name="install-the-custom-vision-sdk"></a>Özel Görüntü İşleme SDK’sını yükleme

PowerShell'de aşağıdaki komutu çalıştırarak Python için Özel Görüntü İşleme Hizmeti SDK’sını yükleyin:

```PowerShell
pip install azure-cognitiveservices-vision-customvision
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [python-get-images](includes/python-get-images.md)]


## <a name="add-the-code"></a>Kod ekleme

Tercih ettiğiniz proje dizininde *sample.py* adlı yeni bir dosya oluşturun.

### <a name="create-the-custom-vision-service-project"></a>Özel Görüntü İşleme hizmeti projesi oluşturma

Yeni bir Özel Görüntü İşleme hizmeti projesi oluşturmak için betiğinize aşağıdaki kodu ekleyin. Abonelik anahtarlarınızı uygun tanımlara ekleyin.

```Python
from azure.cognitiveservices.vision.customvision.training import CustomVisionTrainingClient
from azure.cognitiveservices.vision.customvision.training.models import ImageFileCreateEntry

ENDPOINT = "https://southcentralus.api.cognitive.microsoft.com"

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"

trainer = CustomVisionTrainingClient(training_key, endpoint=ENDPOINT)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My New Project")
```

### <a name="create-tags-in-the-project"></a>Projede etiketler oluşturma

Projenizde sınıflandırma etiketleri oluşturmak için, *sample.py* dosyasının sonuna aşağıdaki kodu ekleyin:

```Python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

### <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Projeye örnek görüntüleri eklemek için etiket oluşturduktan sonra aşağıdaki kodu ekleyin. Bu kod, her görüntüyü ilgili etiketiyle birlikte karşıya yükler. Bilişsel Hizmetler Python SDK'sı Örnekleri projesini indirdiğiniz konuma bağlı olarak, temel görüntünün URL yolunu girmeniz gerekir.

> [!NOTE]
> Bilişsel Hizmetler Python SDK'sı Örnekleri projesini daha önce indirdiğiniz konuma bağlı olarak görüntülerin yolunu değiştirmeniz gerekir.

```Python
base_image_url = "<path to project>"

print("Adding images...")

image_entry = lambda image_path, tag_id: ImageFileCreateEntry(
    name=image_path.split("/")[-1], contents=image_path, tag_ids=[tag_id]
)

image_list = [
    image_entry(
        base_image_url + "Images/Hemlock/hemlock_{}.jpg".format(image_num),
        hemlock_tag.id,
    )
    for image_num in range(1, 10)
] + [
    image_entry(
        base_image_url
        + "Images/Japanese Cherry/japanese_cherry_{}.jpg".format(image_num),
        cherry_tag.id,
    )
    for image_num in range(1, 10)
]

trainer.create_images_from_files(project.id, images=image_list)
```

### <a name="train-the-classifier"></a>Sınıflandırıcıyı eğitme

Bu kod, projedeki ilk yinelemeyi oluşturur ve bunu varsayılan yineleme olarak işaretler. Varsayılan yineleme, tahmin isteklerine yanıt verecek modelin sürümünü yansıtır. Bu modeli her yeniden eğitişinizde bunu güncelleştirmeniz gerekir.

```Python
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

### <a name="get-and-use-the-default-prediction-endpoint"></a>Varsayılan tahmin uç noktasını alma ve kullanma

Tahmin uç noktasına bir görüntü göndermek ve tahmini almak için dosyanın sonuna aşağıdaki kodu ekleyin:

```python
from azure.cognitiveservices.vision.customvision.prediction import CustomVisionPredictionClient

# Now there is a trained endpoint that can be used to make a prediction

predictor = CustomVisionPredictionClient(prediction_key, endpoint=ENDPOINT)

test_img_url = base_image_url + "Images/Test/test_image.jpg"
results = predictor.predict_image_url(project.id, iteration.id, url=test_img_url)

# Display the results.
for prediction in results.predictions:
    print ("\t" + prediction.tag_name + ": {0:.2f}%".format(prediction.probability * 100))
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

*Sample.py*'yi çalıştırın.

```PowerShell
python sample.py
```

Uygulamanın çıkışı aşağıdaki metne benzer olmalıdır:

```console
Creating project...
Adding images...
Training...
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

Ardından test görüntüsünün (**<base_image_url>/Images/Test/** yolunda bulunur) düzgün etiketlendiğini doğrulayabilirsiniz. Ayrıca [Özel Görüntü İşleme web sitesine](https://customvision.ai) geri dönebilir ve yeni oluşturulan projenizin geçerli durumunu görebilirsiniz.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık kodda görüntü sınıflandırma işleminin her adımının nasıl uygulanabileceğini gördünüz. Bu örnek tek bir eğitim yinelemesi yürütür ama modelinizin daha doğru olmasını sağlamak için çoğunlukla birden çok kez eğitmeniz ve test etmeniz gerekecektir.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)
