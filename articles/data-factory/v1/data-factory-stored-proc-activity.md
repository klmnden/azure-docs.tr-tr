---
title: "SQL Server saklı yordam etkinliği"
description: "SQL Server saklı yordam etkinliği bir saklı yordam bir Azure SQL Database veya Azure SQL veri ambarı Data Factory işlem hattı çağırmak için nasıl kullanabileceğinizi öğrenin."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: be0bdf771327e57a75a4f95b513f9e80aeaef5a4
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="sql-server-stored-procedure-activity"></a>SQL Server saklı yordam etkinliği
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive etkinliği](data-factory-hive-activity.md) 
> * [Pig etkinliği](data-factory-pig-activity.md)
> * [MapReduce Activity](data-factory-map-reduce.md)
> * [Hadoop akış etkinliği](data-factory-hadoop-streaming-activity.md)
> * [Spark etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinlik](data-factory-use-custom-activities.md)

> [!NOTE]
> Bu makale, Azure Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [dönüştürme saklı yordam etkinliği Data Factory sürüm 2 kullanarak verileri](../transform-data-using-stored-procedure.md).

## <a name="overview"></a>Genel Bakış
Veri fabrikasında veri dönüştürme etkinlikleri kullanma [ardışık düzen](data-factory-create-pipelines.md) dönüştürmek ve Öngörüler ve öngörü ham verileri işlemek için. Saklı yordam etkinliği Data Factory destekleyen dönüştürme etkinlikleri biridir. Bu makalede derlemeler [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi, veri dönüştürme ve veri fabrikası'nda desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

Kuruluşunuzdaki veya bir Azure sanal makine (VM) üzerinde bir saklı yordam aşağıdaki veri depolarına birinde çağırmak için saklı yordam etkinliği kullanabilirsiniz: 

- Azure SQL Database
- Azure SQL Veri Ambarı
- SQL Server veritabanı.  SQL Server kullanıyorsanız, veri yönetimi ağ geçidi veritabanını barındıran aynı makine üzerindeki veya veritabanına erişimi ayrı bir makineye yükleyin. Veri Yönetimi ağ geçidi, veri bağlayan bir bileşen şirket içi/açma Azure VM bulut Hizmetleri ile güvenli ve yönetilen bir şekilde kaynakları ' dir. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) Ayrıntılar için makale.

> [!IMPORTANT]
> Azure SQL Database veya SQL Server veri kopyalama işlemi sırasında yapılandırabileceğiniz **SqlSink** kullanarak bir saklı yordam çağrılacak kopyalama etkinliğinde **sqlWriterStoredProcedureName** özelliği. Daha fazla bilgi için bkz: [çağırma kopyalama etkinliği bir saklı yordamdan](data-factory-invoke-stored-procedure-from-copy-activity.md). Bağlayıcı makaleler özelliği hakkında daha fazla bilgi için bkz: [Azure SQL veritabanı](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties). Kopyalama etkinliği kullanarak bir Azure SQL Data Warehouse'a veri kopyalama sırasında bir saklı yordam çağırma desteklenmiyor. Ancak, SQL Data Warehouse saklı yordama çağırmak için saklı yordam etkinliği kullanabilirsiniz. 
>  
> Azure SQL Database veya SQL Server veya Azure SQL Data Warehouse veri kopyalama işlemi sırasında yapılandırabileceğiniz **SqlSource** kullanarak kaynak veritabanından veri okumak için bir saklı yordam çağrılacak kopyalama etkinliğinde  **sqlReaderStoredProcedureName** özelliği. Daha fazla bilgi için aşağıdaki bağlayıcı makalelerine bakın: [Azure SQL veritabanı](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL veri ambarı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          


Aşağıdaki örneklerde saklı yordam etkinliği bir Azure SQL veritabanındaki bir saklı yordam çağırmak için bir işlem hattı kullanır. 

## <a name="walkthrough"></a>Kılavuz
### <a name="sample-table-and-stored-procedure"></a>Örnek tablo ve saklı yordam
1. Aşağıdaki oluşturma **tablo** SQL Server Management Studio veya tanımanız başka bir araç kullanarak, Azure SQL veritabanındaki. Tarih ve saat karşılık gelen kimliği oluşturulduğunda datetimestamp bir sütundur.

    ```SQL
    CREATE TABLE dbo.sampletable
    (
        Id uniqueidentifier,
        datetimestamp nvarchar(127)
    )
    GO

    CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
    GO
    ```
    Kimliği tanımlanan benzersizdir ve karşılık gelen kimliği oluşturulduğunda saat ve tarihi datetimestamp sütundur.
    
    ![Örnek veriler](./media/data-factory-stored-proc-activity/sample-data.png)

    Bu örnekte, bir Azure SQL veritabanında saklı yordamı vardır. Saklı yordam bir Azure SQL veri ambarı ve SQL Server veritabanı varsa, benzer bir yaklaşımdır. SQL Server veritabanı için yüklemeniz gereken bir [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md).
2. Aşağıdaki oluşturma **saklı yordamı** için verileri ekler **sampletable**.

    ```SQL
    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > **Ad** ve **büyük/küçük harf** (Bu örnekte DateTime) parametresinin parametre ardışık düzen/JSON etkinliğinde belirtilen eşleşmelidir. Saklı yordam tanımı'nda emin  **@**  parametresi için bir önek olarak kullanılır.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
1. [Azure portalı](https://portal.azure.com/)’nda oturum açın.
2. Tıklatın **yeni** sol menüsünde **Intelligence + analiz**, tıklatıp **Data Factory**.

    ![Yeni data factory](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. İçinde **yeni data factory** dikey penceresinde girin **SProcDF** adı. Azure Data Factory adları **genel benzersiz**. Veri Fabrikası adı Fabrika başarılı oluşturulmasını sağlamak üzere sizin adınızla öneki gerekir.

   ![Yeni data factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. Seçin, **Azure aboneliği**.
5. İçin **kaynak grubu**, aşağıdaki adımlardan birini yapın:
   1. Tıklatın **Yeni Oluştur** ve kaynak grubu için bir ad girin.
   2. Tıklatın **var olanı kullan** ve varolan bir kaynak grubu seçin.  
6. Data factory için **konum** seçin.
7. Seçin **panoya Sabitle** böylece oturum sonraki açışınızda Panoda data factory görebilirsiniz.
8. **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.
9. Oluşturulmakta veri fabrikası gördüğünüz **Pano** Azure portalının. Data factory sorunsuz oluşturulduktan sonra data factory sayfasını görürsünüz, burada size data factory içeriği gösterilir.

   ![Data Factory giriş sayfası](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Bir Azure SQL bağlı hizmeti oluşturma
Veri Fabrikası oluşturduktan sonra bir Azure SQL oluşturun sampletable tablo ve sp_sample içeren Azure SQL veritabanınızı bağlantılar bağlantılı hizmet saklı yordamı, veri fabrikası için.

1. Tıklatın **yazar ve dağıtma** üzerinde **Data Factory** dikey **SProcDF** Data Factory düzenleyiciyi başlatmak için.
2. Tıklatın **yeni veri deposu** komut çubuğu ve seçin **Azure SQL veritabanı**. Düzenleyicide Azure SQL bağlı hizmeti oluşturmak için JSON betiğini görmeniz gerekir.

   ![Yeni veri deposu](media/data-factory-stored-proc-activity/new-data-store.png)
3. JSON komut dosyasında aşağıdaki değişiklikleri yapın:

   1. Değiştir `<servername>` Azure SQL veritabanı sunucunuzun adını.
   2. Değiştir `<databasename>` içinde oluşturduğunuz tablo ve saklı yordamı veritabanı ile.
   3. Değiştir `<username@servername>` veritabanına erişimi olan kullanıcı hesabı ile.
   4. Değiştir `<password>` kullanıcı hesabı için parola ile.

      ![Yeni veri deposu](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. Bağlantılı hizmet dağıtmak için **dağıtma** komut çubuğunda. AzureSqlLinkedService soldaki ağaç görünümünde gördüğünüzü onaylayın.

    ![bağlantılı hizmet bulunduğu ağaç görünümü](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Çıktı veri kümesi oluşturma
Saklı yordam herhangi bir veri üretmez olsa bile bir çıkış veri kümesi için bir saklı yordam etkinliği belirtmeniz gerekir. Etkinlik (ne sıklıkta etkinlik - saatlik çalıştırılır, günlük, vb.) zamanlamasını sürücüleri çıktı veri kümesi olduğundan olmasıdır. Çıktı veri kümesi kullanmalısınız bir **bağlantılı hizmeti** bir Azure SQL Database veya bir Azure SQL Data Warehouse veya çalıştırmak için saklı yordam istediğiniz SQL Server veritabanı başvuruyor. Çıktı veri kümesi için saklı yordam sonucunu başka bir etkinlik tarafından işleme sonraki geçirmek için bir yol olarak hizmet verebilir ([etkinlikleri zincirleme](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) ardışık düzeninde. Ancak, veri fabrikası otomatik olarak bir saklı yordam çıktısını bu veri kümesine yazmaz. Çıktı veri kümesi işaret eden bir SQL tablosuna Yazar saklı yordam değil. Bazı durumlarda, çıktı veri kümesi olabilir bir **kukla dataset** (gerçekten saklı yordamı çıktısını tutmaz bir tabloya işaret eden bir veri kümesi). Sahte bu veri kümesi yalnızca saklı yordam etkinliği çalıştırmak için zamanlama belirtmek için kullanılır. 

1. Düğmeyi görmüyorsanız araç çubuğunda **... Daha fazla** araç çubuğunda tıklatın **yeni veri kümesi**, tıklatıp **Azure SQL**. **Yeni veri kümesi** seçin ve komut çubuğunda **Azure SQL**.

    ![bağlantılı hizmet bulunduğu ağaç görünümü](media/data-factory-stored-proc-activity/new-dataset.png)
2. JSON Düzenleyicisi için aşağıdaki JSON komut dosyasında kopyalayıp yapıştırın.

    ```JSON
    {                
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "sampletable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
3. Veri kümesi dağıtmak için **dağıtma** komut çubuğunda. Veri kümesi ağaç görünümünde gördüğünüzü onaylayın.

    ![Bağlı hizmetlerin bulunduğu ağaç görünümü](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>SqlServerStoredProcedure etkinliği ile işlem hattı oluşturma
Şimdi, saklı yordam etkinliği ile işlem hattı oluşturalım. 

Aşağıdaki özelliklere dikkat edin: 

- **Türü** özelliği ayarlanmış **SqlServerStoredProcedure**. 
- **StoredProcedureName** türü özellikleri ayarlanmış **sp_sample** (saklı yordam adı).
- **StoredProcedureParameters** bölüm adlı bir parametre içeren **saniyeyi tarih /**. Ad ve büyük/küçük harf JSON içinde parametresinin adını ve büyük/küçük harf saklı yordam tanımındaki parametre eşleşmesi gerekir. Bir parametre için null başarılı olursa söz dizimini kullanın: `"param1": null` (tüm küçük harf).
 
1. Düğmeyi görmüyorsanız araç çubuğunda **... Daha fazla** tıklatın ve komut çubuğunda **yeni işlem hattı**.
2. Aşağıdaki JSON kod parçacığını kopyalayıp yapıştırın:   

    ```JSON
    {
        "name": "SprocActivitySamplePipeline",
        "properties": {
            "activities": [
                {
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
                        "storedProcedureName": "sp_sample",
                        "storedProcedureParameters": {
                            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                        }
                    },
                    "outputs": [
                        {
                            "name": "sprocsampleout"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "SprocActivitySample"
                }
            ],
             "start": "2017-04-02T00:00:00Z",
             "end": "2017-04-02T05:00:00Z",
            "isPaused": false
        }
    }
    ```
3. İşlem hattını dağıtmak için **dağıtma** araç çubuğunda.  

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme
1. Data Factory Düzenleyici dikey penceresini kapatmak ve Data Factory dikey penceresine dönmek için **X** işaretine, sonra da **Diyagram**’a tıklayın.

    ![Diyagram kutucuğu](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. **Diyagram Görünümü**nde, işlem hatlarına ve bu öğreticide kullanılan veri kümelerine bir genel bakış görürsünüz.

    ![Diyagram kutucuğu](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. Diyagram Görünümü'nde veri kümesine çift `sprocsampleout`. Dilim hazır durumunda bakın. Başlangıç zamanı ve bitiş zamanı JSON öğesinden arasındaki her bir saat dilimi oluşturduğundan beş dilimler olmalıdır.

    ![Diyagram kutucuğu](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. Bir dilim olduğunda **hazır** Çalıştır durumunda, bir `select * from sampletable` verileri de tabloya saklı yordam tarafından eklenmiş olduğunu doğrulamak için Azure SQL veritabanında sorgu.

   ![Çıktı verileri](./media/data-factory-stored-proc-activity/output.png)

   Bkz: [işlem hattını izleme](data-factory-monitor-manage-pipelines.md) Azure Data Factory işlem hatlarını izleme hakkında ayrıntılı bilgi.  


## <a name="specify-an-input-dataset"></a>Bir giriş veri kümesi belirtin
Kılavuzda saklı yordam etkinliği herhangi bir giriş veri kümesi yok. Bir giriş veri kümesi belirtirseniz, saklı yordam etkinliği girdi veri kümesi dilimin (hazır durumda) kullanılabilir hale gelene kadar çalışmaz. Veri kümesi (başka bir etkinliğin aynı ardışık düzen tarafından üretilen değil) bir dış veri kümesi ya da bir Yukarı Akış etkinliği (Bu etkinlik önce çalışır etkinliği) tarafından üretilen bir iç veri kümesi olabilir. Saklı yordam etkinliği için birden çok giriş veri kümesi belirtebilirsiniz. Bunu yaparsanız, yalnızca tüm girdi veri kümesi dilimleri (hazır durumda) kullanılabilir saklı yordam etkinliği çalıştırılır. Girdi veri kümesi saklı yordam, bir parametre olarak kullanılamaz. Yalnızca saklı yordam etkinliği başlatmadan önce bağımlılık denetlemek için kullanılır.

## <a name="chaining-with-other-activities"></a>Diğer etkinliklerle zincirleme
Bu etkinliği olmayan bir Yukarı Akış etkinliği zincir istiyorsanız, Yukarı Akış etkinliği çıktısını Bu etkinliğin bir giriş olarak belirtin. Bunu yaptığınızda, saklı yordam etkinliğinin çıkış veri kümesi Yukarı Akış etkinliği (hazır durumunda) kullanılabilir ve Yukarı Akış etkinliği tamamlar kadar çalışmaz. Birden çok Yukarı Akış etkinliği çıkış kümelerini saklı yordam etkinliği giriş veri kümeleri belirtebilirsiniz. Bunu yaptığınızda, tüm giriş veri kümesi dilimler kullanılabilir olduğunda saklı yordam etkinliği çalıştırır.  

Aşağıdaki örnekte, kopya etkinliğinin çıkışıdır: saklı yordam etkinliği bir girdi OutputDataset. Bu nedenle, saklı yordam etkinliği kopyalama etkinliği tamamlar ve OutputDataset dilim (hazır durumda) kullanılabilir oluncaya kadar çalışmaz. Birden çok giriş veri kümeleri belirtirseniz, saklı yordam etkinliği tüm girdi veri kümesi dilimleri (hazır durumda) kullanılabilir oluncaya kadar çalışmaz. Giriş veri kümeleri, doğrudan saklı yordam etkinliği için parametre olarak kullanılamaz. 

Etkinlikler zincirleme daha fazla bilgi için bkz: [bir ardışık düzende birden çok etkinlik](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)

```json
{

    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob to blob",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [ { "name": "InputDataset" } ],
                "outputs": [ { "name": "OutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst"
                },
                "name": "CopyFromBlobToSQL"
            },
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "SPSproc"
                },
                "inputs": [ { "name": "OutputDataset" } ],
                "outputs": [ { "name": "SQLOutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunStoredProcedure"
            }

        ],
        "start": "2017-04-12T00:00:00Z",
        "end": "2017-04-13T00:00:00Z",
        "isPaused": false,
    }
}
```

Benzer şekilde, mağaza yordamı etkinlikle bağlamak için **aşağı akış etkinlikleri** (saklı yordam etkinliği tamamlandıktan sonra çalıştırılan etkinlikleri), bir giriş olarak saklı yordam etkinliğinin çıkış veri kümesi belirtin Ardışık Düzen aşağı akış etkinliğinde.

> [!IMPORTANT]
> Azure SQL Database veya SQL Server veri kopyalama işlemi sırasında yapılandırabileceğiniz **SqlSink** kullanarak bir saklı yordam çağrılacak kopyalama etkinliğinde **sqlWriterStoredProcedureName** özelliği. Daha fazla bilgi için bkz: [çağırma kopyalama etkinliği bir saklı yordamdan](data-factory-invoke-stored-procedure-from-copy-activity.md). Özelliği hakkında daha fazla ayrıntı için aşağıdaki bağlayıcı makalelerine bakın: [Azure SQL veritabanı](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).
>  
> Azure SQL Database veya SQL Server veya Azure SQL Data Warehouse veri kopyalama işlemi sırasında yapılandırabileceğiniz **SqlSource** kullanarak kaynak veritabanından veri okumak için bir saklı yordam çağrılacak kopyalama etkinliğinde  **sqlReaderStoredProcedureName** özelliği. Daha fazla bilgi için aşağıdaki bağlayıcı makalelerine bakın: [Azure SQL veritabanı](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL veri ambarı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          

## <a name="json-format"></a>JSON biçimi
Bir saklı yordam etkinliği tanımlamak için JSON biçimi şöyledir:

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs":  [ { "name": "inputtable"  } ],
    "outputs":  [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of the stored procedure>",
        "storedProcedureParameters":  
        {
            "param1": "param1Value"
            …
        }
    }
}
```

Aşağıdaki tabloda bu JSON özellikleri açıklanmaktadır:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad | Etkinlik adı |Evet |
| açıklama |Etkinlik hangi amaçla kullanıldığına açıklayan metin |Hayır |
| type | Ayarlanmalıdır: **SqlServerStoredProcedure** | Evet |
| Girişleri | İsteğe bağlı. Bir giriş veri kümesi belirtirseniz, ('Hazır' durumunda) kullanılabilir olmalıdır çalıştırmak saklı yordam etkinliği. Girdi veri kümesi saklı yordam, bir parametre olarak kullanılamaz. Yalnızca saklı yordam etkinliği başlatmadan önce bağımlılık denetlemek için kullanılır. |Hayır |
| çıkışlar | Bir çıkış veri kümesi için bir saklı yordam etkinliği belirtmeniz gerekir. Çıktı veri kümesi belirtir **zamanlama** saklı yordam etkinliği (saatlik, haftalık, aylık, vb.). <br/><br/>Çıktı veri kümesi kullanmalısınız bir **bağlantılı hizmeti** bir Azure SQL Database veya bir Azure SQL Data Warehouse veya çalıştırmak için saklı yordam istediğiniz SQL Server veritabanı başvuruyor. <br/><br/>Çıktı veri kümesi için saklı yordam sonucunu başka bir etkinlik tarafından işleme sonraki geçirmek için bir yol olarak hizmet verebilir ([etkinlikleri zincirleme](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) ardışık düzeninde. Ancak, veri fabrikası otomatik olarak bir saklı yordam çıktısını bu veri kümesine yazmaz. Çıktı veri kümesi işaret eden bir SQL tablosuna Yazar saklı yordam değil. <br/><br/>Bazı durumlarda, çıktı veri kümesi olabilir bir **kukla dataset**, yalnızca saklı yordam etkinliği çalıştırmak için zamanlama belirtmek için kullanılır. |Evet |
| storedProcedureName |Azure SQL veya çıktı tablosu kullanır bağlantılı hizmet tarafından temsil edilen Azure SQL Data Warehouse veya SQL Server veritabanı içinde saklı yordamın adını belirtin. |Evet |
| storedProcedureParameters |Saklı yordam parametreleri için değerleri belirtin. Bir parametre için null geçmesi gerekiyorsa, söz dizimini kullanın: "param1": null (tüm küçük harf). Bu özellik kullanma hakkında bilgi edinmek için aşağıdaki örnek bakın. |Hayır |

## <a name="passing-a-static-value"></a>Statik değer geçirme
Şimdi, şimdi 'örnek belge' adlı bir statik değeri içeren tabloda 'Senaryo' adlı başka bir sütun eklemeyi göz önünde bulundurun.

![Örnek veri 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

**Tablo:**

```SQL
CREATE TABLE dbo.sampletable2
(
    Id uniqueidentifier,
    datetimestamp nvarchar(127),
    scenario nvarchar(127)
)
GO

CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable2(Id);
```

**Saklı yordam:**

```SQL
CREATE PROCEDURE sp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

Şimdi, geçirmek **senaryo** parametresi ve değeri saklı yordam etkinliğinden. **TypeProperties** önceki örnek bölümünde aşağıdaki kod parçacığını gibi görünür:

```JSON
"typeProperties":
{
    "storedProcedureName": "sp_sample",
    "storedProcedureParameters":
    {
        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
        "Scenario": "Document sample"
    }
}
```

**Data Factory veri kümesi:**

```JSON
{
    "name": "sprocsampleout2",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "sampletable2"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Data Factory işlem hattı**

```JSON
{
    "name": "SprocActivitySamplePipeline2",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample2",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
                        "Scenario": "Document sample"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout2"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
        "start": "2016-10-02T00:00:00Z",
        "end": "2016-10-02T05:00:00Z"
    }
}
```