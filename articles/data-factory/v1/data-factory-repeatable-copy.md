---
title: "Azure Data Factory yinelenebilir kopyasında | Microsoft Docs"
description: "Yinelenen verileri kopyalayan bir dilim birden çok kez çalıştırma olsa bile kaçının öğrenin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 0519a9c60386f8fea0047a661e48f008d3141c5e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="repeatable-copy-in-azure-data-factory"></a>Azure Data Factory yinelenebilir kopyalama

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan yinelenebilir okuma
İlişkisel veri kopyalama verileri depoladığında, Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırmak kaç kez geçtiğinden bağımsız okuduğunuzdan emin olmanız gerekir.  
 
> [!NOTE]
> Aşağıdaki örnekler için Azure SQL ancak dikdörtgen veri kümeleri destekleyen herhangi bir veri deposuna uygulanabilir. Ayarlamanız gerekebilir **türü** kaynağının ve **sorgu** özelliği (örneğin: Sorgu sqlReaderQuery yerine) verilerini depolamak.   

Genellikle, ilişkisel depoları okurken, yalnızca o dilim karşılık gelen verileri okumak istediğiniz. Bunu yapmanın bir yolu, Azure Data Factory'de kullanılabilir WindowStart ve WindowEnd sistem değişkenleri kullanılarak olacaktır. Azure Data Factory'de burada işlevleri ve değişkenler hakkında bilgi [Azure Data Factory - işlevler ve sistem değişkenleri](data-factory-functions-variables.md) makalesi. Örnek: 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
Bu sorgu MyTable tablosundan (WindowStart -> WindowEnd) dilim süresi aralığını kalan verileri okur. Bu dilimin yeniden çalıştırın, ayrıca her zaman aynı veri okuduğunuzdan emin olun. 

Diğer durumlarda, tüm tablo bölümünü okumanız önerilir ve sqlReaderQuery gibi tanımlayın:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-to-sqlsink"></a>SqlSink yinelenebilir yazma
Veri kopyalama işlemi sırasında **Azure SQL/SQL Server** diğer veri depolarına, Yinelenebilirlik istenmeyen sonuçları önlemek için göz önünde bulundurmanız gerekir. 

Azure SQL/SQL Server veritabanına veri kopyalama, kopyalama etkinliği verileri varsayılan olarak havuz tabloya ekler. Bir Azure SQL/SQL Server veritabanında aşağıdaki tabloya iki kayıtlarını içeren bir CSV (virgülle ayrılmış değerler) dosyasından veri kopyalama söyleyin. Bir dilim çalıştığında, iki kayıt SQL tablosuna kopyalanır. 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Kaynak dosyasında hata buldu ve miktarını aşağı boru 2-4 güncelleştirilmiş varsayalım. Veri dilimi belirli bir döneme ait el ile yeniden, Azure SQL/SQL Server veritabanına eklenen iki yeni kayıtlar bulabilirsiniz. Bu örnek, tablodaki sütunların hiçbiri birincil anahtar kısıtlaması olduğunu varsayar.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Bu davranışı önlemek için aşağıdaki iki mekanizma birini kullanarak UPSERT semantiği belirtmeniz gerekir:

### <a name="mechanism-1-using-sqlwritercleanupscript"></a>Mekanizması 1: sqlWriterCleanupScript kullanma
Kullanabileceğiniz **sqlWriterCleanupScript** bir dilim çalıştırıldığında verileri eklemeden önce havuz tablodaki verileri temizlemek için özellik. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Bir dilim çalıştığında, temizleme betiğini dilimi SQL tablosundan karşılık gelen verileri silmek için önce çalıştırılır. Kopyalama etkinliği, ardından veri SQL tablosuna ekler. Dilimi yeniden çalıştırın, miktarı güncelleştirilir istenen şekilde.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Düz rondela kaydı özgün csv kaldırılana varsayalım. Dilim yeniden çalıştırma aşağıdaki sonucu oluşturur: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

Kopyalama etkinliği, dilim karşılık gelen verileri silmek için temizleme betiğini verdi. Giriş (hangi sonra yalnızca bir kayıt bulunan) csv okuma sonra ve tabloya eklenen. 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a>Mekanizması 2: Sliceıdentifiercolumnname kullanma
> [!IMPORTANT]
> Şu anda Sliceıdentifiercolumnname Azure SQL veri ambarı için desteklenmiyor. 

Yinelenebilirlik elde etmek için ikinci hedef tablo ayrılmış sütun (Sliceıdentifiercolumnname) sağlayarak mekanizmadır. Azure Data Factory tarafından bu sütun kaynak ve hedef eşitlenmesine emin olmak için kullanılır. Bu yaklaşım, değiştirme veya hedef SQL tablo şemasını tanımlama esneklik olduğunda çalışır. 

Bu sütun Yinelenebilirlik amaçlar için Azure Data Factory tarafından kullanılır ve işlem sırasında tablonun herhangi bir şema değişikliği Azure Data Factory yapmaz. Bu yaklaşımı kullanmak için yol:

1. Türünde bir sütun tanımlamak **ikili (32)** hedef SQL tablosu. Bu sütunda hiç bir kısıtlama olması gerekir. Şimdi bu sütun, bu örnek için AdfSliceIdentifier adlandırın.


    Kaynak tablosu:

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL
    )
    ```

    Hedef Tablo: 

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL,
       [AdfSliceIdentifier] [binary](32) NULL
    )
    ```

2. Kopyalama etkinliği şu şekilde kullanın:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

Azure Data Factory bu sütun kaynak ve hedef eşitlenmesine emin olmak için gerek göredir doldurur. Bu sütundaki değerleri bu bağlamı dışında kullanılmamalıdır. 

Benzer şekilde mekanizması 1, kopyalama etkinliği otomatik olarak verilen dilim hedef SQL tablosu için verileri temizler. Ardından veri kaynağından hedef tabloya ekler. 

## <a name="next-steps"></a>Sonraki adımlar
Tam JSON örnekler için makaleler aşağıdaki Bağlayıcısı'nı gözden geçirin: 

- [Azure SQL Veritabanı](data-factory-azure-sql-connector.md)
- [Azure SQL Veri Ambarı](data-factory-azure-sql-data-warehouse-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)