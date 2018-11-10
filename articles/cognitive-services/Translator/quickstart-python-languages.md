---
title: 'Hızlı Başlangıç: Desteklenen dillerin listesini alma, Python - Translator Metin Çevirisi API’si'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Python ile Translator Metin Çevirisi API’sini kullanarak çeviri, başka alfabeye çevirme ve sözlük arama için desteklenen dillerin ve örneklerin bir listesini alacaksınız.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 10/29/2018
ms.author: erhopf
ms.openlocfilehash: 4c261a46b96865900a92bc53e4973fa3df094289
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50246567"
---
# <a name="quickstart-use-the-translator-text-api-to-get-a-list-of-supported-languages-using-python"></a>Hızlı Başlangıç: Python ve Translator Metin Çevirisi API'sini kullanarak desteklenen dillerin listesini alma

Bu hızlı başlangıçta Python ve Translator Metin Çevirisi API'sini kullanarak desteklenen dillerin listesini döndüren bir GET isteği göndermeyi öğreneceksiniz.

Bu hızlı başlangıç, Translator Metin Çevirisi kaynağına sahip bir [Azure Bilişsel Hizmetler hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) gerektirir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç şunları gerektirir:

* Python 2.7.x veya 3.x
* Translator Metin Çevirisi için Azure abonelik anahtarı

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun. Ardından bu kod parçacığını projenizde `get-languages.py` adlı bir dosyaya kopyalayın.

```python
# -*- coding: utf-8 -*-
import os, requests, uuid, json
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `pip install requests uuid`.

İlk açıklama Python yorumlayıcısına UTF-8 kodlaması kullanması gerektiğini bildirir. Ardından bir ortam değişkeninden abonelik anahtarınızı okumak, HTTP isteğini yapılandırmak, benzersiz bir tanımlayıcı oluşturmak ve Translator Metin Çevirisi API'si tarafından döndürülen JSON yanıtını işlemek için gereken modüller içeri aktarılır.

## <a name="set-the-subscription-key-base-url-and-path"></a>Abonelik anahtarını, temel URL’yi ve yolu ayarlayın

Bu örnek, `TRANSLATOR_TEXT_KEY` ortam değişkeninden Translator Metin Çevirisi abonelik anahtarınızı okumaya çalışır. Ortam değişkenlerini bilmiyorsanız, `subscriptionKey` öğesini dize olarak ayarlayabilir ve koşul deyimini açıklama satırı yapabilirsiniz.

Bu kodu projenize kopyalayın:

```python
# Checks to see if the Translator Text subscription key is available
# as an environment variable. If you are setting your subscription key as a
# string, then comment these lines out.
if 'TRANSLATOR_TEXT_KEY' in os.environ:
    subscriptionKey = os.environ['TRANSLATOR_TEXT_KEY']
else:
    print('Environment variable for TRANSLATOR_TEXT_KEY is not set.')
    exit()
# If you want to set your subscription key as a string, uncomment the line
# below and add your subscription key.
#subscriptionKey = 'put_your_key_here'
```

Şu anda Translator Metin Çevirisi için bir uç nokta kullanılabilir ve `base_url` olarak ayarlanmıştır. `path`, `languages` rotasını ayarlar ve API sürüm 3’ü kullanmak istediğimizi belirler.

>[!NOTE]
> Uç noktalar, rotalar ve istek parametreleri hakkında daha fazla bilgi için bkz. [Translator Metin Çevirisi API’si 3.0: Diller](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages).

```python
base_url = 'https://api.cognitive.microsofttranslator.com'
path = '/languages?api-version=3.0'
constructed_url = base_url + path
```

## <a name="add-headers"></a>Üst bilgileri ekleme

Bir isteğin kimliğini doğrulamanın en kolay yolu, abonelik anahtarınızda bir `Ocp-Apim-Subscription-Key` üst bilgisi olarak geçirmektir. Bu örnekte bu yöntem kullanılır. Alternatif olarak, abonelik anahtarınızı bir erişim belirteciyle değiştirebilir ve isteğinizi doğrulamak için erişim belirtecini bir `Authorization` üst bilgisi olarak geçirebilirsiniz. Daha fazla bilgi için bkz. [Kimlik doğrulaması](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

Bu kod parçacığını projenize kopyalayın:

```python
headers = {
    'Ocp-Apim-Subscription-Key': subscriptionKey,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}
