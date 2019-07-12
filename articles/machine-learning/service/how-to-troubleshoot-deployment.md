---
title: Dağıtım sorunlarını giderme kılavuzu
titleSuffix: Azure Machine Learning service
description: Geçici çözüm, çözmek ve Azure Kubernetes hizmeti ve Azure Machine Learning hizmetini kullanarak Azure Container Instances ile ortak Docker dağıtım hatalarını giderme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: chris-lauren
ms.author: clauren
ms.reviewer: jmartens
ms.date: 07/09/2018
ms.custom: seodec18
ms.openlocfilehash: e0f4b024d717c08df3514df057abf89d55be1dc9
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707032"
---
# <a name="troubleshooting-azure-machine-learning-service-azure-kubernetes-service-and-azure-container-instances-deployment"></a>Azure Machine Learning hizmeti Azure Kubernetes hizmeti ve Azure Container Instances dağıtımı sorunlarını giderme

Geçici çözüm veya Azure Container Instances'a (ACI) ve Azure Machine Learning hizmetini kullanarak Azure Kubernetes Service (AKS) ile ortak Docker dağıtım hatalarını çözmek öğrenin.

Azure Machine Learning hizmetinde bir model dağıtımına, sistemin bir dizi görevi gerçekleştirir. Dağıtım görevleri şunlardır:

1. Çalışma alanı model kayıt defterinde modeli kaydedin.

2. Bir Docker görüntüsü oluşturun dahil olmak üzere:
    1. Kayıt defterinden kayıtlı modeli indirin. 
    2. Bir dockerfile ortam yaml dosyasında belirttiğiniz bağımlılıkları temel bir Python ortamı oluşturun.
    3. Model dosyalarınızı ve sağladığınız Puanlama betiği dockerfile'da ekleyin.
    4. Dockerfile'ı kullanarak yeni bir Docker görüntüsü oluşturun.
    5. Çalışma alanı ile ilişkili Azure Container Registry ile Docker görüntü kaydedin.

    > [!IMPORTANT]
    > Kodunuzu bağlı olarak, görüntü oluşturma durum otomatik olarak girişinizi.

3. Docker görüntüsünü Azure Container örneği (ACI) hizmetine veya Azure Kubernetes Service (AKS) dağıtın.

4. Yeni bir kapsayıcı (veya kapsayıcıları) ACI veya AKS başlatın. 

Bu işlem hakkında daha fazla bilgi [Model Yönetimi](concept-model-management-and-deployment.md) giriş.

## <a name="before-you-begin"></a>Başlamadan önce

Herhangi bir sorun çalıştırırsanız, yapılacak ilk şey dağıtım görevi bölmektir (önceki açıklanmıştır) sorunu ayırt etmek için tek tek adımlara.

Dağıtım görevlerinizde iyice bozucu kullanıyorsanız yararlı [Webservice.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) API veya [Webservice.deploy_from_model()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-) API, Bu işlevlerden her ikisini gerçekleştirmek yukarıda sözü edilen adımlardan bir tek bir eylem. Genellikle bu API'leri kullanışlıdır ancak bunları değiştirerek sorun giderme adımları kesilecek şekilde yardımcı olan API çağrılarının aşağıda.

1. Modeli kaydedin. Bazı örnek kodlar aşağıda verilmiştir:

    ```python
    # register a model out of a run record
    model = best_run.register_model(model_name='my_best_model', model_path='outputs/my_model.pkl')

    # or, you can register a file or a folder of files as a model
    model = Model.register(model_path='my_model.pkl', model_name='my_best_model', workspace=ws)
    ```

2. Görüntü oluşturun. Bazı örnek kodlar aşağıda verilmiştir:

    ```python
    # configure the image
    image_config = ContainerImage.image_configuration(runtime="python",
                                                      entry_script="score.py",
                                                      conda_file="myenv.yml")

    # create the image
    image = Image.create(name='myimg', models=[model], image_config=image_config, workspace=ws)

    # wait for image creation to finish
    image.wait_for_creation(show_output=True)
    ```

