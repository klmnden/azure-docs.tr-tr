---
title: Azure veri fabrikası eşleme veri akışını dönüştürme Çöz
description: Azure veri fabrikası eşleme veri akışını dönüştürme Çöz
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 14326714fc8258e184024edb83666d3ed0c3eee7
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272140"
---
# <a name="azure-data-factory-mapping-data-flow-unpivot-transformation"></a>Azure veri fabrikası eşleme veri akışını dönüştürme Çöz

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Unpıvot ADF eşleme veri akışı tek bir kayıt içindeki birden fazla sütundaki değerleri tek bir sütunda aynı değerlerle birden fazla kayda genişleterek daha normalleştirilmiş bir sürümüne Normalleştirilmemiş bir veri kümesini açmak için bir yol kullanın.

![Dönüştürme Çöz](media/data-flow/unpivot1.png "Çöz seçenekleri 1")

## <a name="ungroup-by"></a>Tarafından grubunu Çöz

![Dönüştürme Çöz](media/data-flow/unpivot5.png "Çöz seçenekleri 2")

İlk olarak, pivot toplama için istediğiniz Gruplanacak sütunları ayarlayın. İle çözme için bir veya daha fazla sütun kümesi + sütun listesi yanındaki işareti.

## <a name="unpivot-key"></a>Anahtar Çöz

![Dönüştürme Çöz](media/data-flow/unpivot6.png "seçenekleri 3 Çöz")

ADF satırdan sütuna Özet sütun Pivot anahtardır. Varsayılan olarak, bu alan için veri kümesindeki her bir benzersiz değer için bir sütun Özet. Ancak, Özet sütun değerleri istediğiniz veri kümesinden isteğe bağlı olarak değerleri girebilirsiniz.

## <a name="unpivoted-columns"></a>Özet sütunlar çözüldü

![Dönüştürme Çöz](media/data-flow//unpivot7.png "seçenekleri 4 Çöz")

Son olarak, ve yeni çıkış projeksiyon dönüşümü gelen görüntülenecek sütunları yönteminizi özetlenmiş değerler için kullanmak istediğiniz toplama seçin.

(İsteğe bağlı) Bir adlandırma deseni bir önek, Orta ve satır değerleri her yeni sütun adına eklenecek son eki ile ayarlayabilirsiniz.

Örneğin, "Bölge" göre özetleyerek "Sales" yalnızca yeni sütun değerlerini her satış değeri verirsiniz. Örneğin: "25", "50", "1000", ... "Sales" ön eki değeri ayarlarsanız, ancak ardından "Sales" değerleri için önek olarak eklenir.

<img src="media/data-flow/unpivot3.png" width="400">

Sütun düzenini "Normal" ayarını Grup birlikte tüm toplanan değerleriyle özetlenmiş sütun. Sütunları düzenleme için "Yanal" ayarı, sütun ve değer arasında alternatif.

![Dönüştürme Çöz](media/data-flow//unpivot7.png "seçenekleri 5 Çöz")

Ayrı bir satır değerlerine Çözüldü artık sütun toplamları son Çözüldü veri sonuç kümesi gösterir.
