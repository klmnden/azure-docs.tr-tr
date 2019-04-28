---
title: .NET için yeniden deneme mantığı, Media Services SDK'sı | Microsoft Docs
description: Konu, .NET için Media Services SDK'sını yeniden deneme mantığı genel bir bakış sağlar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 527b61a6-c862-4bd8-bcbc-b9aea1ffdee3
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 63715f668438519131eba5bfff7aa38fc73267d0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61094666"
---
# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>.NET için Media Services SDK'sı mantığı yeniden deneyin  

Microsoft Azure Hizmetleri ile çalışırken, geçici hatalar oluşabilir. Geçici bir hata oluşursa, çoğu durumda, birkaç denemeden sonra işlemi başarılı olur. .NET için Media Services SDK'sı, sorgular, değişiklikler ve depolama işlemleri kaydediliyor yürütme özel durumları ve web istekleri, oluşan hataları ile ilgili geçici hataları işlemek için yeniden deneme mantığını uygular.  Varsayılan olarak, .NET için Media Services SDK'sını uygulamanıza özel durumu yeniden atamadan önce dört yeniden denemenin yürütür. Uygulamanızdaki kod bu özel durumun ardından düzgün şekilde işlemelidir.  

 Web isteği, depolama, sorgu ve SaveChanges ilkelerin kısa bir kılavuz aşağıda verilmiştir:  

* Saklama ilkesi, blob depolama işlemleri (karşıya yükleme veya indirme varlık dosyalarının) için kullanılır.  
* Web isteği ilkesi, genel web istekleri (örneğin, bir kimlik doğrulama belirteci alma ve kullanıcıların küme uç noktası çözümleme) için kullanılır.  
* REST (örneğin, mediaContext.Assets.Where(...)). varlıkları sorgulama için kullanılan Sorgu İlkesi  
* SaveChanges İlkesi (örneğin, bir işlem için bir hizmet işlevi çağırmak, bir varlığı güncelleştirmek için bir varlık oluşturma) hizmetinde verileri değiştiren herhangi bir şey yapmak için kullanılır.  
  
  Bu konu, özel durum türlerini listeler ve .NET için Media Services SDK'sı tarafından işlenen hata kodları yeniden deneme mantığı.  

## <a name="exception-types"></a>Özel durum türleri
Aşağıdaki tablo, .NET için Media Services SDK'sı, işleyen veya geçici hataların neden olabilecek bazı işlemler için işlemediği özel durumları açıklar.  

| Özel durum | Web isteği | Depolama | Sorgu | SaveChanges |
| --- | --- | --- | --- | --- |
| WebException ise<br/>Daha fazla bilgi için [WebException durum kodları](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) bölümü. |Evet |Evet |Evet |Evet |
| DataServiceClientException<br/> Daha fazla bilgi için [HTTP Hatası durum kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Evet |Evet |Evet |
| DataServiceQueryException<br/> Daha fazla bilgi için [HTTP Hatası durum kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Evet |Evet |Evet |
| DataServiceRequestException<br/> Daha fazla bilgi için [HTTP Hatası durum kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Hayır |Evet |Evet |Evet |
| DataServiceTransportException |Hayır |Hayır |Evet |Evet |
| TimeoutException |Evet |Evet |Evet |Hayır |
| SocketException |Evet |Evet |Evet |Evet |
| StorageException |Hayır |Evet |Hayır |Hayır |
| Ioexception |Hayır |Evet |Hayır |Hayır |

### <a name="WebExceptionStatus"></a> WebException durum kodları
Aşağıdaki tablo, hangi WebException hata kodları için yeniden deneme mantığı uygulanır gösterir. [WebExceptionStatus](https://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) durum kodları sabit listesi tanımlar.  

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
| Zaman Aşımı |Evet |Evet |Evet |Hayır |
| ProtocolError <br/>Yeniden deneme ProtocolError HTTP durum kodu işleme tarafından denetlenir. Daha fazla bilgi için [HTTP Hatası durum kodları](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Evet |Evet |Evet |Evet |

### <a name="HTTPStatusCode"></a> HTTP Hatası durum kodları
Sorgu ve SaveChanges işlemleri DataServiceClientException, DataServiceQueryException veya DataServiceQueryException throw, StatusCode özelliğinde HTTP Hatası durum kodu döndürülür.  Aşağıdaki tablo, hangi hata kodları için yeniden deneme mantığı uygulanan gösterir.  

| Durum | Web isteği | Depolama | Sorgu | SaveChanges |
| --- | --- | --- | --- | --- |
| 401 |Hayır |Evet |Hayır |Hayır |
| 403 |Hayır |Evet<br/>İşleme yeniden deneme ile uzun bekler. |Hayır |Hayır |
| 408 |Evet |Evet |Evet |Evet |
| 429 |Evet |Evet |Evet |Evet |
| 500 |Evet |Evet |Evet |Hayır |
| 502 |Evet |Evet |Evet |Hayır |
| 503 |Evet |Evet |Evet |Evet |
| 504 |Evet |Evet |Evet |Hayır |

Gerçek uygulamalar .NET yeniden deneme mantığı için Media Services SDK'sının göz atın istiyorsanız, bkz. [azure SDK'sı için medya Hizmetleri](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

