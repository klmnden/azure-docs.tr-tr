---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 24bb7a1fcb1569922fb34034fb3c0d003cdd7061
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66147249"
---
## <a name="repeatability-during-copy"></a>Kopyalama sırasında yinelenebilirliği
Diğer verilerin Azure SQL/SQL Server için veri kopyalamayı depoladığında bir yinelenebilirliği istenmeyen sonuçlar önlemek için akılda tutulması gerekir. 

Azure SQL/SQL Server veritabanına veri kopyalama, kopyalama etkinliği varsayılan ekleme havuz tablosu veri kümesi varsayılan olarak çalışır. Örneğin, verileri Azure SQL/SQL Server veritabanına iki kayıtlarını içeren bir CSV (virgülle ayrılmış değerler veri kaynağından) dosya kopyalarken, bu tablonun benzer.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Kaynak dosyada hatalar bulundu ve aşağı boru, kaynak dosyadaki 4 miktarı 2 güncelleştirilmiş varsayalım. Veri dilimi belirli bir döneme ait yeniden çalıştırırsanız, Azure SQL/SQL Server veritabanına eklenen iki yeni kayıt bulabilirsiniz. Aşağıdaki tablodaki sütunların hiçbirinin birincil anahtar kısıtlaması varsayar.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Bunu önlemek için aşağıdakilerden birini yararlanarak UPSERT semantiği belirtin gerekecektir aşağıda belirtilen 2 mekanizmaları aşağıda.

> [!NOTE]
> Bir dilim otomatik olarak Azure Data Factory'de belirtilen yeniden deneme ilkesi uyarınca yeniden çalıştırılabilir.
> 
> 

### <a name="mechanism-1"></a>Mechanism 1
Yararlanabileceğiniz **sqlWriterCleanupScript** dilim çalıştırdığınızda, ilk temizleme eylemi gerçekleştirmek için özellik. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Temizleme betiği yürütülen bu dilime karşılık gelen SQL tablosundan verileri siler, belirli bir dilim için kopyalama sırasında ilk. Etkinlik, veri sonradan SQL tablosuna ekler. 

Dilimi yeniden çalıştırın ve ardından, miktar olarak güncelleştirilir bulur şimdi ise istediğiniz.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Düz rondela kaydı özgün csv dosyasından kaldırılır varsayalım. Ardından dilimi yeniden çalıştırmak aşağıdaki sonucu verir: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
Yeni bir şey yapmanız gerekiyordu. Kopyalama etkinliği, dilim için karşılık gelen verileri silmek için temizleme betiği çalıştırdınız. Giriş (Bu, daha sonra yer alan yalnızca 1 kaydı) bir csv dosyasından okumak sonra ve tabloya eklenecek. 

### <a name="mechanism-2"></a>Mekanizması 2
> [!IMPORTANT]
> Sliceıdentifiercolumnname şu anda Azure SQL veri ambarı için desteklenmiyor. 

Yinelenebilirliği sağlamak için başka bir mekanizma tarafından atanmış bir sütun yaşanmaktadır (**Sliceıdentifiercolumnname**) ' % s'hedef tablosu. Azure Data Factory tarafından bu sütun kaynak ve hedef eşitlenmiş kalmasını sağlamak için kullanılır. Bu yaklaşım, değiştirme veya hedef SQL tablo şemasını tanımlama esneklik olduğunda çalışır. 

Bu sütun yinelenebilirliği amacıyla Azure Data Factory tarafından kullanılacak ve işlemde Azure Data Factory herhangi bir şema değişikliği tablosuna olmasını sağlamaz. Bu yaklaşımı kullanmak için yolu:

1. Bir sütun türü ikili (32) hedef SQL tablosunu tanımlayın. Bu sütunda hiçbir kısıtlama olmalıdır. Şimdi bu sütun, bu örnek için 'ColumnForADFuseOnly' adlandırın.
2. Kopyalama etkinliği şu şekilde kullanın:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

Azure Data Factory bu sütun kaynak ve hedef eşitlenmiş kalmasını sağlamak için gereksinime doldurur. Bu sütundaki değerleri kullanıcı tarafından bu bağlamı dışında kullanılmamalıdır. 

Benzer şekilde mekanizması 1, kopyalama etkinliği otomatik olarak ayarlanır önce verilen dilimin hedef SQL tablosu veri temizleyin ve ardından normal veri kaynağından o dilim için hedef eklemek için kopyalama etkinliği çalıştırın. 

