---
title: Video Indexer - Azure birden çok kiracıyla yönetme
description: Bu makalede, Video Indexer'ın birden çok kiracıyla yönetmek için farklı tümleştirme seçenekleri önerir.
services: media-services
documentationcenter: ''
author: ika-microsoft
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 05/15/2019
ms.author: ikbarmen
ms.openlocfilehash: 2919e021d6b70ce82a6ff6b1d1972dd89de95104
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799481"
---
# <a name="manage-multiple-tenants"></a>Birden çok kiracıyı yönetme

Bu makalede, Video Indexer'ın birden çok kiracıyla yönetmek için farklı seçenekler açıklanır. Senaryonuz için en uygun bir yöntem seçin:

* Video Indexer hesabınız Kiracı başına
* Tüm kiracılar için tek bir Video Indexer hesabınız
* Kiracı başına Azure aboneliği

## <a name="video-indexer-account-per-tenant"></a>Video Indexer hesabınız Kiracı başına

Bu mimari kullanırken, bir Video Indexer hesabınız için her bir kiracı oluşturulur. Kiracılar, tam yalıtım kalıcı olması ve katman işlem.  

![Video Indexer hesabınız Kiracı başına](./media/manage-multiple-tenants/video-indexer-account-per-tenant.png)

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Müşteriler, depolama hesapları (el ile müşteri tarafından şekilde yapılandırılmadıkça) paylaşmayın.
* Müşteriler, işlem (ayrılmış birimler) paylaşmayın ve başka bir işleme işleri sürelerini etkisi yoktur.
* Video Indexer hesabınız silerek bir kiracı sistemden kolayca kaldırabilirsiniz.
* Kiracılar arasında özel modelleri paylaşma olanağı yoktur.

    Özel modelleri paylaşmak için bir iş gerekliliğiniz olduğundan emin olun.
* Birden fazla Video Indexer (ve ilişkili Media Services nedeniyle), Kiracı başına hesaplarını yönetmek daha zor.

> [!TIP]
> Sisteminizde için bir yönetici kullanıcı oluşturma [Video Indexer Geliştirici Portalı](https://api-portal.videoindexer.ai/) ve kiracılarınız ilgili sağlamak için yetkilendirme API'yi kullanmak [hesap erişim belirtecini](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token).

## <a name="single-video-indexer-account-for-all-users"></a>Tüm kullanıcılar için tek bir Video Indexer hesabınız

Bu mimari kullanırken, Kiracı yalıtımı için müşteri sorumludur. Tüm kiracılar, tek bir Video Indexer hesabınız ile tek bir Azure medya hizmeti hesabı kullanmak zorunda. Karşıya yüklerken, arama veya içeriği silme müşteri bu Kiracı için uygun sonuçları filtrelemek gerekir.

![Tüm kullanıcılar için tek bir Video Indexer hesabınız](./media/manage-multiple-tenants/single-video-indexer-account-for-all-users.png)

Bu seçenek belirtilmişse, özelleştirme modelleri (kişi, dil ve markalar) paylaşılan veya modelleri filtreleyerek Kiracı tarafından kiracılar arasında yalıtılır.

Zaman [videoları karşıya yükleme](https://api-portal.videoindexer.ai/docs/services/operations/operations/Upload-video?), Kiracı başına bir bölümle özniteliği belirtebilirsiniz. Bu yalıtım modunda sağlayacak [arama APİ'si](https://api-portal.videoindexer.ai/docs/services/operations/operations/Search-videos?). Arama API'si bölüm öznitelik belirterek yalnızca belirtilen bölüm sonuçları alırsınız. 

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Kiracılar arasında içerik ve özelleştirme modelleri paylaşma olanağı.
* Bir kiracının diğer kiracılara performansını etkiler.
* Video Indexer üzerine bir karmaşık yönetim katmanı oluşturmak müşteri gerekir.

> [!TIP]
> Kullanabileceğiniz [öncelik](upload-index-videos.md) kiracılar işlerini önceliklendirmek için özniteliği.

## <a name="azure-subscription-per-tenant"></a>Kiracı başına Azure aboneliği 

Bu mimari kullanırken, her kiracının kendi Azure aboneliği gerekir. Her bir kullanıcı için yeni bir Video Indexer hesabına Kiracı aboneliği oluşturur.

![Kiracı başına Azure aboneliği](./media/manage-multiple-tenants/azure-subscription-per-tenant.png)

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Fatura ayrılmasını sağlayan tek seçenek budur.
* Bu tümleştirme, daha fazla yönetim yükü daha Kiracı başına Video Indexer hesabınız vardır. Faturalandırma, bir gereksinim değildir, bu makalede açıklanan diğer seçeneklerden birini kullanmak için önerilir.

## <a name="next-steps"></a>Sonraki adımlar

[Genel bakış](video-indexer-overview.md)
