---
title: BLEU puanı nedir? -Özel Translator
titleSuffix: Azure Cognitive Services
description: BLEU bir otomatik çeviri ve bir veya daha fazla insan tarafından oluşturulan başvuru çevirileri aynı kaynak cümlenin arasındaki farkların ölçüsüdür. BLEU algoritması, otomatik çeviri Ardışık tümceleri başvuru çeviriyi bulur ve ağırlıklı bir biçimde eşleşme sayısını sayar Ardışık tümceleri ile karşılaştırır.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-pawal
ms.openlocfilehash: 08be79dde62a1d624fa59816cf855e702245b622
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66385159"
---
# <a name="what-is-a-bleu-score"></a>BLEU puanı nedir?

[BLEU (iki dilli değerlendirme Understudy)](https://en.wikipedia.org/wiki/BLEU) bir otomatik çeviri ve bir veya daha fazla insan tarafından oluşturulan başvuru çevirileri aynı kaynak cümlenin arasındaki farkların ölçüsüdür.

## <a name="scoring-process"></a>Puanlama sürecinde

BLEU algoritması, otomatik çeviri Ardışık tümceleri başvuru çeviriyi bulur ve ağırlıklı bir biçimde eşleşme sayısını sayar Ardışık tümceleri ile karşılaştırır. Bu eşleşen konum bağımsız olan. Daha yüksek bir eşleşme ölçüde daha ileri düzeyde bir başvuru çeviri ve daha yüksek puanı ile benzerlik gösterir. Anlaşılabilirliği ve dilbilgisi doğruluk dikkate alınır değil.

## <a name="how-bleu-works"></a>BLEU nasıl çalışır?

BLEU'ın gücü, her bir cümle için tam İnsan yükümlülükten insanlara çalışılıyor yerine bir cümle yükümlülükten hataları bir test topluluğunuza ortalaması da insan yükümlülükten ile ilişkilendiren ' dir.

Daha kapsamlı bir tartışmadır BLEU puan [burada](https://youtu.be/-UqDljMymMg).

BLEU sonuçları kesinlikle bağlı olduğu etki alanı, eğitim ve veriler ayarlama test verilerle tutarlılığını kapsamını ve ne kadar veri eğitmek kullanılabilir olan. Modellerinizi dar bir etki alanında eğitilen ve eğitim verilerinizi test verilerinizle tutarlı, yüksek bir BLEU puan bekleyebilirsiniz.

>[!NOTE]
>BLEU puanları arasında bir karşılaştırma justifiable için BLEU sonuç aynı Test kümesini, aynı dil çifti ve aynı MT altyapısı ile karşılaştırılır. Farklı test kümesinden bir BLEU puan farklı olmasına bağlıdır.
