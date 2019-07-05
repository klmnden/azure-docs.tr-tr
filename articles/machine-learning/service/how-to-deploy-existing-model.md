---
title: Mevcut bir modelin nasıl kullanılacağını
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti hizmetinin dışında eğitilen modelleri ile nasıl kullanabileceğinizi öğrenin. Azure Machine Learning hizmetinin dışında oluşturulmuş modelleri kaydetmek ve bunları bir web hizmeti veya Azure IOT Edge modülü olarak dağıtabilirsiniz.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 06/19/2019
ms.openlocfilehash: 332129c9847c317369d5631c3af584da9430e9dd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67453690"
---
# <a name="how-to-use-an-existing-model-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile mevcut modelini kullanma

Mevcut bir machine learning modeli Azure Machine Learning hizmeti ile kullanmayı öğrenin.

Bir machine learning, Azure Machine Learning hizmetinin dışında eğitilen modeli varsa, hizmet modeli bir web hizmeti olarak veya bir IOT Edge cihazına dağıtmak için kullanmaya devam edebilirsiniz. 

> [!TIP]
> Bu makalede, kaydetme ve var olan bir model dağıtımına temel bilgileri sağlar. Dağıtıldıktan sonra Azure Machine Learning hizmeti için modelinizin izleme sağlar. Ayrıca, veri değişikliklerini analiz veya eğitim yeni sürümlerini modeli için kullanılabilen dağıtım gönderilen girdi verilerini depolamak sağlar.
>
> Burada kullanılan terimleri ve kavramları hakkında daha fazla bilgi için bkz. [yönetin, dağıtın ve izleyin, makine öğrenimi modelleri](concept-model-management-and-deployment.md).
>
> Dağıtım işlemi hakkında genel bilgi için bkz. [dağıtma modeller Azure Machine Learning hizmeti ile](how-to-deploy-and-where.md).

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure Machine Learning hizmeti çalışma alanı. Daha fazla bilgi için [çalışma alanı oluşturma](setup-create-workspace.md).

    > [!TIP]
    > Bu makalede Python örnekleri `ws` Azure Machine Learning hizmeti çalışma alanında, değişkeni ayarlanır.
    >
    > CLI örneklerde, bir yer tutucu `myworkspace` ve `myresourcegroup`. Bu çalışma alanınızı ve içerdiği kaynak grubu adı ile değiştirin.

* Azure Machine SDK Learning. Daha fazla bilgi için Python SDK'sı bölümüne bakın. [çalışma alanı oluşturma](setup-create-workspace.md#sdk).

* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve [Machine Learning CLI uzantısını](reference-azure-machine-learning-cli.md).

* Eğitilen bir modeli. Bir veya daha fazla dosyalar üzerinde geliştirme ortamınızı modeli kalıcı gerekir.

    > [!NOTE]
    > Azure Machine Learning hizmetinin dışında eğitilmiş bir modeli kaydetme göstermek için bu makaledeki örnek kod parçacıkları Paolo Ripamonti'nın Twitter yaklaşım analizi projesi tarafından oluşturulmuş modelleri kullanma: [ https://www.kaggle.com/paoloripamonti/twitter-sentiment-analysis ](https://www.kaggle.com/paoloripamonti/twitter-sentiment-analysis).

## <a name="register-the-models"></a>Model kaydetme

Model kaydediliyor, sürüm, depolamak ve meta veri çalışma alanınızdaki modelleri hakkında izlemenize olanak sağlar. Aşağıdaki Python ve CLI örneklerde `models` dizinini içeren `model.h5`, `model.w2v`, `encoder.pkl`, ve `tokenizer.pkl` dosyaları. Bu örnekte yer alan dosyaları karşıya yükler `models` adlı yeni bir model kaydı olarak dizin `sentiment`:

```python
from azureml.core.model import Model
# Tip: When model_path is set to a directory, you can use the child_paths parameter to include
#      only some of the files from the directory
model = Model.register(model_path = "./models",
                       model_name = "sentiment",
                       description = "Sentiment analysis model trained outside Azure Machine Learning service",
                       workspace = ws)
```

