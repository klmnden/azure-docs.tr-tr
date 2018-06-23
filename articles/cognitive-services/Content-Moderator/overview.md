---
title: Azure içerik denetleyicinin genel bakış | Microsoft Docs
description: İçerik denetleyici izlemek, bayrak, değerlendirmek ve kullanıcı tarafından oluşturulan içeriğin uygunsuz içeriği filtrelemek için nasıl kullanılacağını öğrenin.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/15/2017
ms.author: sajagtap
ms.openlocfilehash: cd9e2e9c10e9dc5ba118c8319f76174bf6f0da9f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355144"
---
# <a name="what-is-content-moderator"></a>İçerik denetleyici nedir?

İçerik Yönetimi için olası rahatsız edici, istenmeyen ve riskli içerik izleme işlemidir. Yönetilen içerikler görüntüleri, metin ve videolar olabilir.

## <a name="where-it-is-used"></a>Nerede kullanılır

Aşağıdaki listede, içerik denetleyici kullanıldığı birkaç örnek senaryolar gösterilmektedir:

- Çevrimiçi Pazar ürün katalogları ve oluşturulan kullanıcı içeriği yönetme
- Kullanıcı yönetme oyun şirketler oyun yapıları ve sohbet odaları oluşturulan
- Sosyal Mesajlaşma platform görüntüleri, metin ve bunların kullanıcılar tarafından eklenen videolar yönetme
- Kurumsal ortam şirketler içeriklerini için merkezi içerik yönetimini uygulama
- Öğrenciler ve eğitimcilere için hatalı ve rahatsız edici içeriği filtreleme K-12 eğitim çözüm sağlayıcıları

## <a name="what-it-includes"></a>Neleri içerir

İçerik birçok web hizmeti denetleyicinin oluşur API'leri ve yerleşik İnsan-içinde--döngü Orta görüntüleri, metin ve videolar yardımcı aracı gözden geçirin.

![İçerik denetleyici Blok Diyagramı](images/content-moderator-block-diagram.png)

## <a name="apis"></a>API'ler

İçerik denetleyici aşağıdaki API'leri içerir:
  - [**Metin denetleme API**](text-moderation-api.md): olası uygunsuz metin açık, müstehcen, rahatsız edici ve kişisel bilgileri (PII) için metin taramak için bu API'yi kullanın.
  - [**Özel terim listesi API'si**](try-terms-list-api.md): yerleşik koşulları yanı sıra terimlerin özel listeler eşleştirilecek bu API'yi kullanın. Engellemek veya içeriği, içerik ilkelerinize göre izin vermek için bu listeyi kullanın.  
  - [**Görüntü Yönetimi API**](image-moderation-api.md): görüntüleri yetişkin ve saldırganlardan içerik için tarama, metin optik karakter tanıma özelliği görüntülerle algılamak ve yüzeyleri algılamak için bu API kullanın.
  - [**Özel görüntü listesi API'si**](try-image-list-api.md): görüntüleri, yeniden sınıflandırmaya gerek yoktur önceden tanımlanan içerik özel listeler karşı eşleştirmek için bu API'yi kullanın.
  - [**Video yönetimini API**](video-moderation-api.md): olası yetişkin ve saldırganlardan içeriği için videolar taramak için bu API'yi kullanın.
  - [**Gözden API'leri**](try-review-api-job.md): kullanın [işleri](try-review-api-job.md), [incelemeler](try-review-api-review.md), ve [iş akışı](try-review-api-workflow.md) oluşturmak ve İnsan-içinde--döngü iş akışlarını otomatikleştirmek için işlemler gözden geçirme aracı.

## <a name="human-review-tool"></a>İnsan tarafından inceleme aracı

Yerleşik içerik denetleyici aboneliğiniz içerir [İnsan gözden geçirme aracı](Review-Tool-User-Guide/human-in-the-loop.md). Metin, görüntüler ve son kararlar almak, İnsan araburucu için videolar incelenmesi oluşturmak için yukarıda açıklanan gözden geçirme API kullanın.

![İçerik denetleyici video aracı gözden geçirin](images/video-review-default-view.png)

## <a name="next-steps"></a>Sonraki adımlar

Kullanım [Hızlı Başlangıç](quick-start.md) içerik Denetleyici ile çalışmaya başlamak için.
