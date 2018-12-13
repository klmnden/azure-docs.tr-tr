---
title: 'Görüntü sınıflandırma Öğreticisi: Modelleri dağıtma'
titleSuffix: Azure Machine Learning service
description: Bu öğretici, Azure Machine Learning hizmetini kullanarak Python Jupyter not defterinde scikit-learn ile bir görüntü sınıflandırma modelinin nasıl dağıtıldığını gösterir. Bu öğretici, iki bölümden oluşan bir serinin ikinci bölümüdür.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: ea446c89fc74fca444793a5e0f803a54fa251ed1
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53312179"
---
# <a name="tutorial--deploy-an-image-classification-model-in-azure-container-instance"></a>Öğretici:  Bir Azure Container Instance görüntü sınıflandırma modelinde dağıtma

Bu öğretici, **iki bölümden oluşan bir öğretici serisinin ikinci bölümüdür**. [Önceki öğreticide](tutorial-train-models-with-aml.md) makine öğrenmesi modellerini eğittiniz ve buluttaki çalışma alanınıza modeli kaydettiniz.  

Artık, bir web hizmeti olarak modeli dağıtacağız hazır [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/). Web hizmeti, puanlama mantığını ve modelin kendisini kapsülleyen bir görüntüdür; buradaki örnekte Docker görüntüsüdür. 

Öğreticinin bu bölümünde Azure Machine Learning hizmeti için kullanabilirsiniz:

> [!div class="checklist"]
> * Test ortamınızı ayarlama
> * Çalışma alanınızdan modeli alma
> * Modeli yerel olarak test etme
> * Container Instances için model dağıtma
> * Dağıtılan modeli test etme

Container Instances üretim dağıtımları için ideal değildir, ancak test ve iş akışını anlamak için idealdir. Ölçeklenebilir üretim dağıtımları için Azure Kubernetes hizmeti kullanmayı düşünün. Daha fazla bilgi için [nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md).

## <a name="get-the-notebook"></a>Not defterini alma

Kolaylık olması için, bu öğretici bir [Jupyter notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part2-deploy.ipynb) olarak sağlanır. Çalıştırma `tutorials/img-classification-part2-deploy.ipynb` not defterine Azure defterlerinde veya kendi Jupyter notebook sunucusu.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

