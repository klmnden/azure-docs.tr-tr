---
title: Azure veri fabrikası veri akışı kaynak dönüştürme eşlemesi
description: Azure veri fabrikası veri akışı kaynak dönüştürme eşlemesi
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 35f4e794caf84aba860b98e68eadcdcd88e77952
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272182"
---
# <a name="azure-data-factory-mapping-data-flow-source-transformation"></a>Azure veri fabrikası veri akışı kaynak dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Kaynak dönüşümü, veri akışınız verileri getirmek için kullanmak istediğiniz bir veri kaynağı yapılandırır. Tek bir akış veri dönüştürme 1'den fazla kaynak olabilir. Her zaman kaynağı ile veri akışları tasarlama başlar.

> [!NOTE]
> Her veri akışı en az bir kaynak dönüştürme gerektiriyor. Gereksinim duyduğunuz kadar çok ek kaynaklar ekleme

![Dönüştürme seçenekleri kaynak](media/data-flow/source.png "kaynak")

Veri akışı kaynak verilerinizi okuma veya yazma konumunu ve şekli tanımlar tam olarak bir ADF Dataset ilişkilendirilmiş olması gerekir.

## <a name="data-flow-staging-areas"></a>Hazırlama alanları veri akışı

ADF veri akışı veri Bağlantılarınızdaki gerçekleştirmek için Azure içindeki 5 birincil "Hazırlama" alanları için satır görüş sahiptir: Azure Blob, ADLS Gen 1, ADLS Gen 2, Azure SQL veritabanı ve Azure SQL DW. ADF yaklaşık 80 farklı yerel bağlayıcıları erişebilir, bu nedenle bu diğer veri kaynakları, veri akışı dahil etmek için önce bu verileri bu beş birincil veri akışı hazırlama alanların birini ilk kopyalama etkinliği'ni kullanarak aşama:

## <a name="options"></a>Seçenekler

### <a name="allow-schema-drift"></a>Şema değişikliklerini izin ver
Kaynak sütunları genellikle değiştirirseniz, şema değişikliklerini izin seçin. Bu ayar, kaynak havuzuna dönüştürmeleri akışına gelen tüm alanları izin verir.

### <a name="fail-if-columns-in-the-dataset-are-not-found"></a>Veri kümesindeki sütunları bulunamazsa başarısız
Kaynağınızdan beklenen sütun mevcut değilse, veri akışı başarısız olur bir kaynak şema doğrulaması zorlamak için bu seçeneği belirleyin.

### <a name="sampling"></a>Örnekleme
Örnekleme kaynağınızdan alınan satır sayısını sınırlamak için kullanın.  Bu, test ve hata ayıklama için yalnızca bir örnek, kaynak verilerin gerektiğinde kullanışlıdır.

### <a name="define-schema"></a>Şema tanımlayın

![Dönüştürme kaynağı](media/data-flow/source2.png "kaynak 2")

### <a name="you-can-modify-the-name-of-the-source-columns-and-their-associated-data-types"></a>Kaynak sütunlar ve ilişkili veri türlerini adını değiştirebilirsiniz.

Kesin türü belirtilmiş (yani düz dosyalar Parquet dosyalarını aksine) olmayan bir kaynak dosya türleri için veri kümesi olarak kaynak dönüşümünde burada her bir alan için veri türlerini tanımlamanız gerekir.

Sütun adlarını ve türlerini akış veri görmüyorsanız, bunları havuzu tanımlayan bir şema bölümünde tanımlamadığı için bu olasıdır. Veri akışı'nın şema değişikliklerini işleme kullanıyorsanız bunu yapmanız gerekecektir.

"Tanımlamak şemada" kaynak dönüşümü sekmesinde veri türlerini ve biçimlerini ayarlayabildiğiniz şu şekildedir:

![Dönüştürme kaynağı](media/data-flow/source003.png "veri türleri")

### <a name="optimize"></a>İyileştirme

![Kaynak bölümler](media/data-flow/sourcepart.png "bölümleme")

Bir kaynak dönüştürme için İyileştir sekmesinde, "Kaynak" adlı ek bir bölümleme türü görürsünüz. Bu yalnızca açık kaynak Azure SQL DB seçtiğinizde yukarı. ADF bağlantılar, Azure SQL DB kaynağına karşı büyük sorgularını yürütmek için paralel hale getirmek istediğiniz olmasıdır.

SQL DB kaynak verileri bölümleme isteğe bağlıdır, ancak büyük sorgular için yararlıdır. İki seçeneğiniz vardır:

### <a name="column"></a>Sütun

Bir sütun bölüm için kaynak tablosundan seçin. Maksimum bağlantı sayısını de ayarlamanız gerekir.

### <a name="query-condition"></a>Sorgu koşulu

İsteğe bağlı bir sorgu üzerindeki bağlantıları bölümlemek seçebilirsiniz. Bu seçenek için WHERE koşulu içerikte basitçe. I.e. year > 1980
