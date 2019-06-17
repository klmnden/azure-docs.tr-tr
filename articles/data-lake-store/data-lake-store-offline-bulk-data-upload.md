---
title: Çevrimdışı yöntemleri kullanarak Azure Data Lake depolama Gen1 büyük miktarlarda veri yükleme | Microsoft Docs
description: Azure Data Lake depolama Gen1 için Azure depolama bloblarından veri kopyalamak için AdlCopy aracını kullanın
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 45321f6a-179f-4ee4-b8aa-efa7745b8eb6
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 4a8126d658f227d9eed372cd51cf06f8f12c99f9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60194986"
---
# <a name="use-the-azure-importexport-service-for-offline-copy-of-data-to-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 veri çevrimdışı kopyalama için Azure içeri/dışarı aktarma hizmetini kullanma
Bu makalede, büyük veri kümelerini kopyalama hakkında bilgi edineceksiniz (> 200 GB) gibi çevrimdışı kopya yöntemleri kullanarak Azure Data Lake depolama Gen1 içine [Azure içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md). Özellikle, bu makaledeki örnek olarak kullanılan 339,420,860,416 bayt veya yaklaşık 319 GB disk üzerinde dosyasıdır. Bu dosya 319GB.tsv adlandıralım.

Azure içeri/dışarı aktarma hizmeti, daha güvenli bir şekilde büyük miktarlarda verinin bir Azure veri merkezine sabit disk sürücüleri sevkiyat tarafından Azure Blob depolama alanına aktarmak için yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure depolama hesabınız**.
* **Bir Azure Data Lake depolama Gen1 hesap**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md)

## <a name="preparing-the-data"></a>Verileri hazırlama

İçeri/dışarı aktarma hizmetini kullanmadan önce aktarılacak veri dosya sonu **en az 200 GB olan kopya halinde** boyutu. İçeri aktarma aracı 200 GB'tan büyük dosyalarla çalışmaz. Bu öğreticide, biz dosya 100 GB'lık parçalara bölün. Kullanarak bunu yapabilirsiniz [Cygwin](https://cygwin.com/install.html). Cygwin Linux komutlarını destekler. Bu durumda, aşağıdaki komutu kullanın:

    split -b 100m 319GB.tsv

Bölme işlemi aşağıdaki adlarla dosyaları oluşturur.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Veri diskleri hazırlanın
Bölümündeki yönergeleri [Azure içeri/dışarı aktarma hizmetini kullanarak](../storage/common/storage-import-export-service.md) (altında **sürücülerinizin hazırlama** bölümü) sabit sürücülerinizi hazırlamak için. Genel sırası şöyledir:

1. Azure içeri/dışarı aktarma hizmeti için kullanılacak gereksinimini karşılayan bir sabit disk edinin.
2. Verileri Azure veri merkezi IP'sine sevk edildikten sonra kopyalanacağı bir Azure depolama hesabını belirleyin.
3. Kullanım [Azure içeri/dışarı aktarma aracı](https://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), bir komut satırı yardımcı programı. Aracının nasıl kullanılacağını gösteren bir örnek kod parçacığı aşağıda verilmiştir.

    ```
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ```
    Bkz: [Azure içeri/dışarı aktarma hizmetini kullanarak](../storage/common/storage-import-export-service.md) daha fazla örnek kod parçacıkları için.
4. Yukarıdaki komut, belirtilen konumda bir günlük dosyası oluşturur. Öğesinden içeri aktarma işi oluşturmak için bu günlük dosyası kullanmak [Azure portalında](https://portal.azure.com).

## <a name="create-an-import-job"></a>İçeri aktarma işi oluşturma
İçindeki yönergeleri kullanarak içeri aktarma işi artık oluşturabilirsiniz [Azure içeri/dışarı aktarma hizmetini kullanarak](../storage/common/storage-import-export-service.md) (altında **içeri aktarma işi oluşturma** bölümü). Diğer ayrıntıları ile bu içeri aktarma işi için disk sürücülerini hazırlanırken oluşturulan günlük dosyasını da sağlar.

## <a name="physically-ship-the-disks"></a>Diski fiziksel olarak gönderin
Diskleri Azure veri merkezi artık fiziksel olarak gönderebilirsiniz. Burada, veriler üzerinden Azure depolama BLOB'ları içeri aktarma işi oluşturma sırasında sağladığınız kopyalanır. Ayrıca, proje oluştururken, daha sonra izleme bilgisi sağlamak üzere seçtiyseniz artık geri alma işinize gidin ve takip numarasını güncelleştirin.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 için Azure depolama bloblarından veri kopyalama
İçeri aktarma işinin durumu tamamlandı olduğunu gösterir. sonra veriler, belirttiğiniz Azure depolama BLOB'ları kullanılabilir olup olmadığını doğrulayabilirsiniz. Ardından, verileri Azure Data Lake depolama Gen1 bloblarından taşımak için çeşitli yöntemler kullanabilirsiniz. Karşıya veri yükleme için tüm kullanılabilir seçenekleri için bkz: [Data Lake depolama Gen1 veri alma](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-storage-gen1).

Bu bölümde, veri kopyalamak için bir Azure Data Factory işlem hattı oluşturmak için kullanabileceğiniz JSON tanımları ile sunuyoruz. Bu JSON tanımları kullanabileceğiniz [Azure portalında](../data-factory/tutorial-copy-data-portal.md) veya [Visual Studio](../data-factory/tutorial-copy-data-dot-net.md).

### <a name="source-linked-service-azure-storage-blob"></a>Bağlı kaynağı hizmeti (Azure depolama blobu)
```
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="target-linked-service-azure-data-lake-storage-gen1"></a>Bağlı hizmeti (Azure Data Lake depolama Gen1) hedef
```
{
    "name": "AzureDataLakeStorageGen1LinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Storage Gen1 account with your access rights>",
            "dataLakeStoreUri": "https://<adlsg1_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
```
### <a name="input-data-set"></a>Giriş veri kümesi
```
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
### <a name="output-data-set"></a>Çıkış veri kümesi
```
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStorageGen1LinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
```
### <a name="pipeline-copy-activity"></a>İşlem hattı (kopyalama etkinliği)
```
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
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
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
```
Daha fazla bilgi için [Azure Data Factory kullanarak Azure depolama blobu'ndan Azure Data Lake depolama Gen1 için veri taşıma](../data-factory/connector-azure-data-lake-store.md).

## <a name="reconstruct-the-data-files-in-azure-data-lake-storage-gen1"></a>Azure Data Lake depolama Gen1 veri dosyaları yeniden yapılandırma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir dosyayla 319 GB ve böylece Azure içeri/dışarı aktarma hizmetini kullanarak aktarılabilir, aşağı daha küçük boyut dosyalarına kesildi Başladık. Verileri Azure Data Lake depolama Gen1 olduğuna göre biz dosyanın özgün boyutuna yeniden oluşturabilir. Bunu yapmak için aşağıdaki Azure PowerShell cmdlet'lerini kullanabilirsiniz.

```
# Login to our account
Connect-AzAccount

# List your subscriptions
Get-AzSubscription

# Switch to the subscription you want to work with
Set-AzContext -SubscriptionId
Register-AzResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzDataLakeStoreItem -AccountName "<adlsg1_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv"
```

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake depolama Gen1 ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight ile Data Lake depolama Gen1 kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
