---
title: Dağıtım sorunlarını giderme kılavuzu
titleSuffix: Azure Machine Learning service
description: Geçici çözüm, çözmek ve AKS ve Azure Machine Learning hizmetini kullanarak ACI ile ortak Docker dağıtım hatalarını giderme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: chris-lauren
ms.author: clauren
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 815be7400e0a0560ace7e07b317aeb25c2feacd5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60817424"
---
# <a name="troubleshooting-azure-machine-learning-service-aks-and-aci-deployments"></a>Azure Machine Learning hizmeti AKS ve ACI dağıtım sorunlarını giderme

Bu makalede, geçici bir çözüm veya Azure Container Instances'a (ACI) ve Azure Machine Learning hizmetini kullanarak Azure Kubernetes Service (AKS) ile ortak Docker dağıtım hatalarını çözmek öğreneceksiniz.

Azure Machine Learning hizmetinde bir model dağıtımına, sistemin bir dizi görevi gerçekleştirir. Bu karmaşık bir olay dizisi ve bazen sorunları ortaya çıkar. Dağıtım görevleri şunlardır:

1. Çalışma alanı model kayıt defterinde modeli kaydedin.

2. Bir Docker görüntüsü oluşturun dahil olmak üzere:
    1. Kayıt defterinden kayıtlı modeli indirin. 
    2. Bir dockerfile ortam yaml dosyasında belirttiğiniz bağımlılıkları temel bir Python ortamı oluşturun.
    3. Model dosyalarınızı ve sağladığınız Puanlama betiği dockerfile'da ekleyin.
    4. Dockerfile'ı kullanarak yeni bir Docker görüntüsü oluşturun.
    5. Çalışma alanı ile ilişkili Azure Container Registry ile Docker görüntü kaydedin.

3. Docker görüntüsünü Azure Container örneği (ACI) hizmetine veya Azure Kubernetes Service (AKS) dağıtın.

4. Yeni bir kapsayıcı (veya kapsayıcıları) ACI veya AKS başlatın. 

Bu işlem hakkında daha fazla bilgi [Model Yönetimi](concept-model-management-and-deployment.md) giriş.

## <a name="before-you-begin"></a>Başlamadan önce

Herhangi bir sorun çalıştırırsanız, yapılacak ilk şey dağıtım görevi bölmektir (önceki açıklanmıştır) sorunu ayırt etmek için tek tek adımlara. 

Kullanıyorsanız yararlıdır `Webservice.deploy` API veya `Webservice.deploy_from_model` olduğundan, bu işlevler, tek bir eyleme yukarıda sözü edilen adımları gruplamak API. Genellikle bu API'leri kullanışlıdır ancak bunları değiştirerek sorun giderme adımları kesilecek şekilde yardımcı olan API çağrılarının aşağıda.

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
                                                      execution_script="score.py",
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
Sistem, Docker görüntüsünü derleyin kaydedemediği `image.wait_for_creation()` çağrı bazı ipuçları sunduğu bazı hata iletileri ile başarısız olur. Görüntü oluşturma günlüğü hataları ile ilgili daha fazla ayrıntı bulabilirsiniz. Aşağıda bazı örnek kodlar görüntü derleme günlük URI'si bulma göstermez.

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

Azure anahtar kasası erişim ilkesi ile ilgili bir sorun nedeniyle görüntü derleme de başarısız olabilir. Çalışma alanı ve ilişkili kaynakları (Azure anahtar kasası dahil), birden çok kez oluşturmak için bir Azure Resource Manager şablonu kullandığınızda, bu durum oluşabilir. Örneğin, şablon bir sürekli tümleştirme ve dağıtım işlem hattı bir parçası olarak aynı parametrelere sahip birden çok kez kullanma.

Şablonlar aracılığıyla çoğu kaynak oluşturma işlemleri bir kere etkili olur, ancak anahtar kasası erişim ilkeleri şablon kullanılan her zaman temizler. Bu erişim keser kullandığı tüm mevcut bir çalışma alanı için Key vault'a. Yeni görüntüleri oluşturmaya çalıştığınızda bu hatalara neden olur. Alabileceğiniz hataların örnekleri şunlardır:

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
* Anahtar kasası erişim ilkelerini incelemek ve bunu ayarlamak için kullanın `accessPolicies` özelliği.
* Key Vault kaynağı zaten mevcut olup olmadığını denetleyin. Varsa, şablonu aracılığıyla yeniden oluşturmayın. Örneğin, zaten varsa, anahtar kasası kaynak oluşturma devre dışı bırakmanıza olanak tanıyan bir parametre ekleyin.

## <a name="service-launch-fails"></a>Hizmet başlatma başarısız
Görüntü başarıyla oluşturulduktan sonra sistem ACI veya AKS Dağıtım yapılandırmanıza bağlı olarak bir kapsayıcı başlatma girişiminde bulunur. ACI dağıtım daha basit bir tek kapsayıcı dağıtımı olduğundan ilk olarak, denemek için önerilir. Bu şekilde tüm AKS özgü sorunu çıkarabilirsiniz.

Kapsayıcı başlatma artırma işleminin bir parçası olarak `init()` işlevi Puanlama komut dosyanızdaki sistem tarafından çağrılır. İçinde yakalanmamış istisnalar varsa `init()` görebileceğiniz işlev **CrashLoopBackOff** hata hata iletisi. Sorun gidermenize yardımcı olacak birkaç ipucu aşağıda verilmiştir.

### <a name="inspect-the-docker-log"></a>Docker günlüğünü inceleyin
Hizmet nesnesinden ayrıntılı Docker altyapısı günlük iletilerini yazdırabilirsiniz.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

