---
title: Word hizalama bilgileri Microsoft Çeviricisi metin API ile | Microsoft Docs
description: Word hizalama bilgileri Microsoft Çeviricisi metin API'SİNDEN alırsınız.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 9d8284db61235284ec0d5a1594c34687c89560e9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352607"
---
# <a name="how-to-receive-word-alignment-information"></a>Word hizalama bilgileri alma

## <a name="receiving-word-alignment-information"></a>Word hizalama bilgileri alma
Hizalama bilgileri almak için çeviri yöntemini kullanın ve isteğe bağlı includeAlignment parametresini ekleyin.

## <a name="alignment-information-format"></a>Hizalama bilgileri biçimi
Hizalama her word kaynak için aşağıdaki biçimde bir dize değeri olarak döndürülür. Her sözcüğün bilgilerini alanı ayrılmış (komut dosyaları) gibi diller Çince dahil olmak üzere bir boşlukla ayrılır:

[[SourceTextStartIndex]: [SourceTextEndIndex]-[TgtTextStartIndex]: [TgtTextEndIndex]] *

Örnek hizalama dizesi: "0:0-7:10 1:2-11:20 3:4-0:3 3:4-4:6 5:5-21:21".

Diğer bir deyişle, iki nokta üst üste başlangıç ayırır ve bitiş dizini, tire dilleri ayırır ve sözcükleri alan ayırır. Bir sözcük sıfır, bir veya birden çok sözcük başka bir dilde hizalamak ve hizalanmış sözcükler bitişik olmayan olabilir. Hiçbir hizalama bilgileri kullanılabilir olduğunda hizalama öğesi boş olur. Yöntemi bu durumda herhangi bir hata döndürür.

## <a name="restrictions"></a>Kısıtlamalar
Hizalama dil çiftleri bir kısmı için yalnızca bu noktada verilir:
* başka bir dil için İngilizce;
* herhangi diğer bir dilden-Basitleştirilmiş Çince, Geleneksel Çince ve Letonca-İngilizce dışında İngilizce
* Tamamlanmış bir çeviri cümle ise, Kore dili için Japonca veya Kore dili, Japonca hizalama bilgileri almazsınız. Tamamlanmış bir çeviri örneğidir "Bu olan bir test", "I sevdiğiniz" ve diğer yüksek yoğunlukta cümleleri.

## <a name="example"></a>Örnek

Örnek JSON

```json
[
  {
    "translations": [
      {
        "text": "Kann ich morgen Ihr Auto fahren?",
        "to": "de",
        "alignment": {
          "proj": "0:2-0:3 4:4-5:7 6:10-25:30 12:15-16:18 17:19-20:23 21:28-9:14 29:29-31:31"
        }
      }
    ]
  }
]
```
