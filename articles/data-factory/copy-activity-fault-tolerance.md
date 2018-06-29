---
title: Hataya dayanıklılık kopya etkinliği Azure Data factory'de | Microsoft Docs
description: Kopya etkinliği Azure Data Factory'de uyumsuz satırları atlayarak için hataya dayanıklılık ekleme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: jingwang
ms.openlocfilehash: 6c76820b39f31d92362295d54984069393fa0dec
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059115"
---
#  <a name="fault-tolerance-of-copy-activity-in-azure-data-factory"></a>Kopya etkinliği Azure Data factory'de hataya dayanıklılık
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-copy-activity-fault-tolerance.md)
> * [Geçerli sürüm](copy-activity-fault-tolerance.md)

Azure Data Factory kopyalama etkinliğinde uyumsuz satırları kaynak ve havuz veri depoları arasında veri kopyalama işlemi sırasında işleme için iki yol sunar:

- İptal etmek ve kopyalama başarısız uyumsuz veriler olduğunda etkinliği karşılaştı (varsayılan davranıştır).
- Tüm verileri hata toleransı ekleme ve uyumsuz veri satırları atlanıyor kopyalama devam edebilirsiniz. Ayrıca, Azure Blob storage veya Azure Data Lake Store uyumsuz satırları oturum açabilir. Ardından, hatanın nedenini öğrenmek, veri kaynağındaki verileri düzeltin ve kopyalama etkinliği yeniden deneme için günlüğünü de inceleyebilirsiniz.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Kopyalama etkinliği algılama, atlanıyor ve uyumsuz verilerini günlüğe kaydetme için üç senaryoları destekler:

- **Kaynak veri türü ve havuz yerel türü arasındaki uyumsuzluk**. 

    Örneğin: veri kopyalama bir Blob depolamada CSV dosyasından üç INT türü sütunları içeren bir şema tanımı olan bir SQL veritabanına. 123,456,789 gibi sayısal veriler içeren CSV dosyası satırları başarıyla havuz deposuna kopyalanır. Ancak, 123,456 gibi sayısal olmayan değerler içeren satırları abc uyumsuz olarak algılanır ve atlanır.

- **Kaynak ve havuz arasında sütun sayısı uyuşmazlığı**.

    Örneğin: veri kopyalama bir Blob depolamada CSV dosyasından altı sütunları içeren bir şema tanımı olan bir SQL veritabanına. Altı sütunları içeren CSV dosyası satırları başarıyla havuz deposuna kopyalanır. Daha fazla veya az altı sütunları içeren CSV dosyası satırları uyumsuz olarak algılanır ve atlanır.

- **SQL Server/Azure SQL veritabanı/Azure Cosmos DB'ye yazılırken birincil anahtar ihlali**.

    Örneğin: veri kopyalama SQL Server'dan SQL veritabanına. Havuz SQL veritabanında tanımlı bir birincil anahtara, ancak böyle bir birincil anahtar kaynak SQL Server'da tanımlanmadı. Havuz için kaynak olarak mevcut yinelenen satırları kopyalanamaz. Kopyalama etkinliği yalnızca ilk satır kaynak verilerin havuz kopyalar. Yinelenen birincil anahtar değeri içeren sonraki kaynak satırları uyumsuz olarak algılanır ve atlanır.

>[!NOTE]
>Kopyalama etkinliği, dış veri mekanizması dahil olmak üzere yükleme çağırmak için yapılandırıldığında, bu özelliği uygulanmaz [Azure SQL veri ambarı PolyBase](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) veya [Amazon Redshift Unload](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift). PolyBase'nın yerel hataya dayanıklılık destek belirterek kullanmak verileri PolyBase kullanarak SQL Data Warehouse'a veri yüklemek için "[polyBaseSettings](connector-azure-sql-data-warehouse.md#azure-sql-data-warehouse-as-sink)" kopyalama etkinliğinde.

## <a name="configuration"></a>Yapılandırma
Aşağıdaki örnek kopyalama etkinliği uyumsuz satırları atlanıyor yapılandırmak için JSON tanımını sağlar:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },
    "enableSkipIncompatibleRow": true,
    "redirectIncompatibleRowSettings": {
         "linkedServiceName": {
              "referenceName": "<Azure Storage or Data Lake Store linked service>",
              "type": "LinkedServiceReference"
            },
            "path": "redirectcontainer/erroroutput"
     }
}
```

Özellik | Açıklama | İzin verilen değerler | Gerekli
-------- | ----------- | -------------- | -------- 
enableSkipIncompatibleRow | Veya kopyalama sırasında uyumsuz satırları atlanacak belirtir. | True<br/>False (varsayılan) | Hayır
redirectIncompatibleRowSettings | Zaman uyumsuz satırları günlüğe kaydetmek istediğiniz bir grup olabilir özellik belirtilmiş. | &nbsp; | Hayır
linkedServiceName | Bağlı hizmetin adı [Azure Storage](connector-azure-blob-storage.md#linked-service-properties) veya [Azure Data Lake Store](connector-azure-data-lake-store.md#linked-service-properties) Atlanan satırları içeren günlüğü depolamak için. | Adı bir `AzureStorage` veya `AzureDataLakeStore` günlük dosyasını depolamak için kullanmak istediğiniz örneğine başvurur bağlantılı hizmet türü. | Hayır
yol | Atlanan satır içeren bir günlük dosyası yolu. | Uyumsuz verileri günlüğe kaydetmek için kullanmak istediğiniz yolu belirtin. Bir yol belirtmezseniz, hizmet bir kapsayıcı oluşturur. | Hayır

## <a name="monitor-skipped-rows"></a>Atlanan satır izleme
Çalıştırma kopyalama etkinliği tamamlandıktan sonra kopyalama etkinliği çıktıda Atlanan satır sayısını görebilirsiniz:

```json
"output": {
            "dataRead": 95,
            "dataWritten": 186,
            "rowsCopied": 9,
            "rowsSkipped": 2,
            "copyDuration": 16,
            "throughput": 0.01,
            "redirectRowPath": "https://myblobstorage.blob.core.windows.net//myfolder/a84bf8d4-233f-4216-8cb5-45962831cd1b/",
            "errors": []
        },

```
Uyumsuz satırları oturum yapılandırırsanız, günlük dosyası bu yolunda bulabilirsiniz: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv`. 

Günlük dosyaları, yalnızca csv dosyaları olabilir. Geçiliyor özgün veriler virgülle sütun sınırlayıcısı gerekirse günlüğe kaydedilir. İki daha fazla sütun "ErrorCode" ve "ErrorMessage" ek olarak özgün kaynak verilere günlük dosyası kök görebileceğiniz eklediğimiz uyumsuzluk nedeni. ErrorCode ve ErrorMessage çift tırnak içine quoted. 

Günlük dosyası içeriğine bir örnek aşağıdaki gibidir:

```
data1, data2, data3, "UserErrorInvalidDataValue", "Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'."
data4, data5, data6, "2627", "Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4)."
```

## <a name="next-steps"></a>Sonraki adımlar
Bir kopyalama etkinliği makalelere bakın:

- [Kopya etkinliği genel bakış](copy-activity-overview.md)
- [Kopya etkinliği performansının](copy-activity-performance.md)


