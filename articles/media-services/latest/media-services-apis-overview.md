---
title: V3 API'ler - Azure ile geliştirme | Microsoft Docs
description: Bu makalede, Media Services v3 ile geliştirirken varlıkları ve API'ler için geçerli kurallar açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/15/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ed10354047060825b4368e02160d4655e33bc8f6
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617405"
---
# <a name="developing-with-media-services-v3-apis"></a>V3 API'ler Media Services ile geliştirme

Bu makalede, Media Services v3 ile geliştirirken varlıkları ve API'ler için geçerli kurallar açıklanmaktadır.

## <a name="accessing-the-azure-media-services-api"></a>Azure Media Services API erişme

Azure Media Services kaynaklara erişmek için Azure Active Directory (AD) hizmet sorumlusu kimlik doğrulaması kullanabilirsiniz.
Media Services API'sine gerektirir kullanıcı veya uygulamanın REST API'si kullanın ve Media Services hesabı kaynağa erişimi istekleri bir **katkıda bulunan** veya **sahibi** rol. API ile erişilebilir **okuyucu** ancak yalnızca Rol **alma** veya **listesi**   işlemleri kullanıma sunulacaktır. Daha fazla bilgi için [Media Services hesapları için rol tabanlı erişim denetimi](rbac-overview.md).

Bir hizmet sorumlusu oluşturmak yerine, Azure Resource Manager ile Media Services API'sine erişmek için Azure kaynakları için yönetilen kimlikleri kullanmayı düşünün. Azure kaynakları için yönetilen kimlikleri hakkında daha fazla bilgi için bkz: [Azure kaynakları için yönetilen kimlikleri nedir](../../active-directory/managed-identities-azure-resources/overview.md).

### <a name="azure-ad-service-principal"></a>Azure AD hizmet sorumlusu 

Bir Azure AD uygulaması ve hizmet sorumlusu oluşturuyorsanız, uygulama kendi kiracısında olması gerekir. Uygulamayı oluşturduktan sonra uygulamayı vermek **katkıda bulunan** veya **sahibi** Media Services hesabına erişim rolü. 

Azure AD uygulaması oluşturmak için izinlere sahip olup olmadığından emin değilseniz, [gerekli izinler](../../active-directory/develop/howto-create-service-principal-portal.md#required-permissions).

Aşağıdaki şekilde kronolojik sırada isteklerinin akışını sayıları temsil eder:

![Orta katman uygulamaları](../previous/media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

1. Aşağıdaki parametrelere sahip bir Azure AD erişim belirteci Orta katmanlı bir uygulama ister:  

   * Azure AD Kiracı uç noktası.
   * Media Services kaynak URI'si.
   * Kaynak REST Media Services için URI.
   * Azure AD uygulama değerlerini: istemci Kimliğini ve istemci gizli anahtarı.
   
   Gerekli tüm değerleri almak için bkz: [erişim Azure Media Services API'sine Azure CLI ile](access-api-cli-how-to.md)

2. Azure AD erişim belirteci için orta katman gönderilir.
4. Orta katman Azure AD belirteciyle Azure medya REST API isteği gönderir.
5. Orta katman veri Media Services'den geri alır.

## <a name="naming-conventions"></a>Adlandırma kuralları

Azure Media Services v3 kaynaklarının adları (Varlıklar, İşler, Dönüşümler gibi), Azure Resource Manager adlandırma kısıtlamalarına tabidir. Azure Resource Manager uyarınca kaynak adları her zaman benzersizdir. Bu nedenle kaynaklarınızda benzersiz tanıtıcı dizeleri (GUID gibi) kullanabilirsiniz. 

Media Services kaynak adları şu karakterleri içeremez: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', tek tırnak karakteri veya kontrol karakterleri. Diğer tüm karakterlere izin verilir. Bir kaynağın adı en fazla 260 karakter olabilir. 

Azure Resource Manager adlandırma hakkında daha fazla bilgi için bkz: [Adlandırma gereksinimlerini](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) ve [adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="long-running-operations"></a>Uzun süre çalışan işlemler

Sahip olarak işaretlenen işlemler `x-ms-long-running-operation` Azure Media Services [dosyaları swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/streamingservice.json) uzun süren işlemlere. 

Azure zaman uyumsuz işlemleri izleme hakkında daha fazla ayrıntı için bkz: [zaman uyumsuz işlemler](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations#monitor-status-of-operation).

Media Services, aşağıdaki uzun süre çalışan işlemler bulunur:

* Livestream oluşturma
* Livestream güncelleştir
* Delete LiveEvent
* Livestream Başlat
* Livestream Durdur
* Livestream Sıfırla
* LiveOutput oluşturma
* LiveOutput Sil
* StreamingEndpoint oluşturma
* Güncelleştirme StreamingEndpoint
* StreamingEndpoint Sil
* StreamingEndpoint Başlat
* StreamingEndpoint Durdur
* Ölçek StreamingEndpoint

## <a name="filtering-ordering-paging-of-media-services-entities"></a>Media Services varlıkların filtreleme, sıralama, sayfalama

Bkz: [filtreleme, sıralama, Azure Media Services varlıklarının sayfalandırma](entities-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

[Media Services v3 API SDK'ları / araçlarla geliştirmeye başlayın](developers-guide.md)
