---
title: Hataya dayanıklılık uyumsuz satırların atlayarak Azure Data Factory kopyalama etkinliği ekleyin. | Microsoft Docs
description: Kopyalama sırasında uyumsuz satırların atlayarak hata toleransı Azure Data Factory kopyalama etkinliği eklemeyi öğrenin
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 3a255b21e8bfd7d78954603e9aa6e5ca39cee95b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60566076"
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a>Hataya dayanıklılık uyumsuz satırların atlayarak kopyalama etkinliği ekleyin.

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](data-factory-copy-activity-fault-tolerance.md)
> * [Sürüm 2 (geçerli sürüm)](../copy-activity-fault-tolerance.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [hataya dayanıklılık kopyalama etkinliği, Data Factory'nin](../copy-activity-fault-tolerance.md).

Azure Data Factory [kopyalama etkinliği](data-factory-data-movement-activities.md) kaynak ve havuz veri deposu arasında veri kopyalama sırasında uyumsuz satırların işlemek için iki yol sunar:

- İptal ve kopyalama başarısız uyumsuz veriler olduğunda etkinliği (varsayılan davranış) karşılaştı.
- Hataya dayanıklılık ekleme ve uyumlu veri satırları tüm verileri kopyalamak devam edebilirsiniz. Ayrıca, Azure Blob Depolama alanında uyumsuz satırları oturum açabilirsiniz. Ardından, kopyalama etkinliği yeniden hatanın nedenini öğrenin ve veri kaynağındaki verileri düzeltmek için günlüğünü inceleyebilirsiniz.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Kopyalama etkinliği algılama, atlanıyor ve uyumsuz verilerini günlüğe kaydetmek için üç senaryoları destekler:

- **Kaynak veri türü ve havuz yerel türü arasındaki uyumsuzluk**

    Örneğin: Bir SQL veritabanına üçünü içeren bir şema tanımı ile Blob Depolama alanında bir CSV dosyasından veri kopyalama **INT** sütunları yazın. Gibi sayısal veriler içeren CSV dosyası satırları `123,456,789` havuz deposu için başarıyla kopyalandı. Ancak, satırları içeren sayısal olmayan değerleri gibi `123,456,abc` uyumsuz algılanır ve atlanır.

- **Kaynak ve havuz arasında sütun sayısında uyuşmazlık**

    Örneğin: Bir SQL veritabanına altı sütunları içeren bir şema tanımı ile Blob Depolama alanında bir CSV dosyasından verileri kopyalayın. Altı sütunları içeren CSV dosyası satırları başarıyla için havuz deposu olarak kopyalanır. Daha fazla veya altıdan az sütun içeren CSV dosyası satırları, uyumsuz olarak algılanır ve atlanır.

- **SQL Server/Azure SQL veritabanı/Azure Cosmos DB'ye yazarken birincil anahtar ihlali**

    Örneğin: Verileri SQL Server'dan SQL veritabanı'na kopyalayın. Havuz SQL veritabanı'nda birincil anahtar tanımlı, ancak böyle bir birincil anahtar kaynak SQL server tanımlanır. Havuz için kaynak olarak mevcut yinelenen satırları kopyalanamıyor. Kopyalama etkinliği, kaynak verilerin yalnızca ilk satır havuz kopyalar. Yinelenen birincil anahtar değeri içeren sonraki kaynak satırları, uyumsuz olarak algılanır ve atlanır.

>[!NOTE]
>Kopyalama etkinliği, dış veri mekanizması dahil olmak üzere yükleme çağırmak için yapılandırıldığında, bu özellik uygulanmaz [Azure SQL veri ambarı PolyBase](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) veya [Amazon Redshift kaldırma](data-factory-amazon-redshift-connector.md#use-unload-to-copy-data-from-amazon-redshift). Verileri PolyBase kullanarak SQL Data Warehouse'a veri yüklemek için PolyBase'nın yerel hata toleransı destek belirterek kullanın. "[polyBaseSettings](data-factory-azure-sql-data-warehouse-connector.md#sqldwsink)" kopyalama etkinliğindeki.

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
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| **Enableskipıncompatiblerow** | Atlanıyor uyumsuz satırları kopyalama sırasında veya etkinleştirin. | True<br/>False (varsayılan) | Hayır |
| **redirectIncompatibleRowSettings** | Zaman uyumsuz satırları günlüğe kaydetmek istediğiniz bir grup olabilir özellik belirtildi. | &nbsp; | Hayır |
| **linkedServiceName** | Atlanan satır içeren günlüğü depolamak için Azure depolama bağlı hizmeti. | Adı bir [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) veya [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) bağlı hizmeti, günlük dosyasını depolamak için kullanmak istediğiniz depolama örneğine başvurur. | Hayır |
| **Yolu** | Atlanan satır içeren bir günlük dosyası yolu. | Uyumsuz verilerini günlüğe kaydetmek için kullanmak istediğiniz Blob Depolama yolu belirtin. Bir yol belirtmezseniz, hizmet sizin için bir kapsayıcı oluşturur. | Hayır |

## <a name="monitoring"></a>İzleme
Kopyalama etkinliği çalıştırmasının tamamlandıktan sonra izleme bölümünde Atlanan satır sayısı görebilirsiniz:

![İzleyici uyumsuz satırların atlandı](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Uyumsuz satırları oturum yapılandırırsanız, günlük dosyası bu yolda bulabilirsiniz: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` Günlük dosyasında atlandı satırları ve uyumsuzluk nedenine görebilirsiniz.

Özgün veriler hem karşılık gelen hata dosyasına kaydedilir. Günlük dosyası içerikleri örneği aşağıdaki gibidir:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği hakkında daha fazla bilgi için bkz: [kopyalama etkinliğiyle veri taşıma](data-factory-data-movement-activities.md).