3. Görüntü hizmeti olarak dağıtalım. Bazı örnek kodlar aşağıda verilmiştir:

    ```python
    # configure an ACI-based deployment
    aci_config = AciWebservice.deploy_configuration(cpu_cores=1, memory_gb=1)

    aci_service = Webservice.deploy_from_image(deployment_config=aci_config, 
                                               image=image, 
                                               name='mysvc', 
                                               workspace=ws)
    aci_service.wait_for_deployment(show_output=True)    
    ```

Tek tek görevler dağıtım işlemine aşağı kıran sonra en yaygın hataların bazıları göz atabilirsiniz.

## <a name="image-building-fails"></a>Görüntü oluşturma başarısız

Docker görüntüsünü alınamazsa [image.wait_for_creation()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-) veya [service.wait_for_deployment()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#wait-for-deployment-show-output-false-) çağrı bazı ipuçları sunduğu bazı hata iletileri ile başarısız olur. Görüntü oluşturma günlüğü hataları ile ilgili daha fazla ayrıntı bulabilirsiniz. Aşağıda bazı örnek kodlar görüntü derleme günlük URI'si bulma göstermez.

```python
# if you already have the image object handy
print(image.image_build_log_uri)

# if you only know the name of the image (note there might be multiple images with the same name but different version number)
print(ws.images['myimg'].image_build_log_uri)

# list logs for all images in the workspace
for name, img in ws.images.items():
    print (img.name, img.version, img.image_build_log_uri)
```

Görüntü günlük URI'si, Azure blob Depolama'nızda depolanan bir günlük dosyasına işaret eden bir SAS URL'si ' dir. Yalnızca kopyalama ve yapıştırma URI ve bir tarayıcı penceresi içinde indirin ve günlük dosyasını görüntüleyin.

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Azure anahtar kasası erişim ilkesi ve Azure Resource Manager şablonları

Azure anahtar kasası erişim ilkesi ile ilgili bir sorun nedeniyle görüntü derleme de başarısız olabilir. Çalışma alanı ve ilişkili kaynakları (Azure anahtar kasası dahil), birden çok kez oluşturmak için bir Azure Resource Manager şablonu kullandığınızda, bu durum ortaya çıkabilir. Örneğin, şablon bir sürekli tümleştirme ve dağıtım işlem hattı bir parçası olarak aynı parametrelere sahip birden çok kez kullanma.

Şablonlar aracılığıyla çoğu kaynak oluşturma işlemleri bir kere etkili olur, ancak anahtar kasası erişim ilkeleri şablon kullanılan her zaman temizler. Key Vault kullandığı tüm mevcut bir çalışma alanı için erişim ilkeleri sonları erişimi temizleniyor. Yeni görüntüleri oluşturmaya çalıştığınızda bu durum hatalara neden olur. Alabileceğiniz hataların örnekleri şunlardır:

__Portal__:
```text
Create image "myimage": An internal server error occurred. Please try again. If the problem persists, contact support.
```

__SDK'SI__:
```python
image = ContainerImage.create(name = "myimage", models = [model], image_config = image_config, workspace = ws)
Creating image
Traceback (most recent call last):
  File "C:\Python37\lib\site-packages\azureml\core\image\image.py", line 341, in create
    resp.raise_for_status()
  File "C:\Python37\lib\site-packages\requests\models.py", line 940, in raise_for_status
    raise HTTPError(http_error_msg, response=self)
requests.exceptions.HTTPError: 500 Server Error: Internal Server Error for url: https://eastus.modelmanagement.azureml.net/api/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>/images?api-version=2018-11-19

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Python37\lib\site-packages\azureml\core\image\image.py", line 346, in create
    'Content: {}'.format(resp.status_code, resp.headers, resp.content))
azureml.exceptions._azureml_exception.WebserviceException: Received bad response from Model Management Service:
Response Code: 500
Headers: {'Date': 'Tue, 26 Feb 2019 17:47:53 GMT', 'Content-Type': 'application/json', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'api-supported-versions': '2018-03-01-preview, 2018-11-19', 'x-ms-client-request-id': '3cdcf791f1214b9cbac93076ebfb5167', 'x-ms-client-session-id': '', 'Strict-Transport-Security': 'max-age=15724800; includeSubDomains; preload'}
Content: b'{"code":"InternalServerError","statusCode":500,"message":"An internal server error occurred. Please try again. If the problem persists, contact support"}'
```

__CLI__:
```text
ERROR: {'Azure-cli-ml Version': None, 'Error': WebserviceException('Received bad response from Model Management Service:\nResponse Code: 500\nHeaders: {\'Date\': \'Tue, 26 Feb 2019 17:34:05
GMT\', \'Content-Type\': \'application/json\', \'Transfer-Encoding\': \'chunked\', \'Connection\': \'keep-alive\', \'api-supported-versions\': \'2018-03-01-preview, 2018-11-19\', \'x-ms-client-request-id\':
\'bc89430916164412abe3d82acb1d1109\', \'x-ms-client-session-id\': \'\', \'Strict-Transport-Security\': \'max-age=15724800; includeSubDomains; preload\'}\nContent:
b\'{"code":"InternalServerError","statusCode":500,"message":"An internal server error occurred. Please try again. If the problem persists, contact support"}\'',)}
```

Bu sorunu önlemek için aşağıdaki yaklaşımlardan birini önerilir:

* Şablon, birden çok kez aynı parametreleri dağıtılmaz. Veya bunları yeniden oluşturmak için bu şablonu kullanmadan önce var olan kaynakları silin.
* Anahtar kasası erişim ilkelerini inceleyin ve ardından bu ilkeleri ayarlamak için `accessPolicies` özelliği.
* Key Vault kaynağı zaten mevcut olup olmadığını denetleyin. Varsa, şablonu aracılığıyla yeniden oluşturmayın. Örneğin, zaten varsa, anahtar kasası kaynak oluşturma devre dışı bırakmanıza olanak tanıyan bir parametre ekleyin.

## <a name="debug-locally"></a>Yerel olarak hata ayıklama

ACI veya AKS için bir model dağıtımına sorunlarla karşılaşırsanız, bir yerel web hizmeti olarak dağıtmayı deneyin. Bir yerel web hizmeti kullanarak, sorunlarını gidermek kolaylaştırır. Modeli içeren bir Docker görüntüsü indirilir ve yerel sisteminizde başlatıldı.

> [!IMPORTANT]
> Yerel web hizmeti dağıtımları, çalışan bir yerel sisteminizde Docker yükleme gerektirir. Bir yerel web hizmetini dağıtmadan önce docker çalışıyor olması gerekir. Yükleme ve Docker'ı kullanma hakkında daha fazla bilgi için bkz: [ https://www.docker.com/ ](https://www.docker.com/).

> [!WARNING]
> Yerel web hizmeti dağıtımları üretim senaryoları için desteklenmez.

Yerel olarak dağıtmak için kullanılacak kodunuzu değiştirmek `LocalWebservice.deploy_configuration()` bir dağıtım yapılandırması oluşturmak için. Ardından `Model.deploy()` hizmeti dağıtmak için. Aşağıdaki örnek bir model dağıtır (bulunan `model` değişkeni) bir yerel web hizmeti olarak:

```python
from azureml.core.model import InferenceConfig,Model
from azureml.core.webservice import LocalWebservice

# Create inference configuration. This creates a docker image that contains the model.
inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml")

# Create a local deployment, using port 8890 for the web service endpoint
deployment_config = LocalWebservice.deploy_configuration(port=8890)
# Deploy the service
service = Model.deploy(ws, "mymodel", [model], inference_config, deployment_config)
# Wait for the deployment to complete
service.wait_for_deployment(True)
# Display the port that the web service is available on
print(service.port)
```

Bu noktada, normal olarak service ile çalışabilirsiniz. Örneğin, aşağıdaki kod, verileri hizmete gönderme gösterir:

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})

