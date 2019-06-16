---
title: Azure Data Factory - Team Data Science Process ile SQL Server verileri için SQL Azure
description: Bulutta ve şirket veritabanları arasında verileri günlük olarak birlikte taşımak iki veri taşıma etkinlikleri ölçeklemesini ADF işlem hattı ayarlayın.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 59f8b8b253fc914e5723a9c41475ec78bc3f376e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61429357"
---
# <a name="move-data-from-an-on-premises-sql-server-to-sql-azure-with-azure-data-factory"></a>Azure Data Factory ile SQL Azure için bir şirket içi SQL Server'dan veri taşıma

Bu makalede, Azure Data Factory (ADF) kullanarak verileri şirket içi SQL Server veritabanından Azure Blob Depolama ile bir SQL Azure veritabanına taşıma gösterilmektedir.

Bir Azure SQL veritabanı'na veri taşımak için çeşitli seçenekler özetlenmektedir bir tablo için bkz: [veri taşıma için bir Azure SQL veritabanı için Azure Machine Learning](move-sql-azure.md).

## <a name="intro"></a>Giriş: ADF nedir ve ne zaman, verileri geçirmek için kullanılmalıdır?
Azure Data Factory, taşımayı ve dönüştürmeyi düzenleyen ve otomatikleştiren bir tam olarak yönetilen bulut tabanlı veri tümleştirme hizmetidir. ADF modelinde en önemli kavram, işlem hattı ' dir. Bir işlem hattı her biri veri kümelerinde bulunan veriler üzerinde gerçekleştirilecek eylemleri tanımlar. bir mantıksal etkinlik grubudur. Bağlı hizmetler için veri kaynaklarına bağlanmak Data Factory'ye gereken bilgileri tanımlamak için kullanılır.

ADF ile mevcut veri işleme Hizmetleri içinde bulutta yüksek oranda kullanılabilir ve yönetilen veri işlem hatları kullanılamayacağı. Bu veri işlem hatları, içe alma, hazırlama, dönüştürme, analiz ve veri yayımlama zamanlanabilir ve ADF yönetir ve işleme bağımlılıkları ve karmaşık veri düzenler. Çözümleri hızla oluşturulabilen ve bulutta, şirket içi giderek artan sayıda bağlanma dağıtılan ve bulut veri kaynakları.

ADF kullanmayı dikkate alın:

* sürekli olarak geçirilmesi verilere ihtiyaç duyduğunda erişen hem de karma bir senaryoda şirket içi ve bulut kaynakları
* veri işlem temelli veya değiştirilmesi veya kendisine geçirilen eklenmesi iş mantığına sahip olmak gerekir.

Zamanlama ve düzenli aralıklarla veri taşıma işlemlerini yönetmek basit JSON betiklerini kullanarak işleri izlemek için ADF sağlar. ADF karmaşık işlemleri desteği gibi diğer özellikleri de vardır. ADF ile ilgili daha fazla bilgi için bkz: adresindeki belgelere [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).

## <a name="scenario"></a>Senaryo
İki veri taşıma etkinlikleri ölçeklemesini bir ADF ardışık ayarladık. Birlikte bunların verileri günlük olarak bir şirket içi SQL veritabanı ve Azure SQL veritabanı bulutta arasında taşıyın. İki etkinliği şunlardır:

* bir Azure Blob Depolama hesabı için bir şirket içi SQL Server veritabanından veri kopyalama
* verileri Azure Blob Depolama hesabından bir Azure SQL veritabanı'na kopyalayın.

