<properties
   pageTitle="Azure Data Factory ile veri yükleme | Microsoft Azure"
   description="Azure Data Factory ile veri yüklemeyi öğrenin"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="04/29/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Azure Data Factory ile veri yükleme

> [AZURE.SELECTOR]
- [Data Factory][]
- [PolyBase][]
- [BCP][]

 Bu öğreticide, Azure Storage Blobundan SQL Data Warehouse'a veri taşımak üzere Azure Data Factory'de nasıl işlem hattı oluşturacağınız gösterilmiştir. Sonraki adımlarda şunları gerçekleştireceksiniz:

+ Bir Azure Storage Blobunda örnek veri oluşturma
+ Kaynakları Azure Data Factory'ye bağlama
+ Storage Bloblarından SQL Data Warehouse'a veri taşımak üzere bir işlem hattı oluşturma

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## Başlamadan önce

Azure Data Factory hakkında bilgi edinmek için bkz. [Azure Data Factory'ye giriş][].

### Kaynak oluşturma veya tanımlama

Bu öğreticiye başlamadan önce aşağıdaki kaynaklara sahip olmanız gerekir.

   + **Azure Storage Blobu**: Bu öğreticide Azure Data Factory işlem hattı için veri kaynağı olarak Azure Storage Blobu kullanılır; bu nedenle örnek verileri saklamak için bir Azure Storage Blobuna sahip olmanız gerekir. Azure Storage Blobunuz yoksa [Depolama hesabı oluşturma][] işlemini nasıl gerçekleştireceğinizi öğrenin.

   + **SQL Data Warehouse**: Bu öğreticide Azure Storage Blobundan SQL Data Warehouse'a veri taşınır; bu nedenle AdventureWorksDW örnek verileriyle yüklü çevrimiçi bir veri ambarınızın olması gerekir. Veri ambarınız yoksa nasıl [sağlayacağınızı][SQL Data Warehouse oluşturma] öğrenin. Veri ambarınız var ancak bu veri ambarına örnek veri sağlamadıysanız [el ile yükleme][SQL Data Warehouse'a örnek veri yükleme] işlemini gerçekleştirebilirsiniz.

   + **Azure Data Factory**: Azure Data Factory gerçek yüklemeyi tamamlar; bu nedenle veri taşıma işlem hattı oluşturmak için kullanmak üzere bir Azure Data Factory'nizin olması gerekir. Azure Data Factory'niz yoksa [Azure Data Factory (Data Factory Editor) ile çalışmaya başlama][] makalesinin 1. Adımına göz atarak nasıl oluşturacağınızı öğrenin.

   + **AZCopy**: Yerel istemcinizden Azure Storage Blobuna örnek veri kopyalamak için AZCopy gerekir. Yükleme yönergeleri için bkz. [AZCopy belgeleri][].

## 1. Adım: Azure Storage Blobuna örnek veri kopyalama

Her şey hazırsa Azure Storage Blobunuza örnek veri kopyalamaya başlayabilirsiniz.

1. [Örnek veri indirin][]. Bu işlem sonrasında AdventureWorksDW örnek verilerinize üç yıla ilişkin satış verileri eklenir.

2. Üç yıla ilişkin verileri Azure Storage Blobunuza kopyalamak için bu AZCopy komutunu kullanın.

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## 2. Adım: Kaynakları Azure Data Factory'ye bağlama

Verileri aldığınıza göre verileri Azure Blob depolama alanından SQL Data Warehouse'a taşımak üzere Azure Data Factory işlem hattını oluşturabiliriz.

Başlamak için [Azure portalını][] açıp sol taraftaki menüden veri fabrikanızı seçin.

### 2.1. Adım: Bağlı Hizmet oluşturma

Azure depolama hesabınızı ve SQL Data Warehouse'unuzu veri fabrikanıza bağlayın.  

1. Öncelikle veri fabrikanızın "Bağlı Hizmetler" bölümüne ve ardından "Yeni veri deposu" öğesine tıklayarak kayıt işlemine başlayın. Azure depolama alanınızı hangi adla kaydedeceğinizi seçin. Tür olarak Azure Storage'ı belirleyip Hesap Adı ve Hesap Anahtarı bilgilerinizi girin.

2. SQL Data Warehouse'u kaydetmek için "Geliştir ve Dağıt" bölümüne giderek "Yeni Veri Deposu" seçeneğini ve ardından "Azure SQL Data Warehouse" seçeneğini belirleyin. Kopyalama ve yapıştırma işlemlerini bu şablonda gerçekleştirip size özgü bilgileri girin.

```JSON
{
    "name": "<Linked Service Name>",
    "properties": {
        "description": "",
        "type": "AzureSqlDW",
        "typeProperties": {
             "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
         }
    }
}
```

