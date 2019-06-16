---
title: 'Hızlı Başlangıç: Metin okuma sesleri, Python - konuşma hizmetlerinin listesi'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, bir bölge/Python kullanarak uç noktası için standart ve sinir sesleri tam listesini almak nasıl öğreneceksiniz. Liste, JSON olarak döndürülür ve ses kullanılabilirlik bölgeye göre değişir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 03/22/2019
ms.author: erhopf
ms.openlocfilehash: 5d43060ee5303c1df3a1b8448f086ecc32a1f8c3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056863"
---
# <a name="quickstart-get-the-list-of-text-to-speech-voices-using-python"></a>Hızlı Başlangıç: Python kullanarak metin okuma seslerini'nın listesini alın

Bu hızlı başlangıçta, bir bölge/Python kullanarak uç noktası için standart ve sinir sesleri tam listesini almak nasıl öğreneceksiniz. Liste, JSON olarak döndürülür ve ses kullanılabilirlik bölgeye göre değişir. Desteklenen bölgelerin bir listesi için bkz. [bölgeleri](regions.md).

Bu hızlı başlangıç bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) konuşma Hizmetleri kaynağa sahip. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](get-started.md) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* Python 2.7.x veya 3.x
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), veya en sevdiğiniz metin düzenleyiciyi
* Konuşma Hizmetleri için bir Azure aboneliği anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun. Ardından bu kod parçacığını projenizde `get-voices.py` adlı bir dosyaya kopyalayın.

```python
import requests
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `pip install requests`.

İstek, metin okuma hizmetine HTTP istekleri için kullanılır.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Abonelik anahtarı ve bir komut istemi için TTS oluşturma

Sonraki birkaç bölümde yetkilendirmeyi ele alırken, metin okuma API'si çağırmayı ve yanıt doğrulamak için yöntem oluşturacaksınız. Bir sınıf oluşturarak başlayalım. Biz bizim belirteç değişimi ve metin okuma API'si Çağırma yöntemlerini nerede giriyorum budur.

```python
class GetVoices(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.access_token = None
```

`subscription_key` Azure portalından, benzersiz anahtar.

## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Bu uç nokta kimlik doğrulaması için bir erişim belirteci gerektirir. Erişim belirteci almak için bir exchange gereklidir. Bu örnek konuşma Hizmetleri abonelik anahtarınızı kullanarak bir erişim belirteci için birbiriyle değiştirir `issueToken` uç noktası.

Bu örnek, konuşma Hizmetleri aboneliğinize Batı ABD bölgesinde olduğunu varsayar. Farklı bir bölgeye kullanıyorsanız, değerini güncelleştirin `fetch_token_url`. Tam bir listesi için bkz [bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Bu kodu kopyalayın `GetVoices` sınıfı:

```python
def get_token(self):
    fetch_token_url = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    headers = {
        'Ocp-Apim-Subscription-Key': self.subscription_key
    }
    response = requests.post(fetch_token_url, headers=headers)
    self.access_token = str(response.text)
```

> [!NOTE]
> Kimlik doğrulaması hakkında daha fazla bilgi için bkz. [bir erişim belirteci ile kimlik doğrulama](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="make-a-request-and-save-the-response"></a>Bir istekte bulunmak ve yanıt Kaydet

Burada, derleme isteği ve döndürülen seslerle listesine kaydetmek için yedekleyeceksiniz. İlk olarak, ayarlanacak ihtiyacınız `base_url` ve `path`. Bu örnek, Batı ABD uç nokta kullanmakta olduğunuz varsayılır. Kaynağınız için farklı bir bölgede kayıtlı değilse, güncelleştirdiğinizden emin olun `base_url`. Daha fazla bilgi için [konuşma Hizmetleri bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Ardından, istek için gerekli üst bilgileri ekleyin. Son olarak, hizmete istek yapacaksınız. İstek başarılı olur ve 200 durum kodu döndürülmesine ise yanıt dosyasına yazılır.

Bu kodu kopyalayın `GetVoices` sınıfı:

```python
def get_voices(self):
    base_url = 'https://westus.tts.speech.microsoft.com'
    path = '/cognitiveservices/voices/list'
    get_voices_url = base_url + path
    headers = {
        'Authorization': 'Bearer ' + self.access_token
    }
    response = requests.get(get_voices_url, headers=headers)
    if response.status_code == 200:
        with open('voices.json', 'wb') as voices:
            voices.write(response.content)
            print("\nStatus code: " + str(response.status_code) + "\nvoices.json is ready to view.\n")
    else:
        print("\nStatus code: " + str(
            response.status_code) + "\nSomething went wrong. Check your subscription key and headers.\n")
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Neredeyse bitti. Son adım, sınıfının örneği ve işlevlerinizin çağrı sağlamaktır.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = GetVoices(subscription_key)
    app.get_token()
    app.get_voices()
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu kadar örneği çalıştırmak hazır olursunuz. Komut satırını (veya terminal oturumu), proje dizinine gidin ve çalıştırın:

```console
python get-voices.py
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan olan gizli bilgilerin kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub üzerinde Python örnekleri keşfedin](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/Python)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Özel ses tipi olarak oluşturma](how-to-customize-voice-font.md)
* [Özel ses oluşturma kayıt ses örnekleri](record-custom-voice-samples.md)