Daha fazla bilgi için [Model.register()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model(class)?view=azure-ml-py#register-workspace--model-path--model-name--tags-none--properties-none--description-none--datasets-none--model-framework-none--model-framework-version-none--child-paths-none-) başvuru.

```azurecli
az ml model register -p ./models -n sentiment -w myworkspace -g myresourcegroup
```

Daha fazla bilgi için [az ml model kaydı](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-register) başvuru.


Genel olarak, model kaydı hakkında daha fazla bilgi için bkz [yönetin, dağıtın ve izleyin, makine öğrenimi modelleri](concept-model-management-and-deployment.md).


## <a name="define-inference-configuration"></a>Çıkarım yapılandırmasını tanımlayın

Çıkarım yapılandırması, dağıtılmış bir modelinin çalıştırmak için kullanılan ortam tanımlar. Çıkarım yapılandırma modeli dağıtıldığında çalıştırmak için kullanılan aşağıdaki dosyaları başvuruyor:

* Çalışma zamanı. Yalnızca geçerli çalışma zamanı için şu anda Python değerdir.
* Bir giriş betiğine girildi. Bu dosya (adlı `score.py`) dağıtılan hizmet başladığında modeli yükler. Veri alma, modeline geçirme ve ardından bir yanıt döndürme sorumludur.
* Conda ortam dosyası. Bu dosya, model ve giriş betiği çalıştırmak için gereken Python paketlerini tanımlar. 

Aşağıdaki örnek Python SDK'sını kullanarak bir temel çıkarımı yapılandırması gösterilmektedir:

```python
from azureml.core.model import InferenceConfig

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml")
```

Daha fazla bilgi için [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) başvuru.

CLI'yı çıkarımı yapılandırma YAML dosyadan yükler:

```yaml
{
   "entryScript": "score.py",
   "runtime": "python",
   "condaFile": "myenv.yml"
}
```

Çıkarım yapılandırma hakkında daha fazla bilgi için bkz. [dağıtma modeller Azure Machine Learning hizmeti ile](how-to-deploy-and-where.md).

### <a name="entry-script"></a>Giriş betiği

Yalnızca iki gerekli işlevi, giriş betik vardır `init()` ve `run(data)`. Bu işlevler, hizmet başlatma sırasında başlatmak ve bir istemci tarafından geçirilen istek verilerini kullanarak modeldeki çalıştırmak için kullanılır. Betiğin geri kalanını yükleniyor ve modellere çalışan işler.

> [!IMPORTANT]
> Tüm modeller için uygun bir genel giriş komut dosyası değil. Kullanılan modeline her zaman özeldir. Yükleme modeli anlamalısınız modelin veri biçimi ve model kullanarak verileri puanlamak nasıl.

Aşağıdaki Python kodu bir örneği girdi betiğidir (`score.py`):

```python
import pickle
import json
import time
from keras.models import load_model
from keras.preprocessing.sequence import pad_sequences
from gensim.models.word2vec import Word2Vec
from azureml.core.model import Model

# SENTIMENT
POSITIVE = "POSITIVE"
NEGATIVE = "NEGATIVE"
NEUTRAL = "NEUTRAL"
SENTIMENT_THRESHOLDS = (0.4, 0.7)
SEQUENCE_LENGTH = 300

# Called when the deployed service starts
def init():
    global model
    global tokenizer
    global encoder
    global w2v_model

    # Get the path where the model(s) registered as the name 'sentiment' can be found.
    model_path = Model.get_model_path('sentiment')
    # load models
    model = load_model(model_path + '/model.h5')
    w2v_model = Word2Vec.load(model_path + '/model.w2v')

    with open(model_path + '/tokenizer.pkl','rb') as handle:
        tokenizer = pickle.load(handle)

    with open(model_path + '/encoder.pkl','rb') as handle:
        encoder = pickle.load(handle)

# Handle requests to the service
def run(data):
    try:
        # Pick out the text property of the JSON request.
        # This expects a request in the form of {"text": "some text to score for sentiment"}
        data = json.loads(data)
        prediction = predict(data['text'])
        #Return prediction
        return prediction
    except Exception as e:
        error = str(e)
        return error

# Determine sentiment from score
def decode_sentiment(score, include_neutral=True):
    if include_neutral:
        label = NEUTRAL
        if score <= SENTIMENT_THRESHOLDS[0]:
            label = NEGATIVE
        elif score >= SENTIMENT_THRESHOLDS[1]:
            label = POSITIVE
        return label
    else:
        return NEGATIVE if score < 0.5 else POSITIVE

# Predict sentiment using the model
def predict(text, include_neutral=True):
    start_at = time.time()
    # Tokenize text
    x_test = pad_sequences(tokenizer.texts_to_sequences([text]), maxlen=SEQUENCE_LENGTH)
    # Predict
    score = model.predict([x_test])[0]
    # Decode sentiment
    label = decode_sentiment(score, include_neutral=include_neutral)

    return {"label": label, "score": float(score),
       "elapsed_time": time.time()-start_at}  
```

Giriş betikler hakkında daha fazla bilgi için bkz. [dağıtma modeller Azure Machine Learning hizmeti ile](how-to-deploy-and-where.md).

### <a name="conda-environment"></a>Conda ortamı

Aşağıdaki YAML, model ve giriş betiği çalıştırmak için gereken conda ortam açıklanmaktadır:

```yaml
name: inference_environment
dependencies:
- python=3.6.2
- tensorflow
- numpy
- scikit-learn
- pip:
    - azureml-defaults
    - keras
```

Daha fazla bilgi için [dağıtma modeller Azure Machine Learning hizmeti ile](how-to-deploy-and-where.md).

## <a name="define-deployment"></a>Dağıtım tanımlayın

[Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice?view=azure-ml-py) paket dağıtımı için kullanılan sınıfları içerir. Kullandığınız sınıfı, modelin dağıtıldığı belirler. Örneğin, Azure Kubernetes Service'teki bir web hizmeti olarak dağıtmak için kullanın [AksWebService.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py#deploy-configuration-autoscale-enabled-none--autoscale-min-replicas-none--autoscale-max-replicas-none--autoscale-refresh-seconds-none--autoscale-target-utilization-none--collect-model-data-none--auth-enabled-none--cpu-cores-none--memory-gb-none--enable-app-insights-none--scoring-timeout-ms-none--replica-max-concurrent-requests-none--max-request-wait-time-none--num-replicas-none--primary-key-none--secondary-key-none--tags-none--properties-none--description-none--gpu-cores-none--period-seconds-none--initial-delay-seconds-none--timeout-seconds-none--success-threshold-none--failure-threshold-none--namespace-none-) dağıtım yapılandırması oluşturmak için.

Aşağıdaki Python kodu yerel bir dağıtım için bir dağıtım yapılandırması tanımlar. Bu yapılandırma, yerel bilgisayarınıza bir web hizmeti olarak modeli dağıtır.

> [!IMPORTANT]
> Bir çalışan yüklemesine sahip yerel bir dağıtım gerektirir [Docker](https://www.docker.com/) yerel bilgisayarınızda:

```python
from azureml.core.webservice import LocalWebservice

deployment_config = LocalWebservice.deploy_configuration()
```

Daha fazla bilgi için [LocalWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.localwebservice?view=azure-ml-py#deploy-configuration-port-none-) başvuru.

CLI dağıtımı yapılandırması, bir YAML dosyadan yükler:

```YAML
{
    "computeType": "LOCAL"
}
```

Azure Kubernetes Service'teki Azure Bulutu gibi farklı işlem hedef dağıtma, dağıtım yapılandırması değiştirme olarak oldukça kolaydır. Daha fazla bilgi için [nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md).

## <a name="deploy-the-model"></a>Modeli dağıtma

Aşağıdaki örnekte adlı kayıtlı modeli hakkında bilgi yükler `sentiment`ve ardından adlı bir hizmet olarak dağıtır `sentiment`. Dağıtım sırasında dağıtım yapılandırması ve çıkarım yapılandırmayı oluşturup service ortamı yapılandırmak için kullanılır:

```python
from azureml.core.model import Model

model = Model(ws, name='sentiment')
service = Model.deploy(ws, 'myservice', [model], inference_config, deployment_config)

service.wait_for_deployment(True)
print(service.state)
print("scoring URI: " + service.scoring_uri)
```

Daha fazla bilgi için [Model.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-) başvuru.

CLI'yı modelden dağıtmak için aşağıdaki komutu kullanın. Bu komut kayıtlı modeli 1 sürümünü dağıtır (`sentiment:1`) içinde depolanan çıkarımı ve Dağıtım Yapılandırması'nı kullanarak `inferenceConfig.json` ve `deploymentConfig.json` dosyaları:

```azurecli
az ml model deploy -n myservice -m sentiment:1 --ic inferenceConfig.json --dc deploymentConfig.json
```

Daha fazla bilgi için [az ml model dağıtma](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) başvuru.

Dağıtım hakkında daha fazla bilgi için bkz. [nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md).

## <a name="request-response-consumption"></a>İstek-yanıt tüketim

Dağıtımdan sonra Puanlama URI görüntülenir. Bu URI, hizmete istekler göndermek için istemciler tarafından kullanılabilir. Aşağıdaki örnek, veri hizmetine gönderir ve yanıtı görüntüleyen temel bir Python istemci aynıdır:

```python
import requests
import json

scoring_uri = 'scoring uri for your service'
headers = {'Content-Type':'application/json'}

test_data = json.dumps({'text': 'Today is a great day!'})

response = requests.post(scoring_uri, data=test_data, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Dağıtılan hizmetini kullanma hakkında daha fazla bilgi için bkz. [istemci oluşturma](how-to-consume-web-service.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning Modellerinizi Application Insights ile izleme](how-to-enable-app-insights.md)
* [Üretimde modelleri için veri toplama](how-to-enable-data-collection.md)
* [Nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md)
* [Dağıtılan bir modelde bir istemci oluşturma](how-to-consume-web-service.md)