### 2.2. Adım: Veri kümesini tanımlama

Bağlı hizmetleri oluşturduktan sonra veri kümelerini tanımlamamız gerekir.  Bu, depolama alanınızdan veri ambarınıza taşınan verilerin yapısının tanımlanması anlamına gelir.  Oluşturma işlemi hakkında daha fazla bilgi edinebilirsiniz

1. Veri fabrikanızın "Geliştir ve Dağıt" bölümüne giderek bu işlemi başlatın.

2. Depolama alanınızı veri fabrikanıza bağlamak için "Yeni veri kümesi" ve ardından "Azure Blob depolama alanı" seçeneğine tıklayın.  Azure Blob depolama alanındaki verilerinizi tanımlamak için şu betiği kullanabilirsiniz:

```JSON
{
    "name": "<Dataset Name>",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "<linked storage name>",
        "typeProperties": {
            "folderPath": "<containter name>",
            "fileName": "FactInternetSales.csv",
            "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```


3. Şimdi biz de SQL Data Warehouse'a ilişkin veri kümemizi tanımlayacağız.  Aynı şekilde başlayarak önce "Yeni veri kümesi" seçeneğine ve ardından "Azure SQL Data Warehouse" seçeneğine tıklıyoruz.

```JSON
{
    "name": "DWDataset",
    "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "FactInternetSales"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

## 3. Adım: İşlem hattınızı oluşturma ve çalıştırma

Son olarak Azure Data Factory'de işlem hattı oluşturup çalıştıracağız.  Bu işlem ile gerçek veri taşıma işlemi tamamlanır.  SQL Data Warehouse ve Azure Data Factory ile gerçekleştirebileceğiniz işlemlerin tam görünümünü [burada][Azure Data Factory kullanarak Azure'a/Azure'dan veri taşıma] bulabilirsiniz.

Şimdi ise "Geliştir ve Dağıt" bölümünde "Daha Fazla Komut" seçeneğine ve ardından "Yeni İşlem Hattı" seçeneğine tıklayın.  İşlem hattını oluşturduktan sonra verileri veri ambarınıza aktarmak için aşağıdaki kodu kullanabilirsiniz:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
              }
            ],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## Sonraki adımlar

Şunları görüntüleyerek daha fazla bilgi edinmeye başlayabilirsiniz:

- [Azure Data Factory öğrenme yolu][]
- [Azure SQL Data Warehouse Bağlayıcısı][]. Bu, Azure SQL Data Warehouse ile Azure Data Factory kullanımına yönelik temel başvuru konu başlığıdır.


Bu konu başlıklarında Azure Data Factory hakkında ayrıntılı bilgi sağlanmıştır. Bunlarda, Azure SQL Database veya HDinsight hakkında bilgi verilmiştir ancak bu bilgiler Azure SQL Data Warehouse için de geçerlidir.

- [Öğretici: Azure Data Factory ile çalışmaya başlama][] Bu, Azure Data Factory ile veri işlemeye yönelik temel öğreticidir. Bu öğreticide web günlüklerini aylık olarak dönüştürmek ve çözümlemek üzere HDInsight'ı kullanan ilk işlem hattınızı oluşturacaksınız. Not: Bu öğreticide herhangi bir kopyalama etkinliği yoktur.
- [Öğretici: Azure Storage Blobundan Azure SQL Database'e veri kopyalama][]. Bu öğreticide Azure Storage Blobundan Azure SQL Database'e veri kopyalamak üzere Azure Data Factory'de bir işlem hattı oluşturacaksınız.
- [Gerçek dünya senaryoları öğreticisi][]. Bu, Azure Data Factory kullanımına yönelik kapsamlı bir öğreticidir.

<!--Image references-->

<!--Article references-->
[AZCopy belgeleri]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Bağlayıcısı]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[SQL Data Warehouse oluşturma]: sql-data-warehouse-get-started-provision.md
[Depolama hesabı oluşturma]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Azure Data Factory (Data Factory Editor) ile çalışmaya başlama]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Azure Data Factory'ye giriş]: ../data-factory/data-factory-introduction.md
[SQL Data Warehouse'a örnek veri yükleme]: sql-data-warehouse-load-sample-databases.md
[Azure Data Factory kullanarak Azure'a/Azure'dan veri taşıma]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Gerçek dünya senaryoları öğreticisi]: ../data-factory/data-factory-tutorial.md
[Öğretici: Azure Storage Blobundan Azure SQL Database'e veri kopyalama]: ../data-factory/data-factory-get-started
[Öğretici: Azure Data Factory ile çalışmaya başlama]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory öğrenme yolu]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portalını]: https://portal.azure.com
[Örnek veri indirin]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv



<!--HONumber=Aug16_HO1-->


