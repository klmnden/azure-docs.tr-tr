---
title: Küfür filtresini - Translator metin çevirisi API'si
titlesuffix: Azure Cognitive Services
description: Translator metin çevirisi API'si filtreleme küfür kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 87814571e6f1c20b219020651eb798fa49a28deb
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127942"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Translator metin çevirisi API'si ile filtreleme küfür Ekle

Normalde Translator hizmeti, mevcut çeviri kaynağında küfür korur. Küfür ve sözcükler Küfürlü yapan bağlam derecesini kültürler arasında farklılık gösterir. Sonuç olarak hedef dilde küfür derecesini yükseltilmiş sınırlı veya olabilir.

Küfür çevirisini (kaynak metni küfür varsa bile) görmekten kaçınmak istiyorsanız, Translate() yöntemi seçeneğinde filtreleme küfür kullanın. Bu seçenek silinmiş ya da uygun etiketlerle işaretlenmiş küfür görmek isteyip istemediğinizi veya hiçbir eyleme girişilmedi seçmenize olanak tanır.

Translate() yöntemi yeni bir öğe "ProfanityAction" içeren bir "Seçenekler" parametresi alır. Kabul edilen ProfanityAction "NoAction," "Olarak işaretlenmiş" ve "Silinmiş" değerleri

## <a name="accepted-values-of-profanityaction-and-examples"></a>Kabul edilen değerler ProfanityAction ve örnekler
|ProfanityAction değeri | Eylem | Örnek: Kaynak - Japonca | Örnek: Hedef - İngilizce|
| :---|:---|:---|:---|
| NoAction | Varsayılan. Aynı seçenek ayarı bulunamadı. Küfür kaynaktan hedefe aktarır. | 彼は変態です。 | He aptalın olur. |
| İşaretli | Küfürlü sözcükleri tarafından XML etiketleri arasına \<küfür >... \</profanity >. | 彼は変態です。 | He's bir \<küfür > jerk\</profanity >. |
| Silinen | Küfürlü sözcükleri değiştirme yapmadan çıktıdan kaldırıldı. | 彼は。 | He's bir. |

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Translator API çağrınızı filtreleme küfür Uygula](reference/v3-0-translate.md)

