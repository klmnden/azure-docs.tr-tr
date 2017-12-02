---
title: "Azure Machine Learning modeli Yönetim Web hizmeti dağıtımı | Microsoft Docs"
description: "Bu belgede, Azure Machine Learning modelini yönetim kullanarak bir makine öğrenimi modeline dağıtma adımları açıklanmaktadır."
services: machine-learning
author: raymondl
ms.author: raymondl, aashishb
manager: neerajkh
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 0d59dccec4532ff0903972f2b15ed9dd8429a2ed
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2017
---
# <a name="deploying-a-machine-learning-model-as-a-web-service"></a>Makine öğrenimi modeline web hizmeti olarak dağıtma

Azure Machine Learning modeli yönetim Docker tabanlı web hizmetlerinde kapsayıcılı gibi modelleri dağıtmak için arabirim sağlar. Spark, Microsoft Bilişsel Araç Seti (CNTK), Keras, Tensorflow ve Python gibi çerçeveleri kullanarak oluşturduğunuz modelleri dağıtabilirsiniz. 

Bu belge, Azure Machine Learning modeli yönetim komut satırı arabirimi (CLI) kullanarak web Hizmetleri olarak Modellerinizi dağıtma adımları kapsar.

## <a name="deploying-web-services"></a>Web Hizmetleri dağıtma
CLIs kullanarak bir küme veya yerel makine üzerinde çalışmak için web hizmetleri dağıtabilirsiniz.

Yerel bir dağıtımı ile başlayan öneririz. İlk modeli ve kod, ardından çalıştığını doğrulamak üretim ölçeği kullanmak için bir küme için web hizmetini dağıtma. Küme dağıtımı için ortamınızı kurma hakkında daha fazla bilgi için bkz: [Model Yönetim Yapılandırması](deployment-setup-configuration.md). 

Dağıtım adımları şunlardır:
1. Kaydedilmiş, eğitilen, Machine Learning modelinizi kullanın
2. Web hizmetinizin girdi ve çıktı verileri için bir şema oluşturun
3. Docker tabanlı kapsayıcı görüntü oluşturma
4. Oluşturma ve web hizmetini dağıtma

### <a name="1-save-your-model"></a>1. Modelinizi Kaydet
Örneğin, mymodel.pkl, kaydedilen eğitilen model dosyasıyla başlatın. Dosya uzantısı, eğitmek ve model kaydetmek için kullanılan platforma göre değişir. 

Örnek olarak, Python'un Pickle kitaplığı eğitilen bir modelin bir dosyaya kaydetmek için kullanabilirsiniz. Burada bir [örnek](http://scikit-learn.org/stable/modules/model_persistence.html) Iris veri kümesini kullanarak:

```python
import pickle
from sklearn import datasets
iris = datasets.load_iris()
X, y = iris.data, iris.target
clf.fit(X, y)  
saved_model = pickle.dumps(clf)
```

### <a name="2-create-a-schemajson-file"></a>2. Schema.json dosyası oluşturma
Bu adım isteğe bağlıdır. 

Otomatik olarak giriş ve çıkış web hizmetinizin doğrulamak için bir şema oluşturun. CLIs şema web hizmetiniz için Swagger belgesinin oluşturmak için de kullanabilirsiniz.

Şema oluşturmak için aşağıdaki kitaplıklar içeri aktarın:

```python
from azureml.api.schema.dataTypes import DataTypes
from azureml.api.schema.sampleDefinition import SampleDefinition
from azureml.api.realtime.services import generate_schema
```
Ardından, Spark dataframe, Pandas dataframe veya NumPy dizi gibi giriş değişkenleri tanımlayın. Aşağıdaki örnek, bir Numpy dizi kullanır:

```python
inputs = {"input_array": SampleDefinition(DataTypes.NUMPY, yourinputarray)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```
Aşağıdaki örnek, Spark dataframe kullanır:

```python
inputs = {"input_df": SampleDefinition(DataTypes.SPARK, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```

Aşağıdaki örnek, bir PANDAS dataframe kullanır:

```python
inputs = {"input_df": SampleDefinition(DataTypes.PANDAS, yourinputdataframe)}
generate_schema(run_func=run, inputs=inputs, filepath='service_schema.json')
```

### <a name="3-create-a-scorepy-file"></a>3. Score.py dosyası oluşturma
Modelinizi yükler ve modeli kullanan tahmin sonuç döndüren bir score.py dosyası sağlar.

