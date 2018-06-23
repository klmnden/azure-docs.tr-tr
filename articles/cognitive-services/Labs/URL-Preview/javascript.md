---
title: JavaScript hızlı başlangıç projesi URL önizleme - Microsoft Bilişsel hizmetler için | Microsoft Docs
description: Hızlı bir şekilde Azure üzerinde Microsoft Bilişsel Hizmetleri'ndeki Bing URL önizleme API'si ile çalışmaya başlamak için komut dosyası örneği.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/16/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 9041a88a292fb2dabead69195ebc3074e2ecf486
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354023"
---
# <a name="url-preview-in-javascript"></a>JavaScript URL önizleme 

Aşağıdaki tek sayfalı uygulama SwiftKey site için URL önizleme oluşturmak için JavaScript kullanır: https://swiftkey.com/en. 

## <a name="prerequisites"></a>Önkoşullar

Ücretsiz deneme sürümü için bir erişim anahtarı alma [Bilişsel hizmetler Laboratuvarları](https://aka.ms/answersearchsubscription)

## <a name="code-scenario"></a>Kod senaryosu
Şu javascript örneği burada önizlemek için URL kullanıcının girdiği textbox giriş nesnesi içerir.  Kullanıcı tıkladığında **Önizleme** button, onclick yöntemi yollarını `getPreview` burada kod oluşturur Web isteğine **UrlPreview** uç noktası.

Kod oluşturur bir *XMLHttpRequest*, ekler *Apim abonelik anahtar Ocp* üstbilgi ve anahtar ve isteği gönderir.  Yanıta işlemek için zaman uyumsuz olay işleyicisi ekler.

Yanıt başarıyla döndürürse, işleyici yanıta JSON metnini atar `demo` paragraf sayfasında. Diğer yanıt öğeleri görüntülemek için aşağıdaki paragraflara ayarlanır.

**Ham JSON yanıtı**

````
{
  "_type": "WebPage",
  "name": "SwiftKey - Smart prediction technology for easier mobile typing",
  "url": "https://swiftkey.com/en",
  "description": "Discover the best Android and iPhone and iPad apps for faster, easier typing with emoji, colorful themes and more - download SwiftKey Keyboard free today.",
  "isFamilyFriendly": true,
  "primaryImageOfPage": {
    "contentUrl": "https://swiftkey.com/images/og/default.jpg"
  }
}

````

**Çalışan Tanıtımı**

![JavaScript Url önizleme örneği](./media/java-script-demo.png)

## <a name="running-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak için:

1. Değiştir `YOUR-SUBSCRIPTION-KEY` aboneliğiniz için geçerli erişim anahtarı ile değer.
2. HTML ve komut dosyası .html uzantılı bir dosyaya kaydedin.
3. Web sayfasını bir tarayıcıda çalıştırın.
4. Varolan URL'yi kullanın veya başka bir metin kutusuna girin.
5. Tıklatın **Önizleme** düğmesi.

**Kaynak kodu:**

```
<!DOCTYPE html>

<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="utf-8" />
    <title>urlPreview Demo</title>
</head>
<body>
    <h3>URL Preview Demo</h3>

    Page to preview: <input type="url" id="myURL" value="https://swiftkey.com/en">
    <button onclick="getPreview()">Preview</button>

    <p id="demo"></p>
    <br />
    <p id="jsonDesc"></p>
    <p><a id="familyFriendly"></a></p>
    <a id="contentUrl"></a>
    <p />
    <img id="jsonImage" />

    <script>

        var BING_ENDPOINT = "https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search"; 
        var key = "YOUR-SUBSCRIPTION-KEY";
        var xhr;

        function getPreview() {
            xhr = new XMLHttpRequest();

            var queryUrl = BING_ENDPOINT + "?q=" +
                encodeURIComponent(document.getElementById("myURL").value);
            xhr.open('GET', queryUrl, true);
            xhr.setRequestHeader("Ocp-Apim-Subscription-Key", key);

            xhr.send();

            xhr.addEventListener("readystatechange", processRequest, false);
        }

        function processRequest(e) {

            if (xhr.readyState == 4 && xhr.status == 200) {
                document.getElementById("demo").innerHTML = xhr.responseText;
                var obj = JSON.parse(xhr.responseText);
                document.getElementById("jsonDesc").innerHTML = obj.description;
                document.getElementById("familyFriendly").innerHTML = "Family Friendly: " + obj.isFamilyFriendly;
                document.getElementById("contentUrl").innerHTML = obj.url;
                document.getElementById("contentUrl").href = obj.url;
                document.getElementById("jsonImage").width = 350;
                document.getElementById("jsonImage").src = obj.primaryImageOfPage.contentUrl;

            }

        }

    </script>

</body>
</html>

```

## <a name="next-steps"></a>Sonraki adımlar
- [C# hızlı başlangıç](csharp.md)
- [Java hızlı başlangıç](java-quickstart.md)
- [Düğüm hızlı başlangıç](node-quickstart.md)
- [Python hızlı başlangıç](python-quickstart.md)