---
title: 'Görüntü sınıflandırma Öğreticisi: Modelleri dağıtma'
titleSuffix: Azure Machine Learning service
description: Bu öğretici, Azure Machine Learning hizmetini kullanarak Python Jupyter not defterinde scikit-learn ile bir görüntü sınıflandırma modelinin nasıl dağıtıldığını gösterir. Bu öğretici, iki bölümden oluşan bir serinin ikinci bölümüdür.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: sdgilley
ms.author: sgilley
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: 9709d18b00d65578ca3a63fe5044e0b9f7b52d58
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66515576"
---
# <a name="tutorial-deploy-an-image-classification-model-in-azure-container-instances"></a>Öğretici: Azure Container ınstances'da bir görüntü sınıflandırma model dağıtma

Bu öğretici, **iki bölümden oluşan bir öğretici serisinin ikinci bölümüdür**. [Önceki öğreticide](tutorial-train-models-with-aml.md) makine öğrenmesi modellerini eğittiniz ve buluttaki çalışma alanınıza modeli kaydettiniz.  

Bir web hizmeti olarak modeli dağıtacağız hazırsınız artık [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/). Bu örnekte bir Docker görüntüsü, bir görüntü bir web hizmetidir. Bu, Puanlama mantığı ve model kapsüller. 

Öğreticinin bu bölümünde Azure Machine Learning hizmeti için aşağıdaki görevleri kullanın:

> [!div class="checklist"]
> * Test ortamınızı ayarlayın.
> * Model çalışma alanınızdan alın.
> * Modeli yerel olarak test edin.
> * Model Container Instances'a dağıtacaksınız.
> * Dağıtılan modeli test edin.

Container Instances, test ve iş akışını anlamak için harika bir çözümdür. Ölçeklenebilir üretim dağıtımları için Azure Kubernetes hizmeti kullanmayı düşünün. Daha fazla bilgi için [nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md).

>[!NOTE]
> Bu makalede kod, Azure Machine Learning SDK sürüm 1.0.41 ile test edilmiştir.

