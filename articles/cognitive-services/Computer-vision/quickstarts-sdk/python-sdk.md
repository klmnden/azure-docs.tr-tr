---
title: "Hızlı Başlangıç: Python SDK'sı"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, ortak görevler için Python SDK'sını kullanın, gibi resmi çözümleme, Al açıklaması, metin tanıma ve küçük resim oluşturma hakkında bilgi edinin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: 9b126d5ccbbf3cb1f22163ffb6ac53a8aff61004
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357345"
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
* Ücretsiz [görüntü işleme anahtarı] [ computervision_resource] ve ilişkili uç noktası. Örneğini oluşturduğunuzda bu değerlere ihtiyacınız olur [ComputerVisionClient] [ ref_computervisionclient] istemci nesnesi. Bu değerleri almak için aşağıdaki yöntemlerden birini kullanın.

### <a name="if-you-dont-have-an-azure-subscription"></a>Bir Azure aboneliğiniz yoksa

Ücretsiz bir anahtar ile 7 gün için geçerli oluşturma **[deneyin] [ computervision_resource]** deneyimi için görüntü işleme hizmeti. Anahtar oluşturulduğunda, anahtarını ve uç nokta adı kopyalayın. Bunun için gerekir [istemcisi oluşturma](#create-client).

Anahtar oluşturulduktan sonra aşağıdakilere dikkat edin:

* Anahtar değerini: biçimi ile 32 karakter dizesi `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
* Anahtar uç noktası: temel uç nokta URL'si, https\://westcentralus.api.cognitive.microsoft.com

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

## <a name="authentication"></a>Kimlik Doğrulaması

Görüntü işleme kaynağınızı oluşturduktan sonra ihtiyacınız kendi **uç nokta**, diğeri kendi **hesap anahtarları** istemci nesnesi örneklemek için.

Örneği oluşturmak için bu değerleri kullanabilirsiniz [ComputerVisionClient] [ ref_computervisionclient] istemci nesnesi.

Örneğin, Bash terminal ortam değişkenlerini ayarlamak için kullanın:

```Bash
ACCOUNT_ENDPOINT=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>
```

### <a name="for-azure-subscription-users-get-credentials-for-key-and-endpoint"></a>Azure abonelik kullanıcıları için anahtarını ve uç noktası için kimlik bilgilerini alma

Uç noktasını ve anahtarı anımsamıyorsanız bulmak için aşağıdaki yöntemi kullanabilirsiniz. Bir anahtarı ve uç noktası oluşturmanız gerekiyorsa, yöntemi için kullanabileceğiniz [Azure abonelik sahipleri](#if-you-have-an-azure-subscription) veya [kullanıcıların bir Azure aboneliği olmadan](#if-you-dont-have-an-azure-subscription).

Kullanım [Azure CLI] [ cloud_shell] görüntü işleme hesabıyla iki ortam değişkenleri doldurmak için aşağıdaki kod parçacığında **uç nokta** ve kendi **anahtarları**(Ayrıca bu değerleri bulabilirsiniz [Azure portalında][azure_portal]). Kod parçacığı, Bash kabuğunda biçimlendirilir.

```Bash
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

export ACCOUNT_ENDPOINT=$(az cognitiveservices account show \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query endpoint \
    --output tsv)

export ACCOUNT_KEY=$(az cognitiveservices account keys list \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query key1 \
    --output tsv)
```


### <a name="create-client"></a>İstemcisi oluşturma

Ortam değişkenlerinden uç noktasını ve anahtarı alın ardından oluşturma [ComputerVisionClient] [ ref_computervisionclient] istemci nesnesi.

```Python
from azure.cognitiveservices.vision.computervision import ComputerVisionClient
from azure.cognitiveservices.vision.computervision.models import VisualFeatureTypes
from msrest.authentication import CognitiveServicesCredentials

# Get endpoint and key from environment variables
import os
endpoint = os.environ['ACCOUNT_ENDPOINT']
key = os.environ['ACCOUNT_KEY']

# Set credentials
credentials = CognitiveServicesCredentials(key)

# Create client
client = ComputerVisionClient(endpoint, credentials)
```

## <a name="examples"></a>Örnekler

Gereksinim duyduğunuz bir [ComputerVisionClient] [ ref_computervisionclient] aşağıdaki görevlerden herhangi birini kullanmadan önce istemci nesnesi.

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

Bir görüntüden herhangi bir el yazısı veya yazılı metni alabilirsiniz. Bu iki SDK çağrıları gerektirir: [ `batch_read_file` ](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python#batch-read-file-url--mode--custom-headers-none--raw-false----operation-config-) ve [ `get_read_operation_result` ](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python#get-read-operation-result-operation-id--custom-headers-none--raw-false----operation-config-). Çağrı `batch_read_file` zaman uyumsuzdur. Sonuçlarında `get_read_operation_result` çağrısı gereken ilk çağrısı tamamlandı, denetlenecek [ `TextOperationStatusCodes` ] [ ref_computervision_model_textoperationstatuscodes] metin verileri ayıklama önce. Sonuçları metin için sınırlama kutusu koordinatları yanı sıra metni içerir.

```Python
# import models
from azure.cognitiveservices.vision.computervision.models import TextOperationStatusCodes
import time

url = "https://azurecomcdn.azureedge.net/cvt-1979217d3d0d31c5c87cbd991bccfee2d184b55eeb4081200012bdaf6a65601a/images/shared/cognitive-services-demos/read-text/read-1-thumbnail.png"
raw = True
custom_headers = None
numberOfCharsInOperationId = 36

# Async SDK call
rawHttpResponse = client.batch_read_file(url, custom_headers,  raw)

# Get ID from returned headers
operationLocation = rawHttpResponse.headers["Operation-Location"]
idLocation = len(operationLocation) - numberOfCharsInOperationId
operationId = operationLocation[idLocation:]

# SDK call
while True:
    result = client.get_read_operation_result(operationId)
    if result.status not in ['NotStarted', 'Running']:
        break
    time.sleep(1)

# Get data
if result.status == TextOperationStatusCodes.succeeded:
    for textResult in result.recognition_results:
        for line in textResult.lines:
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

Ne zaman etkileşim [ComputerVisionClient] [ ref_computervisionclient] Python SDK'sını kullanarak istemci nesnesi [ `ComputerVisionErrorException` ] [ ref_computervision_computervisionerrorexception] sınıfı kullanılır hatalarını döndürmek için. Hizmet tarafından döndürülen hataları için REST API istekleri döndürülen HTTP durum kodları karşılık gelir.

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
        print("Error unauthorized. Make sure your key and endpoint are correct.")
    else:
        raise
```

### <a name="handle-transient-errors-with-retries"></a>Yeniden deneme ile geçici hata işleme

İle çalışırken [ComputerVisionClient] [ ref_computervisionclient] istemci, geçici hatalar nedeniyle çalıştırdığınızca [oran sınırları] [ computervision_request_units] Hizmet veya ağ kesintileri gibi diğer geçici sorunlar tarafından zorunlu tutulur. Bu tür hataları işleme hakkında daha fazla bilgi için bkz: [yeniden deneme düzeni] [ azure_pattern_retry] bulut tasarımı desenleri Kılavuzu ve ilgili [devre kesici düzeni] [azure_pattern_circuit_breaker].

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görüntülere içerik etiket uygulama](../concept-tagging-images.md)

<!-- LINKS -->
[pip]: https://pypi.org/project/pip/
[python]: https://www.python.org/downloads/

[azure_cli]: https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create
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


[computervision_resource]: https://azure.microsoft.com/try/cognitive-services/?

[computervision_docs]: https://docs.microsoft.com/azure/cognitive-services/computer-vision/home

[ref_computervisionclient]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_analyze_image]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_list_models]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_analyze_image_by_domain]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_describe_image]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_get_text_operation_result]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python

[ref_computervisionclient_generate_thumbnail]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionclient?view=azure-python


[ref_computervision_model_visualfeatures]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python

[ref_computervision_model_textoperationstatuscodes]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.textoperationstatuscodes?view=azure-python

[computervision_request_units]:https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/
