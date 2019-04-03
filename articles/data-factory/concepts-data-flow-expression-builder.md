---
title: Azure veri fabrikası eşleme veri akışı ifade oluşturucusu
description: İfade oluşturucu için Azure Data Factory eşleme veri akışları
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 7e80455a2b83c034bea2fbdf774e5a175aed51a4
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885749"
---
# <a name="mapping-data-flow-expression-builder"></a>Eşleme veri akışı ifade oluşturucusu

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Azure Data Factory eşleme veri akışı içinde veri dönüştürme için ifadeler girebileceğiniz iletişim kutularında bulabilirsiniz. Bu kutularında, sütunlar, alanlar, değişkenleri, parametreleri, veri akışınız işlevleri kullanın. İfade oluşturmak için dönüştürme içindeki ifade metin kutusuna tıklayarak başlatılan ifade oluşturucu kullanın. Bazen, dönüştürme için sütunları seçerken "Hesaplanan sütun" seçenekleri görürsünüz. ' A tıkladığınızda, deyim başlatılan Oluşturucusu'nu görürsünüz.

![İfade Oluşturucu](media/data-flow/expression.png "ifade oluşturucusu")

İfade Oluşturucu aracı metin düzenleyicisi seçeneğini kullanır. Otomatik Tamamlama özelliği tüm Azure Data Factory, veri akışı nesne modelinden denetleme ve vurgulama sözdizimiyle okur.

![İfade oluşturucusu otomatik tamamlama](media/data-flow/expb1.png "ifade oluşturucusu otomatik tamamlama")

## <a name="currently-working-on-field"></a>Alan şu anda çalışıyor

![İfade Oluşturucu](media/data-flow/exp3.png "üzerinde şu anda çalışıyor")

İfade Oluşturucu UI en sol üst köşesindeki, üzerinde çalıştığınız alanın adıyla "Şu anda çalışan üzerinde" adlı bir alan görürsünüz. Yalnızca şu anda çalışma alanına kullanıcı Arabiriminde yapı ifade uygulanır. Başka bir alan dönüştürmek isterseniz, geçerli çalışmanızı kaydedin ve başka bir alan seçin ve diğer alanlar için bir ifade oluşturmak için bu açılan kullanın.

## <a name="data-preview-in-debug-mode"></a>Hata ayıklama modunda veri önizlemesi

![İfade Oluşturucu](media/data-flow/exp4b.png "ifade veri önizlemesi")

İfadeler üzerinde çalışırken, isteğe bağlı olarak Azure Data Factory, veri akışı tasarım yüzeyinde, hata ayıklama modunda üzerinde veri sonuçlarınızdan derlemekte olduğunuz ifade Canlı sürüyor önizlemesini etkinleştirme geçiş yapabilirsiniz. Gerçek zamanlı Canlı hata ayıklama, ifadeler için etkinleştirilir.

![Hata ayıklama modu](media/data-flow/debugbutton.png "düğmesi hata ayıklama")


![İfade Oluşturucu](media/data-flow/exp5.png "ifade veri önizlemesi")

## <a name="comments"></a>Yorumlar

Tek satır ve çok satırlı açıklama söz dizimi kullanarak, ifadeleri için açıklamalar ekleyin:

![Açıklamalar](media/data-flow/comments.png "açıklamaları")

## <a name="regular-expressions"></a>Normal İfadeler

Azure Data Factory, veri akışı ifade dili [tam başvuru belgelerine](https://aka.ms/dataflowexpressions), normal ifade söz dizimi dahil işlevleri sağlar. Normal ifade işlevlerine kullanırken, ters eğik çizgi yorumlamak ifade oluşturucu deneyecek (\\) olarak bir kaçış karakteri dizisi. Ters eğik çizgi normal ifadeniz kullanırken ya da tüm normal ifade dalgalanmasındaki alın (\`) veya çift ters eğik çizgi kullanın.

Saat döngüsü kullanarak örneği

```
regex_replace('100 and 200', `(\d+)`, 'digits')
```

veya çift eğik çizgi kullanma

```
regex_replace('100 and 200', '(\\d+)', 'digits')
```

## <a name="addressing-array-indexes"></a>Dizi dizinleri adresleme

Dizi döndüren ifade işlevleri ile belirli dizinleri, döndürülen dizi nesnesi içinde ele almak için köşeli ayraçlar [] kullanın. Dizi olanları tabanlıdır.

![İfade Oluşturucu dizi](media/data-flow/expb2.png "ifade veri önizlemesi")

## <a name="handling-names-with-special-characters"></a>Özel karakterleri içeren işleme

Özel karakterler veya boşluk içeren sütun adları olduğunda adı kaşlı ayraçlar ile çevreleyin.
* ```{[dbo].this_is my complex name$$$}```

## <a name="next-steps"></a>Sonraki adımlar

[Veri dönüştürme ifadeleri oluşturmaya başlayın](data-flow-expression-functions.md)