## <a name="prerequisites"></a>Önkoşullar
Atlamak [geliştirme ortamını](#start) not defteri adımlarını okumak için.  

Not Defteri çalıştırmak için ilk modeli eğitimi tamamlayın [öğretici (Kısım 1): Bir Azure Machine Learning hizmeti ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md).   Ardından çalıştırın **öğreticiler/img-sınıflandırma-bölüm 2-deploy.ipynb** aynı notebook sunucusu kullanarak not defteri.


## <a name="start"></a>Ortamı ayarlama

Başlangıç olarak test ortamını ayarlayın.

### <a name="import-packages"></a>Paketleri içeri aktarma

Bu öğretici için gerekli Python paketlerini içeri aktarın:

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

Önceki öğreticide çalışma alanınızda bir modeli kaydetmiştiniz. Şimdi bu çalışma alanını yüklemek ve modeli, yerel dizine indirin:


```python
from azureml.core import Workspace
from azureml.core.model import Model
import os 
ws = Workspace.from_config()
model=Model(ws, 'sklearn_mnist')

model.download(target_dir=os.getcwd(), exist_ok=True)

# verify the downloaded model file
file_path = os.path.join(os.getcwd(), "sklearn_mnist_model.pkl")

os.stat(file_path)
```

## <a name="test-the-model-locally"></a>Modeli yerel olarak test etme

Dağıtmadan önce modelinizi yerel olarak çalışır durumda olduğundan emin olun:
* Yük testi verileri.
* Test verilerini tahmin edin.
* Karışıklık matrisi inceleyin.

### <a name="load-test-data"></a>Test verilerini yükleme

Test verileri yük **. /data/** eğitim öğretici sırasında oluşturulan dizin:

```python
from utils import load_data
import os

data_folder = os.path.join(os.getcwd(), 'data')
# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the neural network converge faster
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_test = load_data(os.path.join(data_folder, 'test-labels.gz'), True).reshape(-1)
```

### <a name="predict-test-data"></a>Test verilerini tahmin etme

Öngörüler elde etmek için test veri modeline akış:

```python
import pickle
from sklearn.externals import joblib

clf = joblib.load( os.path.join(os.getcwd(), 'sklearn_mnist_model.pkl'))
y_hat = clf.predict(X_test)
```

###  <a name="examine-the-confusion-matrix"></a>Karışıklık matrisini inceleme

Test kümesinden kaç örneğin doğru sınıflandırıldığını görmek için karışıklık matrisi oluşturun. Doğru tahminler elde etmek için bildireceğinizi değeri dikkat edin: 

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
   

Karışıklık matrisini grafik olarak görüntülemek için `matplotlib` kullanın. Bu grafikte, gerçek değerleri x ekseni gösterir ve y ekseni tahmin edilen değerleri gösterir. Her bir kılavuz rengi hata hızını gösterir. Renk ne kadar açıksa hata oranı da o kadar yüksektir. Örneğin, çok 5 kişinin 3'ın bildireceğinizi. (5,3) en parlak bir kılavuz görürsünüz:

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

## <a name="deploy-as-a-web-service"></a>Bir web hizmeti olarak dağıtma

Model test ve Sonuçlardan memnun sonra model kapsayıcı örneklerini barındırılan bir web hizmeti olarak dağıtın. 

Container Instances için doğru ortamı oluşturmak için aşağıdaki bileşenler sağlar:
* Modelin nasıl kullanılacağını göstermek için Puanlama betiğine.
* Hangi paketlerin yüklenmesi gereken göstermek için bir ortam dosyası.
* Kapsayıcı örneği oluşturmak için bir yapılandırma dosyası.
* Önceden eğitilmiş model.

<a name="make-script"></a>

### <a name="create-scoring-script"></a>Puanlama betiği oluşturma

Adlı Puanlama betik oluşturma **score.py**. Web hizmeti çağrısı, modelin nasıl kullanılacağını göstermek için bu betiği kullanır.

Puanlama betik bu iki gerekli işlevleri içerir:
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
    # retrieve the path to the model file using the model name
    model_path = Model.get_model_path('sklearn_mnist')
    model = joblib.load(model_path)

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    # you can return any data type as long as it is JSON-serializable
    return y_hat.tolist()
```

<a name="make-myenv"></a>

### <a name="create-environment-file"></a>Ortam dosyası oluşturma

Sonraki adlı bir ortam dosyası oluşturma **myenv.yml**, betiğin paket bağımlılıklarının tümü belirtir. Bu dosya, tüm bu bağımlılıkların Docker görüntüsünü yüklü olduğundan emin olmak için kullanılır. Bu model gereken `scikit-learn` ve `azureml-sdk`:

```python
from azureml.core.conda_dependencies import CondaDependencies 

myenv = CondaDependencies()
myenv.add_conda_package("scikit-learn")

with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
```
İçeriği gözden `myenv.yml` dosyası:

```python
with open("myenv.yml","r") as f:
    print(f.read())
```

### <a name="create-a-configuration-file"></a>Yapılandırma dosyası oluşturma

Bir dağıtım yapılandırma dosyası oluşturun. CPU sayısı ve Container Instances kapsayıcınızı gerekli RAM miktarını gigabayt belirtin. Varsayılan bir Çekirdeği ve 1 gigabayt RAM, modelinize bağlı olsa da, çok sayıda model için yeterli olur. Daha sonra gerekirse, görüntüsünü yeniden oluşturmak ve hizmeti yeniden dağıtmanız gerekir.

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1, 
                                               memory_gb=1, 
                                               tags={"data": "MNIST",  "method" : "sklearn"}, 
                                               description='Predict MNIST with sklearn')
```

### <a name="deploy-in-container-instances"></a>Kapsayıcı örneklerini dağıtın
Dağıtımı tamamlamak için tahimini süre **yaklaşık 7-sekiz dakika**.

Görüntüyü yapılandırın ve dağıtın. Aşağıdaki kod şu adımlardan geçer:

1. Bu dosyalar'ı kullanarak bir görüntü oluşturun:
   * Puanlama dosyası `score.py`.
   * Ortam dosyası `myenv.yml`.
   * Model dosyası.
1. Çalışma alanı altındaki görüntüyü kaydedin. 
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

REST istemci çağrılarını kabul eden puanlama web hizmetinin HTTP uç noktasını alın. Bu uç nokta web hizmeti test etmek veya bir uygulamaya tümleştirmek isteyen kişiyle paylaşabilirsiniz: 

```python
print(service.scoring_uri)
```


## <a name="test-the-deployed-service"></a>Dağıtılan hizmet test

Daha önce tüm test verileri modelin yerel sürüm ile puanlanmış. Artık test verileri 30 görüntüleri rastgele oluşturulmuş bir örnek ile dağıtılan modeli test edebilirsiniz.  

Aşağıdaki kod şu adımlardan geçer:
1. Container Instances içinde barındırılan web hizmeti verileri bir JSON olarak dizi Gönder. 

1. Hizmeti çağırmak için SDK'nın `run` API’sini kullanma. Herhangi bir HTTP aracı gibi kullanarak ham çağrı yapabilirsiniz **curl**.

1. Döndürülen tahminleri yazdırma ve bunları giriş görüntüleriyle birlikte çizme. Kırmızı yazı tipi ve ters görüntü, siyah, beyaz bildireceğinizi örnekleri vurgulamak için kullanılır. 

Model doğruluğu yüksek olduğundan, bildireceğinizi örnek görebilmek için önce aşağıdaki kodu birkaç kez çalıştırın. gerekebilir:

```python
import json

# find 30 random samples from test set
n = 30
sample_indices = np.random.permutation(X_test.shape[0])[0:n]

test_samples = json.dumps({"data": X_test[sample_indices].tolist()})
test_samples = bytes(test_samples, encoding='utf8')

# predict using the deployed model
result = service.run(input_data=test_samples)

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

Bir rastgele test görüntülerinin örnekten oluşur:

![Sonuçları gösteren grafik](./media/tutorial-deploy-models-with-aml/results.png)

Ayrıca, web hizmeti test etmek için ham bir HTTP isteği gönderebilirsiniz:

```python
import requests

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

+ Tüm hakkında bilgi edinin [Azure Machine Learning hizmeti için dağıtım seçenekleri](how-to-deploy-and-where.md).
+ Bilgi edinmek için nasıl [istemcileri web hizmeti oluşturma](how-to-consume-web-service.md).
+  [Büyük miktarlarda veri tahminlerde](how-to-run-batch-predictions.md) zaman uyumsuz olarak.
+ Azure Machine Learning Modellerinizi izleme [Application Insights](how-to-enable-app-insights.md).
+ Denemenin [otomatik algoritması seçimi](tutorial-auto-train-models.md) öğretici. 