>[!NOTE]
> Bu makalede kod, Azure Machine Learning SDK sürümü 1.0.2 ile test edilmiştir.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki not defterinde modeli eğitimi tamamlayın: [Öğretici #1: Bir Azure Machine Learning hizmeti ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).  


## <a name="set-up-the-environment"></a>Ortamı ayarlama

Başlangıç olarak test ortamını ayarlayın.

### <a name="import-packages"></a>Paketleri içeri aktarma

Bu öğretici için gereken Python paketlerini içeri aktarın.

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
 
import azureml
from azureml.core import Workspace, Run

# display the core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="retrieve-the-model"></a>Modeli alma

Önceki öğreticide çalışma alanınızda bir modeli kaydetmiştiniz. Şimdi, bu çalışma alanını yükleyin ve modeli yerel dizininize indirin.


```python
from azureml.core import Workspace
from azureml.core.model import Model

ws = Workspace.from_config()
model=Model(ws, 'sklearn_mnist')
model.download(target_dir = '.')
import os 
# verify the downloaded model file
os.stat('./sklearn_mnist_model.pkl')
```

## <a name="test-model-locally"></a>Modeli yerel olarak test etme

Modelinizi dağıtmadan önce, aşağıdakileri yaparak modelin yerel olarak çalıştığından emin olun:
* Test verilerini yükleme
* Test verilerini tahmin etme
* Karışıklık matrisini inceleme

### <a name="load-test-data"></a>Test verilerini yükleme

Test verilerini, eğitim öğreticisi sırasında oluşturulan **./data/** dizininden yükleyin.

```python
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the neural network converge faster

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

```

### <a name="predict-test-data"></a>Test verilerini tahmin etme

Tahminleri almak için test veri kümesinin modele akışını gerçekleştirin.

```python
import pickle
from sklearn.externals import joblib

clf = joblib.load('./sklearn_mnist_model.pkl')
y_hat = clf.predict(X_test)
```

###  <a name="examine-the-confusion-matrix"></a>Karışıklık matrisini inceleme

Test kümesinden kaç örneğin doğru sınıflandırıldığını görmek için karışıklık matrisi oluşturun. Doğru tahminler elde etmek için bildireceğinizi değeri dikkat edin. 

```python
from sklearn.metrics import confusion_matrix

conf_mx = confusion_matrix(y_test, y_hat)
print(conf_mx)
print('Overall accuracy:', np.average(y_hat == y_test))
```

Çıkış, karışıklık matrisini gösterir:

    [[ 960    0    1    2    1    5    6    3    1    1]
     [   0 1112    3    1    0    1    5    1   12    0]
     [   9    8  920   20   10    4   10   11   37    3]
     [   4    0   17  921    2   21    4   12   20    9]
     [   1    2    5    3  915    0   10    2    6   38]
     [  10    2    0   41   10  770   17    7   28    7]
     [   9    3    7    2    6   20  907    1    3    0]
     [   2    7   22    5    8    1    1  950    5   27]
     [  10   15    5   21   15   27    7   11  851   12]
     [   7    8    2   13   32   13    0   24   12  898]]
    Overall accuracy: 0.9204
   

Karışıklık matrisini grafik olarak görüntülemek için `matplotlib` kullanın. Bu grafikte X ekseni asıl değerleri, Y ekseni de tahmini değerleri gösterir. Her kılavuzun rengi, hata oranını gösterir. Renk ne kadar açıksa hata oranı da o kadar yüksektir. Örneğin, çok 5 kişinin 3'ın bildireceğinizi. Bu nedenle, (5,3) en parlak bir kılavuzuna bakın.

```python
# normalize the diagonal cells so that they don't overpower the rest of the cells when visualized
row_sums = conf_mx.sum(axis=1, keepdims=True)
norm_conf_mx = conf_mx / row_sums
np.fill_diagonal(norm_conf_mx, 0)

fig = plt.figure(figsize=(8,5))
ax = fig.add_subplot(111)
cax = ax.matshow(norm_conf_mx, cmap=plt.cm.bone)
ticks = np.arange(0, 10, 1)
ax.set_xticks(ticks)
ax.set_yticks(ticks)
ax.set_xticklabels(ticks)
ax.set_yticklabels(ticks)
fig.colorbar(cax)
plt.ylabel('true labels', fontsize=14)
plt.xlabel('predicted values', fontsize=14)
plt.savefig('conf.png')
plt.show()
```

![Grafik gösteren karışıklık Matrisi](./media/tutorial-deploy-models-with-aml/confusion.png)

## <a name="deploy-as-web-service"></a>Web hizmeti olarak dağıtma

Model test ettikten ve Sonuçlardan memnun olana sonra model kapsayıcı örneklerini barındırılan bir web hizmeti olarak dağıtın. 

Container Instances için doğru ortamı oluşturmak için aşağıdakileri sağlar:
* Modelin nasıl kullanılacağını gösteren puanlama betiği
* Hangi paketlerin yüklenmesi gerektiğini gösteren bir ortam dosyası
* Kapsayıcı örneği oluşturmak için bir yapılandırma dosyası
* Daha önce eğittiğiniz model

<a name="make-script"></a>

### <a name="create-scoring-script"></a>Puanlama betiği oluşturma

Puanlama betiği score.PY adlı oluşturun. Web hizmeti çağrısı, bu modelin nasıl kullanılacağını göstermek için kullanır.

Puanlama betiğine gerekli iki işlevi eklemelisiniz:
* Normalde modeli genel bir nesneye yükleyen `init()` işlevi. Bu işlev, Docker kapsayıcısı başlatıldığında tek bir kez çalıştırılır. 

* `run(input_data)` işlevi, giriş verileri temelinde bir değer tahmin etmek için modeli kullanır. Çalıştırmanın girişleri ve çıkışlarında serileştirme ve seriden çıkarma için normal olarak JSON kullanılır ama başka biçimler de desteklenir.

```python
%%writefile score.py
import json
import numpy as np
import os
import pickle
from sklearn.externals import joblib
from sklearn.linear_model import LogisticRegression

from azureml.core.model import Model

def init():
    global model
    # retreive the path to the model file using the model name
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    return json.dumps(y_hat.tolist())
```

<a name="make-myenv"></a>

### <a name="create-environment-file"></a>Ortam dosyası oluşturma

Ardından, betiğin tüm paket bağımlılıklarını belirten myenv.yml adlı bir ortam dosyası oluşturun. Bu dosya, söz konusu bağımlılıkların tümünün Docker görüntüsüne yüklendiğinden emin olmak için kullanılır. Bu modele `scikit-learn` ve `azureml-sdk` gerekir.

```python
from azureml.core.conda_dependencies import CondaDependencies 

myenv = CondaDependencies()
myenv.add_conda_package("scikit-learn")

with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
```
`myenv.yml` dosyasının içeriğini gözden geçirin.

```python
with open("myenv.yml","r") as f:
    print(f.read())
```

### <a name="create-configuration-file"></a>Yapılandırma dosyası oluşturma

Dağıtım yapılandırma dosyası oluşturma ve CPU sayısını ve Container Instances kapsayıcınızı gerekli RAM miktarını gigabayt belirtin. Modelinize bağlı olsa da, birçok model için varsayılan 1 çekirdek ve 1 gigabayt RAM çoğunlukla yeterlidir. Daha sonra fazlasına ihtiyacınız olduğunu düşünüyorsanız, görüntüyü yeniden oluşturabilir ve hizmeti yeniden dağıtabilirsiniz.

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1, 
                                               memory_gb=1, 
                                               tags={"data": "MNIST",  "method" : "sklearn"}, 
                                               description='Predict MNIST with sklearn')
```

### <a name="deploy-in-container-instances"></a>Kapsayıcı örneklerini dağıtın
Tahmini tamamlanma süresi: **yaklaşık 7-8 dakika**

Görüntüyü yapılandırın ve dağıtın. Aşağıdaki kod şu adımlardan geçer:

1. Bir görüntü kullanarak oluşturun:
   * Puanlama dosyası (`score.py`).
   * Ortam dosyası (`myenv.yml`).
   * Model dosyası.
1. Bu görüntüyü çalışma alanının altına kaydedin. 
1. Container Instances kapsayıcınızı için görüntüyü gönderin.
1. Yukarı Container ınstances'da bir kapsayıcı görüntüsü kullanarak başlatın.
1. Web hizmeti HTTP uç noktasını alın.


```python
%%time
from azureml.core.webservice import Webservice
from azureml.core.image import ContainerImage

# configure the image
image_config = ContainerImage.image_configuration(execution_script="score.py", 
                                                  runtime="python", 
                                                  conda_file="myenv.yml")

service = Webservice.deploy_from_model(workspace=ws,
                                       name='sklearn-mnist-svc',
                                       deployment_config=aciconfig,
                                       models=[model],
                                       image_config=image_config)

service.wait_for_deployment(show_output=True)
```

REST istemci çağrılarını kabul eden puanlama web hizmetinin HTTP uç noktasını alın. Bu uç nokta web hizmeti test etmek veya bir uygulamaya tümleştirmek isteyen kişiyle paylaşabilirsiniz. 

```python
print(service.scoring_uri)
```


## <a name="test-deployed-service"></a>Dağıtılan hizmeti test etme

Daha önce tüm test verileri modelin yerel sürüm ile puanlanmış. Şimdi, test verilerinden alınan rastgele 30 görüntü örneğiyle dağıtılan modeli test edebilirsiniz.  

Aşağıdaki kod şu adımlardan geçer:
1. Container Instances içinde barındırılan web hizmeti verileri bir JSON olarak dizi Gönder. 

1. Hizmeti çağırmak için SDK'nın `run` API’sini kullanma. Ham çağrıları curl gibi herhangi bir HTTP aracını kullanarak da yapabilirsiniz.

1. Döndürülen Öngörüler yazdırma ve bunları birlikte girdi görüntülerini çizim. Yanlış sınıflandırılmış örnekleri vurgulamak için kırmızı yazı tipi ve ters görüntü (siyah üzerine beyaz) kullanılır. 

 Model doğruluğu yüksek olduğundan, yanlış sınıflandırılmış örneği görebilmek için önce aşağıdaki kodu birkaç kez çalıştırmanız gerekebilir.

```python
import json

# find 30 random samples from test set
n = 30
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding = 'utf8')

# predict using the deployed model
result = json.loads(service.run(input_data=test_samples))

# compare actual value vs. the predicted values:
i = 0
plt.figure(figsize = (20, 1))

for s in sample_indices:
    plt.subplot(1, n, i + 1)
    plt.axhline('')
    plt.axvline('')
    
    # use different color for misclassified sample
    font_color = 'red' if y_test[s] != result[i] else 'black'
    clr_map = plt.cm.gray if y_test[s] != result[i] else plt.cm.Greys
    
    plt.text(x=10, y =-10, s=result[i], fontsize=18, color=font_color)
    plt.imshow(X_test[s].reshape(28, 28), cmap=clr_map)
    
    i = i + 1
plt.show()
```

Sonucunu test görüntülerinin rastgele oluşturulmuş bir örnek aşağıda verilmiştir: ![Sonuçları gösteren grafik](./media/tutorial-deploy-models-with-aml/results.png)

Web hizmetini test etmek için ham HTTP isteği de gönderebilirsiniz.

```python
import requests
import json

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

headers = {'Content-Type':'application/json'}

# for AKS deployment you'd need to the service key in the header as well
# api_key = service.get_key()
# headers = {'Content-Type':'application/json',  'Authorization':('Bearer '+ api_key)} 

resp = requests.post(service.scoring_uri, input_data, headers=headers)

print("POST to url", service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer öğreticiler ve araştırma için kaynak grubu ve çalışma alanı tutmak için bu API çağrısı kullanarak yalnızca Container Instances dağıtımı silebilirsiniz:

```python
service.delete()
```

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]


## <a name="next-steps"></a>Sonraki adımlar

+ Tüm hakkında bilgi edinin [Azure Machine Learning hizmeti için dağıtım seçeneklerini](how-to-deploy-and-where.md)ACI, Azure Kubernetes hizmeti, FPGA ve IOT Edge dahil olmak üzere.

+ Nasıl Azure Machine Learning hizmeti otomatik seçim modeliniz için en iyi algoritmayı ayarlamak ve bu model, derleme bakın. Denemenin [otomatik algoritması seçimi](tutorial-auto-train-models.md) öğretici. 