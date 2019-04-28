---
title: Azure Media Services encoding hata kodları | Microsoft Docs
description: Bu konu, kodlama görevi yürütme sırasında bir hatayla karşılaşıldı durumunda döndürülebilen hata kodları listeler...
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 82a8eecb4fb211117000c94c51644ce1fd27916d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61463922"
---
# <a name="encoding-error-codes"></a>Kodlama hata kodları

Aşağıdaki tabloda, kodlama görevi yürütme sırasında bir hatayla karşılaşıldı durumunda döndürülebilen hata kodları listelenmektedir.  .NET kodunuzu hata ayrıntıları almak için kullanın [ErrorDetails](https://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) sınıfı. REST kodunuzda hata ayrıntıları almak için kullanın [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.

| ErrorDetail.Code | Hata için olası nedenler |
| --- | --- |
| Bilinmeyen |Görev yürütülürken bilinmeyen hata |
| ErrorDownloadingInputAssetMalformedContent |Hatalı dosya adları, yanlış sıfır uzunluğu dosyaları gibi giriş varlığı yükleme hataları kapsayan hata kategorisi ve benzeri biçimlendirir. |
| ErrorDownloadingInputAssetServiceFailure |Hizmet tarafında - yüklerken örnek ağ veya depolama hatalarını sorunları kapsar hata kategorisi. |
| ErrorParsingConfiguration |Hata kategorisi nerede görev <see cref="MediaTask.PrivateData"/> (yapılandırma) geçerli değil, örneğin yapılandırması geçersiz XML içeriyor ya da geçerli bir sistem hazır değil. |
| ErrorExecutingTaskMalformedContent |Girdi medya dosyalarının içindeki sorunları hata neden olduğu görevin yürütülmesi sırasında hata kategorisi. |
| ErrorExecutingTaskUnsupportedFormat |Burada sağlanan dosyaları medya işleyicisini işleyemiyor - medya biçimlendirilmeyeceğini hata kategorisi desteklenen veya yapılandırmayla eşleşmiyor. Örneğin, bir yalnızca ses çıkışı, yalnızca video sahip bir varlık oluşturmak çalışıyor |
| ErrorProcessingTask |İçeriği ilişkisiz bir medya işleyicisini görev işlenmesi sırasında karşılaştığı diğer hata kategorisi. |
| ErrorUploadingOutputAsset |Çıktı varlığına karşıya yüklenirken bir hata kategorisi |
| ErrorCancelingTask |Görev iptal edilmeye çalışılırken hatalar karşılamak için hata kategorisi |
| TransientError |(Örn. geçici sorunu ele almak için hata kategorisi Azure depolama ile geçici ağ sorunları) |

Yardım almak için **Media Services** takım, açık bir [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler
* [Gelişmiş kodlama görevleri, Media Encoder Standard hazır ayarlarını özelleştirerek gerçekleştirin](media-services-custom-mes-presets-with-dotnet.md)
* [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
