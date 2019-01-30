---
title: Küfür filtresini - Translator metin çevirisi API'si
titlesuffix: Azure Cognitive Services
description: Translator metin çevirisi API'si filtreleme küfür kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 5ff6666eceeaee2296c6323eaa4e7201841a1526
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55212469"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Translator metin çevirisi API'si ile filtreleme küfür Ekle

Normalde Translator hizmeti, mevcut çeviri kaynağında küfür korur. Küfür ve sözcükler Küfürlü yapan bağlam derecesini kültürler arasında farklılık gösterir. Sonuç olarak, hedef dilde küfür derecesini yükseltilmiş sınırlı veya olabilir.

Küfür kaynak metni mevcut olsa da, küfür çevirisini görmekten kaçınmak istiyorsanız, filtreleme seçeneği Translate() yöntemi sağlanır küfür kullanın. Bu seçenek, uygun etiketlerle işaretlenmiş silinmiş küfür görmek isteyip istemediğinizi seçin veya hiçbir eyleme girişilmedi sağlar.

Translate() yöntemi yeni bir öğe "ProfanityAction" içerir "options" parametresi alır. Kabul edilen ProfanityAction "NoAction", "Marked" ve "Silinmiş" değerleri

## <a name="accepted-values-of-profanityaction-and-examples"></a>Kabul edilen değerler ProfanityAction ve örnekler
|ProfanityAction değeri | Eylem | Örnek: Kaynak - Japonca | Örnek: Target - İngilizce|
| :---|:---|:---|:---|
| NoAction | Varsayılan. Aynı seçenek ayarı bulunamadı. Küfür kaynaktan hedefe aktarır. | 彼は変態です。 | He aptalın olur. |
| İşaretli | Küfürlü sözcükleri tarafından XML etiketleri arasına \<küfür >... \</profanity >. | 彼は変態です。 | He's bir \<küfür > jerk\</profanity >. |
| Silinen | Küfürlü sözcükleri değiştirme yapmadan çıktıdan kaldırıldı. | 彼は。 | He's bir. |

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Translator API çağrınızı filtreleme küfür Uygula](reference/v3-0-translate.md)
