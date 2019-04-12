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
ms.date: 04/08/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 18b72ceaee0ca0747a0bf2144d5f9ffddbee8b8c
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59492150"
---
# <a name="developing-with-media-services-v3-apis"></a>V3 API'ler Media Services ile geliştirme

Bu makalede, Media Services v3 ile geliştirirken varlıkları ve API'ler için geçerli kurallar açıklanmaktadır.

## <a name="naming-conventions"></a>Adlandırma kuralları

Azure Media Services v3 kaynaklarının adları (Varlıklar, İşler, Dönüşümler gibi), Azure Resource Manager adlandırma kısıtlamalarına tabidir. Azure Resource Manager uyarınca kaynak adları her zaman benzersizdir. Bu nedenle kaynaklarınızda benzersiz tanıtıcı dizeleri (GUID gibi) kullanabilirsiniz. 

Media Services kaynak adları şu karakterleri içeremez: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', tek tırnak karakteri veya kontrol karakterleri. Diğer tüm karakterlere izin verilir. Bir kaynağın adı en fazla 260 karakter olabilir. 

Azure Resource Manager adlandırma hakkında daha fazla bilgi için bkz: [Adlandırma gereksinimlerini](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) ve [adlandırma kuralları](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="v3-api-design-principles-and-rbac"></a>V3 API tasarım ilkeleri ve RBAC

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. V3 API'ler döndürmeyen parolaları veya kimlik üzerinde **alma** veya **listesi** operations. Anahtarlar her zaman null, boş veya yanıttan ayıklanmış olur. Kullanıcı parolaları veya kimlik bilgilerini almak için ayrı bir eylem yöntemini çağırmak gerekir. **Okuyucu** Asset.ListContainerSas, StreamingLocator.ListContentKeys, ContentKeyPolicies.GetPolicyPropertiesWithSecrets gibi işlemler çağrılamıyor şekilde rol operations çağrılamıyor. Ayrı Eylemler sahip isterseniz özel bir rol daha ayrıntılı RBAC güvenlik izinleri ayarlamanızı sağlar.

Daha fazla bilgi için bkz.

- [Yerleşik rol tanımları](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles)
- [Erişimi yönetmek için RBAC kullanma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)
- [Media Services hesapları için rol tabanlı erişim denetimi](rbac-overview.md)
- [İçerik anahtarı ilkesi - .NET edinme](get-content-key-policy-dotnet-howto.md).

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