```

## <a name="create-a-request-to-get-a-list-of-supported-languages"></a>Desteklenen dillerin listesini almak için bir istek oluşturma

Şimdi `requests` modülünü kullanarak bir GET isteği oluşturalım. İki bağımsız değişken kabul eder: Birleştirilmiş URL ve istek üst bilgileri:

```python
request = requests.get(constructed_url, headers=headers)
response = request.json()
```

## <a name="print-the-response"></a>Yanıtı yazdırma

Son adım sonuçları yazdırmaktır. Bu kod parçacığı anahtarları sıralayarak, girintiyi ayarlayarak ve öğe ve anahtar ayırıcıları bildirerek sonuçların daha iyi görünmesini sağlar.

```python
print(json.dumps(response, sort_keys=True, indent=4, ensure_ascii=False, separators=(',', ': ')))
```

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

İşte bu kadar! Translator Metin Çevirisi API'sini çağıran ve bir JSON yanıtı döndüren basit bir program oluşturdunuz. Artık programınızı çalıştırmak zamanı geldi:

```console
python get-languages.py
```

Kodunuzu bizimkiyle karşılaştırmak isterseniz, tam örnek kodu [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)’da bulabilirsiniz.

## <a name="sample-response"></a>Örnek yanıt

Bu örnek sonucun bir bölümünü gösterecek şekilde kısaltılmıştır:

```json
{
    "translation": {
        "af": {
            "name": "Afrikaans",
            "nativeName": "Afrikaans",
            "dir": "ltr"
        },
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "dir": "rtl"
        },
        ...
    },
    "transliteration": {
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "scripts": [
                {
                    "code": "Arab",
                    "name": "Arabic",
                    "nativeName": "العربية",
                    "dir": "rtl",
                    "toScripts": [
                        {
                            "code": "Latn",
                            "name": "Latin",
                            "nativeName": "اللاتينية",
                            "dir": "ltr"
                        }
                    ]
                },
                {
                    "code": "Latn",
                    "name": "Latin",
                    "nativeName": "اللاتينية",
                    "dir": "ltr",
                    "toScripts": [
                        {
                            "code": "Arab",
                            "name": "Arabic",
                            "nativeName": "العربية",
                            "dir": "rtl"
                        }
                    ]
                }
            ]
        },
      ...
    },
    "dictionary": {
        "af": {
            "name": "Afrikaans",
            "nativeName": "Afrikaans",
            "dir": "ltr",
            "translations": [
                {
                    "name": "English",
                    "nativeName": "English",
                    "dir": "ltr",
                    "code": "en"
                }
            ]
        },
        "ar": {
            "name": "Arabic",
            "nativeName": "العربية",
            "dir": "rtl",
            "translations": [
                {
                    "name": "English",
                    "nativeName": "English",
                    "dir": "ltr",
                    "code": "en"
                }
            ]
        },
      ...
    }
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Abonelik anahtarınızı programınıza sabit kodladıysanız, bu hızlı başlangıcı tamamladıktan sonra abonelik anahtarını kaldırdığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub’da Python örneklerini keşfedin](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Python)

## <a name="see-also"></a>Ayrıca bkz.

Translator Metin Çevirisi API'sini metni başka alfabeye çevirmenin yanı sıra aşağıdakileri yapmak için kullanabilirsiniz:

* [Metin çevirme](quickstart-python-translate.md)
* [Metni başka dilde yazma](quickstart-python-transliterate.md)
* [Girişe göre dili belirleyin](quickstart-python-detect.md)
* [Alternatif çeviriler edinme](quickstart-python-dictionary.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-python-sentences.md)
