---
title: "Hızlı Başlangıç: Translator metin çevirisi API'si - Node.js olan desteklenen dillerin listesini alın"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Node.js ile Translator Metin Çevirisi API’sini kullanarak çeviri, başka alfabeye çevirme ve sözlük arama için desteklenen dillerin ve örneklerin bir listesini alacaksınız.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/21/2019
ms.author: erhopf
ms.openlocfilehash: df637ee16d2116696af42fa1b162e1c77a880c19
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64922177"
---
# <a name="quickstart-use-the-translator-text-api-to-get-a-list-of-supported-languages-with-nodejs"></a>Hızlı Başlangıç: Node.js ile desteklenen dillerin listesini almak için Translator Text API kullanın

Bu hızlı başlangıçta Node.js ve Translator Metin Çevirisi API'sini kullanarak desteklenen dillerin listesini döndüren bir GET isteği göndermeyi öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Node 8.12.x veya üzeri](https://nodejs.org/en/)

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Favori IDE ortamınızda veya düzenleyicide yeni bir proje oluşturun. Ardından bu kod parçacığını projenizde `get-languages.js` adlı bir dosyaya kopyalayın.

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `npm install request uuidv4`.

Bu modüller HTTP isteği ve `'X-ClientTraceId'` üst bilgisi için benzersiz tanıtıcı oluşturmak için gereklidir.

## <a name="configure-the-request"></a>İsteği yapılandırma

İstek modülü aracılığıyla kullanıma sunulan `request()` yöntemi HTTP yöntemi, URL, istek parametreleri, üst bilgileri ve JSON gövdesi bileşenlerini `options` nesnesi olarak geçirmemizi sağlar. Bu kod parçacığında isteği yapılandıracağız:

>[!NOTE]
> Uç noktaları, yollar ve istek parametreleri hakkında daha fazla bilgi için bkz: [Translator Text API 3.0: Diller](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages).

```javascript
let options = {
    method: 'GET',
    baseUrl: 'https://api.cognitive.microsofttranslator.com/',
    url: 'languages',
    qs: {
      'api-version': '3.0',
    },
    headers: {
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    json: true,
};
```

## <a name="make-the-request-and-print-the-response"></a>İstekte bulunma ve yanıtı yazdırma

Ardından, `request()` yöntemini kullanarak isteği oluşturacağız. Bu istek ilk bağımsız değişken olarak bir önceki bölümde oluşturduğumuz `options` nesnesini alır ve düzeltilmiş JSON yanıtını yazdırır.

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> Bu örnekte HTTP isteğini `options` nesnesinde tanımlıyoruz. Ancak istek modülü `.post` ve `.get` gibi kullanışlı yöntemleri de destekler. Daha fazla bilgi için bkz. [kullanışlı yöntemler](https://github.com/request/request#convenience-methods).

## <a name="put-it-all-together"></a>Hepsini bir araya getirin

İşte bu kadar! Translator Metin Çevirisi API'sini çağıran ve bir JSON yanıtı döndüren basit bir program oluşturdunuz. Artık programınızı çalıştırmak zamanı geldi:

```console
node get-languages.js
```

Kodunuzu bizimkiyle karşılaştırmak isterseniz, tam örnek kodu [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS)’da bulabilirsiniz.

## <a name="sample-response"></a>Örnek yanıt

Bu ülke kısaltması Bul [dillerin listesini](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).

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
> [GitHub'da Node.js örneklerini keşfedin](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS)

## <a name="see-also"></a>Ayrıca bkz.

Translator Metin Çevirisi API'sini dil algılamaya ek olarak aşağıdakileri yapmak için kullanabilirsiniz:

* [Metin çevirme](quickstart-nodejs-translate.md)
* [Metni başka dilde yazma](quickstart-nodejs-transliterate.md)
* [Girişe göre dili belirleyin](quickstart-nodejs-detect.md)
* [Alternatif çeviriler edinme](quickstart-nodejs-dictionary.md)
* [Girişten tümce uzunluklarını belirleme](quickstart-nodejs-sentences.md)
