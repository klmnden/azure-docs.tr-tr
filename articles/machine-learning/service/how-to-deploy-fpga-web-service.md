---
title: FPGA modelleri dağıtma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti için çıkarım Ultra düşük gecikme süresi ile bir FPGA üzerinde çalışan bir modeli ile bir web hizmetini dağıtma konusunda bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: tedway
author: tedway
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: cfe21d2119b92665c5950d792dec6500257c6316
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024175"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile bir FPGA üzerinde bir web hizmeti olarak model dağıtma

Bir model üzerinde bir web hizmeti olarak dağıtabilirsiniz [alan programlanabilir kapı dizileri (FPGA)](concept-accelerate-with-fpgas.md) Azure Machine Learning donanım hızlandırılmış modelleri ile. FPGA kullanarak, bir tek bir toplu iş boyutu ile bile son derece düşük gecikme süresi çıkarım sağlar.

Bu model şu anda kullanılabilir:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16
  - SSD-VGG

FPGA Azure şu bölgelerde kullanılabilir:
  - Doğu ABD
  - Batı ABD 2
  - Batı Avrupa
  - Güneydoğu Asya

> [!IMPORTANT]
> Gecikme süresi ve aktarım hızını iyileştirmek için istemcinizi FPGA modele veri gönderen (modelde dağıtılan bir) yukarıda bölgelerden birine olması gerekir.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

- Bir Azure Machine Learning hizmeti çalışma alanında ve yüklü Python için Azure Machine Learning SDK'sı. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.
 
  - Donanım hızlandırmalı modelleri için Python SDK'sını yükleyin:

    ```shell
    pip install --upgrade azureml-accel-models
    ```

## <a name="sample-notebooks"></a>Örnek not defterleri

