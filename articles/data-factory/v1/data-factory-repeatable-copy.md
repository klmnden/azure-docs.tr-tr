---
title: Azure Data factory'de tekrarlanabilir kopyalama | Microsoft Docs
description: Veri kopyalayan bir dilim birden çok kez çalıştırmak olsa da, yinelenenleri önlemek öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 20c916275acd6bb79675c592711b17b277c9fc78
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60605208"
---
# <a name="repeatable-copy-in-azure-data-factory"></a>Azure Data factory'de tekrarlanabilir kopyalama

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri kopyalama verileri depoladığında yinelenebilirliği istenmeyen sonuçlar önlemek için göz önünde bulundurun. Azure Data Factory'de bir dilim el ile çalıştırabilirsiniz. Bir hata oluştuğunda bir dilimi yeniden çalıştırmak için bir veri kümesi için yeniden deneme ilkesi de yapılandırabilirsiniz. Bir dilim her iki yolla yeniden çalıştırıldığında, aynı veri dilimi çalıştırılan kaç kez olursa olsun okuma emin olmanız gerekir.  
 
> [!NOTE]
> Aşağıdaki örnekler için Azure SQL ancak dikdörtgen veri kümeleri destekleyen herhangi bir veri deposuna uygulanabilir. Ayarlamanız gerekebilir **türü** kaynağının ve **sorgu** özelliği (örneğin: Sorgu sqlReaderQuery yerine) depoladığınız veriler için.   

Genellikle, ilişkisel mağazalardan okurken, yalnızca o dilime karşılık gelen verileri okumak istediğiniz. Bunu yapmanın bir yolu, Azure Data Factory'de kullanılabilir WindowStart ve WindowEnd sistem değişkenlerini kullanarak olacaktır. Azure Data Factory'de burada işlevler ve değişkenler hakkında bilgi edinin [Azure Data Factory - işlevler ve sistem değişkenleri](data-factory-functions-variables.md) makalesi. Örnek: 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
Bu sorgu MyTable tablosundan (WindowStart WindowEnd ->) dilim süresi aralığı giren veri okur. Bu diliminin yeniden de her zaman aynı veri okunur sağlar. 

Diğer durumlarda, tablonun tamamını okumanız önerilir ve sqlReaderQuery gibi tanımlayın:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-to-sqlsink"></a>SqlSink tekrarlanabilir yazma
Veri kopyalama işlemi sırasında **Azure SQL/SQL Server** diğer veri depolarından istenmeyen sonuçları önlemenize yinelenebilirliği sağlamak gerekir. 

Azure SQL/SQL Server veritabanına veri kopyalama, kopyalama etkinliği havuz tabloya verileri varsayılan olarak ekler. Azure SQL/SQL Server veritabanı aşağıdaki tabloda iki kayıt içeren bir CSV (virgülle ayrılmış değerler) dosyasından veri kopyalama varsayalım. Bir dilim çalıştığında, iki kayıt SQL tablosuna kopyalanır. 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Kaynak dosyada hatalar bulundu ve miktarını aşağı boru 2 4 güncelleştirilmiş varsayalım. Veri dilimi bu dönem için el ile yeniden, Azure SQL/SQL Server veritabanına eklenen iki yeni kayıt bulabilirsiniz. Bu örnek, hiçbir sütun birincil anahtar kısıtlaması olduğunu varsayar.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Bu davranışı önlemek için aşağıdaki iki mekanizma kullanarak UPSERT semantiği belirtmeniz gerekir:

### <a name="mechanism-1-using-sqlwritercleanupscript"></a>Mekanizması 1: sqlWriterCleanupScript kullanma
Kullanabileceğiniz **sqlWriterCleanupScript** dilim çalıştırıldığında, verileri eklemeden önce havuz tablodaki verileri temizlemek için özellik. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Bir dilim çalıştığında, temizleme betiğini dilimi SQL tablosundan karşılık gelen verileri silmek için önce çalıştırılır. Kopyalama etkinliği, ardından veri SQL tablosuna ekler. Dilimi yeniden çalıştırmak yoksa miktarı güncelleştirilmiştir istenen şekilde.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Düz rondela kaydı özgün csv dosyasından kaldırılır varsayalım. Dilim artırarak algoritmanın yeniden çalıştırılması aşağıdaki sonucu verir: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

Kopyalama etkinliği, dilim için karşılık gelen verileri silmek için temizleme betiği çalıştırdınız. Giriş (hangi sonra yalnızca tek bir kayıtta yer alan) bir csv dosyasından okumak sonra ve tabloya eklenecek. 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a>Mekanizması 2: Sliceıdentifiercolumnname kullanma
> [!IMPORTANT]
> Şu anda Sliceıdentifiercolumnname Azure SQL veri ambarı için desteklenmiyor. 

Yinelenebilirliği sağlamak için ikinci tablo hedef adanmış bir sütun (Sliceıdentifiercolumnname) sağlayarak mekanizmadır. Azure Data Factory tarafından bu sütun kaynak ve hedef eşitlenmiş kalmasını sağlamak için kullanılır. Bu yaklaşım, değiştirme veya hedef SQL tablo şemasını tanımlama esneklik olduğunda çalışır. 

Bu sütun yinelenebilirliği amacıyla Azure Data Factory tarafından kullanılır ve işlem sırasında tablonun herhangi bir şema değişikliği Azure Data Factory yapmaz. Bu yaklaşımı kullanmak için yolu:

1. Türünde bir sütun tanımlayın **ikili (32)** hedef SQL tablosu. Bu sütunda hiçbir kısıtlama olmalıdır. Şimdi bu sütun, bu örnekte AdfSliceIdentifier adlandırın.


    Kaynak Tablo:

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

1. Kopyalama etkinliği şu şekilde kullanın:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

Azure Data Factory, bu sütun kaynak ve hedef eşitlenmiş kalmasını sağlamak için gereksinime doldurur. Bu sütundaki değerleri bu bağlamı dışında kullanılmamalıdır. 

Benzer şekilde mekanizması 1, kopyalama etkinliği otomatik olarak verilen dilimin hedef SQL tablosu veri temizler. Ardından veri kaynağından hedef tabloya ekler. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki Bağlayıcısı'nı gözden geçirme için tam JSON örnekler belirten makaleleri: 

- [Azure SQL Veritabanı](data-factory-azure-sql-connector.md)
- [Azure SQL Veri Ambarı](data-factory-azure-sql-data-warehouse-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
