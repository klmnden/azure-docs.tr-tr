---
title: "Hızlı Başlangıç: Python SDK'sı"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, ortak görevler için Python SDK'sını kullanmayı öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: pafarley
ms.openlocfilehash: 23db6f889e2ca4266b7e3566c18cf9a85d4062a8
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517564"
---
# <a name="azure-cognitive-services-computer-vision-sdk-for-python"></a>Azure Bilişsel hizmetler görüntü işleme için Python SDK'sı

Görüntü İşleme hizmeti geliştiricilerin görüntü işlemeye ve bilgi döndürmeye yönelik gelişmiş algoritmalara erişmesini sağlar. Bilgisayar işleme algoritmaları, görüntü içeriğini ilgilendiğiniz görsel özelliklere bağlı olarak, farklı şekillerde analiz edin. 

* [Resim çözümleme](#analyze-an-image)
* [Konu etki alanı listesini alma](#get-subject-domain-list)
* [Etki alanına göre bir resmi çözümleme](#analyze-an-image-by-domain)
* [Metin açıklama Görüntü Al](#get-text-description-of-an-image)
* [Resimlerdeki el yazısı görüntüsünü Al](#get-text-from-image)
* [Küçük resim oluşturma](#generate-thumbnail)

Bu hizmet hakkında daha fazla bilgi için bkz. [görüntü işleme nedir?] [computervision_docs].

Daha fazla belgelerini mi arıyorsunuz?

* [SDK başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)
* [Bilişsel hizmetler görüntü işleme belgeleri](https://docs.microsoft.com/azure/cognitive-services/computer-vision/)

## <a name="prerequisites"></a>Önkoşullar

* [Python 3.6 +][python]
* Ücretsiz [görüntü işleme anahtarı] [ computervision_resource] ve ilişkili bölge. Örneğini oluşturduğunuzda bu değerlere ihtiyacınız olur [ComputerVisionAPI] [ ref_computervisionclient] istemci nesnesi. Bu değerleri almak için aşağıdaki yöntemlerden birini kullanın. 

### <a name="if-you-dont-have-an-azure-subscription"></a>Bir Azure aboneliğiniz yoksa

Ücretsiz bir anahtar ile 7 gün için geçerli oluşturma **[deneyin] [ computervision_resource]** deneyimi için görüntü işleme hizmeti. Anahtarı oluştururken anahtar ve bölge adını kopyalayın. Bunun için gerekir [istemcisi oluşturma](#create-client).

Anahtar oluşturulduktan sonra aşağıdakilere dikkat edin:

* Anahtar değerini: biçimi ile 32 karakter dizesi `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` 
* Anahtar bölgesi: uç nokta URL'sinin alt etki alanı https://**westcentralus**. api.cognitive.microsoft.com

### <a name="if-you-have-an-azure-subscription"></a>Bir Azure aboneliğiniz varsa

Aboneliğinizde bir kaynak oluşturmak için en kolay yöntem aşağıdaki kullanmaktır [Azure CLI] [ azure_cli] komutu. Bu, birçok bilişsel hizmetler kullanılabilir bir Bilişsel hizmet anahtarı oluşturur. Seçim yapması _mevcut_ kaynak grubu adı, örneğin, "cogserv-grubum" ve yeni bilgisayar işleme kaynak adı, "my-bilgisayar-işleme-kaynak gibi". 

```Bash
RES_REGION=westeurope 
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

az cognitiveservices account create \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --location $RES_REGION \
    --kind CognitiveServices \
    --sku S0 \
    --yes
```

<!--
## Installation

Install the Azure Cognitive Services Computer Vision SDK with [pip][pip], optionally within a [virtual environment][venv].

### Configure a virtual environment (optional)

Although not required, you can keep your base system and Azure SDK environments isolated from one another if you use a [virtual environment][virtualenv]. Execute the following commands to configure and then enter a virtual environment with [venv][venv], such as `cogsrv-vision-env`:

```Bash
python3 -m venv cogsrv-vision-env
source cogsrv-vision-env/bin/activate
```
-->

### <a name="install-the-sdk"></a>SDK yükle

Python için Azure Bilişsel hizmetler bilgisayar işleme SDK yükleme [paket] [ pypi_computervision] ile [pip][pip]:

```Bash
pip install azure-cognitiveservices-vision-computervision
```

## <a name="authentication"></a>Authentication

Görüntü işleme kaynağınızı oluşturduktan sonra ihtiyacınız kendi **bölge**, diğeri kendi **hesap anahtarları** istemci nesnesi örneklemek için.

Örneği oluşturmak için bu değerleri kullanabilirsiniz [ComputerVisionAPI] [ ref_computervisionclient] istemci nesnesi. 

Örneğin, Bash terminal ortam değişkenlerini ayarlamak için kullanın:

```Bash
ACCOUNT_REGION=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>
```

### <a name="for-azure-subscription-users-get-credentials-for-key-and-region"></a>Azure abonelik kullanıcıları için anahtar ve bölgenize yönelik kimlik bilgilerini alma

Bölge ve anahtar anımsamıyorsanız bulmak için aşağıdaki yöntemi kullanabilirsiniz. Bir anahtar ve bölge oluşturmanız gerekiyorsa, yöntemi için kullanabileceğiniz [Azure abonelik sahipleri](#if-you-have-an-azure-subscription) veya [kullanıcıların bir Azure aboneliği olmadan](#if-you-dont-have-an-azure-subscription).

Kullanım [Azure CLI] [ cloud_shell] görüntü işleme hesabıyla iki ortam değişkenleri doldurmak için aşağıdaki kod parçacığında **bölge** ve kendi **anahtarları**(Ayrıca bu değerleri bulabilirsiniz [Azure portalında][azure_portal]). Kod parçacığı, Bash kabuğunda biçimlendirilir.

```Bash
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

export ACCOUNT_REGION=$(az cognitiveservices account show \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query location \
    --output tsv)

export ACCOUNT_KEY=$(az cognitiveservices account keys list \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query key1 \
    --output tsv)
```


### <a name="create-client"></a>İstemcisi oluşturma

Ortam değişkenlerinden bölge ve anahtarını alın ardından oluşturma [ComputerVisionAPI] [ ref_computervisionclient] istemci nesnesi.  

```Python
from azure.cognitiveservices.vision.computervision import ComputerVisionAPI
from azure.cognitiveservices.vision.computervision.models import VisualFeatureTypes
from msrest.authentication import CognitiveServicesCredentials

# Get region and key from environment variables
import os
region = os.environ['ACCOUNT_REGION']
key = os.environ['ACCOUNT_KEY']

# Set credentials
credentials = CognitiveServicesCredentials(key)

# Create client
client = ComputerVisionAPI(region, credentials)
```

## <a name="examples"></a>Örnekler

Gereksinim duyduğunuz bir [ComputerVisionAPI] [ ref_computervisionclient] aşağıdaki görevlerden herhangi birini kullanmadan önce istemci nesnesi.

### <a name="analyze-an-image"></a>Resim çözümleme

Bir görüntü ile belirli özellikler için çözümleyebilirsiniz [ `analyze_image` ] [ ref_computervisionclient_analyze_image]. Kullanım [ `visual_features` ] [ ref_computervision_model_visualfeatures] görüntü üzerinde gerçekleştirmek için analizi türleri ayarlamak için özellik. Ortak değerler `VisualFeatureTypes.tags` ve `VisualFeatureTypes.description`.

```Python
url = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Broadway_and_Times_Square_by_night.jpg/450px-Broadway_and_Times_Square_by_night.jpg"

image_analysis = client.analyze_image(url,visual_features=[VisualFeatureTypes.tags])

for tag in image_analysis.tags:
    print(tag)
```

### <a name="get-subject-domain-list"></a>Konu etki alanı listesini alma

Görüntünüzü ile analiz etmek için kullanılan konu etki alanlarını gözden geçirmenize [ `list_models` ] [ ref_computervisionclient_list_models]. Bu etki alanı adları kullanılır, [etki alanına göre bir resmi çözümleme](#analyze-an-image-by-domain). Bir etki alanı örneğidir `landmarks`.

```Python
models = client.list_models()

for x in models.models_property:
    print(x)
```

### <a name="analyze-an-image-by-domain"></a>Etki alanına göre bir resmi çözümleme

Bir görüntü ile konu etki alanına göre analiz edebilirsiniz [ `analyze_image_by_domain` ] [ ref_computervisionclient_analyze_image_by_domain]. Alma [desteklenen konu etki alanları listesi](#get-subject-domain-list) doğru etki alanı adını kullanmak için.  

```Python
# type of prediction
domain = "landmarks"

# Public domain image of Eiffel tower
url = "https://images.pexels.com/photos/338515/pexels-photo-338515.jpeg"

# English language response
language = "en"

analysis = client.analyze_image_by_domain(domain, url, language)

for landmark in analysis.result["landmarks"]:
    print(landmark["name"])
    print(landmark["confidence"])
```

### <a name="get-text-description-of-an-image"></a>Metin açıklama Görüntü Al

Dil tabanlı metin açıklamasını görüntüsüne alabilirsiniz [ `describe_image` ] [ ref_computervisionclient_describe_image]. Bazı açıklamalar ile istek `max_description` görüntüyle ilişkilendirilen anahtar sözcükler için metin analizi yapıyorsanız özelliği. Aşağıdaki görüntüde metin açıklamasını örnekler `a train crossing a bridge over a body of water`, `a large bridge over a body of water`, ve `a train crossing a bridge over a large body of water`.

```Python
domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

analysis = client.describe_image(url, max_descriptions, language)

for caption in analysis.captions:
    print(caption.text)
    print(caption.confidence)
```

### <a name="get-text-from-image"></a>Görüntüden SMS alın

Bir görüntüden herhangi bir el yazısı veya yazılı metni alabilirsiniz. Bu iki SDK çağrıları gerektirir: [ `recognize_text` ] [ ref_computervisionclient_recognize_text] ve [ `get_text_operation_result` ] [ ref_computervisionclient_get_text_operation_result]. Recognize_text çağrı zaman uyumsuzdur. İlk çağrısı tamamlandı, denetlenecek ihtiyacınız get_text_operation_result arama sonuçlarında [ `TextOperationStatusCodes` ] [ ref_computervision_model_textoperationstatuscodes] metin verileri ayıklama önce. Sonuçları metin için sınırlama kutusu koordinatları yanı sıra metni içerir. 

```Python
# import models
from azure.cognitiveservices.vision.computervision.models import TextRecognitionMode
from azure.cognitiveservices.vision.computervision.models import TextOperationStatusCodes

url = "https://azurecomcdn.azureedge.net/cvt-1979217d3d0d31c5c87cbd991bccfee2d184b55eeb4081200012bdaf6a65601a/images/shared/cognitive-services-demos/read-text/read-1-thumbnail.png"
mode = TextRecognitionMode.handwritten
raw = True
custom_headers = None
numberOfCharsInOperationId = 36

# Async SDK call
rawHttpResponse = client.recognize_text(url, mode, custom_headers,  raw)

# Get ID from returned headers
operationLocation = rawHttpResponse.headers["Operation-Location"]
idLocation = len(operationLocation) - numberOfCharsInOperationId
operationId = operationLocation[idLocation:]

# SDK call
while result.status in ['NotStarted', 'Running']:
    time.sleep(1)
    result = client.get_text_operation_result(operationId)

# Get data
if result.status == TextOperationStatusCodes.succeeded:

    for line in result.recognition_result.lines:
        print(line.text)
        print(line.bounding_box)
```

### <a name="generate-thumbnail"></a>Küçük resim oluşturma

Küçük resmi (JPG) ile görüntü oluşturabilirsiniz [ `generate_thumbnail` ] [ ref_computervisionclient_generate_thumbnail]. Küçük resim özgün görüntü olarak aynı oranlarını olması gerekmez. 

Yükleme **Pillow** Bu örneği kullanmak için:

```bash
pip install Pillow
``` 

Paket aşağıdaki kod örneğinde Pillow yüklendikten sonra küçük resim görüntüsünü oluşturmak için kullanın.

```Python
# Pillow package
from PIL import Image

# IO package to create local image
import io

width = 50
height = 50
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"

thumbnail = client.generate_thumbnail(width, height, url)

for x in thumbnail:
    image = Image.open(io.BytesIO(x))

image.save('thumbnail.jpg')
```

## <a name="troubleshooting"></a>Sorun giderme

### <a name="general"></a>Genel

Ne zaman etkileşim [ComputerVisionAPI] [ ref_computervisionclient] Python SDK'sını kullanarak istemci nesnesi [ `ComputerVisionErrorException` ] [ ref_computervision_computervisionerrorexception] için kullanılan sınıfı hataları döndürür. Hizmet tarafından döndürülen hataları için REST API istekleri döndürülen HTTP durum kodları karşılık gelir.

Örneğin, geçersiz bir anahtara sahip bir resmi çözümleme denerseniz bir `401` hata döndürülür. Aşağıdaki kod parçacığında [hata] [ ref_httpfailure] istisnayı ve hatayla ilgili ek bilgileri görüntüleyerek düzgün bir şekilde ele alınır.

```Python

domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

try:
    analysis = client.describe_image(url, max_descriptions, language)

    for caption in analysis.captions:
        print(caption.text)
        print(caption.confidence)
except HTTPFailure as e:
    if e.status_code == 401:
        print("Error unauthorized. Make sure your key and region are correct.")
    else:
        raise
```

### <a name="handle-transient-errors-with-retries"></a>Yeniden deneme ile geçici hata işleme

İle çalışırken [ComputerVisionAPI] [ ref_computervisionclient] istemci, geçici hatalar nedeniyle çalıştırdığınızca [oran sınırları] [ computervision_request_units] zorunlu Hizmet veya ağ kesintileri gibi diğer geçici sorunlar. Bu tür hataları işleme hakkında daha fazla bilgi için bkz: [yeniden deneme düzeni] [ azure_pattern_retry] bulut tasarımı desenleri Kılavuzu ve ilgili [devre kesici düzeni] [azure_pattern_circuit_breaker].

### <a name="more-sample-code"></a>Daha fazla örnek kod

Birden çok bilgisayar işleme Python SDK'sı örnekleri SDK'ın GitHub deposunda kullanılabilir. Bu örnekler, görüntü işleme ile çalışırken sık karşılaşılan ilave senaryolar için örnek kod sağlayın:

* [recognize_text][recognize-text]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görüntülere içerik etiket uygulama](../concept-tagging-images.md)

<!-- LINKS -->
[pip]: https://pypi.org/project/pip/
[python]: https://www.python.org/downloads/

[azure_cli]: https://docs.microsoft.com/en-us/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create
[azure_pattern_circuit_breaker]: https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker
[azure_pattern_retry]: https://docs.microsoft.com/azure/architecture/patterns/retry
[azure_portal]: https://portal.azure.com
[azure_sub]: https://azure.microsoft.com/free/

[cloud_shell]: https://docs.microsoft.com/azure/cloud-shell/overview

[venv]: https://docs.python.org/3/library/venv.html
[virtualenv]: https://virtualenv.pypa.io

[source_code]: https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-vision-computervision

[pypi_computervision]:https://pypi.org/project/azure-cognitiveservices-vision-computervision/
[pypi_pillow]:https://pypi.org/project/Pillow/

[ref_computervision_sdk]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision?view=azure-python
[ref_computervision_computervisionerrorexception]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.computervisionerrorexception?view=azure-python
[ref_httpfailure]: https://docs.microsoft.com/python/api/msrest/msrest.exceptions.httpoperationerror?view=azure-python


[computervision_resource]: https://azure.microsoft.com/en-us/try/cognitive-services/?

[computervision_docs]: https://docs.microsoft.com/azure/cognitive-services/computer-vision/home

[ref_computervisionclient]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_analyze_image]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_list_models]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_analyze_image_by_domain]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_describe_image]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_recognize_text]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_get_text_operation_result]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_generate_thumbnail]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python


[ref_computervision_model_visualfeatures]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python

[ref_computervision_model_textoperationstatuscodes]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.textoperationstatuscodes?view=azure-python

[computervision_request_units]:https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/

[recognize-text]:https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/vision/computer_vision_samples.py

