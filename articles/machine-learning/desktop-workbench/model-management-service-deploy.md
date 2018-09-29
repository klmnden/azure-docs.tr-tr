---
title: Azure Machine Learning model Yönetimi Web hizmeti dağıtımı | Microsoft Docs
description: Bu belge Azure Machine Learning model Yönetimi'ni kullanarak makine öğrenme modeli dağıtma adımlarını açıklar.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 01/03/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 8753463f90ae97d4b98d557eec5bd737b4853480
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47433982"
---
# <a name="deploying-a-machine-learning-model-as-a-web-service"></a>Makine öğrenme modeli bir web hizmeti olarak dağıtma

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Azure Machine Learning Model Yönetimi kapsayıcılı Docker tabanlı web Hizmetleri olarak modelleri dağıtma arabirimleri sağlar. Spark, Microsoft Bilişsel Araç Seti (CNTK), Keras, Tensorflow ve Python gibi çerçeveleri kullanarak oluşturduğunuz modellerini dağıtabilirsiniz. 

Bu belge, Modellerinizi web Hizmetleri olarak Azure Machine Learning Model yönetimi komut satırı arabirimi (CLI) kullanarak dağıtma adımları kapsar.

## <a name="what-you-need-to-get-started"></a>Başlamak için ihtiyacınız olanlar

En iyi bu kılavuz için bir Azure aboneliği veya Modellerinizi için dağıtabileceğiniz bir kaynak grubu Katılımcısı erişimi olması gerekir.
CLI'yı Azure Machine Learning Workbench ve üzerinde önceden yüklü olarak gelen [Azure Dsvm'leri](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-virtual-machine-overview).  Tek başına bir paket olarak da yüklenebilir.

Ayrıca, bir model Yönetimi hesabı ve dağıtım ortamı zaten ayarlanmış olması gerekir.  Model Yönetimi hesabı ve ortamınız için yerel ve Küme dağıtımı ayarlama hakkında daha fazla bilgi için bkz. [Model yönetimi yapılandırma](deployment-setup-configuration.md).

## <a name="deploying-web-services"></a>Web hizmetlerini dağıtma
Clı'yi kullanarak, yerel makinede veya bir kümede çalıştırmak için web hizmetleri dağıtabilir.

Yerel bir dağıtımı ile başlamanızı öneririz. İlk model ve koda, ardından çalıştığını doğrulamak bir kümeye üretim ölçeği-kullanılacak web hizmetini dağıtma.

Dağıtım adımları şunlardır:
1. Kaydedilmiş, eğitilen, makine öğrenimi modelinizi kullanın
2. Web hizmetinizin girdi ve çıktı verilerini bir şema oluşturun
3. Docker tabanlı kapsayıcı görüntüsü oluşturma
4. Web hizmeti oluşturma ve dağıtma

### <a name="1-save-your-model"></a>1. Modelinizi Kaydet
Örneğin, mymodel.pkl, kaydedilmiş eğitilen model dosyasıyla başlatın. Dosya uzantısı eğitme ve modeli kaydetmek için kullandığınız platformu göre değişir. 

Örneğin, Python'un Pickle kitaplığı eğitilen bir modelin bir dosyaya kaydetmek için kullanabilirsiniz. İşte bir [örnek](http://scikit-learn.org/stable/modules/model_persistence.html) Iris veri kümesini kullanarak:

```python
import pickle
from sklearn import datasets
iris = datasets.load_iris()
X, y = iris.data, iris.target
clf = linear_model.LogisticRegression()
clf.fit(X, y)  
saved_model = pickle.dumps(clf)
```

### <a name="2-create-a-schemajson-file"></a>2. Schema.json dosyası oluşturma

Şeması oluşturma isteğe bağlı olsa da, daha iyi işleme istek ve giriş değişken biçimini tanımlamak için önemle tavsiye edilir.

Giriş ve çıkış web hizmetinizin otomatik olarak doğrulamak için bir şema oluşturun. Clı'yi hem de şemayı web hizmetiniz için bir Swagger belgesi oluşturmak için de kullanabilirsiniz.

Şema oluşturmak için aşağıdaki kitaplıklar içeri aktarın:

```python
from azureml.api.schema.dataTypes import DataTypes
from azureml.api.schema.sampleDefinition import SampleDefinition
from azureml.api.realtime.services import generate_schema
```
Ardından, Spark dataframe, Pandas dataframe veya NumPy dizi gibi giriş değişkenleri tanımlayın. Aşağıdaki örnek, bir Numpy dizi kullanır:

```python
inputs = {"input_array": SampleDefinition(DataTypes.NUMPY, yourinputarray)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```
Aşağıdaki örnek, bir Spark dataframe kullanır:

```python
inputs = {"input_df": SampleDefinition(DataTypes.SPARK, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```

Aşağıdaki örnek, bir PANDAS dataframe kullanır:

```python
inputs = {"input_df": SampleDefinition(DataTypes.PANDAS, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```

Aşağıdaki örnek, genel bir JSON biçimi kullanır:

```python
inputs = {"input_json": SampleDefinition(DataTypes.STANDARD, yourinputjson)}
generate_schema(run_func=run, inputs=inputs, filepath='./outputs/service_schema.json')
```

### <a name="3-create-a-scorepy-file"></a>3. Score.py dosyası oluşturma
Modelinizi yükler ve modeli kullanarak tahmin sonuç döndüren bir score.py dosyası sağlayın.

