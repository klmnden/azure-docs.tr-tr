---
title: Azure Data Factory kopyalama etkinliği saklı yordam çağırma | Microsoft Docs
description: Bir Azure Data Factory kopyalama etkinliği Azure SQL veritabanı veya SQL Server saklı bir yordam çağırma hakkında bilgi edinin.
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
ms.openlocfilehash: 35e9347039a7b9939ab4d2719f9738429dec168c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60824261"
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Azure veri fabrikasında kopyalama etkinliği saklı yordam çağırma
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Data Factory'de saklı yordam etkinliği kullanarak verileri dönüştürme](../transform-data-using-stored-procedure.md).


Veri kopyalama işlemi sırasında [SQL Server](data-factory-sqlserver-connector.md) veya [Azure SQL veritabanı](data-factory-azure-sql-connector.md), yapılandırabileceğiniz **SqlSink** bir saklı yordam çağırmak için kopyalama etkinliğindeki. Depolanan kullanmak isteyebilirsiniz (sütun değerleri, birden çok tablo, vb.'içine ekleme bakarak, birleştirme) herhangi bir ek işleme gerçekleştirmek için yordamı, verileri hedef tabloya eklemeden önce gereklidir. Bu özellik yararlanır [Table-Valued parametreleri](https://msdn.microsoft.com/library/bb675163.aspx). 

Aşağıdaki örnek, bir Data Factory işlem hattından (kopyalama etkinliği) bir SQL Server veritabanında bir saklı yordam çağırma işlemi gösterilmektedir:  

## <a name="output-dataset-json"></a>Çıktı veri kümesi JSON
Çıkış veri kümesinde JSON ayarlamak **türü** için: **SqlServerTable**. Ayarlayın **AzureSqlTable** bir Azure SQL veritabanı ile kullanmak için. Değeri **tableName** özelliği, ilk parametresinin saklı yordamın adı eşleşmelidir.  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a>Kopyalama etkinliği JSON SqlSink bölümünde
Tanımlama **SqlSink** gibi kopyalama etkinliği JSON bölümü. Havuz/hedef veritabanına veri eklerken bir saklı yordam çağırmak için değerleri hem belirtin **SqlWriterStoredProcedureName** ve **SqlWriterTableType** özellikleri. Bu özelliklerin açıklamaları için bkz. [SQL Server Bağlayıcısı makalesi SqlSink bölümünde](data-factory-sqlserver-connector.md#sqlsink).

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a>Saklı yordam tanımında 
Veritabanınızda, aynı ada sahip bir saklı yordam tanımlamak **SqlWriterStoredProcedureName**. Saklı yordam, kaynak veri deposundan girdi verilerini işleyen ve hedef veritabanındaki bir tabloya veri ekler. Saklı yordam, ilk parametresinin adı JSON (Marketing) kümesinde tanımlanan tableName eşleşmesi gerekir.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>Tablo tür tanımı
Veritabanınızda, aynı ada sahip bir tablo türü tanımlayan **SqlWriterTableType**. Tablo türü şemasını giriş veri kümesi şemasını eşleşmesi gerekir.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki Bağlayıcısı'nı gözden geçirme için tam JSON örnekler belirten makaleleri: 

- [Azure SQL Veritabanı](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
