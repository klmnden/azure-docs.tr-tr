---
title: Gözden geçirme aracı kavramları - Content Moderator hakkında bilgi
titlesuffix: Azure Cognitive Services
description: Content Moderator gözden geçirme aracı hakkında birleştirilmiş yapay ZEKA ve insan tarafından İnceleme denetimi çaba koordine eden bir Web sitesi öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.date: 03/15/2019
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.author: sajagtap
ms.openlocfilehash: b7ec997fd3e9bfe294050893d80fd57a96a47aae
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58755869"
---
# <a name="content-moderator-review-tool"></a>Content Moderator İnceleme aracı

Azure Content Moderator, machine learning içerik denetleme incelemelere, birleştirmek için hizmetler sağlar ve [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com) bu hizmetler için ayrıntılı erişim sağlayan kullanıcı dostu bir ön uç Web sitesidir.

![Bir tarayıcıda gözden geçirme aracı Panosu](./images/0-dashboard.png)

## <a name="what-it-does"></a>Ne yapar?

[Gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com), makine Yardımlı resim denetimi API'leri ile birlikte kullanılan, içerik denetleme işlemi aşağıdaki görevleri gerçekleştirmenize olanak tanır:

- Orta (metin, görüntü ve video) birden çok biçimde içerik için bir dizi araç kullanın.
- İnsan oluşturulmasını otomatikleştirin [incelemeleri](../review-api.md#reviews) denetimi API'si gelen sonuçları zaman içinde.
- Atayın veya içerik kategori veya deneyim düzeyine göre düzenlenmiş birden çok gözden geçirme ekibi için içerik incelemeleri ilerletebilirsiniz.
- Varsayılan veya özel mantığı filtreleri kullanın ([iş akışları](../review-api.md#workflows)) sıralama ve içerik, herhangi bir kod yazmadan izlemek için.
- Kullanım [Bağlayıcılar](./configure.md#connectors) Microsoft PhotoDNA, metin analizi ve içerik Moderator API'leri yanı sıra yüz tanıma API'leri ile içeriğini işlemek için.
- İş akışları için herhangi bir API oluşturmak için kendi Bağlayıcınızı oluşturun veya iş süreci.
- Temel performans ölçümlerini, içerik denetleme işlemleri alın.

## <a name="review-tool-dashboard"></a>Gözden geçirme aracı Panosu

Üzerinde **Pano** sekme, içerik incelemeleri aracın içinden yapılan ilişkin ana ölçümleri görebilirsiniz. Toplam, tam, ve resim, metin ve video içeriği gözden geçirmeleri bekleyen bakın. Kullanıcılar ve henüz uygulanmamış denetimi etiketleri yanı sıra, incelemeler tamamlandı takımlar dökümünü de görebilirsiniz.

![Panoyu görüntüleyin](images/0-dashboard.png)

## <a name="review-tool-credentials"></a>Aracı kimlik bilgileri gözden geçirin

Oturum açtığınızda ile [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com), bir Azure bölgesi seçin, hesap istenir. Bunun nedeni, [gözden geçirme aracı](https://contentmoderator.cognitive.microsoft.com) Content Moderator Azure Hizmetleri için ücretsiz bir deneme anahtar oluşturur, bir REST çağrısı veya istemci SDK'sı hizmetlerinden herhangi birine erişmek için bu anahtar gerekir. Anahtar ve API uç nokta URL'nizi seçerek görüntüleyebilirsiniz **ayarları** > **kimlik bilgilerini**.

![Content Moderator kimlik bilgileri](images/settings-6-credentials.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [gözden geçirme aracı yapılandırma](./configure.md) gözden geçirme aracı kaynaklara erişebilir ve ayarlarını değiştirme hakkında bilgi edinmek için.