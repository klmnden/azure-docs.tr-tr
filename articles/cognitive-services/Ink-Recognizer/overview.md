---
title: Mürekkep tanıyıcı nedir? -Mürekkep tanıyıcı API
titlesuffix: Azure Cognitive Services
description: Mürekkep tanıyıcı uygulamalarınızı, Web sitelerini, araçları ve diğer çözümleri sağlamak için tümleştirme...
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 0ed1a72a5dc61458200b72c768ad722656b820d8
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65027232"
---
# <a name="what-is-the-ink-recognizer-api"></a>Mürekkep Tanıma API’si nedir?


Mürekkep tanıyıcı Bilişsel hizmet çözümlemek ve dijital mürekkep içeriğini tanımak için bulut tabanlı REST API sağlar. Optik karakter tanıma (OCR) kullanan hizmetler, giriş olarak dijital mürekkep vuruşu veri API'si gerektirir. Dijital mürekkep vuruşlarını zamana göre sıralı giriş araçlarıyla dijital kalemler veya parmağınızı hareket temsil eden 2B noktaları (X, Y koordinatları) kümeleridir. Şekiller ve giriş el ile yazılmış içerikten algılar ve tanınan tüm varlıkları içeren bir JSON yanıtı döndürür.

![Mürekkep vuruşu giriş API'sine gönderme açıklayan bir akış çizelgesi](media/ink-recognizer-pen-graph.png)

## <a name="features"></a>Özellikler

Mürekkep tanıyıcı API'si ile uygulamalarınızda el yazısı içeriği kolayca tanıyabilirsiniz. 

|Özellik  |Açıklama  |
|---------|---------|
| El yazısı tanıma | El yazısı içerikleri 63 core'da [dil ve yerel ayar](language-support.md). | 
| Düzen tanıma | Dijital Mürekkep içeriği yapısal bilgi alın. İçerik, bölgeleri, paragraf, satırlar, sözcük, madde işaretli listeler yazıya bölün. Uygulamalarınızı daha sonra otomatik listesi biçimlendirme gibi ek özellikler oluşturmaya ilişkin düzen bilgilerini kullanın ve hizalama şekil. |
| Şekil tanıma | En sık kullanılan tanımak [geometrik şekiller](concepts/send-ink-data.md#shapes-recognized-by-the-ink-recognizer-api) notları çekerken. |
| Birleşik şekiller ve metin tanıma | Hangi mürekkep vuruşlarını şekilleri veya el ile yazılmış içerik ait tanıması ve ayrı olarak sınıflandırın.|

## <a name="workflow"></a>İş Akışı

Mürekkep tanıyıcı API'si bir RESTful web, HTTP istekleri ve JSON Ayrıştır tüm programlama dilinden çağrı kolaylaştırma hizmetidir.

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

Kaydolduktan sonra:

1. Mürekkep vuruşu verilerinizi alın ve [biçimlendirmeniz](concepts/send-ink-data.md#sending-ink-data) geçerli JSON içinde.
1. Verilerinizle mürekkep tanıyıcı API'sine bir istek gönderin.
1. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

Mürekkep tanıyıcı API'sine çağrı yapmaya başlamak için şu dillerde bir hızlı başlangıcı deneyin.
* [C#](quickstarts/csharp.md)
* [Java](quickstarts/java.md)
* [JavaScript](quickstarts/csharp.md)

Mürekkep tanıma API'si dijital mürekkep bir uygulamada nasıl çalıştığını görmek için Github'da aşağıdaki örnek uygulamaları göz atın:
* [C# Evrensel Windows Platformu (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript web tarayıcı uygulaması](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java ve Android mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift ve iOS mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089805)