### <a name="debug-the-docker-image-locally"></a>Docker görüntüsünü yerel olarak hata ayıklama
Zamanlarda Docker günlüğünü yanlış neler hakkında yeterli bilgi vermez. Bir adım ötesine gidin ve yerleşik Docker görüntüsü çekin, yerel bir kapsayıcı başlatma ve doğrudan Canlı kapsayıcısının içinde etkileşimli olarak hata ayıklayın. Yerel bir kapsayıcı başlatma için Docker altyapısının yerel olarak çalışıyor olmalıdır ve'iniz de çok daha kolay olurdu [azure-cli](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yüklü.

İlk olarak biz görüntü konumunu bulmak gerekir:

```python
# print image location
print(image.image_location)
```

Görüntü konumu bu biçimdedir: `<acr-name>.azurecr.io/<image-name>:<version-number>`, gibi `myworkpaceacr.azurecr.io/myimage:3`. 

Şimdi komut satırı pencerenize gidin. Azure-cli yüklü varsa, görüntünün depolandığı çalışma alanı ile ilişkili ACR (Azure Container Registry) oturum açmak için aşağıdaki komutları yazabilirsiniz. 

```sh
# log on to Azure first if you haven't done so before
$ az login

# make sure you set the right subscription in case you have access to multiple subscriptions
$ az account set -s <subscription_name_or_id>

# now let's log in to the workspace ACR
# note the acr-name is the domain name WITHOUT the ".azurecr.io" postfix
# e.g.: az acr login -n myworkpaceacr
$ az acr login -n <acr-name>
```
Azure-cli yüklü yoksa, kullanabileceğiniz `docker login` ACR oturum komutu. Ancak kullanıcı adını ve parolayı ACR, Azure portalından ilk alınması gerekir.

ACR oturum açtıktan sonra Docker görüntüsünü çekme ve yerel olarak bir kapsayıcı başlatabilir ve ardından kullanarak hata ayıklama için bir bash oturumu başlatmak `docker run` komutu:

```sh
# note the image_id is <acr-name>.azurecr.io/<image-name>:<version-number>
# for example: myworkpaceacr.azurecr.io/myimage:3
$ docker run -it <image_id> /bin/bash
```

Çalışmakta olan kapsayıcıyı bir bash oturumu başlatma sonra Puanlama betiklerinizi bulabilirsiniz `/var/azureml-app` klasör. Ardından, Puanlama betiklerinizi hatalarını ayıklamak için bir Python oturumu da başlatabilirsiniz. 

```sh
# enter the directory where scoring scripts live
cd /var/azureml-app

# find what Python packages are installed in the python environment
pip freeze

# sanity-check on score.py
# you might want to edit the score.py to trigger init().
# as most of the errors happen in init() when you are trying to load the model.
python score.py
```
Komut dosyalarınızı değiştirmek için bir metin düzenleyicisi gerektiği durumlarda, vim, nano, Emacs veya diğer tercih ettiğiniz düzenleyiciyi yükleyebilirsiniz.

```sh
# update package index
apt-get update

# install a text editor of your choice
apt-get install vim
apt-get install nano
apt-get install emacs

# launch emacs (for example) to edit score.py
emacs score.py

# exit the container bash shell
exit
```

Ayrıca, yerel olarak web hizmeti oluşturmaya başlayın ve HTTP trafiğini göndermek. Flask sunucusunu Docker kapsayıcısındaki 5001 bağlantı noktasında çalışıyor. Herhangi diğer bağlantı noktaları konak makinesi üzerinde kullanılabilir eşleyebilirsiniz.
```sh
# you can find the scoring API at: http://localhost:8000/score
$ docker run -p 8000:5001 <image_id>
```

## <a name="function-fails-getmodelpath"></a>İşlevi başarısız: get_model_path()
Genellikle, `init()` Puanlama betiği işlevinde `Model.get_model_path()` işlevi, bir model dosyası veya bir model dosya klasörü kapsayıcıda bulmak için çağrılır. Model dosya veya klasörün bulunamazsa genellikle bir başarısızlık kaynağıdır, budur. Çalıştırmak için bu hata ayıklama için en kolay yolu olan Python kodu kapsayıcı Kabuğu'nda aşağıdaki:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
from azureml.core.model import Model
print(Model.get_model_path(model_name='my-best-model'))
```

Bu yerel yolu yazdırır (göreli `/var/azureml-app`) burada Puanlama betiğinizi bekliyor model dosyası veya klasörü bulmak için kapsayıcıda. Ardından, dosya veya klasörün aslında burada olması beklenmektedir olup olmadığını doğrulayabilirsiniz.

Hata ayıklama için günlüğe kaydetme düzeyini ayarlama hatası tanımlanmasına yararlı olabilecek günlüğe kaydedilecek neden ek bilgi sağlayabilir.

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
**Not**: Hata iletilerini döndüren `run(input_data)` sadece hata ayıklama için çağrısı yapılmalıdır. Güvenlik nedeniyle bir üretim ortamında Bunun iyi bir fikir olmayabilir.

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

Ayarı hakkında daha fazla bilgi için `autoscale_target_utilization`, `autoscale_max_replicas`, ve `autoscale_min_replicas` için bkz: [AksWebservice](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) modül başvurusu.


## <a name="next-steps"></a>Sonraki adımlar

Dağıtım hakkında daha fazla bilgi edinin: 
* [Nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md)

* [Öğretici: Eğitim ve modelleri dağıtma](tutorial-train-models-with-aml.md)
