---
title: FPGA modelleri dağıtma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti için çıkarım Ultra düşük gecikme süresi ile bir FPGA üzerinde çalışan bir modeli ile bir web hizmetini dağıtma konusunda bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 1/29/2019
ms.custom: seodec18
ms.openlocfilehash: 7aa0e11ed47219829830369d17b300270d3fbffb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60819436"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile bir FPGA üzerinde bir web hizmeti olarak model dağıtma

Bir model üzerinde bir web hizmeti olarak dağıtabilirsiniz [alan programlanabilir kapı dizileri (FPGA)](concept-accelerate-with-fpgas.md).  FPGA kullanarak, bir tek bir toplu iş boyutu ile bile son derece düşük gecikme süresi çıkarım sağlar.  Bu model şu anda kullanılabilir:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16   

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

- Bir Azure Machine Learning hizmeti çalışma alanında ve yüklü Python için Azure Machine Learning SDK'sı. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.
 
  - Çalışma alanınızda yer alması gerekir *Doğu ABD 2* bölge.

  - Contrib ek özellikler yükleyin:

    ```shell
    pip install --upgrade azureml-sdk[contrib]
    ```

  - Şu anda yalnızca tensorflow sürüm < = 1.10 desteklenir, diğer tüm yüklemeleri tamamlandıktan sonra yükleme:

    ```shell
    pip install "tensorflow==1.10"
    ```

## <a name="create-and-deploy-your-model"></a>Modelinizi oluşturup
Girdi görüntüsünün önceden işlenir, ResNet-50 üzerinde bir FPGA kullanarak bir özelliği yapmak için bir işlem hattı oluşturma ve Imagenet veri kümesinde eğitim almış bir sınıflandırıcı aracılığıyla Özellikler'i çalıştırın.

Yönergeleri izleyin:

* Model işlem hattı tanımlayın
* Modeli dağıtma
* Dağıtılan modeli kullanma
* Dağıtılan Hizmetleri Sil

> [!IMPORTANT]
> Gecikme süresi ve aktarım hızını iyileştirmek için istemci uç noktası ile aynı Azure bölgesinde olmalıdır.  Şu anda API'leri, Doğu ABD Azure bölgesi oluşturulur.



### <a name="preprocess-image"></a>Görüntü önceden işlenir
İlk aşamada işlem hattının görüntüleri önişle sağlamaktır.

```python
import os
import tensorflow as tf

# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.contrib.brainwave.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="add-featurizer"></a>Özelliği Oluşturucu Ekle
Model başlatın ve bir TensorFlow denetim noktası bir özelliği Oluşturucu kullanılacak ResNet50 quantized sürümünü indirin.

```python
from azureml.contrib.brainwave.models import QuantizedResnet50
model_path = os.path.expanduser('~/models')
model = QuantizedResnet50(model_path, is_frozen = True)
feature_tensor = model.import_graph_def(image_tensors)
print(model.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Sınıflandırıcı Ekle
Bu sınıflandırıcı Imagenet veri kümesinde eğitim.

```python
classifier_output = model.get_default_classifier(feature_tensor)
```

### <a name="create-service-definition"></a>Hizmet tanımı oluşturma
Ön işleme görüntü özelliği oluşturucu ve hizmetin üzerinde çalıştığı sınıflandırıcı tanımlanan, bir hizmet tanımı oluşturabilirsiniz. Hizmet tanımı FPGA hizmete dağıtılan modelinden oluşturulan dosyalar kümesidir. Hizmet tanımı, bir işlem hattı oluşur. İşlem hattı, sırayla çalıştırılır aşamaları dizisidir.  Aşamaları TensorFlow, Keras aşamaları ve BrainWave aşamaları desteklenir.  Aşamalar sonraki aşama giriş olma her bir aşamanın çıktısı ile Service sırayla çalıştırılır.

TensorFlow aşama oluşturun, grafiğin (Bu durumda varsayılan grafik kullanılır) ve giriş içeren bir oturum belirtin ve bu aşamaya tensors çıkış için.  Bu bilgiler, hizmette çalıştırmak üzere grafiği kaydetmek için kullanılır.

```python
from azureml.contrib.brainwave.pipeline import ModelDefinition, TensorflowStage, BrainWaveStage

save_path = os.path.expanduser('~/models/save')
model_def_path = os.path.join(save_path, 'model_def.zip')

model_def = ModelDefinition()
with tf.Session() as sess:
    model_def.pipeline.append(TensorflowStage(sess, in_images, image_tensors))
    model_def.pipeline.append(BrainWaveStage(sess, model))
    model_def.pipeline.append(TensorflowStage(sess, feature_tensor, classifier_output))
    model_def.save(model_def_path)
    print(model_def_path)
```

### <a name="deploy-model"></a>Model dağıtma
Bir hizmeti hizmet tanımını oluşturun.  Çalışma alanınız, Doğu ABD 2 konumunda olması gerekiyor.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')

from azureml.core.model import Model
model_name = "resnet-50-rtai"
registered_model = Model.register(ws, model_def_path, model_name)

from azureml.core.webservice import Webservice
from azureml.exceptions import WebserviceException
from azureml.contrib.brainwave import BrainwaveWebservice, BrainwaveImage
service_name = "imagenet-infer"
service = None
try:
    service = Webservice(ws, service_name)
except WebserviceException:
    image_config = BrainwaveImage.image_configuration()
    deployment_config = BrainwaveWebservice.deploy_configuration()
    service = Webservice.deploy_from_model(ws, service_name, [registered_model], image_config, deployment_config)
    service.wait_for_deployment(True)
```

### <a name="test-the-service"></a>Hizmeti test etme
API için bir görüntü gönderme ve yanıt sınamak için bir eşleme Imagenet sınıf adı çıkış sınıfı kimliği ekleyin.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()
```

Hizmetinizi arayın ve aşağıda "image.jpg bilgisayarınızı" dosya adı, makineden bir görüntü ile değiştirin. 

```python
with open('your-image.jpg') as f:
    results = service.run(f)
# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
``` 

### <a name="clean-up-service"></a>Hizmetini kurma Temizle
Hizmeti silin.

```python
service.delete()
    
registered_model.delete()
```

## <a name="secure-fpga-web-services"></a>FPGA web hizmetlerini güvence altına alma

FPGA web hizmetleri güvenli hale getirme hakkında daha fazla bilgi için bkz: [güvenli web Hizmetleri](how-to-secure-web-service.md) belge.


## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [ML Model dağıtılan web hizmeti olarak Tüket](how-to-consume-web-service.md).
