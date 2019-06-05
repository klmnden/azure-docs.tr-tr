---
title: SQL Server saklı yordam etkinliği
description: Saklı yordam bir Azure SQL veritabanı veya Azure SQL veri ambarı, bir Data Factory işlem hattından çağırma için SQL Server saklı yordam etkinliğine nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
author: nabhishek
ms.author: abnarain
manager: craigg
robots: noindex
ms.openlocfilehash: 77842b60108629168f423f25eb03b01079cf55e5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61256062"
---
# <a name="sql-server-stored-procedure-activity"></a>SQL Server saklı yordam etkinliği
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive etkinliği](data-factory-hive-activity.md)
> * [Pig etkinliği](data-factory-pig-activity.md)
> * [MapReduce etkinliği](data-factory-map-reduce.md)
> * [Hadoop akış etkinliğinde](data-factory-hadoop-streaming-activity.md)
> * [Spark etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinliği](data-factory-use-custom-activities.md)

> [!NOTE]
> Bu makale, Azure Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Data Factory'de saklı yordam etkinliği kullanarak verileri dönüştürme](../transform-data-using-stored-procedure.md).

## <a name="overview"></a>Genel Bakış
Data Factory'de veri dönüştürme etkinlikleri kullanma [işlem hattı](data-factory-create-pipelines.md) dönüştürmek ve Öngörüler ve öngörüleri ham verileri işlemek için. Saklı yordam etkinliği, Data Factory destekler dönüştürme etkinlikleri biridir. Bu makalede yapılar [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi, veri dönüştürme ve Data Factory desteklenen dönüştürme etkinliklerinin genel bir bakış sunar.

Saklı yordam etkinliği, kuruluşunuzda veya bir Azure sanal makine'de (VM) aşağıdaki veri depolarını birinde bir saklı yordam çağırmak için kullanabilirsiniz:

- Azure SQL Veritabanı
- Azure SQL Veri Ambarı
- SQL Server veritabanı. SQL Server kullanıyorsanız, veri yönetimi ağ geçidi veritabanını barındıran aynı makinede veya veritabanına erişimi olan ayrı bir makineye yükleyin. Veri Yönetimi ağ geçidi, veri bağlayan bir bileşeni, güvenli ve yönetilen bir şekilde şirket içi/açık bulut Hizmetleri ile Azure VM kaynakları ' dir. Bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md) makale Ayrıntılar için.

> [!IMPORTANT]
> Azure SQL veritabanı ya da SQL Server veri kopyalama yapılırken, yapılandırabileceğiniz **SqlSink** kullanarak bir saklı yordam çağırmak için kopyalama etkinliğindeki **sqlWriterStoredProcedureName** özelliği. Daha fazla bilgi için [kopyalama etkinliğini saklı yordam çağırma](data-factory-invoke-stored-procedure-from-copy-activity.md). Bağlayıcı makaleler özelliği hakkında daha fazla ayrıntı için bkz: [Azure SQL veritabanı](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties). Kopyalama etkinliği'ni kullanarak bir Azure SQL veri ambarı'na veri kopyalama sırasında bir saklı yordam çağırma desteklenmiyor. Ancak, SQL veri ambarı'nda bir saklı yordam çağırmak için saklı yordam etkinliği kullanabilirsiniz.
>
> Azure SQL veritabanı veya SQL Server veya Azure SQL veri ambarı veri kopyalama yapılırken, yapılandırabileceğiniz **SqlSource** kullanarak kaynak veritabanından verileri okumak için bir saklı yordam çağırmak için kopyalama etkinliğindeki  **sqlReaderStoredProcedureName** özelliği. Daha fazla bilgi için aşağıdaki Bağlayıcısı makalelere bakın: [Azure SQL veritabanı](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL veri ambarı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)

Aşağıdaki kılavuzda bir Azure SQL veritabanında bir saklı yordam çağırmak için bir işlem hattı, saklı yordam etkinliği kullanır.

