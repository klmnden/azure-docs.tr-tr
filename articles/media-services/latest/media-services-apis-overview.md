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
ms.date: 05/02/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 4c5b30ab075bbca22b6a58ccf65e55d332820937
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406540"
---
# <a name="developing-with-media-services-v3-apis"></a>V3 API'ler Media Services ile geliştirme

Bu makalede, Media Services v3 ile geliştirirken varlıkları ve API'ler için geçerli kurallar açıklanmaktadır.

## <a name="accessing-the-azure-media-services-api"></a>Azure Media Services API erişme

Media Services kaynakları ve Media Services API'sine erişim iznine sahip olması için önce kimliğinin doğrulanması gerekir. Media Services'ı destekleyen [Azure Active Directory (Azure AD)-temel](../../active-directory/fundamentals/active-directory-whatis.md) kimlik doğrulaması. İki ortak kimlik doğrulama seçenekleri şunlardır:
 
* **Hizmet sorumlusu kimlik doğrulaması** - hizmet kimlik doğrulaması için kullanılır (örneğin: web apps, işlev uygulamaları, logic apps, API ve mikro hizmetler). Genellikle bu kimlik doğrulama yöntemi kullanan uygulamalar, daemon Hizmetleri, orta katman Hizmetleri ya da zamanlanmış işleri çalıştırma uygulamalardır. Örneğin, Web için uygulamalar var. her zaman bir hizmet sorumlusu ile Media Services bağlanan bir orta katmanda olmalıdır.
* **Kullanıcı kimlik doğrulaması** - kullanan uygulama etkileşim kurmak için Media Services kaynaklarla bir kişinin kimliğini doğrulamak için kullanılır. Etkileşimli uygulama önce kullanıcının kimlik bilgilerini girmesini. Kodlama işi izlemek veya canlı akış için yetkili kullanıcılar tarafından kullanılan bir yönetim konsol uygulaması örneğidir.

Media Services API'sine gerektirir kullanıcı veya uygulamanın REST API'si kullanın ve Media Services hesabı kaynağa erişimi istekleri bir **katkıda bulunan** veya **sahibi** rol. API ile erişilebilir **okuyucu** ancak yalnızca Rol **alma** veya **listesi**   işlemleri kullanıma sunulacaktır. Daha fazla bilgi için [Media Services hesapları için rol tabanlı erişim denetimi](rbac-overview.md).

Bir hizmet sorumlusu oluşturmak yerine, Azure Resource Manager ile Media Services API'sine erişmek için Azure kaynakları için yönetilen kimlikleri kullanmayı düşünün. Azure kaynakları için yönetilen kimlikleri hakkında daha fazla bilgi için bkz: [Azure kaynakları için yönetilen kimlikleri nedir](../../active-directory/managed-identities-azure-resources/overview.md).

### <a name="azure-ad-service-principal"></a>Azure AD hizmet sorumlusu 

Bir Azure AD uygulaması ve hizmet sorumlusu oluşturuyorsanız, uygulama kendi kiracısında olması gerekir. Uygulamayı oluşturduktan sonra uygulamayı vermek **katkıda bulunan** veya **sahibi** Media Services hesabına erişim rolü. 

Azure AD uygulaması oluşturmak için izinlere sahip olup olmadığından emin değilseniz, [gerekli izinler](../../active-directory/develop/howto-create-service-principal-portal.md#required-permissions).

Aşağıdaki şekilde kronolojik sırada isteklerinin akışını sayıları temsil eder:

![Orta katman uygulamaları](./media/use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

1. Aşağıdaki parametrelere sahip bir Azure AD erişim belirteci Orta katmanlı bir uygulama ister:  

   * Azure AD Kiracı uç noktası.
   * Media Services kaynak URI'si.
   * Kaynak REST Media Services için URI.
   * Azure AD uygulama değerlerini: istemci Kimliğini ve istemci gizli anahtarı.
   
   Gerekli tüm değerleri almak için bkz: [erişim Azure Media Services API'sine Azure CLI ile](access-api-cli-how-to.md)

2. Azure AD erişim belirteci için orta katman gönderilir.
4. Orta katman Azure AD belirteciyle Azure medya REST API isteği gönderir.
5. Orta katman veri Media Services'den geri alır.

### <a name="samples"></a>Örnekler

Azure AD hizmet sorumlusu ile bağlanma işlemini gösteren aşağıdaki örneklere bakın:

* [REST ile bağlanma](media-rest-apis-with-postman.md)  
* [Java ile bağlanma](configure-connect-java-howto.md)
* [.NET ile bağlanma](configure-connect-dotnet-howto.md)
* [Node.js ile bağlanma](configure-connect-nodejs-howto.md)
* [Python ile bağlanma](configure-connect-python-howto.md)

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

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

[Media Services v3 API SDK'ları / araçlarla geliştirmeye başlayın](developers-guide.md)