test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data=test_sample)
print(prediction)
```

### <a name="update-the-service"></a>Güncelleştirme hizmeti

Yerel test sırasında güncelleştirmeniz gerekebilir `score.py` dosya günlüğü ekleyip keşfettiğinize göre herhangi bir sorunu çözmeyi deneyin. Değişiklikleri yeniden `score.py` dosya, kullanın `reload()`. Örneğin, aşağıdaki kod, hizmet için komut dosyasını yeniden yükler ve verileri gönderir. Güncelleştirilmiş kullanarak verileri puanlanır `score.py` dosyası:

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> Komut dosyası tarafından belirtilen konumda yeniden `InferenceConfig` hizmet tarafından kullanılan nesne.

Model, Conda bağımlılıkları veya dağıtım yapılandırmasını değiştirmek için kullanın [update()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#update--args-). Aşağıdaki örnek, hizmet tarafından kullanılan modelini güncelleştirir:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Hizmeti Sil

Hizmeti silmek için kullanın [delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#delete--).

### <a id="dockerlog"></a> Docker günlüğünü inceleyin

Hizmet nesnesinden ayrıntılı Docker altyapısı günlük iletilerini yazdırabilirsiniz. ACI, AKS ve yerel dağıtımlar için günlüğü görüntüleyebilirsiniz. Aşağıdaki örnek, günlükleri yazdırma gösterilmiştir.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

## <a name="service-launch-fails"></a>Hizmet başlatma başarısız

Görüntü başarıyla oluşturulduktan sonra sistem, Dağıtım Yapılandırması'nı kullanarak bir kapsayıcı başlatma girişiminde bulunur. Kapsayıcı başlatma artırma işleminin bir parçası olarak `init()` işlevi Puanlama komut dosyanızdaki sistem tarafından çağrılır. İçinde yakalanmamış istisnalar varsa `init()` görebileceğiniz işlev **CrashLoopBackOff** hata hata iletisi.

Bilgi kullanın [Docker günlüğünü incelemek](#dockerlog) bölümü günlüklere bakın.

## <a name="function-fails-getmodelpath"></a>İşlevi başarısız: get_model_path()

Genellikle, `init()` Puanlama betiği işlevinde [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) işlevi, bir model dosyası veya bir model dosya klasörü kapsayıcıda bulmak için çağrılır. Model dosya veya klasörün bulunamazsa, işlev başarısız olur. Çalıştırmak için bu hata ayıklama için en kolay yolu olan Python kodu kapsayıcı Kabuğu'nda aşağıdaki:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
from azureml.core.model import Model
print(Model.get_model_path(model_name='my-best-model'))
```

