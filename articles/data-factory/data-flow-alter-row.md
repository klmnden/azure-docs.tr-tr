---
title: Azure veri fabrikası veri akışı Alter satır dönüştürme eşlemesi
description: Azure veri fabrikası eşleme veri akışı Alter satır dönüştürme kullanarak veritabanı hedef güncelleştirme
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: d842898ca700490ae99b46140be6609622a144df
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60627120"
---
# <a name="azure-data-factory-alter-row-transformation"></a>Azure veri fabrikası Alter satır dönüştürme

Satır ekleme, silme, güncelleştirme ve upsert ilkeleri ayarlamak için Alter satır dönüştürme kullanın. Bire çok koşul ifadeleri olarak ekleyebilirsiniz. Bu koşulların her bir satırdaki (veya satır) neden eklenen, güncelleştirilen silindi, veya upsert. ALTER satır hem DDL ve DML işlemleri veritabanına karşı üretebilir.

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Satır ayarlarını değiştir](media/data-flow/alter-row1.png "satır ayarlarını değiştirme")

> [!NOTE]
> ALTER satır dönüştürmeleri, yalnızca veritabanı havuzlarını veri akışınıza çalışacağı. Hata ayıklama oturumları sırasında satır (INSERT, update, delete, upsert) atama işlemleri gerçekleşmez. Bir işlem hattı için bir yürütme veri akışı görev ekleyin ve veritabanı tablolarınızı alter satır ilkeleri uygulamak için işlem hattı hata ayıklama veya tetikleyicileri kullanmanız gerekir.

## <a name="view-policies"></a>İlkeleri görüntüle

Veri akışı hata ayıklama modu açık konuma geçin ve sonra veri Önizleme Bölmesi'nde alter satır ilkelerinizi sonuçları görüntüleyin. Alter satır yürütme veri akışı hata ayıklama modunda DDL veya DML eylem hedef karşı oluşturmaz. Bu eylemlerin ortaya sahip olmak için bir yürütme veri akışı etkinliği içinde bir işlem hattı içinde veri akışı yürütün.

![Satır ilkeleri alter](media/data-flow/alter-row3.png "Alter satır ilkeleri")

Bu, doğrulamak ve her satır, koşullara göre durumunu görüntülemek olanak tanır. Simge her eklemeyi temsil eder, bir işlem hattı içinde veri akışını çalıştırdığınızda, hangi eylemini belirten, veri akışı gerçekleşen güncelleştirme, silme ve upsert eylem gerçekleşir.

## <a name="sink-settings"></a>Havuz ayarları

Havuz türü Alter çalışmak satır için bir veritabanı olmalıdır. Havuz ayarları, her bir eylemin izin verilmesi için ayarlamanız gerekir.

![Satır havuz alter](media/data-flow/alter-row2.png "Alter satır havuz")

ADF veri akışı'de varsayılan davranışı ile veritabanı havuzlarını satırları eklemektir. Güncelleştirme, upsert eder ve silme de izin vermek istiyorsanız, bu eylemleri izin vermek için havuz kutularında de işaretlemeniz gerekir.

> [!NOTE]
> Ekler, güncelleştirmeleri veya upsert eder havuzunda hedef tablodaki şema değiştirirseniz, veri akışınız başarısız olur. Veritabanınızda hedef şemayı değiştirmek için havuza "Tabloyu yeniden oluşturun" seçeneğini tercih etmelisiniz. Bırakın ve yeni şema tanımıyla, tabloyu yeniden oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Alter satır dönüştürme işleminin ardından isteyebileceğiniz [verilerinizi bir hedef veri deposuna havuz](data-flow-sink.md).
