---
title: .NET için Media Services SDK'sı mantığı yeniden dene | Microsoft Docs
description: Konu, .NET için Media Services SDK'sı yeniden deneme mantığı genel bir bakış sağlar.
author: Juliako
manager: cfowler
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 527b61a6-c862-4bd8-bcbc-b9aea1ffdee3
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako
ms.openlocfilehash: 859dd76db4ba06196a853469a1385703d835fa22
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23860797"
---
# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>.NET için Media Services SDK'sı mantığı yeniden dene
Microsoft Azure Hizmetleri ile çalışırken, geçici hataları oluşabilir. Geçici bir hata oluşursa, çoğu durumda, birkaç denemeden sonra işlemi başarılı olur. .NET için Media Services SDK'sı, sorgular, değişiklikler ve depolama işlemleri kaydetme yürütme özel durumlar ve web isteklerine göre neden hataları ile ilişkili geçici hataları işlemek için yeniden deneme mantığı uygular.  Varsayılan olarak, .NET için Media Services SDK'sı, uygulamanızın özel durumu yeniden atmadan önce dört yeniden denemenin yürütür. Uygulamanızdaki kod sonra bu özel durumun düzgün işlemesi gerekir.  

 Web isteği, depolama, sorgu ve SaveChanges ilkelerin kısa bir kılavuz verilmiştir:  

* Depolama ilkesi blob depolama işlemleri (yüklemeleri veya varlık dosyaları indirme) için kullanılır.  
* Web isteği ilkesi, genel web istekleri (örneğin, kimlik doğrulama belirtecini alma ve kullanıcıların küme uç noktası çözümleme) için kullanılır.  
* Sorgu İlkesi REST (örneğin, mediaContext.Assets.Where(...)). varlıkları sorgulamak için kullanılır  
* SaveChanges İlkesi (örneğin, bir hizmet işlevi için bir işlem çağırma bir varlık, güncelleştirme bir varlık oluşturma) hizmeti içindeki veri değişikliklerini herhangi bir şey yapmak için kullanılır.  
  
  Bu konu özel durum türleri listeler ve .NET için Media Services SDK'sı tarafından işlenen hata kodları mantığı yeniden deneyin.  

## <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tabloda, .NET için Media Services SDK'sı işleyen veya geçici hatalarına neden olabilir bazı işlemler için işlemediği özel durumlar açıklanmaktadır.  

| Özel durum | Web isteği | Depolama | Sorgu | SaveChanges |
| --- | --- | --- | --- | --- |
| WebException<br/>Daha fazla bilgi için bkz: [WebException durum kodları](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) bölümü. |Evet |Evet |Evet |Evet |
| DataServiceClientException<br/> Daha fazla bilgi için bkz: [HTTP hata durum kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Evet |Evet |Evet |
| DataServiceQueryException<br/> Daha fazla bilgi için bkz: [HTTP hata durum kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Evet |Evet |Evet |
| DataServiceRequestException<br/> Daha fazla bilgi için bkz: [HTTP hata durum kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Evet |Evet |Evet |
| DataServiceTransportException |Hayır |Hayır |Evet |Evet |
| TimeoutException |Evet |Evet |Evet |Hayır |
| SocketException |Evet |Evet |Evet |Evet |
| StorageException |Hayır |Yes |Hayır |Hayır |
| Ioexception |Hayır |Yes |Hayır |Hayır |

### <a name="WebExceptionStatus"></a>WebException durum kodları
Aşağıdaki tabloda, hangi WebException hata kodları için yeniden deneme mantığı uygulanır gösterir. [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) numaralandırma durum kodları tanımlar.  

| Durum | Web isteği | Depolama | Sorgu | SaveChanges |
| --- | --- | --- | --- | --- |
| ConnectFailure |Evet |Evet |Evet |Evet |
| NameResolutionFailure |Evet |Evet |Evet |Evet |
| ProxyNameResolutionFailure |Evet |Evet |Evet |Evet |
| SendFailure |Evet |Evet |Evet |Evet |
| PipelineFailure |Evet |Evet |Evet |Hayır |
| ConnectionClosed |Evet |Evet |Evet |Hayır |
| KeepAliveFailure |Evet |Evet |Evet |Hayır |
| Başvuruları |Evet |Evet |Evet |Hayır |
| ReceiveFailure |Evet |Evet |Evet |Hayır |
| RequestCanceled |Evet |Evet |Evet |Hayır |
| Zaman aşımı |Evet |Evet |Evet |Hayır |
| ProtocolError <br/>Yeniden deneme ProtocolError HTTP durum kodu işleme tarafından denetlenir. Daha fazla bilgi için bkz: [HTTP hata durum kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Evet |Evet |Evet |Evet |

### <a name="HTTPStatusCode"></a>HTTP hata durum kodları
Sorgu ve SaveChanges işlemleri DataServiceClientException, DataServiceQueryException veya DataServiceQueryException throw, StatusCode özelliğinde HTTP hata durum kodu döndürülür.  Aşağıdaki tabloda, hangi hata kodları için yeniden deneme mantığı uygulanır gösterir.  

| Durum | Web isteği | Depolama | Sorgu | SaveChanges |
| --- | --- | --- | --- | --- |
| 401 |Hayır |Yes |Hayır |Hayır |
| 403 |Hayır |Evet<br/>İşleme deneme ile uzun bekler. |Hayır |Hayır |
| 408 |Evet |Evet |Evet |Evet |
| 429 |Evet |Evet |Evet |Evet |
| 500 |Evet |Evet |Evet |Hayır |
| 502 |Evet |Evet |Evet |Hayır |
| 503 |Evet |Evet |Evet |Evet |
| 504 |Evet |Evet |Evet |Hayır |

.NET yeniden deneme mantığı için Media Services SDK'sı gerçek uyarlamasını bakmak istiyorsanız, bkz: [azure SDK'sı için media services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