Size kolaylık sağlamak için [örnek not defterleri](https://aka.ms/aml-notebooks) aşağıda ve diğer örnekleri ek olarak örnek için kullanılabilir.  Yardım-How-to-kullanın-azureml ve dağıtım klasörlerinin hızlandırılmış modelleri için bakın.

## <a name="create-and-containerize-your-model"></a>Oluşturma ve modelinizi kapsayıcılı hale getirme

Bu belge girdi görüntüsünün önceden işlenir, ResNet-50 üzerinde bir FPGA kullanarak bir özelliği Oluşturucu olun TensorFlow grafik oluşturmayı açıklar ve Imagenet veri kümesinde eğitim almış bir sınıflandırıcı aracılığıyla Özellikler'i çalıştırın.

Yönergeleri izleyin:

* TensorFlow model tanımlama
* Modeli dağıtma
* Dağıtılan modeli kullanma
* Dağıtılan Hizmetleri Sil

### <a name="load-azure-ml-workspace"></a>Azure ML çalışma alanı yükleme

Azure ML çalışma alanınızı yükleyin.

```python
import os
import tensorflow as tf
 
from azureml.core import Workspace
 
ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')
```

### <a name="preprocess-image"></a>Görüntü önceden işlenir

Web hizmeti için bir giriş bir JPEG görüntüsünü olabilir.  İlk adım, JPEG resmi kod çözme ve onu önişle sağlamaktır.  JPEG görüntülerinin, dizeler ve sonucu ResNet-50 modeline giriş olacak tensors olarak kabul edilir.

```python
# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.accel.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="load-featurizer"></a>Yük özelliği Oluşturucu

Model başlatın ve bir TensorFlow denetim noktası bir özelliği Oluşturucu kullanılacak ResNet50 quantized sürümünü indirin.  Kod parçacığı ile "QuantizedResnet50" diğer derin sinir ağı içeri aktararak yerini alabilir:

- QuantizedResnet152
- QuantizedVgg16
- Densenet121

```python
from azureml.accel.models import QuantizedResnet50
save_path = os.path.expanduser('~/models')
model_graph = QuantizedResnet50(save_path, is_frozen = True)
feature_tensor = model_graph.import_graph_def(image_tensors)
print(model_graph.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Sınıflandırıcı Ekle

Bu sınıflandırıcı Imagenet veri kümesinde eğitim.  Öğrenme ve eğitim, özelleştirilmiş ağırlıkları aktarma verilebilir kümesinde kullanılabilir [örnek not defterleri](https://aka.ms/aml-notebooks).

```python
classifier_output = model_graph.get_default_classifier(feature_tensor)
print(classifier_output)
```

### <a name="save-the-model"></a>Modeli kaydedin

Önişlemci ResNet-50 özelliği oluşturucu ve sınıflandırıcı yüklenmiş olan, graf ve ilişkili değişkenler model olarak kaydedin.

```python
model_name = "resnet50"
model_def_path = os.path.join(save_path, model_name)
print("Saving model in {}".format(model_def_path))

with tf.Session() as sess:
    model_graph.restore_weights(sess)
    tf.saved_model.simple_save(sess, model_def_path,
                                   inputs={'images': in_images},
                                   outputs={'output_alias': classifier_output})
```

### <a name="register-model"></a>Modeli kaydetme

[Kayıt](./concept-model-management-and-deployment.md) oluşturduğunuz modeli.  Etiketleri ve diğer meta veriler modelle ilgili ekleme eğitilen Modellerinizi izlemenize yardımcı olur.

```python
from azureml.core.model import Model

registered_model = Model.register(workspace = ws
                                  model_path = model_def_path,
                                  model_name = model_name)

print("Successfully registered: ", registered_model.name, registered_model.description, registered_model.version, sep = '\t')
```

Bir model zaten kaydetmiş ve yüklemek istiyorsanız alabilir.

```python
from azureml.core.model import Model
model_name = "resnet50"
# By default, the latest version is retrieved. You can specify the version, i.e. version=1
registered_model = Model(ws, name="resnet50")
print(registered_model.name, registered_model.description, registered_model.version, sep = '\t')
```

### <a name="convert-model"></a>Model Dönüştür

TensorFlow graf Open sinir ağı Exchange biçimine dönüştürülmesi gerekiyor ([ONNX](https://onnx.ai/)).  Giriş ve çıkış tensors adlarını sağlamanız gerekir ve bu adları web hizmetini kullanma, istemci tarafından kullanılır.

```python
input_tensor = in_images.name
output_tensors = classifier_output.name

print(input_tensor)
print(output_tensors)


from azureml.accel.accel_onnx_converter import AccelOnnxConverter

convert_request = AccelOnnxConverter.convert_tf_model(ws, registered_model, input_tensor, output_tensors)
convert_request.wait_for_completion(show_output=True)

# If the above call succeeded, get the converted model
converted_model = convert_request.result
print(converted_model.name, converted_model.url, converted_model.version, converted_model.id,converted_model.created_time)
```

### <a name="create-docker-image"></a>Docker görüntüsü oluşturma

Dönüştürülen modeli ve tüm bağımlılıkların Docker görüntüsüne eklenir.  Bu bir Docker görüntüsü sonra dağıtılabilir ve Bulut veya desteklenen edge cihazı gibi örneği [Azure veri kutusu Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview).  Kayıtlı Docker görüntünüzü etiketleri ve açıklamaları da ekleyebilirsiniz.

```python
from azureml.core.image import Image
from azureml.accel.accel_container_image import AccelContainerImage

image_config = AccelContainerImage.image_configuration()
image_name = "{}-image".format(model_name)

image = Image.create(name = image_name,
                     models = [converted_model],
                     image_config = image_config, 
                     workspace = ws)


image.wait_for_creation(show_output = True)
```

Etikete göre görüntüleri listeleyin ve tüm hata ayıklama için ayrıntılı günlükleri alın.

```python
for i in Image.list(workspace = ws):
    print('{}(v.{} [{}]) stored at {} with build log {}'.format(i.name, i.version, i.creation_state, i.image_location, i.image_build_log_uri))
```

## <a name="model-deployment"></a>Model dağıtımı

### <a name="deploy-to-the-cloud"></a>Buluta dağıtın

Modelinizi ölçekli üretim web hizmeti olarak dağıtmak için Azure Kubernetes Service (AKS) kullanın. Azure Machine Learning SDK'sı, CLI veya Azure portalını kullanarak yeni bir tane oluşturabilirsiniz.

```python
# Use the default configuration (can also provide parameters to customize)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'my-aks-9' 
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws, 
                                  name = aks_name, 
                                  provisioning_configuration = prov_config)

%%time
aks_target.wait_for_completion(show_output = True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)

#Set the web service configuration (using default here)
aks_config = AksWebservice.deploy_configuration()

%%time
aks_service_name ='aks-service-1'

aks_service = Webservice.deploy_from_image(workspace = ws, 
                                           name = aks_service_name,
                                           image = image,
                                           deployment_config = aks_config,
                                           deployment_target = aks_target)
aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
print(aks_service.scoring_uri)
```

#### <a name="test-the-cloud-service"></a>Bulut hizmetini test edin
Docker görüntüsünü gRPC ve "Tahmin API" TensorFlow tanıtıcısıyla destekler.  Örnek istemci modelden Öngörüler edinmek için Docker görüntüsünü çağırmak için kullanın.  Örnek istemci kodu kullanılabilir:
- [Python](https://github.com/Azure/aml-real-time-ai/blob/master/pythonlib/amlrealtimeai/client.py)
- [C#](https://github.com/Azure/aml-real-time-ai/blob/master/sample-clients/csharp)

TensorFlow tanıtıcısıyla kullanmak istiyorsanız, aşağıdakileri yapabilirsiniz [bir örnek indirip](https://www.tensorflow.org/serving/setup).

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()

# Score image using input and output tensor names
results = client.score_file(path="./snowleopardgaze.jpg", 
                             input_name=input_tensor, 
                             outputs=output_tensors)

# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
```

### <a name="clean-up-the-service"></a>Hizmet temizleme
Web hizmeti, görüntü ve model (bağımlılıkları olduğundan bu sırada yapılmalıdır) silin.

```python
aks_service.delete()
image.delete()
registered_model.delete()
converted_model.delete()
```

## <a name="deploy-to-a-local-edge-server"></a>Yerel uç sunucuya dağıtma

Tüm [Azure veri kutusu uç cihazlarına](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview
) modeli çalıştırmak için bir FPGA içerir.  Yalnızca bir model FPGA üzerinde aynı anda çalışabilir.  Farklı bir model çalıştırmak için yalnızca yeni bir kapsayıcı dağıtın. Yönergeler ve örnek kod bulunabilir [bu Azure örnek](https://github.com/Azure-Samples/aml-hardware-accelerated-models).

## <a name="secure-fpga-web-services"></a>FPGA web hizmetlerini güvence altına alma

FPGA web hizmetleri güvenli hale getirme hakkında daha fazla bilgi için bkz: [güvenli web Hizmetleri](how-to-secure-web-service.md) belge.