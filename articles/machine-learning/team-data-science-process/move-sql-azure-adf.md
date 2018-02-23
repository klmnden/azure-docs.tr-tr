---
title: "Azure Data Factory ile SQL Azure için bir şirket içi SQL Server'dan veri taşıma | Microsoft Docs"
description: "Bulutta içi veritabanları arasında verileri günlük olarak birlikte taşımak iki veri taşıma etkinlikleri oluşturur bir ADF ardışık ayarlayın."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 36837c83-2015-48be-b850-c4346aa5ae8a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: 05884fd39db284e268f31987e5ad7a47b9f87ebf
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="move-data-from-an-on-premises-sql-server-to-sql-azure-with-azure-data-factory"></a>Azure Data Factory ile SQL Azure için bir şirket içi SQL Server'dan veri taşıma
Bu konu, Azure veri fabrikası (ADF) kullanarak verileri bir şirket içi SQL Server veritabanından bir SQL Azure veritabanına Azure Blob Storage nasıl taşınacağı gösterir.

Bir Azure SQL veritabanına veri taşıma için çeşitli seçenekler özetler bir tablo için bkz: [veri taşıma bir Azure SQL veritabanına Azure Machine Learning için](move-sql-azure.md).

## <a name="intro"></a>Giriş: ADF nedir ve ne zaman, verileri geçirmek için kullanılmalı?
Azure Data Factory düzenler ve taşınmasını ve dönüştürülmesini veri otomatikleştiren bir bulut tabanlı tam olarak yönetilen bir veri tümleştirme hizmetidir. Anahtar ADF modelinde ardışık düzen kavramdır. Bir işlem hattı her biri veri kümelerinde bulunan veriler üzerinde gerçekleştirilecek eylemleri tanımlar mantıksal etkinlikleri grubudur. Bağlı hizmetler, veri fabrikası'nın veri kaynaklarına bağlanmak için gereken bilgileri tanımlamak için kullanılır.

ADF ile mevcut veri işleme hizmetlerini bulutta yüksek oranda kullanılabilir ve yönetilen veri ardışık içine birleştirilebilir. Bu veri ardışık alma, hazırlamak, dönüştürmek, analiz ve veri yayımlamak için zamanlanabilir ve ADF yönetir ve karmaşık veri ve işleme bağımlılıkları yönetir. Çözümleri hızlı bir şekilde oluşturulur ve şirket içi artan sayıda bağlanma buluta dağıtılan ve bulut veri kaynakları.

ADF kullanmayı dikkate alın:

* sürekli olarak geçirilecek verilere ihtiyaç duyduğunda her ikisi de erişen karma bir senaryoda şirket içi ve bulut kaynakları
* ne zaman veri işlem yapılan işlem veya değiştirilmiş veya iş mantığı kendisine geçirilen zaman eklenmiş olması gerekir.

