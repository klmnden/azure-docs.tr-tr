---
title: Saklı yordam etkinliği Azure Data Factory kullanarak veri dönüştürme | Microsoft Docs
description: SQL Server saklı yordam etkinliği saklı yordam, bir Data Factory işlem hattı bir Azure SQL veritabanı/veri ambarından çağırmak için nasıl kullanılacağını açıklar.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: douglasl
ms.openlocfilehash: 729974d37351c12f551e165567bb11467a0dd963
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="transform-data-by-using-the-sql-server-stored-procedure-activity-in-azure-data-factory"></a>Azure Data Factory içinde SQL Server saklı yordamı etkinliğini kullanarak veri dönüştürme
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-stored-proc-activity.md)
> * [Sürüm 2 - Önizleme](transform-data-using-stored-procedure.md)


Veri fabrikasında veri dönüştürme etkinlikleri kullanma [ardışık düzen](concepts-pipelines-activities.md) dönüştürmek ve Öngörüler ve öngörü ham verileri işlemek için. Saklı yordam etkinliği Data Factory destekleyen dönüştürme etkinlikleri biridir. Bu makalede derlemeler [verileri](transform-data.md) makalesi, veri dönüştürme ve veri fabrikası'nda desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [saklı yordam etkinliği V1 içinde](v1/data-factory-stored-proc-activity.md).
> 
> Azure Data Factory yeniyseniz okuyun [Azure Data Factory'ye giriş](introduction.md) ve öğretici: [öğretici: verileri](tutorial-transform-data-spark-powershell.md) bu makaleyi okumadan önce. 

Kuruluşunuzdaki veya bir Azure sanal makine (VM) üzerinde bir saklı yordam aşağıdaki veri depolarına birinde çağırmak için saklı yordam etkinliği kullanabilirsiniz: 

- Azure SQL Database
- Azure SQL Veri Ambarı
- SQL Server veritabanı.  SQL Server kullanıyorsanız, kendi kendini barındıran tümleştirmesi çalışma zamanı veritabanını barındıran aynı makine üzerindeki veya veritabanına erişimi ayrı bir makineye yükleyin. Kendini barındıran tümleştirmesi çalışma zamanı verileri bağlayan bir bileşen şirket içi/açma Azure VM bulut Hizmetleri ile güvenli ve yönetilen bir şekilde kaynakları ' dir. Bkz: [tümleştirmesi çalışma zamanı'kendi kendini barındıran](create-self-hosted-integration-runtime.md) Ayrıntılar için makale.

> [!IMPORTANT]
> Azure SQL Database veya SQL Server veri kopyalama işlemi sırasında yapılandırabileceğiniz **SqlSink** kullanarak bir saklı yordam çağrılacak kopyalama etkinliğinde **sqlWriterStoredProcedureName** özelliği. Bağlayıcı makaleler özelliği hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md). Kopyalama etkinliği kullanarak bir Azure SQL Data Warehouse'a veri kopyalama sırasında bir saklı yordam çağırma desteklenmiyor. Ancak, SQL Data Warehouse saklı yordama çağırmak için saklı yordam etkinliği kullanabilirsiniz. 
>
> Azure SQL Database veya SQL Server veya Azure SQL Data Warehouse veri kopyalama işlemi sırasında yapılandırabileceğiniz **SqlSource** kullanarak kaynak veritabanından veri okumak için bir saklı yordam çağrılacak kopyalama etkinliğinde  **sqlReaderStoredProcedureName** özelliği. Daha fazla bilgi için aşağıdaki bağlayıcı makalelerine bakın: [Azure SQL veritabanı](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md), [Azure SQL veri ambarı](connector-azure-sql-data-warehouse.md)          

 

## <a name="syntax-details"></a>Sözdizimi ayrıntıları
Bir saklı yordam etkinliği tanımlamak için JSON biçimi şöyledir:

```json
{
    "name": "Stored Procedure Activity",
    "description":"Description",
    "type": "SqlServerStoredProcedure",
    "linkedServiceName": {
        "referenceName": "AzureSqlLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": {
            "identifier": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" }

        }
    }
}
```

Aşağıdaki tabloda bu JSON özellikleri açıklanmaktadır:

| Özellik                  | Açıklama                              | Gerekli |
| ------------------------- | ---------------------------------------- | -------- |
| ad                      | Etkinlik adı                     | Evet      |
| açıklama               | Etkinlik hangi amaçla kullanıldığına açıklayan metin | Hayır       |
| type                      | Saklı yordam etkinliği için etkinlik türüdür **SqlServerStoredProcedure** | Evet      |
| linkedServiceName         | Başvuru **Azure SQL veritabanı** veya **Azure SQL Data Warehouse** veya **SQL Server** veri fabrikasında bağlı hizmet olarak kayıtlı. Bu bağlantılı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| storedProcedureName       | Çağrılacak saklı yordamın adını belirtin. | Evet      |
| storedProcedureParameters | Saklı yordam parametreleri için değerleri belirtin. Kullanım `"param1": { "value": "param1Value","type":"param1Type" }` parametre değerlerini ve veri kaynağı tarafından desteklenen türlerine geçirmek için. Bir parametre için null geçirmeniz gereken kullanırsanız `"param1": { "value": null }` (tüm küçük harf). | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar
Diğer yollarla verileri dönüştürmek açıklanmaktadır aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Bach yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
