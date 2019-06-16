---
title: Yeni dal dönüştürme Azure veri fabrikası eşleme veri akışı
description: Yeni dal dönüştürme Azure veri fabrikası eşleme veri akışı
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: af2225d749283c7124f89d5a7cd735b2f6bfd121
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61348270"
---
# <a name="mapping-data-flow-union-transformation"></a>Veri akışı birleşim dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

UNION, tek SQL birleşim bu akışları birleşim dönüşümü yeni çıktısı olarak ile birden çok veri akışı birleştirecek. Tüm şema her giriş akışından bir birleştirme anahtarı gerek kalmadan, veri akışı içinde birleştirilebilir.

Hem kaynak verileri, hem de mevcut dönüşümleri akışlar, veri akışı dahil olmak üzere yapılandırılmış her satırın yanındaki "+" simgesini seçerek, n-akış ayarları tablo sayısını birleştirebilirsiniz.

![Birleşim dönüştürme](media/data-flow/union.png "birleşim")

Bu durumda, (Bu örnekte, üç farklı kaynak dosyaları) birden çok kaynaktan birbirinden farklı meta verileri birleştirmek ve bunları tek bir akışa birleştirir:

![Birleşim dönüşümüne genel bakış](media/data-flow/union111.png "birleşim 1")

Bunu başarmak için eklemek istediğiniz tüm kaynak dahil ederek birleşim ayarlarında ek satırlar ekleyin. Ortak bir arama ya da birleştirme anahtarı için gerek yoktur:

![Birleşim dönüştürme ayarlarını](media/data-flow/unionsettings.png "birleşim ayarları")

Sonra birleşim Select dönüştürme ayarlarsanız, çakışan alanlarını veya headerless kaynaklardan adlı olmayan alanları yeniden adlandırma mümkün olacaktır. Bu örnekte üç farklı kaynaklardan 132 toplam sütunlarla birleştirme meta verileri görmek için "inceleyin" tıklayın:

![Birleşim dönüştürme son](media/data-flow/union333.png "birleşim 3")

## <a name="name-and-position"></a>Ad ve konum

"Birleşim adıyla" seçtiğinizde, her bir sütun değerine karşılık gelen sütunun yeni bir birleştirilmiş meta veri şema ile her bir kaynaktan gelen içine kaldıracağız.

"Konumu birleşimiyle" seçeneğini belirlerseniz, her bir sütun değerine her kaynaktan gelen verileri aynı akışa burada eklenen yeni birleştirilmiş veri akışını sonuçta her karşılık gelen bir kaynaktan gelen özgün konuma kaldıracağız:

![Birleşim çıkış](media/data-flow/unionoutput.png "birleşim çıkış")

## <a name="next-steps"></a>Sonraki adımlar

Benzer dönüştürmeler de dahil olmak üzere keşfedin [katılın](data-flow-join.md) ve [EXISTS](data-flow-exists.md).