ADF zamanlama ve işleri düzenli aralıklarla veri hareketini yönetmek basit JSON komut dosyaları kullanarak izlenmesini sağlar. ADF karmaşık işlemleri için destek gibi diğer özellikleri de vardır. Adresindeki ADF hakkında daha fazla bilgi için bkz: [Azure veri fabrikası (ADF)](https://azure.microsoft.com/services/data-factory/).

## <a name="scenario"></a>Senaryo
İki veri taşıma etkinlikleri oluşturur bir ADF ardışık ayarlarız. Birlikte bunlar verileri günlük olarak bir şirket içi SQL database ve Azure SQL Database'i bulutta arasında taşıyın. İki etkinlik şunlardır:

* bir Azure Blob Storage hesabı için bir şirket içi SQL Server veritabanından veri kopyalama
* verileri Azure Blob Depolama hesabından bir Azure SQL veritabanına kopyalayın.

> [!NOTE]
> Burada olmuştur ADF ekibi tarafından sağlanan daha ayrıntılı öğreticiden uyarlanmış gösterilen adımlar: [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md) uygun olduğunda bu konusunun ilgili bölümlerine başvurular sağlanır.
>
>

## <a name="prereqs"></a>Önkoşullar
Bu öğretici, sahip olduğunuz varsayılmaktadır:

* Bir **Azure aboneliği**. Bir aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
* Bir **Azure depolama hesabı**. Bu öğreticide verileri depolamak için bir Azure depolama hesabı kullanın. Azure depolama hesabınız yoksa [Depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account) makalesine bakın. Depolama hesabını oluşturduktan sonra, depolamaya erişmek için kullanılan hesap anahtarını edinmeniz gerekir. Bkz: [depolama erişim tuşlarınızı yönetme](../../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Erişim bir **Azure SQL veritabanı**. Bir Azure SQL veritabanı, konu ayarlanması varsa [Microsoft Azure SQL veritabanı ile çalışmaya başlama ](../../sql-database/sql-database-get-started.md) bir Azure SQL veritabanına yeni bir örneğini sağlayacak bilgiler verilmektedir.
* Yüklenmiş ve yapılandırılmış **Azure PowerShell** yerel olarak. Yönergeler için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

> [!NOTE]
> Bu yordamı kullanır [Azure portal](https://portal.azure.com/).
>
>

## <a name="upload-data"></a> Şirket içi SQL Server'ınızı veri yükleme
Kullanırız [NYC ücreti dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) geçiş işlemini göstermek için. NYC ücreti dataset Azure blob depolama o post belirtildiği gibi kullanılabilir [NYC ücreti verileri](http://www.andresmh.com/nyctaxitrips/). Verileri iki dosya, seyahat ayrıntılarını içeren, trip_data.csv dosya ve her seyahat için ödenen ücreti ayrıntılarını içeren trip_far.csv dosyası vardır. Bir örnek ve bu dosyaların açıklaması sağlanan [NYC ücreti dönüşleri veri kümesi tanımı](sql-walkthrough.md#dataset).

Burada, kendi veri kümesi için sağlanan yordamı uyarlamak veya NYC ücreti dataset kullanarak açıklanan adımları izleyin. NYC ücreti veri kümesi şirket içi SQL Server veritabanınıza karşıya yüklemek için özetlenen yordamı izleyin [toplu içeri aktarma verileri SQL Server veritabanına](sql-walkthrough.md#dbload). SQL Server üzerinde bir Azure sanal makine için bu yönergeleri bağlıdır, ancak şirket içi SQL Server'a yükleme yordamı aynıdır.

## <a name="create-adf"></a> Bir Azure Data Factory oluşturma
Yeni bir Azure Data Factory ve bir kaynak grubu oluşturmak için yönergeleri [Azure portal](https://portal.azure.com/) sağlanan [bir Azure Data Factory oluşturmak](../../data-factory/v1/data-factory-build-your-first-pipeline-using-editor.md#create-a-data-factory). Yeni ADF örnek adı *adfdsp* ve oluşturulan kaynak grubu adı *adfdsprg*.

## <a name="install-and-configure-up-the-data-management-gateway"></a>Yükleme ve veri yönetimi ağ geçidi yapılandırma
Bir şirket içi SQL Server ile çalışmak için bir Azure data factory'de işlem hatlarınızı etkinleştirmek için bu data factory bağlantılı bir hizmet olarak eklemeniz gerekir. Bir şirket içi SQL Server için bağlı hizmet oluşturmak için şunları yapmanız gerekir:

* indirin ve Microsoft Veri Yönetimi ağ geçidi şirket içi bilgisayara yükleyin.
* bağlantılı hizmeti ağ geçidini kullanmak için şirket içi veri kaynağı için yapılandırın.

Veri Yönetimi ağ geçidi serileştirir ve burada barındırılan bilgisayar üzerinde kaynak ve havuz verileri seri durumdan çıkarır.

Kurulum yönergeleri ve veri yönetimi ağ geçidi hakkında ayrıntılar için bkz: [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md)

## <a name="adflinkedservices"></a>Veri kaynaklarına bağlanmak için bağlı hizmetler oluşturma
Bağlı hizmet Azure veri fabrikası'nın bir veri kaynağına bağlanmak için gereken bilgileri tanımlar. Bağlı hizmetler gerekli olan bu senaryoda üç kaynakları sahibiz:

1. Şirket içi SQL Server
2. Azure Blob Depolama
3. Azure SQL veritabanı

Bağlı hizmetler oluşturmak için adım adım yordam sağlanan [bağlı hizmetler oluşturma](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).


## <a name="adf-tables"></a>Tanımlama ve veri kümeleri erişmek nasıl belirlemek için tabloları oluşturma
Aşağıdaki betik tabanlı yordamları yapısını, konum ve veri kümelerinin kullanılabilirliğini belirtin tablolar oluşturun. JSON dosyaları tabloları tanımlamak için kullanılır. Bu dosyalar yapısı hakkında daha fazla bilgi için bkz: [veri kümeleri](../../data-factory/v1/data-factory-create-datasets.md).

> [!NOTE]
> Yürütme `Add-AzureAccount` yürütmeden önce cmdlet [yeni AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet'ini sağ Azure aboneliği için komut yürütme seçili olduğunu doğrulayın. Bu cmdlet belgeleri için bkz: [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).
>
>

Tablolar JSON tabanlı tanımlarında aşağıdaki adları kullanın:

* **tablo adı** şirket içi SQL sunucusudur *nyctaxi_data*
* **kapsayıcı adı** Azure Blob Storage'da hesabıdır *kapsayıcı adı*  

Üç tablo tanımları bu ADF ardışık düzeni için gereklidir:

1. [SQL şirket içi tablosu](#adf-table-onprem-sql)
2. [BLOB tablosu ](#adf-table-blob-store)
3. [SQL Azure tablo](#adf-table-azure-sql)

> [!NOTE]
> Tanımlama ve ADF etkinlikleri oluşturmak için Azure PowerShell Bu yordamları kullanın. Ancak bu görevleri Azure Portalı'nı kullanarak da gerçekleştirilebilir. Ayrıntılar için bkz [veri kümeleri oluşturma](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).
>
>

### <a name="adf-table-onprem-sql">SQL şirket içi tablosu</a>
Şirket içi SQL Server için tablo tanımı aşağıdaki JSON dosyasında belirtilir:

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

Sütun adlarını buraya dahil değil. Sütun adları burada ekleyerek alt seçebilirsiniz (Ayrıntılar için denetleyin [ADF belgelerine](../../data-factory/v1/data-factory-data-movement-activities.md) konu.

Tablosunun JSON tanımını bir dosyaya adlı kopya *onpremtabledef.json* dosya ve bilinen bir konuma kaydedin (burada varsayılır *C:\temp\onpremtabledef.json*). Tablo içinde ADF aşağıdaki Azure PowerShell cmdlet'iyle oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <a name="adf-table-blob-store">BLOB tablosu </a>
Tablosu için çıkış blob konumu tanımıdır (Bu eşler şirket içi Azure blob alınan verileri) aşağıdaki:

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

Tablosunun JSON tanımını bir dosyaya adlı kopya *bloboutputtabledef.json* dosya ve bilinen bir konuma kaydedin (burada varsayılır *C:\temp\bloboutputtabledef.json*). Tablo içinde ADF aşağıdaki Azure PowerShell cmdlet'iyle oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <a name="adf-table-azure-sql">SQL Azure tablo</a>
SQL Azure tablo tanımı çıkış (Bu şemayı eşlemeleri blobundan gelen veriler) aşağıdaki:

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
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

Tablosunun JSON tanımını bir dosyaya adlı kopya *AzureSqlTable.json* dosya ve bilinen bir konuma kaydedin (burada varsayılır *C:\temp\AzureSqlTable.json*). Tablo içinde ADF aşağıdaki Azure PowerShell cmdlet'iyle oluşturun:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


## <a name="adf-pipeline"></a>Tanımlama ve işlem hattı oluşturma
Ardışık düzene ait olan ve aşağıdaki komut dosyası tabanlı yordamları ile işlem hattı oluşturma etkinlikleri belirtin. Bir JSON dosyası ardışık düzen özelliklerini tanımlamak için kullanılır.

* Komut dosyası varsayar **ardışık düzen adı** olan *AMLDSProcessPipeline*.
* Ayrıca, günlük olarak yürütülen ve varsayılan yürütme süresi işi (00: 00 UTC) kullanmak için ardışık düzen periyodikliğini ayarlarız unutmayın.

> [!NOTE]
> Aşağıdaki yordamlar tanımlayın ve ADF ardışık düzen oluşturmak için Azure PowerShell kullanın. Ancak, bu görevi Azure Portalı'nı kullanarak da gerçekleştirilebilir. Ayrıntılar için bkz [oluşturma ardışık düzen](../../data-factory/v1/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).
>
>

Daha önce sağlanan tablo tanımları kullanarak ADF ardışık düzen tanımı gibi belirtilir:

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

Bu ardışık düzen JSON tanımı dosyasına adlı kopya *pipelinedef.json* dosya ve bilinen bir konuma kaydedin (burada varsayılır *C:\temp\pipelinedef.json*). Ardışık Düzen ADF içinde aşağıdaki Azure PowerShell cmdlet'iyle oluşturun:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json


## <a name="adf-pipeline-start"></a>Ardışık Düzen Başlat
Ardışık Düzen aşağıdaki komutu kullanarak şimdi çalıştırabilirsiniz:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

*Startdate* ve *enddate* parametre değerlerini çalıştırmak için ardışık düzen arasında istediğiniz gerçek tarihleri ile değiştirilmesi gerekir.

Ardışık Düzen yürütür sonra blob, günde bir dosya için seçilen kapsayıcıdaki görünmesini verileri görmek görebilmeniz gerekir.

Biz tarafından ADF kanal veri için artımlı olarak sağlanan işlevselliği işlevden olmayan olduğunu unutmayın. Bu ve ADF tarafından sağlanan diğer özellikleri hakkında daha fazla bilgi için bkz: [ADF belgelerine](https://azure.microsoft.com/services/data-factory/).
