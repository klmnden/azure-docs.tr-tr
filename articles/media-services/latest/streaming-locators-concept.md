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
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 51aa33e4ff387a1030dac42bce8d12cf72343b35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61466740"
---
# <a name="streaming-locators"></a>Akış Bulucuları

Çıkış Varlığınızdaki videoların istemcilerde kayıttan yürütülebilmesi için bir [Akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) ve ardından akış URL'si oluşturmanız gerekir. .NET örneği için bkz. [Akış Bulucu edinme](stream-files-tutorial-with-api.md#get-a-streaming-locator).

Oluşturma işlemi bir **akış Bulucu** yayımlama denir. Varsayılan olarak, **akış Bulucu** API çağrılarını hemen sonra geçerli olduğunu ve isteğe bağlı bir başlangıç ve bitiş zamanlarını yapılandırmadığınız sürece silinene kadar sürer. 

Oluştururken bir **akış Bulucu**, belirtmenize gerek [varlık](https://docs.microsoft.com/rest/api/media/assets) adı ve [akış ilke](https://docs.microsoft.com/rest/api/media/streamingpolicies) adı. Özel bir ilke oluşturulur veya önceden tanımlanmış akış ilkelerden birini ya da kullanabilirsiniz. Şu anda kullanılabilen önceden tanımlanmış ilkeleri şunlardır: 'Predefined_DownloadOnly', 'Predefined_ClearStreamingOnly', 'Predefined_DownloadAndClearStreaming', 'Predefined_ClearKey', 'Predefined_MultiDrmCencStreaming' ve 'Predefined_MultiDrmStreaming'. Özel bir kullanırken İlkesi, akış gibi ilkelerin sınırlı sayıda medya hizmeti hesabınız için tasarlamanıza ve aynı seçenek ve protokolleri gerektiğinde bunları yeniden için akış Bulucular. 

Şifreleme seçeneklerini akışınızı belirtmek istiyorsanız, oluşturma [içerik anahtarı ilke](https://docs.microsoft.com/rest/api/media/contentkeypolicies) aracılığıyla anahtar teslim bileşen Media Services'ın istemcileri sonlandırmak için içerik anahtarını nasıl teslim edildiğini yapılandırır. İle bir akış Bulucu ilişkilendirmek **içerik anahtarı ilke** ve içerik anahtarı. Media Services otomatik anahtar sağlayabilirsiniz. Aşağıdaki .NET örnek nasıl bir belirteç kısıtlaması Media Services v3 ile AES şifreleme yapılacağı gösterilmektedir: [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted). **İçerik anahtarı ilkeleri** olan güncelleştirilebilir bir anahtar döndürme yapmanız gerekiyorsa ilke güncelleştirmek isteyebilirsiniz. Bu, güncelleştirme ve güncelleştirilmiş ilke çekme anahtar teslim önbellekleri 15 dakika kadar sürebilir. Yeni bir içerik anahtarı ilkesi için her bir akış Bulucu oluşturmamayı önerilir. Mevcut ilkeleri aynı seçeneklere gerektiğinde yeniden denemelisiniz.

> [!IMPORTANT]
> * Özelliklerini **akış bulucuları** DateTime türü her zaman UTC biçiminde olan.
> * Medya hizmeti hesabınız için sınırlı sayıda ilkeleri tasarlayın ve aynı seçeneklere gerektiğinde bunları yeniden için akış Bulucular gerekir. 

## <a name="associate-filters-with-streaming-locators"></a>Akış bulucuları ile ilişkilendirme filtreleri

Bir listesini belirtebilirsiniz [varlık veya hesap filtreleri](filters-concept.md), için uygulamak, [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators/create#request-body). [Dinamik Paketleyici](dynamic-packaging-overview.md) bu olanlar istemcinizin URL'SİNDE belirtir birlikte filtrelerinin listesi için geçerlidir. Bu birleşim oluşturur bir [dyanamic bildirimi](filters-dynamic-manifest-overview.md), URL'deki filtreleri + akış Bulucu üzerinde belirttiğiniz filtreleri temel. Filtre uygulamak istediğiniz, ancak URL filtresi adlarında kullanıma sunmak istiyorsanız değil, bu özelliği kullanmanızı öneririz.

## <a name="filter-order-page-streaming-locator-entities"></a>Filtre sırası, sayfa akış Bulucu varlıklar

Bkz: [filtreleme, sıralama, Media Services varlıklarının sayfalandırma](entities-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: .NET kullanarak videoları karşıya yükleme, kodlama ve akışla aktarma](stream-files-tutorial-with-api.md)
* [DRM dinamik şifreleme ve lisans teslim hizmetini kullanma](protect-with-drm.md)
