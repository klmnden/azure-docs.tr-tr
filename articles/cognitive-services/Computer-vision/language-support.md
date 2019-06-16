---
title: Dil desteği - görüntü işleme
titleSuffix: Azure Cognitive Services
description: Doğal görüntü işleme özellikleri tarafından desteklenen dillerin listesi.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: article
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: 1a70d1b2ea504d0ccfba925810a2d19d0c7583cc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60759614"
---
# <a name="language-support-for-computer-vision"></a>Görüntü işleme için dil desteği

Görüntü işleme özelliklerinden bazıları birden çok dil desteği: Burada bahsedilen değil herhangi bir özelliği yalnızca İngilizce destekler.

## <a name="text-recognition"></a>Metin tanıma

Görüntü işleme, birden çok dilde metin tanıyabilirsiniz. Özellikle, [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) API, çeşitli dillerde, desteklerken [okuma](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) API ve [metni tanı](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) API desteği yalnızca İngilizce. Bkz: [yazdırılan ve el yazısı metinleri tanıma](concept-recognizing-text.md) bu işlevselliği ve her API avantajları hakkında daha fazla bilgi için.

API çağrısında bir dil kodu belirtmenize gerek yoktur olduğundan OCR giriş malzeme dili otomatik olarak algılar. Ancak, dil kodlarını değeri olarak her zaman döndürülür `"language"` JSON yanıtındaki düğümü.

|Dil| Dil kodu | OCR API |
|:-----|:----:|:-----:|
|Arapça | `ar`|✔ |
|Çince (Basitleştirilmiş) | `zh-Hans`|✔ |
|seçenekleri yerine | `zh-Hant`|✔ |
|Çekçe | `cs` |✔ |
|Danca | `da` |✔ |
|Felemenkçe | `nl` |✔ |
|Türkçe | `en` |✔ |
|Fince | `fi` |✔ |
|Fransızca | `fr` |✔ |
|Almanca | `de` |✔ |
|Yunanca | `el` |✔ |
|Macarca | `hu` |✔ |
|İtalyanca | `it` |✔ |
|Japonca | `ja` |✔ |
|Korece | `ko` |✔ |
|Norveççe | `nb` |✔ |
|Lehçe | `pl` |✔ |
|Portekizce | `pt` |✔ |
|Rumence | `ro` |✔ |
|Rusça | `ru` |✔ |
|Sırpça (Kiril) | `sr-Cyrl` |✔ |
|Sırpça (Latin) | `sr-Latn` |✔ |
|Slovakça | `sk` |✔ |
|İspanyolca | `es` |✔ |
|İsveççe | `sw` |✔ |
|Türkçe | `tr` |✔ |

## <a name="image-analysis"></a>Görüntü analizi

Bazı eylemleri [analiz - görüntü](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API ile belirtilen diğer dillerdeki sonuçlar döndürebilir `language` sorgu parametresi. Diğer Eylemler, dilden bağımsız olarak belirtilir İngilizce sonuçları döndürür ve diğer desteklenmeyen diller için bir özel durum. İle belirtilen eylemleri `visualFeatures` ve `details` sorgu parametreleri; bkz [genel bakış](home.md) görüntü Analizi ile yapabileceğiniz tüm eylemlerin listesi.

|Dil | Dil kodu | Categories | Tags | Açıklama | Yetişkin | Markalar | Renk | Yüzler | Resim Türü | Nesneler | Ünlüler | Yer işareti |
|:---|:---:|:----:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Çince | `zh`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Türkçe | `en`   | ✔ | ✔| ✔|✔|✔|✔|✔|✔|✔|✔|✔|
|Japonca | `ja`   | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Portekizce | `pt` | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|İspanyolca | `es`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzun başlarında da belirtildiği görüntü işleme özelliklerini kullanmaya başlayın.

* [Yerel bir görüntü (REST) analiz edin](./quickstarts/csharp-analyze.md)
* [Yazdırılan metin (REST) ayıklayın](./quickstarts/csharp-print-text.md)