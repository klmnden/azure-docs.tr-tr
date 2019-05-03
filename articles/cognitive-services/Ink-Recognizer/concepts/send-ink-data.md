---
title: Mürekkep verisi mürekkep tanıyıcı API'ye gönderin
titlesuffix: Azure Cognitive Services
description: Farklı uygulamalar için mürekkep çözümleyici API'si çağırma hakkında bilgi edinin
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 5a212c7332d085c15baabef8650572162c47903d
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027099"
---
# <a name="send-ink-data-to-the-ink-recognizer-api"></a>Mürekkep verisi mürekkep tanıyıcı API'ye gönderin 

Dijital Mürekkep giriş el yazısı ve çizim gibi dijital temsillerini etkinleştirme teknolojileri ifade eder. Bu genellikle, bir ekran kalemi gibi giriş cihazların hareketleri yakalayan dönüştürücü kullanarak da sağlanır. Zengin dijital mürekkep deneyimleri sağlamak cihazların devam ederken, yapay zeka ve makine öğrenimi yazılı şekil ve metinler her bağlamda tanınmasını sağlar. Mürekkep tanıyıcı API mürekkep vuruşları Gönder ve bunlar hakkında ayrıntılı bilgi almak sağlar. 

## <a name="the-ink-recognizer-api-vs-ocr-services"></a>Mürekkep tanıyıcı API vs. OCR Hizmetleri

Mürekkep tanıyıcı API, optik karakter tanıma (OCR) kullanmaz. El yazısı ve metin tanıma sağlamak için görüntüleri piksel verilerden OCR Hizmetleri işleyin. Bu, bazen çevrimdışı tanıma denir. Bunun yerine, mürekkep tanıyıcı API giriş cihazının kullanıldıkça yakalanan dijital mürekkep vuruşu veri gerektirir. Dijital Mürekkep verilerin bu şekilde işleme OCR hizmetlerine kıyasla daha doğru tanıma sonuçlar üretebilir. 

## <a name="sending-ink-data"></a>Mürekkep verisi gönderme

Mürekkep tanıyıcı API için ne zaman yükseltilmiş algılama surface dokunduğu andan Giriş bir cihaz tarafından oluşturulan mürekkep vuruşlarını temsil eden X ve Y koordinatları gerektirir. Her vuruş noktalarının virgülle ayrılmış değer bir dize olmalıdır ve aşağıdaki örnekteki gibi biçimlendirilmiş JSON biçiminde. Ayrıca, her bir mürekkep vuruşu her istekte benzersiz bir kimliği olması gerekir. Kimliği aynı istek içinde yinelenir API'si bir hata döndürür. İçin en doğru tanıma sonuçları, ondalık ayırıcıdan sonra en az sekiz basamağı vardır. ' % S'orijin (0,0) tuvalin mürekkep tuvalin sol üst köşesindeki varsayılır.

> [!NOTE]
> Aşağıdaki örnek, geçerli bir JSON değil. Tam bir mürekkep tanıyıcı JSON istek bulabilirsiniz [GitHub](https://go.microsoft.com/fwlink/?linkid=2089909).
 
```json
{
  "language": "en-US",
  "strokes": [
   {
    "id": 43,
    "points": 
        "5.1365, 12.3845,
        4.9534, 12.1301,
        4.8618, 12.1199,
        4.7906, 12.2217,
        4.7906, 12.5372,
        4.8211, 12.9849,
        4.9534, 13.6667,
        5.0958, 14.4503,
        5.3299, 15.2441,
        5.6555, 16.0480,
        ..."
   },
    ...
  ]
}
```

## <a name="ink-recognizer-response"></a>Mürekkep tanıyıcı yanıt

Mürekkep tanıyıcı API mürekkep içeriği, tanınan nesneler hakkında bir analiz yanıtı döndürür. Yanıt farklı mürekkep vuruşlarını arasındaki ilişkileri tanımlayan tanıma birimleri içerir. Örneğin, ayrı, ayrı şekiller oluşturma strokes farklı biriminde yer alır. Her birim tanınan nesnenin koordinatları ve diğer çizim öznitelikleri dahil olmak üzere kendi mürekkep vuruşlarını hakkında ayrıntılı bilgiler içerir.

## <a name="shapes-recognized-by-the-ink-recognizer-api"></a>Mürekkep tanıyıcı API'sı tarafından tanınan şekiller

Mürekkep tanıyıcı API not alma en sık kullanılan şekilleri tanımlayabilirsiniz. Aşağıdaki resim bazı temel örnekler gösterilmektedir. Şekiller ve API'sı tarafından tanınan diğer mürekkep içerik tam listesi için bkz: [API Başvurusu makalesinde](https://go.microsoft.com/fwlink/?linkid=2089907). 

![Mürekkep tanıyıcı API'sı tarafından tanınan şekiller listesi](../media/shapes.png)

## <a name="recommended-calling-patterns"></a>Önerilen arama desenleri

Uygulamanızı göre farklı desenleri mürekkep tanıyıcı REST API'si çağırabilirsiniz. 

### <a name="user-initiated-api-calls"></a>Kullanıcı tarafından başlatılan API çağrıları

Kullanıcı girişi (örneğin, bir not alma veya ek açıklama uygulaması) alan bir uygulama oluşturuyorsanız, ne zaman denetim vermek isteyebilirsiniz ve hangi mürekkep mürekkep tanıyıcı API'ye gönderilen. Metin ve şekiller hem tuval üzerinde mevcut olduğundan ve kullanıcılar her için farklı eylemler gerçekleştirmesini istiyorsanız bu işlev özellikle yararlı olur. Kullanıcıların seçim API'ye gönderilen (örneğin, bir Serbest Şekil veya başka bir geometrik seçim aracını) Seçimi özellikleri eklemeyi düşünün.  

### <a name="app-initiated-api-calls"></a>Uygulama tarafından başlatılan API çağrıları

Ayrıca, bir zaman aşımından sonra uygulama aramanız mürekkep tanıyıcı API sahip olabilir. Mürekkep vuruşlarını geçerli API için düzenli olarak göndererek, tanıma sonuçları API'SİNİN yanıt süresi artırırken oluşturuldukları olarak depolayabilirsiniz. Örneğin, tamamlandıktan sonra kullanıcı algılama API'sine el yazısı metin satırı gönderebilirsiniz. 

Birbirleriyle ilgili olarak tanıma sonuçları önceden sahip mürekkep vuruşlarını özellikleri hakkında bilgi sağlar. Örneğin, aynı kelimenin, satır, liste, paragraf veya şekli oluşturmak için hangi vuruşların gruplandırılır. Bu bilgileri, uygulamanızın mürekkep seçimi özellikleri strokes grupları örneğin aynı anda seçmek erişebildiklerinden geliştirebilirsiniz.

## <a name="integrate-the-ink-recognizer-api-with-windows-ink"></a>Mürekkep tanıyıcı API Windows mürekkep ile tümleştirin

[Windows Ink](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions) cihazları farklı bir dizi dijital mürekkep deneyimleri etkinleştirmek için Araçlar ve teknolojiler sağlar. Windows Ink platform görüntülemek ve dijital mürekkep vuruşlarını yorumlamak uygulamalar oluşturmak için mürekkep tanıma API'si ile birleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Mürekkep tanıyıcı API'si nedir?](../overview.md)
* [Mürekkep tanıyıcı REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2089907)

* Dijital mürekkep vuruşu veri kullanarak gönderme başlatın:
    * [C#](../quickstarts/csharp.md)
    * [Java](../quickstarts/java.md)
    * [JavaScript](../quickstarts/javascript.md)