---
redirect_url: /azure/sql-data-warehouse/sql-data-warehouse-load-with-data-factory
title: "Azure Data Factory ile veri yükleme | Microsoft Belgeleri"
description: "Azure Data Factory ile veri yüklemeyi öğrenin"
services: sql-data-warehouse
documentationcenter: NA
author: twounder
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: ac7ddaa7-a3a5-4e15-b3cf-c696d2d105df
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: mausher;barbkess
translationtype: Human Translation
ms.sourcegitcommit: c0e2324a2b2e6294df6e502f2e7a0ae36ff94158
ms.openlocfilehash: 2f0aa3ab44813529525108758785ea3ceb65311b


---
# <a name="load-data-with-azure-data-factory"></a>Azure Data Factory ile veri yükleme
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)  
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [BCP](sql-data-warehouse-load-with-bcp.md)  
> 
> 

Bu öğreticide, Azure Depolama Blobundan Azure SQL Veri Ambarı'na veri taşımak üzere Azure Data Factory'de nasıl işlem hattı oluşturacağınız gösterilmiştir. Sonraki adımlarda şunları gerçekleştireceksiniz:

* Bir Azure Depolama Blobunda örnek veri oluşturma.
* Kaynakları Azure Data Factory'ye bağlama
* Storage Bloblarından SQL Data Warehouse'a veri taşımak üzere bir işlem hattı oluşturma

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a>Başlamadan önce
Azure Data Factory hakkında bilgi edinmek için bkz. [Azure Data Factory'ye giriş][Introduction to Azure Data Factory].

### <a name="create-or-identify-resources"></a>Kaynak oluşturma veya tanımlama
Bu öğreticiye başlamadan önce aşağıdaki kaynaklara sahip olmanız gerekir:

* **Azure Storage Blobu**: Bu öğreticide Azure Data Factory işlem hattı için veri kaynağı olarak Azure Storage Blobu kullanılır; bu nedenle örnek verileri saklamak için bir Azure Storage Blobuna sahip olmanız gerekir. Azure Depolama Blobunuz yoksa [Depolama hesabı oluşturma][Create a storage account] işlemini nasıl gerçekleştireceğinizi öğrenin.
* **SQL Data Warehouse**: Bu öğreticide Azure Storage Blobundan SQL Data Warehouse'a veri taşınır; bu nedenle AdventureWorksDW örnek verileriyle yüklü çevrimiçi bir veri ambarınızın olması gerekir. Veri ambarınız yoksa nasıl [sağlayacağınızı][Create a SQL Data Warehouse] öğrenin. Veri ambarınız var ancak bu veri ambarına örnek veri sağlamadıysanız [el ile yükleme][Load sample data into SQL Data Warehouse] işlemini gerçekleştirebilirsiniz.
* **Azure Data Factory**: Azure Data Factory, gerçek yükü tamamladığından veri taşıma işlem hattını oluşturmak üzere kullanmak için bir tanesine sahip olmanız gerekir. Henüz sahip değilseniz [Azure Data Factory’yi kullanmaya başlama (Data Factory Düzenleyicisi)][Get started with Azure Data Factory (Data Factory Editor)] bölümünün 1. adımında nasıl oluşturacağınızı öğrenin.
* **AZCopy**: Yerel istemcinizden Azure Storage Blobuna örnek veri kopyalamak için AZCopy gerekir. Yükleme yönergeleri için bkz. [AZCopy belgeleri][AZCopy documentation].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>1. Adım: Azure Storage Blobuna örnek veri kopyalama
Her şey hazırsa Azure Depolama Blobunuza örnek veri kopyalamaya başlayabilirsiniz.

1. [Örnek verileri indirme][Download sample data]. Bu işlem sonrasında AdventureWorksDW örnek verilerinize üç yıla ilişkin satış verileri eklenir.
2. Üç yıla ilişkin verileri Azure Storage Blobunuza kopyalamak için bu AZCopy komutunu kullanın.
   
    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````

## <a name="step-2-connect-resources-to-azure-data-factory"></a>2. Adım: Kaynakları Azure Data Factory'ye bağlama
Verileri aldığınıza göre verileri Azure Blob depolama alanından SQL Data Warehouse'a taşımak üzere Azure Data Factory işlem hattını oluşturabiliriz.

Başlamak için [Azure portalını][Azure portal] açıp sol taraftaki menüden veri fabrikanızı seçin.

### <a name="step-21-create-linked-service"></a>2.1. Adım: Bağlı Hizmet oluşturma
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

### <a name="step-22-define-the-dataset"></a>2.2. Adım: Veri kümesini tanımlama
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
3. Şimdi SQL Veri Ambarı'na ilişkin veri kümemizi tanımlayacağız. Aynı şekilde başlayarak önce "Yeni veri kümesi" seçeneğine ve ardından "Azure SQL Data Warehouse" seçeneğine tıklıyoruz.
   
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

## <a name="step-3-create-and-run-your-pipeline"></a>3. Adım: İşlem hattınızı oluşturma ve çalıştırma
Son olarak Azure Data Factory'de işlem hattı oluşturup çalıştıracağız.  Bu işlem ile gerçek veri taşıma işlemi tamamlanır.  SQL Veri Ambarı ve Azure Data Factory ile gerçekleştirebileceğiniz işlemlerin tam görünümünü [burada][Move data to and from Azure SQL Data Warehouse using Azure Data Factory] bulabilirsiniz.

‘Geliştir ve Dağıt’ bölümünde ‘Daha Fazla Komut’ seçeneğine ve ardından ‘Yeni İşlem Hattı’ seçeneğine tıklayın.  İşlem hattını oluşturduktan sonra verileri veri ambarınıza aktarmak için aşağıdaki kodu kullanabilirsiniz:

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

## <a name="next-steps"></a>Sonraki adımlar
Şunları görüntüleyerek daha fazla bilgi edinmeye başlayabilirsiniz:

* [Azure Data Factory öğrenme yolu][Azure Data Factory learning path].
* [Azure SQL Veri Ambarı Bağlayıcısı][Azure SQL Data Warehouse Connector]. Bu, Azure SQL Data Warehouse ile Azure Data Factory kullanımına yönelik temel başvuru konu başlığıdır.

Bu konu başlıklarında Azure Data Factory hakkında ayrıntılı bilgi sağlanmıştır. Burada Azure SQL Veritabanı veya HDinsight hakkında bilgi verilmiştir, ancak bu bilgiler Azure SQL Veri Ambarı için de geçerlidir.

* [Öğretici: Azure Data Factory ile çalışmaya başlama][Tutorial: Get started with Azure Data Factory] Azure Data Factory ile veri işlemeye yönelik temel öğreticidir. Bu öğreticide, web günlüklerini aylık olarak dönüştürmek ve çözümlemek üzere HDInsight'ı kullanan ilk işlem hattınızı oluşturacaksınız. Not: Bu öğreticide herhangi bir kopyalama etkinliği yoktur.
* [Öğretici: Azure Depolama Blobundan Azure SQL Veritabanı'na veri kopyalama][Tutorial: Copy data from Azure Storage Blob to Azure SQL Database]. Bu öğreticide, Azure Depolama Blobundan Azure SQL Veritabanı'na veri kopyalamak için Azure Data Factory'de bir işlem hattı oluşturursunuz.

<!--Image references-->

<!--Article references-->
[AZCopy documentation]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Create a storage account]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Get started with Azure Data Factory (Data Factory Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Introduction to Azure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Tutorial: Copy data from Azure Storage Blob to Azure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Tutorial: Get started with Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory learning path]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Download sample data]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv



<!--HONumber=Jan17_HO5-->