Dosya iki işlev içermelidir: init ve çalıştırın.

Aşağıdaki kodu modeli giriş ve tahmin verilerini toplayın yardımcı olan veri koleksiyonu işlevini etkinleştirmek için score.py dosyasının en üstüne ekleyin

```python
from azureml.datacollector import ModelDataCollector
```

Denetleme [model veri koleksiyonu](how-to-use-model-data-collection.md) bu özelliğin nasıl kullanılacağı hakkında ayrıntılı bilgi için.

#### <a name="init-function"></a>Init işlevi
İnit işlevi kaydedilmiş modeli yüklemek için kullanın.

Modelin yüklenmesi bir basit init işlev örneği:

```python
def init():  
    from sklearn.externals import joblib
    global model, inputs_dc, prediction_dc
    model = joblib.load('model.pkl')

    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
```

#### <a name="run-function"></a>Çalıştırma işlevi
Çalıştırma işlevi, tahmin döndürülecek model ve giriş verilerini kullanır.

Girişini işleme ve tahmin sonucu döndürerek işlevi çalıştıran basit bir örneği:

```python
def run(input_df):
    # clf2 is the same model as clf1, but loaded from the model.pkl file
    global clf2, inputs_dc, prediction_dc
    try:
        prediction = model.predict(input_df)
        inputs_dc.collect(input_df)
        prediction_dc.collect(prediction)
        return prediction
    except Exception as e:
        return (str(e))
```

### <a name="4-register-a-model"></a>4. Bir modeli kaydedin
Aşağıdaki komut, 1. adımda oluşturulan model kaydetmek için kullanılır,

```
az ml model register --model [path to model file] --name [model name]
```

### <a name="5-create-manifest"></a>5. Bildirim oluşturma
Aşağıdaki komut, modele yönelik bir bildirim oluşturmaya yardımcı olur,

```
az ml manifest create --manifest-name [your new manifest name] -f [path to score file] -r [runtime for the image, e.g. spark-py]
```
Bağımsız değişkenini kullanarak önceden kaydedilmiş bir model için bildirimi ekleyebilirsiniz `--model-id` veya `-i` yukarıda gösterilen komuttaki. Birden çok modeli ile ek belirtilebilir. -i bağımsız değişkenler.

### <a name="6-create-an-image"></a>6. Görüntü oluştur 
Önce bildirim oluşturulduktan sonra seçeneğiyle bir görüntü oluşturabilirsiniz. 

```
az ml image create -n [image name] --manifest-id [the manifest ID]
```

>[!NOTE] 
>Tek bir komut, model kaydı, bildirim ve model oluşturma gerçekleştirmek için de kullanabilirsiniz. Daha fazla ayrıntı için bir komut kullanın -h hizmeti ile oluşturun.

Alternatif olarak, bir modeli kaydedin, bildirim oluşturmak ve görüntü oluşturma (ancak değil oluşturmak ve web hizmeti, henüz dağıtmak için) tek bir komut yoktur adım olarak aşağıdaki gibi.

```
az ml image create -n [image name] --model-file [model file or folder path] -f [code file, e.g. the score.py file] -r [the runtime e.g. spark-py which is the Docker container image base]
```

>[!NOTE]
>Komut parametreleri hakkında daha fazla ayrıntı için türü -h sonunda, örneğin, az ml görüntü komutu -h oluşturun.


### <a name="7-create-and-deploy-the-web-service"></a>7. Web hizmeti oluşturma ve dağıtma
Aşağıdaki komutu kullanarak hizmeti dağıtın:

```
az ml service create realtime --image-id <image id> -n <service name>
```

>[!NOTE] 
>Tek bir komut, önceki tüm 4 adımlarını gerçekleştirmek için de kullanabilirsiniz. Daha fazla ayrıntı için bir komut kullanın -h hizmeti ile oluşturun.

Alternatif olarak, bir modeli kaydedin, bildirim oluşturmak, bir görüntü oluşturma hem de oluşturun ve Web hizmetini bir adım olarak aşağıdaki gibi dağıtın tek bir komut yoktur.

```azurecli
az ml service create realtime --model-file [model file/folder path] -f [scoring file e.g. score.py] -n [your service name] -s [schema file e.g. service_schema.json] -r [runtime for the Docker container e.g. spark-py or python] -c [conda dependencies file for additional python packages]
```


### <a name="8-test-the-service"></a>8. Test hizmeti
Hizmet çağırma hakkında bilgi almak için aşağıdaki komutu kullanın:

```
az ml service usage realtime -i <service id>
```

Komut isteminden çalışma işlevini kullanarak hizmetini çağırın:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [INPUT DATA]}"
```

Aşağıdaki örnek, bir örnek Iris web hizmeti çağırır:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [{\"sepal length\": 3.0, \"sepal width\": 3.6, \"petal width\": 1.3, \"petal length\":0.25}]}"
```

## <a name="next-steps"></a>Sonraki adımlar
Web hizmetinizi yerel olarak çalıştırmak için test edilmiş, büyük ölçekli kullanmak için bir kümeye dağıtabilirsiniz. Web hizmeti dağıtımı için bir küme ayarlama hakkında daha fazla ayrıntı için bkz. [Model Yönetim Yapılandırması](deployment-setup-configuration.md). 