Bu örnek yerel yolu yazdırır (göreli `/var/azureml-app`) burada Puanlama betiğinizi bekliyor model dosyası veya klasörü bulmak için kapsayıcıda. Ardından, dosya veya klasörün aslında burada olması beklenmektedir olup olmadığını doğrulayabilirsiniz.

Hata ayıklama için günlüğe kaydetme düzeyini ayarlama hatası tanımlanmasına yararlı olabilecek günlüğe kaydedilecek ek bilgi neden olabilir.

## <a name="function-fails-runinputdata"></a>İşlevi başarısız: run(input_data)

Hizmet başarıyla dağıtıldı, ancak Puanlama uç noktası veri göndermek çöküyor deyiminde yakalama hata ekleyebilirsiniz, `run(input_data)` ayrıntılı hata iletisi yerine döndürür, böylece işlev. Örneğin:

```python
def run(input_data):
    try:
        data = json.loads(input_data)['data']
        data = np.array(data)
        result = model.predict(data)
        return json.dumps({"result": result.tolist()})
    except Exception as e:
        result = str(e)
        # return error message back to the client
        return json.dumps({"error": result})
```

**Not**: Hata iletilerini döndüren `run(input_data)` sadece hata ayıklama için çağrısı yapılmalıdır. Güvenlik nedenleriyle, hata iletileri bu şekilde bir üretim ortamında döndürmemelidir.

