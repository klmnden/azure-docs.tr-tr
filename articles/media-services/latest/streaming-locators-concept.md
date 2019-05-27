---
title: Azure Media Services Bulucular akış | Microsoft Docs
description: Bu makalede, akış Bulucuyu nedir ve Azure Media Services tarafından nasıl kullanıldıkları bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/22/2019
ms.author: juliako
ms.openlocfilehash: 9a14399117971807c1d18f8eb5fab7d6e6cef2d5
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66120346"
---
# <a name="streaming-locators"></a>Akış Bulucuları

Çıkış Varlığınızdaki videoların istemcilerde kayıttan yürütülebilmesi için bir [Akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) ve ardından akış URL'si oluşturmanız gerekir. .NET örneği için bkz. [Akış Bulucu edinme](stream-files-tutorial-with-api.md#get-a-streaming-locator).

Oluşturma işlemi bir **akış Bulucu** yayımlama denir. Varsayılan olarak, **akış Bulucu** API çağrılarını hemen sonra geçerli olduğunu ve isteğe bağlı bir başlangıç ve bitiş zamanlarını yapılandırmadığınız sürece silinene kadar sürer. 

Oluştururken bir **akış Bulucu**, belirtmelisiniz bir **varlık** adı ve **akış ilke** adı. Daha fazla bilgi için aşağıdaki konulara bakın:

* [Varlıklar](assets-concept.md)
* [Akış İlkeleri](streaming-policy-concept.md)
* [İçerik Anahtarı İlkeleri](content-key-policy-concept.md)

> [!IMPORTANT]
> * Özelliklerini **akış bulucuları** DateTime türü her zaman UTC biçiminde olan.
> * Medya hizmeti hesabınız için sınırlı sayıda ilkeleri tasarlayın ve aynı seçeneklere gerektiğinde bunları yeniden için akış Bulucular gerekir. Daha fazla bilgi için [kotaları ve sınırlamaları](limits-quotas-constraints.md).

## <a name="associate-filters-with-streaming-locators"></a>Akış bulucuları ile ilişkilendirme filtreleri

Bkz: [filtreleri: akış bulucuları ile ilişkilendirme](filters-concept.md#associate-filters-with-streaming-locator).

## <a name="filter-order-page-streaming-locator-entities"></a>Filtre sırası, sayfa akış Bulucu varlıklar

Bkz: [filtreleme, sıralama, Media Services varlıklarının sayfalandırma](entities-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: .NET kullanarak videoları karşıya yükleme, kodlama ve akışla aktarma](stream-files-tutorial-with-api.md)