> [!NOTE]
> Burada gösterilen adımları ADF ekibi tarafından sağlanan daha ayrıntılı öğreticiden uyarlanmıştır: [Verileri bir şirket içi SQL Server veritabanından Azure Blob depolama alanına kopyalamak](https://docs.microsoft.com/azure/data-factory/tutorial-hybrid-copy-portal/) uygun olduğunda o konusunun ilgili bölümlerine başvuruları sağlanır.
>
>

## <a name="prereqs"></a>Önkoşullar
Bu öğreticide, sahip olduğunuz varsayılır:

* Bir **Azure aboneliği**. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı**. Bu öğreticide verilerin depolanması için bir Azure depolama hesabını kullanırsınız. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md) makalesine bakın. Depolama hesabını oluşturduktan sonra, depolamaya erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz: [depolama erişim anahtarlarınızı yönetme](../../storage/common/storage-account-manage.md#access-keys).
* Erişim bir **Azure SQL veritabanı**. Bir Azure SQL veritabanı, konu ayarlamanız gerekir, [Microsoft Azure SQL veritabanı ile çalışmaya başlama](../../sql-database/sql-database-get-started.md) Azure SQL veritabanı yeni bir örneğini sağlama hakkında bilgi sağlar.
* Yüklenmiş ve yapılandırılmış **Azure PowerShell** yerel olarak. Yönergeler için [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

> [!NOTE]
> Bu yordamı kullanır [Azure portalında](https://portal.azure.com/).
>
>

## <a name="upload-data"></a> Şirket içi SQL Server'ınıza veri yükleme
Kullandığımız [NYC taksi dataset](https://chriswhong.com/open-data/foil_nyc_taxi/) geçiş işlemini göstermek için. Bu gönderi, Azure blob depolama içinde belirtildiği gibi NYC taksi veri kümesi kullanılabilir [NYC taksi verileri](https://www.andresmh.com/nyctaxitrips/). Verileri iki dosya, seyahat ayrıntıları içeren trip_data.csv dosyasının ve her seyahat için ücretli taksi ayrıntılarını içeren trip_far.csv dosyası vardır. Bir örnek ve açıklama bu dosyaların sağlanan [NYC taksi Gelişlerin veri kümesi tanımı](sql-walkthrough.md#dataset).

Burada kendi veri kümesi için sağlanan yordamı uyarlamak veya NYC taksi veri kümesini kullanarak açıklanan adımları izleyin. Şirket içi SQL Server veritabanınıza NYC taksi veri kümesini yüklemek için bölümünde açıklanan yordamı izleyin [toplu içeri aktarma verileri SQL Server veritabanına](sql-walkthrough.md#dbload). SQL Server üzerinde bir Azure sanal makine için bu yönergeleri yöneliktir, ancak şirket içi SQL Server'a yükleme yordamı aynıdır.

## <a name="create-adf"></a> Bir Azure veri fabrikası oluşturma
Yeni bir Azure Data Factory ve bir kaynak grubu oluşturmak için yönergeleri [Azure portalında](https://portal.azure.com/) sağlanan [bir Azure veri fabrikası oluşturma](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-data-factory). Yeni ADF örnek adı *adfdsp* ve oluşturduğunuz kaynak grubunu adlandırın *adfdsprg*.

## <a name="install-and-configure-azure-data-factory-integration-runtime"></a>Yükleme ve Azure Data Factory Integration Runtime'ı yapılandırma
Integration Runtime, Azure Data Factory tarafından farklı ağ ortamları veri tümleştirme özellikleri sağlamak için kullanılan bir müşteri yönetilen veri tümleştirme altyapısıdır. Bu çalışma zamanı, eski adıyla "Veri yönetimi ağ geçidi" olarak adlandırılıyordu.

Ayarlamak için [bir işlem hattı oluşturmak için yönergeleri izleyin](https://docs.microsoft.com/azure/data-factory/tutorial-hybrid-copy-portal#create-a-pipeline)

## <a name="adflinkedservices"></a>Veri kaynaklarına bağlanmak için bağlı hizmetler oluşturma
Bağlı hizmet için bir veri kaynağına bağlanmak Azure Data Factory'ye gereken bilgileri tanımlar. Bağlı hizmetler için gerekli olan bu senaryoda üç kaynak sunuyoruz:

1. Şirket içi SQL Server
2. Azure Blob Depolama
3. Azure SQL veritabanı

Bağlı hizmetler oluşturmaya yönelik adım adım yordam sağlanan [bağlı hizmetler oluşturma](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline).


## <a name="adf-tables"></a>Tanımlar ve veri kümelerinin nasıl belirtmek için tablo oluşturma
Şu betiği tabanlı yordamları yapısı, konumu ve kullanılabilirlik kümelerinin belirtin tabloları oluşturun. JSON dosyaları, tabloları tanımlamak için kullanılır. Bu dosyaların yapısı hakkında daha fazla bilgi için bkz. [veri kümeleri](../../data-factory/concepts-datasets-linked-services.md).

> [!NOTE]
> Yürütülmesi gerektiğini `Add-AzureAccount` cmdlet'ini çalıştırmadan önce [yeni AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet'ini komut yürütme için doğru Azure aboneliği'nin seçili olduğunu doğrulayın. Bu cmdlet belgeleri için bkz. [Add-AzureAccount](/powershell/module/servicemanagement/azure/add-azureaccount?view=azuresmps-3.7.0).
>
>

JSON tabanlı tanımları tablolarda aşağıdaki adlar kullanın:

* **tablo adı** şirket içi SQL server olan *nyctaxi_data*
* **kapsayıcı adı** Azure Blob Depolama hesabıdır *containername*

Bu ADF işlem hattı için üç tablo tanımları gerekir:

1. [SQL şirket içi tablo](#adf-table-onprem-sql)
2. [BLOB tablosu](#adf-table-blob-store)
3. [SQL Azure tablosu](#adf-table-azure-sql)

> [!NOTE]
> Bu yordamlar, tanımlamak ve ADF etkinlikleri oluşturmak için Azure PowerShell kullanırsınız. Ancak bu görevleri Azure portalını kullanarak da gerçekleştirilebilir. Ayrıntılar için bkz [veri kümeleri oluşturma](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline).
>
>

### <a name="adf-table-onprem-sql"></a>SQL şirket içi tablo
Şirket içi SQL Server için tablo tanımı aşağıdaki JSON dosyasında belirtilir:

```json
{
    "name": "OnPremSQLTable",
    "properties":
    {
        "location":
        {
            "type": "OnPremisesSqlServerTableLocation",
            "tableName": "nyctaxi_data",
            "linkedServiceName": "adfonpremsql"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1,
            "waitOnExternal":
            {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Sütun adlarını burada bulunmuyordu. Sütun adları burada ekleyerek alt seçebilirsiniz (ayrıntılarını kontrol edin [ADF belgeleri](../../data-factory/copy-activity-overview.md) konu.

Adlı bir dosyaya tablosunun JSON tanımını kopyalama *onpremtabledef.json* dosya ve bilinen bir konuma kaydedin (burada varsayılır *C:\temp\onpremtabledef.json*). Tablo, aşağıdaki Azure PowerShell cmdlet'iyle ADF'de oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <a name="adf-table-blob-store"></a>BLOB tablosu
Çıktı blob konumu için tablo tanımıdır (Bu eşler alınan verileri şirket içinden Azure blob'a) aşağıdaki:

```json
{
    "name": "OutputBlobTable",
    "properties":
    {
        "location":
        {
            "type": "AzureBlobLocation",
            "folderPath": "containername",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": "\t"
            },
            "linkedServiceName": "adfds"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Adlı bir dosyaya tablosunun JSON tanımını kopyalama *bloboutputtabledef.json* dosya ve bilinen bir konuma kaydedin (burada varsayılır *C:\temp\bloboutputtabledef.json*). Tablo, aşağıdaki Azure PowerShell cmdlet'iyle ADF'de oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json

### <a name="adf-table-azure-sql"></a>SQL Azure tablosu
SQL Azure tablosu için tanımı çıkış aşağıdaki (Bu şema eşler blobundan gelen veriler):

```json
{
    "name": "OutputSQLAzureTable",
    "properties":
    {
        "structure":
        [
            { "name": "column1", "type": "String"},
            { "name": "column2", "type": "String"}
        ],
        "location":
        {
            "type": "AzureSqlTableLocation",
            "tableName": "your_db_name",
            "linkedServiceName": "adfdssqlazure_linked_servicename"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

Adlı bir dosyaya tablosunun JSON tanımını kopyalama *AzureSqlTable.json* dosya ve bilinen bir konuma kaydedin (burada varsayılır *C:\temp\AzureSqlTable.json*). Tablo, aşağıdaki Azure PowerShell cmdlet'iyle ADF'de oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json


## <a name="adf-pipeline"></a>Tanımlama ve işlem hattı oluşturma
Etkinlikler işlem hattına ait ve aşağıdaki betiği tabanlı yordamları ile işlem hattı oluşturma belirtin. Bir JSON dosyası, işlem hattı özelliklerini tanımlamak için kullanılır.

* Betik varsayar **işlem hattı adı** olduğu *AMLDSProcessPipeline*.
* Ayrıca, günlük olarak yürütülür ve iş (12: 00 UTC) için varsayılan yürütme süresi kullanmak için işlem hattının dönemsellik ayarladığımız unutmayın.

> [!NOTE]
> Aşağıdaki yordamlar, tanımlamak ve ADF işlem hattını oluşturmak için Azure PowerShell kullanın. Ancak, bu görevi Azure portalını kullanarak da gerçekleştirilebilir. Ayrıntılar için bkz [işlem hattı Oluştur](../../data-factory/tutorial-hybrid-copy-portal.md#create-a-pipeline).
>
>

Daha önce sağlanan tablo tanımlarını kullanarak ADF için işlem hattı tanımını gibi belirtilir:

```json
{
    "name": "AMLDSProcessPipeline",
    "properties":
    {
        "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL to Azure blob",
        "activities":
        [
            {
                "name": "CopyFromSQLtoBlob",
                "description": "Copy data from on-premises SQL server to blob",
                "type": "CopyActivity",
                "inputs": [ {"name": "OnPremSQLTable"} ],
                "outputs": [ {"name": "OutputBlobTable"} ],
                "transformation":
                {
                    "source":
                    {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from nyctaxi_data"
                    },
                    "sink":
                    {
                        "type": "BlobSink"
                    }
                },
                "Policy":
                {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            },
            {
                "name": "CopyFromBlobtoSQLAzure",
                "description": "Push data to Sql Azure",
                "type": "CopyActivity",
                "inputs": [ {"name": "OutputBlobTable"} ],
                "outputs": [ {"name": "OutputSQLAzureTable"} ],
                "transformation":
                {
                    "source":
                    {
                        "type": "BlobSource"
                    },
                    "sink":
                    {
                        "type": "SqlSink",
                        "WriteBatchTimeout": "00:5:00",
                    }
                },
                "Policy":
                {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 2,
                    "timeout": "02:00:00"
                }
            }
        ]
    }
}
```

Bu işlem hattı JSON tanımını bir dosyaya adlı kopya *pipelinedef.json* dosya ve bilinen bir konuma kaydedin (burada varsayılır *C:\temp\pipelinedef.json*). İşlem hattı, aşağıdaki Azure PowerShell cmdlet'iyle ADF'de oluşturun:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json


## <a name="adf-pipeline-start"></a>İşlem hattı başlatma
İşlem hattı artık aşağıdaki komutu kullanarak çalıştırılabilir:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

*Startdate* ve *enddate* parametre değerleri ile gerçek tarihleri arasında işlem hattının çalıştırılmasına istediğiniz değiştirilmesi gerekir.

İşlem hattı yürütür sonra blob, günde bir dosya için seçilen kapsayıcıdaki görünmesini verileri görebildiğine olmalıdır.

Biz ADF tarafından kanal verileri artımlı olarak sağlanan işlevselliği kullanılabilir değil olduğunu unutmayın. Bu ve diğer özellikleri, ADF tarafından sağlanan gerçekleştirme hakkında daha fazla bilgi için bkz. [ADF belgeleri](https://azure.microsoft.com/services/data-factory/).