Dosya iki işlevleri içermelidir: init ve çalıştırın.

Aşağıdaki kodu dosyanın üst kısmındaki model giriş ve tahmin verilerini topla yardımcı olan veri toplama işlevlerini etkinleştirmek için score.py ekleyin

    ```
    from azureml.datacollector import ModelDataCollector
    ```

Denetleme [model veri toplama](how-to-use-model-data-collection.md) bu özelliğinin nasıl kullanılacağı hakkında daha fazla ayrıntı için bölüm.

#### <a name="init-function"></a>Init işlevi
İnit işlevi kaydedilmiş modeli yüklemek için kullanın.

Model yüklenirken basit init işlevi örneği:

```python
def init():  
    from sklearn.externals import joblib
    global model, inputs_dc, prediction_dc
    model = joblib.load('model.pkl')

    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
```

#### <a name="run-function"></a>Çalışma işlevi
Çalışma işlevi, tahmin döndürmek için modeli ve giriş verilerini kullanır.

Girişini işleme ve tahmin sonucu döndürerek işlevi çalıştırma basit bir örneği:

```python
def run(input_df):
    global clf2, inputs_dc, prediction_dc
    try:
        prediction = model.predict(input_df)
        inputs_dc.collect(input_df)
        prediction_dc.collect(prediction)
        return prediction
    except Exception as e:
        return (str(e))
```

### <a name="4-register-a-model"></a>4. Bir model Kaydet
Aşağıdaki komut, 1. adımda oluşturulan model kaydetmek için kullanılır,

```
az ml model register --model [path to model file] --name [model name]
```

### <a name="5-create-manifest"></a>5. Bildirim oluşturma
Komutu aşağıdaki modeli için bir bildirim oluşturmaya yardımcı olan,

```
az ml manifest create --manifest-name [your new manifest name] -f [path to code file] -r [runtime for the image, e.g. spark-py]
```
Bağımsız değişken kullanarak daha önce kaydedilmiş bir model bildirime ekleyebilirsiniz `--model-id` veya `-i` yukarıda gösterilen komuttaki. Birden çok modelleri ile ek belirtilebilir -i bağımsız değişkenler.

### <a name="6-create-an-image"></a>6. Görüntü oluştur 
Önce kendi bildirimi oluşturduktan seçeneği bir görüntü oluşturabilirsiniz. 

```
az ml image create -n [image name] -manifest-id [the manifest ID]
```

Veya bildirimi oluşturma ve tek bir komutla görüntü. 

```
az ml image create -n [image name] --model-file [model file or folder path] -f [code file, e.g. the score.py file] -r [the runtime eg.g. spark-py which is the Docker container image base]
```

>[!NOTE]
>Komut parametreleri hakkında daha fazla ayrıntı için türü -h örneğin az ml görüntü komutunun sonuna -h oluşturun.


### <a name="7-create-and-deploy-the-web-service"></a>7. Oluşturma ve web hizmetini dağıtma
Aşağıdaki komutu kullanarak hizmet dağıtın:

```
az ml service create realtime --image-id <image id> -n <service name>
```

>[!NOTE] 
>Tek bir komut, önceki 4 adımları gerçekleştirmek için de kullanabilirsiniz. Daha fazla ayrıntı için komut kullanın -h hizmeti ile oluşturun.

### <a name="8-test-the-service"></a>8. Hizmeti test
Hizmeti çağırmak nasıl hakkında bilgi almak için aşağıdaki komutu kullanın:

```
az ml service usage realtime -i <service id>
```

Komut isteminden çalıştırma işlevini kullanarak hizmetini arayın:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [INPUT DATA]}"
```

Aşağıdaki örnek, bir örnek Iris web hizmeti çağırır:

```
az ml service run realtime -i <service id> -d "{\"input_df\": [{\"sepal length\": 3.0, \"sepal width\": 3.6, \"petal width\": 1.3, \"petal length\":0.25}]}"
```

## <a name="next-steps"></a>Sonraki adımlar
Web hizmetinizi yerel olarak çalıştırmak için test, büyük ölçekli kullanmak için bir kümeye dağıtabilirsiniz. Küme web hizmet dağıtımı için ayarlama hakkında daha fazla bilgi için bkz: [Model Yönetim Yapılandırması](deployment-setup-configuration.md). 
