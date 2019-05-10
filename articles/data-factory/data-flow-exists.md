---
title: Azure Data Factory eşleştirme veri akış var. dönüştürme
description: EXISTS dönüştürme ile veri fabrikası eşleme verileri kullanarak var olan satırları denetlemek nasıl akar
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: b98b7afb21f2f50d44ba93ed793b6efb20f75164
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235967"
---
# <a name="mapping-data-flow-exists-transformation"></a>Dönüşüm veri akışı eşleme var.

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

EXISTS dönüşümü durdurur veya veri akışına satır sağlayan dönüştürme filtreleme bir satırdır. Mevcut dönüştürme benzer ```SQL WHERE EXISTS``` ve ```SQL WHERE NOT EXISTS```. Mevcut dönüştürme sonra ortaya çıkan satırlar, veri akışından ya da sütun değerleri kaynağından 1, 2 kaynağında bulunduğu tüm satırları içerecek veya 2 kaynağında yok.

![Ayarları var](media/data-flow/exists.png "1 var.")

Veri akışı Stream 2 karşı Stream 1 değerlerinden karşılaştırabilmek için mevcut ikinci kaynağı seçin.

Kaynak 1'den ve kaynak değerleri EXISTS veya yok denetlemek istediğiniz 2 sütun seçin.

## <a name="multiple-exists-conditions"></a>Birden çok koşul var.

Sütun koşullarınız Exists için her satırının yanındaki, bulabilirsiniz bir + oturum kullanılabilir üzerine geldiğinizde satır ulaşın. Bu, mevcut koşulları için birden çok satır eklemek olanak tanır. Her ek koşul bir "Ve".

## <a name="custom-expression"></a>Özel ifade

![Özel ayarlar var](media/data-flow/exists1.png "özel var.")

Bunun yerine bir serbest biçimli ifadesi olarak oluşturmak için "özel ifadesi"'a tıklayın, var veya koşul değil-mevcut. Bu kutunun işaretlenmesi, kendi ifade bir koşul olarak yazmak izin verir.

## <a name="next-steps"></a>Sonraki adımlar

Benzer dönüşümleri olan [arama](data-flow-lookup.md) ve [katılın](data-flow-join.md).
