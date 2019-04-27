---
title: Azure Data Factory'de saklı yordam etkinliği kullanarak verileri dönüştürme | Microsoft Docs
description: SQL Server saklı yordam etkinliği, saklı yordam, bir Azure SQL veritabanı/veri ambarı'na Data Factory işlem hattından çağırma kullanılacağını açıklanmaktadır.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/27/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: 806654b7586895b62b014a49b8b3a00fb18f008f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60764416"
---
# <a name="transform-data-by-using-the-sql-server-stored-procedure-activity-in-azure-data-factory"></a>Azure Data Factory'de SQL Server saklı yordam etkinliği kullanarak verileri dönüştürme
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-stored-proc-activity.md)
> * [Geçerli sürüm](transform-data-using-stored-procedure.md)

Data Factory'de veri dönüştürme etkinlikleri kullanma [işlem hattı](concepts-pipelines-activities.md) dönüştürmek ve Öngörüler ve öngörüleri ham verileri işlemek için. Saklı yordam etkinliği, Data Factory destekler dönüştürme etkinlikleri biridir. Bu makalede yapılar [verileri dönüştürme](transform-data.md) makalesi, veri dönüştürme ve Data Factory desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

> [!NOTE]
> Azure Data Factory kullanmaya yeni başladıysanız, okumak [Azure Data Factory'ye giriş](introduction.md) ve öğretici uygulayın: [Öğretici: verileri dönüştürme](tutorial-transform-data-spark-powershell.md) bu makaleyi okuduktan önce. 

Saklı yordam etkinliği, kuruluşunuzda veya bir Azure sanal makine'de (VM) aşağıdaki veri depolarını birinde bir saklı yordam çağırmak için kullanabilirsiniz: 

- Azure SQL Veritabanı
- Azure SQL Veri Ambarı
- SQL Server veritabanı.  SQL Server kullanıyorsanız, şirket içinde barındırılan tümleştirme çalışma zamanı veritabanını barındıran aynı makinede veya veritabanına erişimi olan ayrı bir makineye yükleyin. Şirket içinde barındırılan Integration runtime, veri bağlayan bir bileşeni, güvenli ve yönetilen bir şekilde şirket içi/açık bulut Hizmetleri ile Azure VM kaynakları ' dir. Bkz: [barındırılan tümleştirme çalışma zamanını](create-self-hosted-integration-runtime.md) makale Ayrıntılar için.

> [!IMPORTANT]
> Azure SQL veritabanı ya da SQL Server veri kopyalama yapılırken, yapılandırabileceğiniz **SqlSink** kullanarak bir saklı yordam çağırmak için kopyalama etkinliğindeki **sqlWriterStoredProcedureName** özelliği. Bağlayıcı makaleler özelliği hakkında daha fazla ayrıntı için bkz: [Azure SQL veritabanı](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md). Kopyalama etkinliği'ni kullanarak bir Azure SQL veri ambarı'na veri kopyalama sırasında bir saklı yordam çağırma desteklenmiyor. Ancak, SQL veri ambarı'nda bir saklı yordam çağırmak için saklı yordam etkinliği kullanabilirsiniz. 
>
> Azure SQL veritabanı veya SQL Server veya Azure SQL veri ambarı veri kopyalama yapılırken, yapılandırabileceğiniz **SqlSource** kullanarak kaynak veritabanından verileri okumak için bir saklı yordam çağırmak için kopyalama etkinliğindeki  **sqlReaderStoredProcedureName** özelliği. Daha fazla bilgi için aşağıdaki Bağlayıcısı makalelere bakın: [Azure SQL veritabanı](connector-azure-sql-database.md), [SQL Server](connector-sql-server.md), [Azure SQL veri ambarı](connector-azure-sql-data-warehouse.md)          

 

## <a name="syntax-details"></a>Söz dizimi ayrıntıları
Bir saklı yordam etkinliğine tanımlamak için JSON biçimi şu şekildedir:

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
        "storedProcedureName": "usp_sample",
        "storedProcedureParameters": {
            "identifier": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" }

        }
    }
}
```

Aşağıdaki tabloda, bu JSON özellikleri açıklanmaktadır:

| Özellik                  | Açıklama                              | Gerekli |
| ------------------------- | ---------------------------------------- | -------- |
| ad                      | Etkinliğin adı                     | Evet      |
| açıklama               | Etkinliğin ne için kullanıldığını açıklayan metin | Hayır       |
| type                      | Saklı yordam etkinliği için etkinlik türdür **SqlServerStoredProcedure** | Evet      |
| linkedServiceName         | Başvuru **Azure SQL veritabanı** veya **Azure SQL veri ambarı** veya **SQL Server** Data Factory öğesinde bağlantılı hizmet olarak kayıtlı. Bu bağlı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| storedProcedureName       | Çağrılacak saklı yordamın adını belirtin. | Evet      |
| storedProcedureParameters | Saklı yordam parametrelerinin değerlerini belirtin. Kullanım `"param1": { "value": "param1Value","type":"param1Type" }` parametre değerleri ve veri kaynağı tarafından desteklenen türlerinin geçirilecek. Bir parametre için null değeri geçirmeye izin ihtiyacınız varsa `"param1": { "value": null }` (küçük harflerle). | Hayır       |

## <a name="error-info"></a>Hata bilgisi

Bir saklı yordam başarısız olur ve hata ayrıntılarını döndürür, hata bilgilerini doğrudan etkinlik çıkışı yakalayamaz. Ancak, Data Factory tüm olayları çalıştırmak için Azure İzleyici, etkinlik pumps. Data Factory için Azure İzleyici pompalara olduğunu arasında olaylar, bu hata ayrıntıları orada iter. Örneğin, e-posta uyarıları, bu olayların ayarlayabilirsiniz. Daha fazla bilgi için bkz. [Azure İzleyicisi'ni kullanarak veri fabrikalarını izleme ve uyarı](monitor-using-azure-monitor.md).

## <a name="next-steps"></a>Sonraki adımlar
Anlatan farklı yollarla verileri dönüştürmek aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliğinde](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Bach yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
