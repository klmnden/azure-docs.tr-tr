---
title: Model bir FPGA Azure Machine Learning ile bir web hizmeti olarak dağıtma
description: Azure Machine Learning ile bir FPGA üzerinde çalışan bir modeli ile bir web hizmetini dağıtma konusunda bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 10/01/2018
ms.openlocfilehash: 925173f85301d6481ae3b9cf891041239b06bc8f
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49113725"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning"></a>Model bir FPGA Azure Machine Learning ile bir web hizmeti olarak dağıtma

Bir model üzerinde bir web hizmeti olarak dağıtabilirsiniz [alan programlanabilir kapı dizileri (FPGA)](concept-accelerate-with-fpgas.md).  FPGA kullanarak, bir tek bir toplu iş boyutu ile bile son derece düşük gecikme süresi çıkarım sağlar.   

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- İster gerekir ve FPGA kotasının onaylanması gerekiyor. Erişim istemek için kota istek formunu doldurun: https://aka.ms/aml-real-time-ai

- Bir Azure Machine Learning hizmeti çalışma alanında ve yüklü Python için Azure Machine Learning SDK'sı. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.
 
  - Çalışma alanınızda yer alması gerekir *Doğu ABD 2* bölge.

  - Contrib ek özellikler yükleyin:

    ```shell
    pip install --upgrade azureml-sdk[contrib]
    ```  

## <a name="create-and-deploy-your-model"></a>Modelinizi oluşturup
Girdi görüntüsünün, özellik kazandırın ResNet-50 üzerinde bir FPGA kullanarak önceden işleme için işlem hattı oluşturma ve Imagenet veri kümesinde eğitilmiş bir classifer aracılığıyla Özellikler'i çalıştırın.

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
from azureml.contrib.brainwave.models import QuantizedResnet50, Resnet50
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
classifier_input, classifier_output = Resnet50.get_default_classifier(feature_tensor, model_path)
```

### <a name="create-service-definition"></a>Hizmet tanımı oluşturma
Ön işleme görüntü özelliği oluşturucu ve hizmetin üzerinde çalıştığı sınıflandırıcı el edindikten sonra bir hizmet tanımı oluşturabilirsiniz. Hizmet tanımı FPGA hizmete dağıtılan modelinden oluşturulan dosyalar kümesidir. Hizmet tanımı, bir işlem hattı oluşur. İşlem hattı, sırayla çalıştırılır aşamaları dizisidir.  Aşamaları TensorFlow, Keras aşamaları ve BrainWave aşamaları desteklenir.  Aşamalar sırayla her sonraki aşama aşama giriş çıktısı ile hizmet üzerinde çalışır.

TensorFlow aşama oluşturun, grafiğin (Bu durumda varsayılan grafik kullanılır) ve giriş içeren bir oturum belirtin ve bu aşamaya tensors çıkış için.  Bu bilgiler, hizmette çalıştırmak üzere grafiği kaydetmek için kullanılır.

```python
from azureml.contrib.brainwave.pipeline import ModelDefinition, TensorflowStage, BrainWaveStage

save_path = os.path.expanduser('~/models/save')
model_def_path = os.path.join(save_path, 'service_def.zip')

model_def = ModelDefinition()
with tf.Session() as sess:
    model_def.pipeline.append(TensorflowStage(sess, in_images, image_tensors))
    model_def.pipeline.append(BrainWaveStage(sess, model))
    model_def.pipeline.append(TensorflowStage(sess, classifier_input, classifier_output))
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
    service.wait_for_deployment(true)
```

### <a name="test-the-service"></a>Test hizmeti
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

Azure Machine Learning modellerini FPGA üzerinde çalışan, SSL desteği ve anahtar tabanlı kimlik doğrulaması sağlar. Bu, hizmet ve istemcileri tarafından gönderilen güvenli veri erişimi sınırlamak sağlar. [Web hizmeti güvenli hale getirme hakkında bilgi edinin](how-to-secure-web-service.md).


## <a name="sample-notebook"></a>Örnek Not Defteri

Bu makaledeki kavramları içinde gösterilen [proje-brainwave/project-brainwave-quickstart.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/project-brainwave/project-brainwave-quickstart.ipynb) dizüstü bilgisayar.

Bu not alın:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