## <a name="http-status-code-503"></a>HTTP durum kodu 503

Azure Kubernetes hizmeti dağıtımları ek yükü desteklemeye eklenecek çoğaltmaları sağlayan otomatik ölçeklendirmeyi destekler. Ancak, otomatik ölçeklendiricinin yönetmek için tasarlanan **aşamalı** değişiklikleri. Saniye başına istek büyük depoları alırsanız, istemcilerin HTTP durum kodu 503 alabilirsiniz.

503 durum kodları önlemeye yardımcı olabilecek iki şey vardır:

* Değişiklik hangi otomatik ölçeklendirme kullanımı düzeyinde yeni kopyalar oluşturur.
    
    Varsayılan olarak, otomatik ölçeklendirme hedef kullanım ayarlanır % 70'e, yani hizmet ani % 30 (RP'ler) saniye başına istek işleyebilir. Kullanım hedefine ayarlayarak yapabilirsiniz `autoscale_target_utilization` daha düşük bir değere.

    > [!IMPORTANT]
    > Bu değişiklik oluşturulacak çoğaltmaları neden olmaz *daha hızlı*. Bunun yerine, bunlar daha düşük bir kullanım eşiğine oluşturulur. % Kullanılan 70 hizmet olana kadar beklemek yerine değerin % 30 değiştirilmesi % 30 kullanımı oluştuğunda oluşturulacak çoğaltmaları neden olur.
    
    Web hizmeti geçerli en fazla yineleme zaten kullanıyor ve 503 durum kodları hala görüyorsanız, artırın `autoscale_max_replicas` çoğaltmaları maksimum sayısını artırmak için değer.

* En az yineleme sayısını değiştirin. En düşük çoğaltmaları artırma gelen ani değişiklikleri işlemek için daha büyük bir havuz sağlar.

    En az yineleme sayısını artırmak için ayarlanmış `autoscale_min_replicas` daha yüksek bir değer. Gerekli çoğaltmaları değerleri projenize belirli değerlerle değiştirerek aşağıdaki kodu kullanarak hesaplayabilirsiniz:

    ```python
    from math import ceil
    # target requests per second
    targetRps = 20
    # time to process the request (in seconds)
    reqTime = 10
    # Maximum requests per container
    maxReqPerContainer = 1
    # target_utilization. 70% in this example
    targetUtilization = .7

    concurrentRequests = targetRps * reqTime / targetUtilization

    # Number of container replicas
    replicas = ceil(concurrentRequests / maxReqPerContainer)
    ```

    > [!NOTE]
    > İstek artışlarını yeni minimum çoğaltmaları işleyebileceğinden daha büyük alırsanız, 503 sn yeniden alabilirsiniz. Örneğin, trafiği, hizmet artışları için en düşük çoğaltmaları artırmak gerekebilir.

Ayarı hakkında daha fazla bilgi için `autoscale_target_utilization`, `autoscale_max_replicas`, ve `autoscale_min_replicas` için bkz: [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) modül başvurusu.


## <a name="advanced-debugging"></a>Gelişmiş hata ayıklama

Bazı durumlarda, etkileşimli olarak da model dağıtımınızda bulunan Python kodunda hata ayıklama gerekebilir. Örneğin, giriş betiği başarısız oluyor ve nedeni tarafından ek günlükler belirlenemiyor. Visual Studio (PTVSD için) Visual Studio Code ve Python Tools kullanarak, Docker kapsayıcısı içinde çalışan kodu ekleyebilirsiniz.

