---
title: Microsoft Çeviricisi metin API'si ile filtreleme uygunsuz metin | Microsoft Docs
description: Microsoft Çeviricisi metin API filtreleme uygunsuz metin kullanın.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: a7172e1e8aa336c011fb06e93fc5c4b54d26a3cd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352589"
---
# <a name="how-to-add-profanity-filtering-with-the-microsoft-translator-text-api"></a>Microsoft Çeviricisi metin API'si ile filtreleme uygunsuz metin ekleme

Normalde Çeviricisi hizmet mevcut çeviri kaynağında uygunsuz metin korur. Uygunsuz metin ve sözcükleri saygısız içerikli yapar bağlam derecesini kültürler arasında farklılık gösterir. Sonuç olarak hedef dilde uygunsuz metin derecesini yükseltilmiş azaltılmış veya olabilir.

Uygunsuz metin (bağımsız olarak kaynak metindeki uygunsuz metin varlığını) çevirisini görmekten kaçınmak istiyorsanız, Translate() yönteminde uygunsuz metin filtre seçeneğini kullanabilirsiniz. Seçeneği, silinmiş veya uygun etiketleriyle işaretlenmiş uygunsuz metin görmeyi veya eylem seçmenizi sağlar.

Translate() yöntemi yeni öğesi "ProfanityAction" içeren bir "Seçenekleri" parametresi alır. Kabul edilen ProfanityAction "NoAction", "İşaretlenmiş" ve "Silinmiş" değerleri.

## <a name="accepted-values-of-profanityaction-and-examples"></a>Kabul edilen değerler ProfanityAction ve örnekler
|ProfanityAction değeri | Eylem | Örnek: Kaynak - Japonca | Örnek: Hedef - İngilizce|
| :---|:---|:---|:---|
| NoAction | Varsayılan. Aynı seçenek ayarı bulunamadı. Uygunsuz metin kaynaktan hedefe geçirir. | 彼は変態です。 | Kendisine aptalın olur. |
| İşaretli | Saygısız içerikli sözcükler tarafından XML etiketleri arasına \<uygunsuz metin >... \</profanity >. | 彼は変態です。 | Gerçekleştirilmesine bir \<uygunsuz metin > jerk\</profanity >. |
| Silinen | Saygısız içerikli sözcükleri değiştirme yapmadan çıktısından kaldırılır. | 彼は。 | Gerçekleştirilmesine bir. |

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Uygunsuz metin Çeviricisi API çağrısı ile filtreleme Uygula](reference/v3-0-translate.md)

