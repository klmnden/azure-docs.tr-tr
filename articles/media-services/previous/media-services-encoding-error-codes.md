---
title: Hata kodları kodlama Azure Media Services | Microsoft Docs
description: Bu konuda kodlama Görev yürütülürken bir hata oluştu durumunda döndürülebilen hata kodları listelenmektedir...
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 7a1733175f796a0d8c0c0d4247b2db2dd47e4674
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="encoding-error-codes"></a>Kodlama hata kodları

Aşağıdaki tabloda kodlama Görev yürütülürken bir hata oluştu durumunda döndürülebilen hata kodları listelenmektedir.  .NET kodunuzda hata ayrıntıları almak için [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) sınıfı. REST kodunuzda hata ayrıntıları almak için [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.

| ErrorDetail.Code | Hatanın olası nedenleri |
| --- | --- |
| Bilinmiyor |Görev yürütülürken bilinmeyen bir hata oluştu |
| ErrorDownloadingInputAssetMalformedContent |Hatalı dosya adları, yanlış sıfır uzunluk dosyaları gibi giriş varlık indirme hataları kapsayan kategori hata ve benzeri biçimlendirir. |
| ErrorDownloadingInputAssetServiceFailure |Hizmet tarafı - indirilirken örnek ağ veya depolama hataları sorunları kapsamaktadır hataları kategorisi. |
| ErrorParsingConfiguration |Kategori hataların nereye görev <see cref="MediaTask.PrivateData"/> (yapılandırma) geçerli değil, örneğin yapılandırması geçersiz XML içeriyor ya da geçerli bir sistem hazır değil. |
| ErrorExecutingTaskMalformedContent |Giriş medya dosyalarının içindeki sorunları hatası neden olduğu görevin yürütülmesi sırasında hatalar kategorisi. |
| ErrorExecutingTaskUnsupportedFormat |Burada medya işlemcisi sağlanan dosyalar işlenemiyor - medya biçimlendirilmemesi hataların kategori desteklenen veya yapılandırması eşleşmiyor. Örneğin, bir yalnızca video sahip bir varlık salt ses çıkışı üretmek çalışıyor |
| ErrorProcessingTask |İçeriği ilişkisiz bir medya işlemcisi görev işleme sırasında karşılaştığı diğer hataları kategorisi. |
| ErrorUploadingOutputAsset |Çıkış varlığına karşıya yüklenirken hata kategorisi |
| ErrorCancelingTask |Görev iptal edilmeye çalışılırken hataları kapak hataların kategorisi |
| TransientError |(Ör. geçici sorunlar ele hataların kategorisi Azure Storage ile geçici ağ sorunları) |

Yardım almak için **Media Services** takım, açık bir [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler
* [Medya Kodlayıcısı standart hazır özelleştirerek gelişmiş kodlama görevleri gerçekleştirme](media-services-custom-mes-presets-with-dotnet.md)
* [Kotalar ve kısıtlamaları](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
