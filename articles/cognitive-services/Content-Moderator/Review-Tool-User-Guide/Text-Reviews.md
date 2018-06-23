---
title: Azure içerik denetleyici metinde gözden | Microsoft Docs
description: İçerik kendi skoru görmek için denetleyici metinde gözden öğrenin ve etiketleri algıladı. İçeriği uygun olup olmadığını bilgileri tanımadığını belirlemek için kullanın.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/03/2017
ms.author: sajagtap
ms.openlocfilehash: cb8774395b7374677e8de3b80630a2d4abf490f9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351515"
---
# <a name="review-text"></a>Metni inceleme

Etiketler algılandı ve Azure içerik denetleyici metin puanları kullanarak gözden geçirmek için kullanabilirsiniz. İçeriği uygun olup olmadığını belirlemek için bilgileri kullanın. 

## <a name="select-or-enter-the-text-to-review"></a>Gözden geçirmek için metin girin veya seçin

İçerik Aracı alanında seçin **deneyin** sekmesi. Ardından, seçin **metin** seçeneği metin denetleme Başlangıç ekranına gidin. Herhangi bir metin girin veya Otomatik Metin denetleme için varsayılan örnek metin gönderin. En çok 1024 karakter girebilirsiniz.

## <a name="get-ready-to-review-results"></a>Sonuçlarını gözden geçirmek hazırlanma

Gözden geçirme Aracı'nı ilk metin denetleme API'sini çağırır. Ardından, algılanan etiketleri kullanarak metin incelemeler oluşturur. Gözden geçirme Aracı'nı ekibinizin dikkat Puanlama sonuçlarını eşleşir.

## <a name="review-text-results"></a>Metin sonuçlarını gözden geçirin

Windows'da ayrıntılı sonuçları görüntülenir. Sonuçları algılanan etiketleri ve metin denetleme API tarafından döndürülen koşulları içerir. Bir etiketin seçim durumunu geçiş yapmak için etiketi seçin. Ayrıca, oluşturmuş olabileceğiniz özel etiketler ile çalışabilirsiniz.

![Metin sonuçlarını gözden geçirin](images/3-review-text-2.png)
