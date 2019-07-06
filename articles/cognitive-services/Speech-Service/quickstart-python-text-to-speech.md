---
title: 'Hızlı Başlangıç: Metin okuma, Python - konuşma Hizmetleri Dönüştür'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta nasıl dönüştürme yapılacağını öğreneceksiniz metin okuma Python ve metin okuma REST API'sini kullanarak. Bu kılavuzda yer örnek metni konuşma sentezi işaretleme dili (SSML'yi) olarak yapılandırılmıştır. Bu, ses ve konuşma yanıtın dili seçmenize olanak sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 60a7fe79de62e3b81e25c17c23ba39a08e8a061c
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604996"
---
# <a name="quickstart-convert-text-to-speech-using-python"></a>Hızlı Başlangıç: Dönüştürme metin okuma Python kullanma

Bu hızlı başlangıçta nasıl dönüştürme yapılacağını öğreneceksiniz metin okuma Python ve metin okuma REST API'sini kullanarak. Bu kılavuzdaki istek gövdesi olarak yapılandırılmış [konuşma sentezi işaretleme dili (SSML'yi)](speech-synthesis-markup.md), ses ve yanıtın dili seçmenize olanak tanıyan.

Bu hızlı başlangıç bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) konuşma Hizmetleri kaynağa sahip. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](get-started.md) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* Python 2.7.x veya 3.x
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), veya en sevdiğiniz metin düzenleyiciyi
* Konuşma Hizmetleri için bir Azure aboneliği anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun. Ardından bu kod parçacığını projenizde `tts.py` adlı bir dosyaya kopyalayın.

```python
import os
import requests
import time
from xml.etree import ElementTree
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `pip install requests`.

Bu modüller, metin okuma API çağrısı bir zaman damgasına sahip bir dosya konuşma yanıtı yazmak ve bir HTTP isteği oluşturmak için kullanılır.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Abonelik anahtarı ve bir komut istemi için TTS oluşturma

Sonraki birkaç bölümde yetkilendirmeyi ele alırken, metin okuma API'si çağırmayı ve yanıt doğrulamak için yöntem oluşturacaksınız. Bu örnek xenapp'i bazı kod Python ile çalışır ekleyerek başlayalım 2.7.x ve 3.x.

```python
try:
    input = raw_input
except NameError:
    pass
```

Ardından, bir sınıf oluşturalım. Biz bizim belirteç değişimi ve metin okuma API'si Çağırma yöntemlerini nerede giriyorum budur.

```python
class TextToSpeech(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.tts = input("What would you like to convert to speech: ")
        self.timestr = time.strftime("%Y%m%d-%H%M")
        self.access_token = None
```

`subscription_key` Azure portalından, benzersiz anahtar. `tts` Konuşma Dönüştürülecek metin girmesini ister. Bu giriş bir dize sabit değeri, olduğundan karakter kaçış karakterleri gerekmez. Son olarak, `timestr` dosyanızın adını kullanacağız geçerli saati alır.

## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Metin okuma REST API, kimlik doğrulaması için bir erişim belirteci gerektirir. Erişim belirteci almak için bir exchange gereklidir. Bu örnek konuşma Hizmetleri abonelik anahtarınızı kullanarak bir erişim belirteci için birbiriyle değiştirir `issueToken` uç noktası.

Bu örnek, konuşma Hizmetleri aboneliğinize Batı ABD bölgesinde olduğunu varsayar. Farklı bir bölgeye kullanıyorsanız, değerini güncelleştirin `fetch_token_url`. Tam bir listesi için bkz [bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Bu kodu kopyalayın `TextToSpeech` sınıfı:

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

Burada, derleme isteği ve konuşma yanıt kaydetmek için yedekleyeceksiniz. İlk olarak, ayarlanacak ihtiyacınız `base_url` ve `path`. Bu örnek, Batı ABD uç nokta kullanmakta olduğunuz varsayılır. Kaynağınız için farklı bir bölgede kayıtlı değilse, güncelleştirdiğinizden emin olun `base_url`. Daha fazla bilgi için [konuşma Hizmetleri bölgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Ardından, istek için gerekli üst bilgileri eklemeniz gerekir. Güncelleştirdiğinizden emin olun `User-Agent` kaynak (Azure Portalı'nda bulunur) ve küme adıyla `X-Microsoft-OutputFormat` tercih edilen ses çıkış için. Çıkış biçimleri tam bir listesi için bkz. [ses çıkarır](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

Ardından, istek gövdesi konuşma sentezi işaretleme dili (SSML'yi) kullanarak oluşturun. Bu örnek, yapısını tanımlar ve kullandığı `tts` daha önce oluşturduğunuz giriş.

>[!NOTE]
> Bu örnekte `ZiraRUS` ses tipi. Ses/dillerde Microsoft tam bir listesi sağlanmaktadır için bkz: [dil desteği](language-support.md).
> Markanız için benzersiz, tanınan bir ses oluşturmak istiyorsanız bkz [özel ses tipi oluşturma](how-to-customize-voice-font.md).

Son olarak, hizmete istek yapacaksınız. İstek başarılı olur ve 200 durum kodu döndürülmesine ise konuşma yanıt zaman damgalı bir dosyaya yazılır.

Bu kodu kopyalayın `TextToSpeech` sınıfı:

```python
def save_audio(self):
    base_url = 'https://westus.tts.speech.microsoft.com/'
    path = 'cognitiveservices/v1'
    constructed_url = base_url + path
    headers = {
        'Authorization': 'Bearer ' + self.access_token,
        'Content-Type': 'application/ssml+xml',
        'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
        'User-Agent': 'YOUR_RESOURCE_NAME'
    }
    xml_body = ElementTree.Element('speak', version='1.0')
    xml_body.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-us')
    voice = ElementTree.SubElement(xml_body, 'voice')
    voice.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-US')
    voice.set(
        'name', 'Microsoft Server Speech Text to Speech Voice (en-US, Guy24KRUS)')
    voice.text = self.tts
    body = ElementTree.tostring(xml_body)

    response = requests.post(constructed_url, headers=headers, data=body)
    if response.status_code == 200:
        with open('sample-' + self.timestr + '.wav', 'wb') as audio:
            audio.write(response.content)
            print("\nStatus code: " + str(response.status_code) +
                  "\nYour TTS is ready for playback.\n")
    else:
        print("\nStatus code: " + str(response.status_code) +
              "\nSomething went wrong. Check your subscription key and headers.\n")
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

Neredeyse bitti. Son adım, sınıfının örneği ve işlevlerinizin çağrı sağlamaktır.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = TextToSpeech(subscription_key)
    app.get_token()
    app.save_audio()
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

İşte bu kadar metin okuma örnek uygulamanızı çalıştırmak hazır olursunuz. Komut satırını (veya terminal oturumu), proje dizinine gidin ve çalıştırın:

```console
python tts.py
```

İstendiğinde, ne olursa olsun metin okuma dönüştürmek istediğiniz içinde yazın. Başarılı olursa, konuşma dosyanın proje klasöründe bulunur. Yürütün, sık kullanılan media player kullanarak.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarları gibi örnek uygulamanızın kaynak kodundan olan gizli bilgilerin kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub üzerinde Python örnekleri keşfedin](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/Python)

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Özel ses tipi olarak oluşturma](how-to-customize-voice-font.md)
* [Özel ses oluşturma kayıt ses örnekleri](record-custom-voice-samples.md)
