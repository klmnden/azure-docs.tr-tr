---
title: 'Hızlı Başlangıç: Küçük resim - REST, Python oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Python ile Görüntü İşleme API'sini kullanarak bir görüntüden küçük resim oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 9962874600e259a639c70b7b180e5fc2a940461f
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59999561"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-python-in-computer-vision"></a>Hızlı Başlangıç: Görüntü işleme Python ve REST API kullanarak küçük resim oluşturma

Bu hızlı başlangıçta, görüntü işleme'nın REST API kullanarak bir görüntüden bir küçük resim oluşturur. İle [küçük resim alma](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) yöntemi, istenen yüksekliğini ve genişliğini ve görüntü işleme kullanan bu bölgede dayalı akıllı bir şekilde ilgi tanımlamak ve kırpma koordinatları oluşturmak için akıllı kırpma belirtebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/try/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Görüntü İşleme için bir abonelik anahtarınız olması gerekir. Ücretsiz bir deneme anahtarından alabilirsiniz [Bilişsel Hizmetler'i deneyin](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Veya yönergeleri [Bilişsel Hizmetler hesabı oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) görüntü işleme için abone ve anahtarınızı alın.
- Bir kod Düzenleyicisi gibi [Visual Studio Code](https://code.visualstudio.com/download)

## <a name="create-and-run-the-sample"></a>Örnek oluşturma ve çalıştırma

Oluşturup örneği çalıştırmak için kod düzenleyicisine şu kodu kopyalayın. 

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
#%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

# Replace <Subscription Key> with your valid subscription key.
subscription_key = "<Subscription Key>"
assert subscription_key

# You must use the same region in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from
# westus, replace "westcentralus" in the URI below with "westus".
#
# Free trial subscription keys are generated in the "westus" region.
# If you use a free trial subscription key, you shouldn't need to change
# this region.
vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

thumbnail_url = vision_base_url + "generateThumbnail"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params  = {'width': '50', 'height': '50', 'smartCropping': 'true'}
data    = {'url': image_url}
response = requests.post(thumbnail_url, headers=headers, params=params, json=data)
response.raise_for_status()

thumbnail = Image.open(BytesIO(response.content))

# Display the thumbnail.
plt.imshow(thumbnail)
plt.axis("off")

# Verify the thumbnail size.
print("Thumbnail is {0}-by-{1}".format(*thumbnail.size))
```

Ardından, aşağıdakileri yapın:
1. `subscription_key` değerini abonelik anahtarınızla değiştirin.
1. Gerekirse `vision_base_url` değerini, abonelik anahtarlarınızı aldığınız Azure bölgesindeki Görüntü İşleme kaynağının uç nokta URL’si ile değiştirin.
1. İsteğe bağlı olarak `image_url` değerini, küçük resmini oluşturmak istediğiniz başka bir görüntünün URL’si ile değiştirin.
1. Kodu, `.py` uzantısıyla bir dosya olarak kaydedin. Örneğin, `get-thumbnail.py`.
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `python` komutunu kullanın. Örneğin, `python get-thumbnail.py`.

## <a name="examine-the-response"></a>Yanıtı inceleme

Küçük resim görüntü verilerini temsil eden bir ikili veri olarak başarılı bir yanıt döndürdü. Örnek, bu görüntüyü görüntülemelidir. İstek başarısız olursa, yanıt komut istemi penceresinde görüntülenir ve hata kodu içermesi gerekir.

## <a name="run-in-jupyter-optional"></a>Jupyter (isteğe bağlı) çalıştırın

İsteğe bağlı olarak bu hızlı başlangıçta bir Jupyter Not Defteri kullanarak adım adım bir biçimde çalıştırabileceğiniz [MyBinder](https://mybinder.org). Bağlayıcıyı başlatmak için aşağıdaki düğmeyi seçin:

[![Bağlayıcı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="next-steps"></a>Sonraki adımlar

Ardından, küçük resim oluşturma özelliği hakkında daha ayrıntılı bilgi edinin.

> [!div class="nextstepaction"]
> [Küçük resimler oluşturma](../concept-generating-thumbnails.md)