> [!IMPORTANT]
> Hata ayıklama bu yöntemi kullanırken çalışmaz `Model.deploy()` ve `LocalWebservice.deploy_configuration` modeli yerel olarak dağıtılacak. Bunun yerine, bir görüntüsünü kullanarak oluşturmanız gerekir [ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py) sınıfı. 
>
> Yerel web hizmeti dağıtımları, çalışan bir yerel sisteminizde Docker yükleme gerektirir. Bir yerel web hizmetini dağıtmadan önce docker çalışıyor olması gerekir. Yükleme ve Docker'ı kullanma hakkında daha fazla bilgi için bkz: [ https://www.docker.com/ ](https://www.docker.com/).

### <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

1. Python Tools üzerinde yerel VS Code geliştirme ortamınızı Visual Studio (PTVSD için) yüklemek için aşağıdaki komutu kullanın:

    ```
    python -m pip install --upgrade ptvsd
    ```

    PTVSD ile VS Code kullanma hakkında daha fazla bilgi için bkz. [uzaktan hata ayıklama](https://code.visualstudio.com/docs/python/debugging#_remote-debugging).

1. Docker görüntüsü ile iletişim kurmak için VS Code yapılandırmak için yeni bir hata ayıklama yapılandırmasını oluşturun:

    1. VS koddan seçin __hata ayıklama__ menüsünü ve ardından __açın yapılandırmaları__. Adlı bir dosya __launch.json__ açılır.

    1. İçinde __launch.json__ dosya, içeren satırı Bul `"configurations": [`ve sonra aşağıdaki metni ekleyin:

        ```json
        {
            "name": "Azure Machine Learning service: Docker Debug",
            "type": "python",
            "request": "attach",
            "port": 5678,
            "host": "localhost",
            "pathMappings": [
                {
                    "localRoot": "${workspaceFolder}",
                    "remoteRoot": "/var/azureml-app"
                }
            ]
        }
        ```

        > [!IMPORTANT]
        > Zaten diğer girişler varsa yapılandırmaları bölümünde, virgül (,), eklediğiniz koddan sonra ekleyin.

        Bu bölümde, bağlantı noktası 5678 kullanarak Docker kapsayıcısı ekler.

    1. Kaydet __launch.json__ dosya.

### <a name="create-an-image-that-includes-ptvsd"></a>PTVSD içeren görüntü oluşturma

1. Dağıtımınız için conda ortam PTVSD içerir şekilde değiştirin. Aşağıdaki örnek, kullanarak eklemeyi gösterir. `pip_packages` parametresi:

    ```python
    from azureml.core.conda_dependencies import CondaDependencies 
    
    # Usually a good idea to choose specific version numbers
    # so training is made on same packages as scoring
    myenv = CondaDependencies.create(conda_packages=['numpy==1.15.4',            
                                'scikit-learn==0.19.1', 'pandas==0.23.4'],
                                 pip_packages = ['azureml-defaults==1.0.17', 'ptvsd'])
    
    with open("myenv.yml","w") as f:
        f.write(myenv.serialize_to_string())
    ```

1. PTVSD başlatmak ve hizmeti başlatıldığında bir bağlantı için bekleme için en üst kısmına aşağıdakileri ekleyin, `score.py` dosyası:

    ```python
    import ptvsd
    # Allows other computers to attach to ptvsd on this IP address and port.
    ptvsd.enable_attach(address=('0.0.0.0', 5678), redirect_output = True)
    # Wait 30 seconds for a debugger to attach. If none attaches, the script continues as normal.
    ptvsd.wait_for_attach(timeout = 30)
    print("Debugger attached...")
    ```

1. Hata ayıklama sırasında görüntü dosyaları yeniden oluşturmak zorunda kalmadan değişiklik yapmak isteyebilirsiniz. Docker görüntüsünü bir metin düzenleyicisi (vim) yüklemek için adlı yeni bir metin dosyası oluşturma `Dockerfile.steps` ve dosyanın içeriğini aşağıdakileri kullanın:

    ```text
    RUN apt-get update && apt-get -y install vim
    ```

    Bir metin düzenleyicisi, yeni bir görüntü oluşturmadan değişiklikleri test etmek için docker görüntüsünü içindeki dosyalara değiştirmenizi sağlar.

1. Kullanan bir görüntü oluşturmak için `Dockerfile.steps` dosya, kullanın `docker_file` görüntü oluşturulurken parametre. Aşağıdaki örnek bunun nasıl yapılacağı gösterilmektedir:

    > [!NOTE]
    > Bu örnek olduğunu varsayar `ws` noktaları, Azure Machine Learning çalışma alanı ve, `model` dağıtılan modeli. `myenv.yml` Dosyası 1. adımda oluşturduğunuz conda bağımlılıklarını içerir.

    ```python
    from azureml.core.image import Image, ContainerImage
    image_config = ContainerImage.image_configuration(runtime= "python",
                                 execution_script="score.py",
                                 conda_file="myenv.yml",
                                 docker_file="Dockerfile.steps")

    image = Image.create(name = "myimage",
                     models = [model],
                     image_config = image_config, 
                     workspace = ws)
    # Print the location of the image in the repository
    print(image.image_location)
    ```

Görüntü oluşturulduktan sonra kayıt defterindeki görüntü konum görüntülenir. Konumu aşağıdaki metne benzer:

```text
myregistry.azurecr.io/myimage:1
```

Bu metin örneğinde, kayıt defteri addır `myregistry` ve görüntü adlı `myimage`. Görüntü sürümü `1`.

### <a name="download-the-image"></a>Bir görüntü indirin

1. Bir komut istemi, terminal ya da diğer kabuğunu açın ve aşağıdaki [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) komutu, Azure Machine Learning çalışma alanını içeren Azure aboneliğine kimliğini doğrulamak için:

    ```azurecli
    az login
    ```

1. Azure kapsayıcı kayıt defteri (içeren görüntü ACR'ye) kimliğini doğrulamak için aşağıdaki komutu kullanın. Değiştirin `myregistry` görüntü kayıtlı bir zaman döndürdü:

    ```azurecli
    az acr login --name myregistry
    ```

1. Yerel bir Docker için kullanmak üzere görüntüyü indirmek için aşağıdaki komutu kullanın. Değiştirin `myimagepath` olduğunda döndürülen konum ile görüntü kayıtlı:

    ```bash
    docker pull myimagepath
    ```

    Görüntü yolu benzer `myregistry.azurecr.io/myimage:1`. Burada `myregistry` , kayıt defteri `myimage` , görüntü ve `1` görüntü sürümüdür.

    > [!TIP]
    > Önceki adımdan gelen kimlik doğrulaması her zaman en son değil. Yeterince kimlik doğrulaması ve çekme komutunu arasında bekleyin, kimlik doğrulama hatası alırsınız. Böyle bir durumda yeniden kimlik doğrulamaya zorlayabilir.

    İndirmeyi tamamlamak için gereken süreyi Internet bağlantınızın hızına bağlıdır. İşlem sırasında bir yükleme durumu görüntülenir. İndirme tamamlandıktan sonra kullanabileceğiniz `docker images` indirilip indirilmediğini doğrulamak için komutu.

1. Görüntüyle çalışmaya kolaylaştırmak için bir etiket eklemek için aşağıdaki komutu kullanın. Değiştirin `myimagepath` 2. adımda konum değerine sahip.

    ```bash
    docker tag myimagepath debug:1
    ```

    Geri kalan adımları için yerel görüntü olarak başvurabilirsiniz `debug:1` yerine tam görüntü yol değeri.

### <a name="debug-the-service"></a>Hizmet hata ayıklama

> [!TIP]
> PTVSD bağlantı zaman aşımını ayarlarsanız `score.py` dosya, VS Code hata ayıklama oturumu için zaman aşımı süresi dolmadan önce bağlamalısınız. VS Code'u başlatın, yerel kopyasını açabilir `score.py`, bir kesme noktası ayarlayın ve varsa bu bölümdeki adımları kullanarak önce gönderilmeye hazır.
>
> Hata ayıklama ve kesme noktaları ayarlama hakkında daha fazla bilgi için bkz. [hata ayıklama](https://code.visualstudio.com/Docs/editor/debugging).

1. Görüntü kullanarak bir Docker kapsayıcısı başlatmak için aşağıdaki komutu kullanın:

    ```bash
    docker run --rm --name debug -p 8000:5001 -p 5678:5678 debug:1
    ```

1. VS Code için PTVSD kapsayıcısının içinde iliştirmek için VS Code açıp anahtar veya select F5 kullanmak __hata ayıklama__. Sorulduğunda, __Azure Machine Learning hizmeti: Docker hata ayıklama__ yapılandırma. Yan çubuğundan debug simgesini de seçebilirsiniz __Azure Machine Learning hizmeti: Docker hata ayıklama__ hata ayıklama açılır menüsünde ve ardından hata ayıklayıcıyı iliştirmek için yeşil ok girişi.

    ![Hata Ayıkla simgesi, hata ayıklama Başlat düğmesi ve yapılandırma Seçicisi](media/how-to-troubleshoot-deployment/start-debugging.png)

Bu noktada, VS Code için PTVSD Docker kapsayıcısı içinde bağlanır ve daha önce ayarladığınız kesme noktasında durur. Çalışırken, kodda adım adım artık değişkenler, vb. görüntüleyin.

Python hata ayıklamak için VS Code kullanma hakkında daha fazla bilgi için bkz. [Python kodunuzdaki hataları ayıklamanıza](https://docs.microsoft.com/visualstudio/python/debugging-python-in-visual-studio?view=vs-2019).

<a id="editfiles"></a>
### <a name="modify-the-container-files"></a>Kapsayıcı dosyalarını değiştirme

Görüntü dosyalarda değişiklik yapmak için çalışan kapsayıcıya ekleme ve bir bash Kabuğu Yürüt. Burada, dosyalarını düzenlemek için vim kullanabilirsiniz:

1. Çalışan kapsayıcıya bağlanmak ve kapsayıcıdaki bir bash kabuğunu başlatın için aşağıdaki komutu kullanın:

    ```bash
    docker exec -it debug /bin/bash
    ```

1. Hizmet tarafından kullanılan dosyaları bulmak için kapsayıcı bash kabuğunda aşağıdaki komutu kullanın:

    ```bash
    cd /var/azureml-app
    ```

    Buradan, vim düzenlemek için kullanabileceğiniz `score.py` dosya. Vim kullanma hakkında daha fazla bilgi için bkz. [Vim Düzenleyicisi'ni kullanarak](https://www.tldp.org/LDP/intro-linux/html/sect_06_02.html).

1. Normalde bir kapsayıcıya değişiklikler kalıcı değildir. Yaptığınız kabuktan çıkış yapma önce aşağıdaki komutu kullanın. değişiklikleri kaydetmek için Yukarıdaki adımda başlatıldı (diğer bir deyişle, başka bir Kabuğu'nda):

    ```bash
    docker commit debug debug:2
    ```

    Bu komut, adlı yeni bir görüntü oluşturur `debug:2` , yaptığınız düzenlemeleri içerir.

    > [!TIP]
    > Geçerli kapsayıcıda durdurmak ve değişiklikler etkili olmadan önce yeni sürümü kullanmaya başlamak ihtiyacınız olacak.

1. Kapsayıcı dosyalarında VS Code kullanan yerel dosyalarla eşitlenmiş yaptığınız değişiklikleri tutmak emin olun. Aksi takdirde, hata ayıklayıcı deneyimi beklendiği gibi çalışmaz.

### <a name="stop-the-container"></a>Kapsayıcı Durdur

Kapsayıcıyı durdurmak için aşağıdaki komutu kullanın:

```bash
docker stop debug
```

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım hakkında daha fazla bilgi edinin:

* [Nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md)
* [Öğretici: Eğitim ve modelleri dağıtma](tutorial-train-models-with-aml.md)
