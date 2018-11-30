---
title: 'Öğretici: Azure Machine Learning hizmetiyle Azure Container Instance’daki (ACI) görüntü sınıflandırma modelini dağıtma'
description: Bu öğretici, Azure Machine Learning hizmetini kullanarak Python Jupyter not defterinde scikit-learn ile bir görüntü sınıflandırma modelinin nasıl dağıtıldığını gösterir.  Bu öğretici iki bölümden oluşan bir serinin ikinci bölümüdür.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 841448f477accb8a73d543447cd317bb9b427408
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52497593"
---
# <a name="tutorial-2--deploy-an-image-classification-model-in-azure-container-instance-aci"></a>Öğretici 2: Azure Container Instance’da (ACI) bir görüntü sınıflandırma modelini dağıtma

Bu öğretici, **iki bölümden oluşan bir öğretici serisinin ikinci bölümüdür**. [Önceki öğreticide](tutorial-train-models-with-aml.md) makine öğrenmesi modellerini eğittiniz ve buluttaki çalışma alanınıza modeli kaydettiniz.  

Şimdi, [Azure Container Instances](https://docs.microsoft.com/azure/container-instances/)’da (ACI) modeli web hizmeti olarak dağıtmaya hazırsınız. Web hizmeti, puanlama mantığını ve modelin kendisini kapsülleyen bir görüntüdür; buradaki örnekte Docker görüntüsüdür. 

Öğreticinin bu bölümünde Azure Machine Learning hizmetini (Önizleme) kullanarak aşağıdakileri yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Test ortamınızı ayarlama
> * Çalışma alanınızdan modeli alma
> * Modeli yerel olarak test etme
> * Modeli ACI’ya dağıtma
> * Dağıtılan modeli test etme

ACI üretim dağıtımları için ideal olmasa da, iş akışını test etmek ve anlamak için çok uygundur. Ölçeklenebilir üretim dağıtımları için [Azure Kubernetes Service](how-to-deploy-to-aks.md)’i kullanmayı göz önünde bulundurabilirsiniz.

## <a name="get-the-notebook"></a>Not defterini alma

Kolaylık olması için, bu öğretici bir [Jupyter notebook](https://aka.ms/aml-notebook-tut-02) olarak sağlanır. `02.deploy-models.ipynb` notebook'unu Azure Notebooks üzerinde veya kendi Jupyter notebook sunucunuzda çalıştırın.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]


## <a name="prerequisites"></a>Önkoşullar

[Öğretici 1: Azure Machine Learning hizmeti ile görüntü sınıflandırma modelini eğitme](tutorial-train-models-with-aml.md) notebook’unda model eğitimini tamamlayın.  


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

Test kümesinden kaç örneğin doğru sınıflandırıldığını görmek için karışıklık matrisi oluşturun. Yanlış tahminler için yanlış sınıflandırılmış değere dikkat edin. 

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
   

Karışıklık matrisini grafik olarak görüntülemek için `matplotlib` kullanın. Bu grafikte X ekseni asıl değerleri, Y ekseni de tahmini değerleri gösterir. Her kılavuzun rengi, hata oranını gösterir. Renk ne kadar açıksa hata oranı da o kadar yüksektir. Örneğin, birçok 5 yanlışlıkla 3 olarak sınıflandırılmıştır. Bu nedenle en parlak kılavuz (5,3) konumundadır.

```python
# normalize the diagnal cells so that they don't overpower the rest of the cells when visualized
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

![karışıklık matrisi](./media/tutorial-deploy-models-with-aml/confusion.png)

## <a name="deploy-as-web-service"></a>Web hizmeti olarak dağıtma

Modeli test edip sonuçlarından da memnun kaldıktan sonra, modeli ACI’da barındırılan bir web hizmeti olarak dağıtın. 

Doğru ACI ortamını oluşturmak için şunları sağlayın:
* Modelin nasıl kullanılacağını gösteren puanlama betiği
* Hangi paketlerin yüklenmesi gerektiğini gösteren bir ortam dosyası
* ACI’yı oluşturmak için bir yapılandırma dosyası
* Daha önce eğittiğiniz model

<a name="make-script"></a>

### <a name="create-scoring-script"></a>Puanlama betiği oluşturma

Modelin nasıl kullanılacağını göstermek için web hizmeti çağrısının kullandığı, score.py adlı puanlama betiğini oluşturun.

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

Dağıtım yapılandırma dosyası oluşturun ve ACI kapsayıcısına gereken CPU sayısını ve gigabayt cinsinden RAM miktarını belirtin. Modelinize bağlı olsa da, birçok model için varsayılan 1 çekirdek ve 1 gigabayt RAM çoğunlukla yeterlidir. Daha sonra fazlasına ihtiyacınız olduğunu düşünüyorsanız, görüntüyü yeniden oluşturabilir ve hizmeti yeniden dağıtabilirsiniz.

```python
from azureml.core.webservice import AciWebservice

aciconfig = AciWebservice.deploy_configuration(cpu_cores=1, 
                                               memory_gb=1, 
                                               tags={"data": "MNIST",  "method" : "sklearn"}, 
                                               description='Predict MNIST with sklearn')
```

### <a name="deploy-in-aci"></a>ACI’da dağıtma
Tahmini tamamlanma süresi: **yaklaşık 7-8 dakika**

Görüntüyü yapılandırın ve dağıtın. Aşağıdaki kod şu adımlardan geçer:

1. Şunları kullanarak bir görüntü oluşturma:
   * Puanlama dosyası (`score.py`)
   * Ortam dosyası (`myenv.yml`)
   * Model dosyası
1. Bu görüntüyü çalışma alanının altına kaydedin. 
1. Görüntüyü ACI kapsayıcısına gönderin.
1. Görüntüyü kullanarak ACI’da bir kapsayıcı başlatın.
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

REST istemci çağrılarını kabul eden puanlama web hizmetinin HTTP uç noktasını alın. Bu uç nokta, web hizmetini test etmek veya bunu bir uygulamayla tümleştirmek isteyen herkesle paylaşılabilir. 

```python
print(service.scoring_uri)
```


## <a name="test-deployed-service"></a>Dağıtılan hizmeti test etme

Daha önce test verilerinin tümünü modelin yerel sürümüyle puanladınız. Şimdi, test verilerinden alınan rastgele 30 görüntü örneğiyle dağıtılan modeli test edebilirsiniz.  

Aşağıdaki kod şu adımlardan geçer:
1. Verileri JSON dizisi olarak ACI’da barındırılan web hizmetine gönderme. 

1. Hizmeti çağırmak için SDK'nın `run` API’sini kullanma. cURL gibi HTTP araçlarını kullanarak ham çağrılar da yapabilirsiniz.

1. Döndürülen tahminleri yazdırma ve bunları giriş görüntüleriyle birlikte çizme. Yanlış sınıflandırılmış örnekleri vurgulamak için kırmızı yazı tipi ve ters görüntü (siyah üzerine beyaz) kullanılır. 

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

Burada test görüntülerinden alınan rastgele bir örneğin sonucu gösterilir: ![sonuçlar](./media/tutorial-deploy-models-with-aml/results.png)

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

Kaynak grubunu ve çalışma alanını diğer öğreticilerde kullanmak üzere saklamak için, şu API çağrısını kullanarak yalnızca ACI dağıtımını silebilirsiniz:

```python
service.delete()
```

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu Azure Machine Learning hizmeti öğreticisinde Python kullanarak aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Test ortamınızı ayarlama
> * Çalışma alanınızdan modeli alma
> * Modeli yerel olarak test etme
> * Modeli ACI’ya dağıtma
> * Dağıtılan modeli test etme
 
Azure Machine Learning hizmetinin modelinize en iyi algoritmayı nasıl otomatik olarak seçip ayarlayabildiğini ve size o modeli oluşturduğunu görmek için [Otomatik algoritma seçimi](tutorial-auto-train-models.md) öğreticisini de deneyebilirsiniz.