## <a name="walkthrough"></a>Kılavuz
### <a name="sample-table-and-stored-procedure"></a>Örnek tablo ve saklı yordam
1. Aşağıdaki oluşturma **tablo** alışık olduğunuz herhangi bir aracı ya da SQL Server Management Studio kullanarak Azure SQL veritabanı. Datetimestamp tarihi ve saati karşılık gelen kimliği oluşturulduğunda bir sütundur.

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
    Tanımlanan benzersiz kimliğidir ve tarihi ve saati karşılık gelen kimliği oluşturulduğunda datetimestamp sütundur.
    
    ![Örnek veriler](./media/data-factory-stored-proc-activity/sample-data.png)

    Bu örnekte, bir Azure SQL veritabanı'nda saklı yordam aynıdır. Saklı yordamı bir Azure SQL veri ambarı ve SQL Server veritabanı varsa, benzer bir yaklaşımdır. Bir SQL Server veritabanı için yüklemeniz gereken bir [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md).
2. Aşağıdaki oluşturma **saklı yordamı** verileri için ekler **sampletable**.

    ```SQL
    CREATE PROCEDURE usp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > **Adı** ve **büyük/küçük harf** (Bu örnekte DateTime) parametre, işlem hattı/JSON etkinliğinde belirtilen parametre eşleşmelidir. Saklı yordam tanımında emin **\@** parametresi için bir önek olarak kullanılır.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
1. [Azure portalı](https://portal.azure.com/)’nda oturum açın.
2. Tıklayın **yeni** sol menüsünde **zeka + analiz**, tıklatıp **Data Factory**.

    ![Yeni veri fabrikası](media/data-factory-stored-proc-activity/new-data-factory.png)
3. İçinde **yeni veri fabrikası** dikey penceresinde girin **SProcDF** adı. Azure Data Factory adları **genel olarak benzersiz**. Veri fabrikasının adı Factory başarılı oluşturmayı etkinleştirmek üzere sizin adınızla önek gerekir.

   ![Yeni veri fabrikası](media/data-factory-stored-proc-activity/new-data-factory-blade.png)
4. **Azure aboneliğinizi** seçin.
5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
   1. Tıklayın **Yeni Oluştur** ve kaynak grubu için bir ad girin.
   2. Tıklayın **var olanı kullan** ve mevcut bir kaynak grubunu seçin.
6. Data factory için **konum** seçin.
7. Seçin **panoya Sabitle** data factory, oturum açtığında Panoda görebilirsiniz.
8. **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.
9. İçinde oluşturduğunuz veri fabrikasına gördüğünüz **Pano** Azure portal'ın. Data factory sorunsuz oluşturulduktan sonra data factory sayfasını görürsünüz, burada size data factory içeriği gösterilir.

   ![Data Factory giriş sayfası](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Bir Azure SQL bağlı hizmeti oluşturma
Veri fabrikasını oluşturduktan sonra bir Azure SQL oluşturma sampletable tablo ve usp_sample içerir, Azure SQL veritabanına bağlanan bağlı hizmeti saklı yordamsa, veri fabrikanızın için.

1. Tıklayın **yazar ve dağıtma** üzerinde **Data Factory** dikey **SProcDF** Data Factory Düzenleyicisi'ni başlatmak için.
2. Tıklayın **yeni veri deposu** komut çubuğu ve seçin **Azure SQL veritabanı**. Düzenleyici'de Azure SQL bağlı hizmeti oluşturmak için JSON betiğini görmeniz gerekir.

   ![Yeni veri deposu](media/data-factory-stored-proc-activity/new-data-store.png)
3. JSON komut dosyasında aşağıdaki değişiklikleri yapın:

   1. Değiştirin `<servername>` ile Azure SQL veritabanı sunucunuzun adını.
   2. Değiştirin `<databasename>` oluşturduğunuz tabloya ve saklı yordamı veritabanı ile.
   3. Değiştirin `<username@servername>` veritabanına erişimi olan kullanıcı hesabı ile.
   4. Değiştirin `<password>` kullanıcı hesabı için parola.

      ![Yeni veri deposu](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. Bağlı hizmeti dağıtmak için **Dağıt** komut çubuğunda. AzureSqlLinkedService soldaki ağaç görünümünde gördüğünüzü onaylayın.

    ![bağlı hizmet bulunduğu ağaç görünümü](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Çıktı veri kümesi oluşturma
Saklı yordam herhangi bir veri oluşturmaz olsa bile bir çıktı veri kümesi için bir saklı yordam etkinliği belirtmeniz gerekir. Etkinlik (ne sıklıkta etkinliği - saatte bir çalışacak, günlük, vb.) zamanlamasını sürücüleri çıktı veri kümesi olduğundan olmasıdır. Çıktı veri kümesi kullanmalısınız bir **bağlı hizmet** Azure SQL veritabanı veya bir Azure SQL veri ambarı veya SQL Server veritabanı saklı yordamı çalıştırmak için istediğiniz başvuruyor. Çıktı veri kümesi için saklı yordam sonucu başka bir etkinlik tarafından işleme sonraki geçirmek için bir yol olarak hizmet verebilen ([zincirleme etkinlikleri](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) işlem hattındaki. Ancak, Data Factory otomatik olarak bir saklı yordam çıktısı bu veri kümesine yazmaz. Bu çıktı veri kümesini işaret eden bir SQL tablosunu yazan saklı yordam aynıdır. Bazı durumlarda, çıktı veri kümesi olabilir bir **işlevsiz bir veri kümesi** (gerçekten saklı yordamın çıkış bulundurmayan tabloya işaret eden bir veri kümesi). İşlevsiz bu veri kümesi yalnızca saklı yordam etkinliği çalıştırmak için zamanlamayı belirtmek için kullanılır.

1. Düğmeyi görmüyorsanız araç çubuğunda **... Daha fazla** araç çubuğunda **yeni veri kümesi**, tıklatıp **Azure SQL**. **Yeni veri kümesi** seçin ve komut çubuğunda **Azure SQL**.

    ![bağlı hizmet bulunduğu ağaç görünümü](media/data-factory-stored-proc-activity/new-dataset.png)
2. JSON Düzenleyicisi için aşağıdaki JSON betiği kopyala/yapıştır.

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
3. Veri kümesini dağıtmak için **Dağıt** komut çubuğunda. Veri kümesini ağaç görünümünde gördüğünüzü onaylayın.

    ![Bağlı hizmetlerin bulunduğu ağaç görünümü](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>SqlServerStoredProcedure etkinliği ile işlem hattı oluşturma
Şimdi github'dan bir saklı yordam etkinliği ile işlem hattı oluşturma.

Aşağıdaki özelliklere dikkat edin:

- **Türü** özelliği **SqlServerStoredProcedure**.
- **StoredProcedureName** türü özellikler ayarlanır **usp_sample** (saklı yordamın adı).
- **StoredProcedureParameters** bölümü adlı bir parametre içeren **DateTime**. Adı ve büyük/küçük harf saklı yordam tanımında parametre adı ve parametre JSON içinde büyük küçük harfleri eşleşmesi gerekir. Bir parametre için null geçmesi, söz dizimini kullanın: `"param1": null` (tamamı küçük harflerle).

1. Düğmeyi görmüyorsanız araç çubuğunda **... Daha fazla** tıklayın ve komut çubuğunda **yeni işlem hattı**.
2. Aşağıdaki JSON kod parçacığında kopyala/yapıştır:

    ```JSON
    {
        "name": "SprocActivitySamplePipeline",
        "properties": {
            "activities": [
                {
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
                        "storedProcedureName": "usp_sample",
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
3. İşlem hattını dağıtmak için **Dağıt** araç.

### <a name="monitor-the-pipeline"></a>İşlem hattını izleme
1. Data Factory Düzenleyici dikey penceresini kapatmak ve Data Factory dikey penceresine dönmek için **X** işaretine, sonra da **Diyagram**’a tıklayın.

    ![Diyagram kutucuğu](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. **Diyagram Görünümü**nde, işlem hatlarına ve bu öğreticide kullanılan veri kümelerine bir genel bakış görürsünüz.

    ![Diyagram kutucuğu](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. Diyagram Görünümü'nde, veri kümesine çift tıklayın `sprocsampleout`. Dilimler hazır durumda görürsünüz. Bir dilim başlangıç zamanı ve bitiş zamanı, JSON arasındaki her saat için oluşturulduğundan beş dilim olmalıdır.

    ![Diyagram kutucuğu](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. Bir dilim olduğunda **hazır** çalıştırma durumu, bir `select * from sampletable` veri içinde tablonun saklı yordam tarafından eklenmiş olduğunu doğrulamak için Azure SQL veritabanında sorgu.

   ![Çıktı verileri](./media/data-factory-stored-proc-activity/output.png)

   Bkz: [işlem hattını izleme](data-factory-monitor-manage-pipelines.md) Azure Data Factory işlem hatlarını izleme hakkında ayrıntılı bilgi için.

## <a name="specify-an-input-dataset"></a>Girdi veri kümesi belirtin
İzlenecek yolda, saklı yordam etkinliği herhangi bir giriş veri kümesi yok. Girdi veri kümesi belirtirseniz, saklı yordam etkinliği giriş veri kümesinin bir dilimi (Ready durumunda) kullanılabilir hale gelene kadar çalışmaz. Veri kümesi (yani aynı işlem hattının başka bir etkinliği tarafından üretilen değil) bir dış veri kümesi ya da bir Yukarı Akış etkinliği (Bu çalıştırmalarını önce etkinliği) tarafından üretilen bir iç veri kümesi olabilir. Saklı yordam etkinliği için birden fazla giriş veri kümesi belirtebilirsiniz. Bunu yaparsanız, saklı yordam etkinliği yalnızca tüm giriş veri kümesinin dilimlerini (Ready durumunda) kullanılabilir olduğunda çalışır. Giriş veri kümesi saklı yordam, bir parametre olarak kullanılamıyor. Yalnızca, saklı yordam etkinliği başlamadan önce bağımlılık denetlemek için kullanılır.

## <a name="chaining-with-other-activities"></a>Diğer etkinliklerle zincirleme
Yukarı Akış etkinlik çıkışı, bu etkinlik bir Yukarı Akış etkinliği zincirleyebilir, yani istiyorsanız, bu etkinliğin bir giriş olarak belirtin. Bunu yaptığınızda, saklı yordam etkinliği Yukarı Akış etkinlik tamamlandıktan ve Yukarı Akış etkinliğinin çıkış veri kümesi (hazır durumda) kullanılabilir kadar çalışmaz. Çıkış veri kümeleri, birden çok Yukarı Akış etkinliği saklı yordam etkinliğin giriş veri kümeleri belirtebilirsiniz. Bunu yaptığınızda, saklı yordam etkinliği yalnızca tüm giriş veri kümesinin dilimlerini kullanılabilir olduğunda çalışır.

Aşağıdaki örnekte, kopyalama etkinliğinin çıkışıdır: OutputDataset saklı yordam etkinliği bir giriştir. Bu nedenle, saklı yordam etkinliği kopyalama etkinliği tamamlar ve OutputDataset dilimi (Ready durumunda) kullanılabilir kadar çalışmaz. Birden fazla giriş veri kümesi belirtirseniz, saklı yordam etkinliği (Ready durumunda) tüm giriş veri kümesinin dilimlerini kullanılabilir olana kadar çalışmaz. Giriş veri kümeleri, doğrudan saklı yordam etkinliği için parametre olarak kullanılamaz.

Zincirleme etkinlikleri ile ilgili daha fazla bilgi için bkz: [bir işlem hattında birden çok etkinlik](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)

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

Benzer şekilde, depolama yordam etkinliği ile ilişkilendirilecek **aşağı akış etkinlikleri** (saklı yordam etkinliği tamamlandıktan sonra çalıştırılan etkinlikler), çıkış veri kümesi saklı yordam etkinliği girdi olarak belirtin işlem hattındaki bir etkinliğin aşağı akış.

> [!IMPORTANT]
> Azure SQL veritabanı ya da SQL Server veri kopyalama yapılırken, yapılandırabileceğiniz **SqlSink** kullanarak bir saklı yordam çağırmak için kopyalama etkinliğindeki **sqlWriterStoredProcedureName** özelliği. Daha fazla bilgi için [kopyalama etkinliğini saklı yordam çağırma](data-factory-invoke-stored-procedure-from-copy-activity.md). Özelliği hakkında daha fazla ayrıntı için aşağıdaki Bağlayıcısı makalelere bakın: [Azure SQL veritabanı](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).
> 
> Azure SQL veritabanı veya SQL Server veya Azure SQL veri ambarı veri kopyalama yapılırken, yapılandırabileceğiniz **SqlSource** kullanarak kaynak veritabanından verileri okumak için bir saklı yordam çağırmak için kopyalama etkinliğindeki  **sqlReaderStoredProcedureName** özelliği. Daha fazla bilgi için aşağıdaki Bağlayıcısı makalelere bakın: [Azure SQL veritabanı](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL veri ambarı](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)

## <a name="json-format"></a>JSON biçimi
Bir saklı yordam etkinliğine tanımlamak için JSON biçimi şu şekildedir:

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs": [ { "name": "inputtable" } ],
    "outputs": [ { "name": "outputtable" } ],
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

Aşağıdaki tabloda, bu JSON özellikleri açıklanmaktadır:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| name | Etkinliğin adı |Evet |
| description |Etkinliğin ne için kullanıldığını açıklayan metin |Hayır |
| type | Ayarlamanız gerekir: **SqlServerStoredProcedure** | Evet |
| inputs | İsteğe bağlı. Girdi veri kümesi belirtirseniz, ('Hazır' durumunda) kullanılabilir olmalıdır çalıştırmak saklı yordam etkinliği. Giriş veri kümesi saklı yordam, bir parametre olarak kullanılamıyor. Yalnızca, saklı yordam etkinliği başlamadan önce bağımlılık denetlemek için kullanılır. |Hayır |
| outputs | Bir saklı yordam etkinliği için bir çıktı veri kümesi belirtmelisiniz. Çıktı veri kümesi belirtir **zamanlama** saklı yordam etkinliği (saatlik, haftalık, aylık, vb.). <br/><br/>Çıktı veri kümesi kullanmalısınız bir **bağlı hizmet** Azure SQL veritabanı veya bir Azure SQL veri ambarı veya SQL Server veritabanı saklı yordamı çalıştırmak için istediğiniz başvuruyor. <br/><br/>Çıktı veri kümesi için saklı yordam sonucu başka bir etkinlik tarafından işleme sonraki geçirmek için bir yol olarak hizmet verebilen ([zincirleme etkinlikleri](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) işlem hattındaki. Ancak, Data Factory otomatik olarak bir saklı yordam çıktısı bu veri kümesine yazmaz. Bu çıktı veri kümesini işaret eden bir SQL tablosunu yazan saklı yordam aynıdır. <br/><br/>Bazı durumlarda, çıktı veri kümesi olabilir bir **işlevsiz bir veri kümesi**, yalnızca saklı yordam etkinliği çalıştırmak için zamanlamayı belirtmek için kullanılır. |Evet |
| storedProcedureName |Çıktı tablosu kullanan bağlı hizmetiyle temsil edilen Azure SQL veri ambarı veya SQL Server veritabanı ve Azure SQL veritabanı saklı yordamın adını belirtin. |Evet |
| storedProcedureParameters |Saklı yordam parametrelerinin değerlerini belirtin. Bir parametre için null değeri geçirmeye gerekiyorsa, söz dizimini kullanın: "param1": null (küçük harflerle). Aşağıdaki örnek bu özelliği kullanma hakkında bilgi edinmek için bkz. |Hayır |

## <a name="passing-a-static-value"></a>Statik bir değer geçirme
Şimdi, şimdi 'örnek belge' adlı statik bir değer içeren tabloda 'Senaryosu' adlı başka bir sütun eklemeniz yararlı olabilir.

![Örnek verileri 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

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
CREATE PROCEDURE usp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

Şimdi, geçmesi **senaryo** parametresi ve saklı yordam etkinliği değeri. **TypeProperties** önceki örnek bölümünde, aşağıdaki kod parçacığı gibi görünür:

```JSON
"typeProperties":
{
    "storedProcedureName": "usp_sample",
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

**Veri Fabrikası işlem hattı**

```JSON
{
    "name": "SprocActivitySamplePipeline2",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "usp_sample2",
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
