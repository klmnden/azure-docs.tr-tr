---
title: Azure Content Moderator metinde gözden geçirin | Microsoft Docs
description: Content Moderator, puan görmek için metin gözden geçirmeyi öğrenin ve etiketleri algılandı. İçeriği uygun olup olmadığını erişmeye bilgileri kullanın.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/03/2017
ms.author: sajagtap
ms.openlocfilehash: 60a8d03a6562c9a66c2f6781ebbc330c989b343e
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2018
ms.locfileid: "51852104"
---
# <a name="review-text"></a>Metni inceleme

Etiketleri algılandı ve Azure Content Moderator puanları kullanarak metin gözden geçirmek için kullanabilirsiniz. İçerik uygun olup olmadığını belirlemek için bilgileri kullanın. 

## <a name="select-or-enter-the-text-to-review"></a>Gözden geçirmek için metin girin veya seçin

Content Moderator seçin **deneyin** sekmesi. Ardından, **metin** metin denetimi Başlangıç ekranına gitmek için seçeneği. Herhangi bir metni girin veya otomatik metin denetimi için varsayılan örnek metinler gönderin. En fazla 1024 karakter girebilirsiniz.

## <a name="get-ready-to-review-results"></a>Sonuçları gözden geçirmek hazırlanma

İnceleme aracını ilk metin denetimi API'sini çağırır. Ardından, algılanan etiketleri kullanarak metin incelemeleri oluşturur. İnceleme aracını takımınızın ilgi puanı sonuçları eşleşir.

## <a name="review-text-results"></a>Metin sonuçlarını gözden geçirin

Ayrıntılı sonuçlar windows görüntülenir. Sonuçlar, algılanan etiket ve metin denetimi API'si tarafından döndürülen koşulları içerir. Etiketin seçim durumu geçiş yapmak için etiketi seçin. Ayrıca, oluşturmuş olabileceğiniz özel etiketler ile de çalışabilir.

![Metin bir Chrome tarayıcı penceresinde gözden geçirme aracı gösteren ekran görüntüsü bayrağı](../images/reviewresults_text.png)
