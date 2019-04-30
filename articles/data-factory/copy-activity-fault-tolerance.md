---
title: Hataya dayanıklılık, Azure veri fabrikasında kopyalama etkinliği | Microsoft Docs
description: Uyumsuz satırları atlayarak Azure veri fabrikasında kopyalama etkinliği için hataya dayanıklılık ekleme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: yexu
ms.openlocfilehash: ef0bb3716a32a0f25b90e74bc44d7291c146b431
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60808822"
---
#  <a name="fault-tolerance-of-copy-activity-in-azure-data-factory"></a>Azure Data Factory’de kopyalama etkinliğinin hataya dayanıklılığı
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-copy-activity-fault-tolerance.md)
> * [Geçerli sürüm](copy-activity-fault-tolerance.md)

Azure Data Factory kopyalama etkinliği kaynak ve havuz veri deposu arasında veri kopyalama sırasında uyumsuz satırların işlemek için iki yol sunar:

- İptal ve kopyalama başarısız uyumsuz veriler olduğunda etkinliği (varsayılan davranış) karşılaştı.
- Hataya dayanıklılık ekleme ve uyumlu veri satırları tüm verileri kopyalamak devam edebilirsiniz. Ayrıca, Azure Blob Depolama veya Azure Data Lake Store uyumsuz satırları oturum açabilirsiniz. Ardından, kopyalama etkinliği yeniden hatanın nedenini öğrenin ve veri kaynağındaki verileri düzeltmek için günlüğünü inceleyebilirsiniz.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Kopyalama etkinliği algılama, atlanıyor ve uyumsuz verilerini günlüğe kaydetmek için üç senaryoları destekler:

- **Kaynak veri türü ve havuz yerel türü arasındaki uyumsuzluk**. 

    Örneğin: Bir SQL veritabanına üç INT türü sütunlar içeren bir şema tanımı ile Blob Depolama alanında bir CSV dosyasından verileri kopyalayın. CSV dosyası satırları 123,456,789 gibi sayısal veri içermesi için havuz deposu başarıyla kopyalanır. Ancak, 123,456 gibi sayısal olmayan değerleri içeren satırları abc uyumsuz olarak algılanır ve atlanır.

- **Kaynak ve havuz arasında sütun sayısında uyuşmazlık**.

    Örneğin: Bir SQL veritabanına altı sütunları içeren bir şema tanımı ile Blob Depolama alanında bir CSV dosyasından verileri kopyalayın. Altı sütunları içeren CSV dosyası satırları başarıyla için havuz deposu olarak kopyalanır. Daha fazla veya altıdan az sütun içeren CSV dosyası satırları, uyumsuz olarak algılanır ve atlanır.

- **SQL Server/Azure SQL veritabanı/Azure Cosmos DB'ye yazarken birincil anahtar ihlali**.

    Örneğin: Verileri SQL Server'dan SQL veritabanı'na kopyalayın. Havuz SQL veritabanı'nda birincil anahtar tanımlı, ancak böyle bir birincil anahtar kaynak SQL server tanımlanır. Havuz için kaynak olarak mevcut yinelenen satırları kopyalanamıyor. Kopyalama etkinliği, kaynak verilerin yalnızca ilk satır havuz kopyalar. Yinelenen birincil anahtar değeri içeren sonraki kaynak satırları, uyumsuz olarak algılanır ve atlanır.

>[!NOTE]
>- Verileri SQL Data Warehouse'a veri yüklemek için PolyBase kullanarak PolyBase'nın yerel hata dayanıklılık ayarlarını yapılandırmak reddedin ilkeleri aracılığıyla belirterek "[polyBaseSettings](connector-azure-sql-data-warehouse.md#azure-sql-data-warehouse-as-sink)" kopyalama etkinliğindeki. Yeniden yönlendirme PolyBase uyumsuz satırların Blob veya ADLS aşağıda gösterildiği gibi normal olarak yine de etkinleştirebilirsiniz.
>- Kopyalama etkinliği çağırmak için yapılandırıldığında bu özellik uygulanmaz [Amazon Redshift kaldırma](connector-amazon-redshift.md#use-unload-to-copy-data-from-amazon-redshift).


## <a name="configuration"></a>Yapılandırma
Aşağıdaki örnek, kopyalama etkinliğinde uyumsuz satırları yapılandırmak için bir JSON tanımı sağlar:

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
Enableskipıncompatiblerow | Veya kopyalama sırasında uyumsuz satırların atlanmayacağını belirtir. | True<br/>False (varsayılan) | Hayır
Redirectıncompatiblerowsettings | Zaman uyumsuz satırları günlüğe kaydetmek istediğiniz bir grup olabilir özellik belirtildi. | &nbsp; | Hayır
linkedServiceName | Bağlı hizmetin adı [Azure depolama](connector-azure-blob-storage.md#linked-service-properties) veya [Azure Data Lake Store](connector-azure-data-lake-store.md#linked-service-properties) Atlanan satır içeren günlüğü depolamak için. | Adı bir `AzureStorage` veya `AzureDataLakeStore` günlük dosyasını depolamak için kullanmak istediğiniz örneğine başvurur bağlantılı hizmet türü. | Hayır
path | Atlanan satır içeren bir günlük dosyası yolu. | Uyumsuz verilerini günlüğe kaydetmek için kullanmak istediğiniz yolu belirtin. Bir yol belirtmezseniz, hizmet sizin için bir kapsayıcı oluşturur. | Hayır

## <a name="monitor-skipped-rows"></a>Atlanan satır izleyin
Kopyalama etkinliği çalıştırmasının tamamlandıktan sonra kopyalama etkinliği çıkışında Atlanan satır sayısını görebilirsiniz:

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
Uyumsuz satırları oturum yapılandırırsanız, günlük dosyası bu yolda bulabilirsiniz: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv`. 

Günlük dosyaları, yalnızca csv dosyalarından yüklenebilir. İki tanesinden özgün veriler virgülle sütun sınırlayıcısı gerekirse günlüğe kaydedilir. "Hata kodu" ve "ErrorMessage" ek olarak iki daha fazla sütun günlük dosyası, özgün kaynak verilerde kök görebileceğiniz eklediğimiz uyumsuzluğun nedenini. Hata kodu ve ErrorMessage çift tırnak işareti tırnak içine alınmaz. 

Günlük dosyası içerikleri örneği aşağıdaki gibidir:

```
data1, data2, data3, "UserErrorInvalidDataValue", "Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'."
data4, data5, data6, "2627", "Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4)."
```

## <a name="next-steps"></a>Sonraki adımlar
Bir kopyalama etkinliği makalelere bakın:

- [Kopyalama etkinliği'ne genel bakış](copy-activity-overview.md)
- [Kopyalama etkinliği performansı](copy-activity-performance.md)


