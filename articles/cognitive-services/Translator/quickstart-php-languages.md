---
title: "Hızlı Başlangıç: Desteklenen dilleri, PHP - Translator metin çevirisi API'si Al"
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, PHP ile Translator Metin Çevirisi API’sini kullanarak çeviri, başka alfabeye çevirme ve sözlük arama için desteklenen dillerin ve örneklerin bir listesini alacaksınız.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
origin.date: 02/08/2019
ms.date: 03/12/2019
ms.author: v-junlch
ms.openlocfilehash: c902296b6ad3ebd4f012f1f6119a9aa5111916fb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60879389"
---
# <a name="quickstart-get-supported-languages-with-the-translator-text-rest-api-php"></a>Hızlı Başlangıç: Translator metin REST API (PHP) ile desteklenen dilleri Al

Bu hızlı başlangıçta, Translator Metin Çevirisi API’sini kullanarak çeviri, başka alfabeye çevirme ve sözlük arama için desteklenen dillerin ve örneklerin bir listesini alacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Bu kodu çalıştırmak için [PHP 5.6.x](https://php.net/downloads.php)’e ihtiyacınız olacak.

## <a name="languages-request"></a>Diller isteği

Aşağıdaki kod, [Diller](./reference/v3-0-languages.md) yöntemini kullanarak çeviri, başka alfabeye çevirme ve sözlük arama için desteklenen dillerin ve örneklerin listesini alır.

1. Sık kullandığınız kod düzenleyicisinde yeni bir PHP projesi oluşturun.
2. Aşağıda sağlanan kodu ekleyin.
3. Programı çalıştırın.

```php
<?php
// NOTE: Be sure to uncomment the following line in your php.ini file.
// ;extension=php_openssl.dll
$host = "https://api.translator.azure.cn";
$path = "/languages?api-version=3.0";
$output_path = "output.txt";
function GetLanguages ($host, $path) {
    $headers = "Content-type: text/xml\r\n";
    // NOTE: Use the key 'http' even if you are making an HTTPS request. See:
    // https://php.net/manual/en/function.stream-context-create.php
    $options = array (
        'http' => array (
            'header' => $headers,
            'method' => 'GET'
        )
    );
    $context  = stream_context_create ($options);
    $result = file_get_contents ($host . $path, false, $context);
    return $result;
}
$result = GetLanguages ($host, $path);
// Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
// We want to avoid escaping any Unicode characters that result contains. See:
// https://php.net/manual/en/function.json-encode.php
$json = json_encode(json_decode($result), JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
// Write the output to file.
$out = fopen($output_path, 'w');
fwrite($out, $json);
fclose($out);
?>
```

## <a name="languages-response"></a>Diller yanıtı

Başarılı bir yanıt, aşağıdaki örnekte gösterildiği gibi JSON biçiminde döndürülür:

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

## <a name="next-steps"></a>Sonraki adımlar

Çeviri ve başka alfabeye çevirme gibi bu ve diğer hızlı başlangıçlardaki örnek kodlarla birlikte GitHub’daki diğer örnek Translator Metin Çevirisi projelerini keşfedin.

> [!div class="nextstepaction"]
> [GitHub’da PHP örneklerini keşfedin](https://aka.ms/TranslatorGitHub?type=&language=php)

