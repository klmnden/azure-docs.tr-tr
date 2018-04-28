---
title: Büyük miktarlarda verinin çevrimdışı yöntemleri kullanarak Data Lake Store karşıya yükleme | Microsoft Docs
description: Azure Storage bloblarından Data Lake Store'a veri kopyalamak için AdlCopy aracını kullanın
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 45321f6a-179f-4ee4-b8aa-efa7745b8eb6
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/30/2018
ms.author: nitinme
ms.openlocfilehash: ee6f4ab1ac5892536d7f419c198158dc34d6f49e
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="use-the-azure-importexport-service-for-offline-copy-of-data-to-data-lake-store"></a>Data Lake Store için çevrimdışı veri kopyası için Azure içeri/dışarı aktarma hizmeti kullanma
Bu makalede, büyük veri kümeleri kopyalamak nasıl öğreneceksiniz (> 200 GB) gibi çevrimdışı kopya yöntemleri kullanarak bir Azure Data Lake Store içine [Azure içeri/dışarı aktarma hizmeti](../storage/common/storage-import-export-service.md). Özellikle, bu makaledeki örnek olarak kullanılan 339,420,860,416 bayt ya da yaklaşık 319 GB disk üzerindeki dosyasıdır. Şimdi bu dosya 319GB.tsv çağırın.

Azure içeri/dışarı aktarma hizmeti, büyük miktarlarda verinin daha güvenli bir şekilde Azure Blob depolama alanına bir Azure veri merkezi sabit disk sürücülerine sevkiyat tarafından aktarmasına yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure depolama hesabı**.
* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)

## <a name="preparing-the-data"></a>Veri hazırlama

İçeri/dışarı aktarma hizmeti kullanmadan önce aktarılacak veri dosyası bölün **200 GB daha az olan kopyaları içine** boyutu. İçeri aktarma aracını 200 GB'den büyük dosyalarla çalışmaz. Bu öğreticide, biz 100 GB öbeklere dosyayı bölün. Kullanarak bunu yapabilirsiniz [Cygwin](https://cygwin.com/install.html). Cygwin Linux komutları destekler. Bu durumda, aşağıdaki komutu kullanın:

    split -b 100m 319GB.tsv

Bölme işlemi aşağıdaki adlarla dosyaları oluşturur.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Veri diskleri hazırlanın
' Ndaki yönergeleri izleyin [Azure içeri/dışarı aktarma hizmeti kullanma](../storage/common/storage-import-export-service.md) (altında **sürücülerinizin hazırlama** bölüm) sabit sürücüler hazırlamak için. Genel sırası şöyledir:

1. Azure içeri/dışarı aktarma hizmeti için kullanılacak gereksinimini karşılayan bir sabit disk temin edin.
2. Verileri Azure veri merkezine sevk edildikten sonra kopyalanacağı bir Azure depolama hesabı belirleyin.
3. Kullanım [Azure içeri/dışarı aktarma aracı](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), bir komut satırı yardımcı programı. Aracının nasıl kullanılacağını gösteren bir örnek parçacığı aşağıda verilmiştir.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Bkz: [Azure içeri/dışarı aktarma hizmeti kullanma](../storage/common/storage-import-export-service.md) daha fazla örnek kod parçacıkları için.
4. Yukarıdaki komut, belirtilen konumda bir günlük dosyası oluşturur. İçeri aktarma işleminden oluşturmak için bu günlük dosyası kullanmak [Azure portal](https://portal.azure.com).

## <a name="create-an-import-job"></a>Bir içeri aktarma işi oluşturma
İçindeki yönergeleri kullanarak, bir içeri aktarma işi şimdi oluşturabilirsiniz [Azure içeri/dışarı aktarma hizmeti kullanma](../storage/common/storage-import-export-service.md) (altında **içeri aktarma işi** bölümü). Bu içeri aktarma işi için diğer ayrıntılarla disk sürücülerini hazırlanırken oluşturulan günlük dosyasını da sağlar.

## <a name="physically-ship-the-disks"></a>Fiziksel diskleri birlikte
Azure veri merkezinde disklere artık fiziksel olarak gönderebilirsiniz. Burada, veriler üzerinde alma işi oluşturma sırasında sağlanan Azure Storage bloblarında kopyalanır. Ayrıca, iş oluşturulurken, daha sonra izleme bilgilerini sağlamak üzere ettiyseniz, şimdi Alma işinizin dönebilir ve izleme numarasını güncelleştirin.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Azure Data Lake Store için Azure Storage bloblarından veri kopyalama
Alma işinin durumunu tamamlanıp tamamlanmadığını gösterir sonra verileri Azure Storage bloblarında belirtilen kullanılabilir olup olmadığını doğrulayabilirsiniz. Ardından, bu verileri Azure Data Lake Store'a bloblarından taşımak için çeşitli yöntemler kullanabilirsiniz. Veri yüklemek için tüm kullanılabilir seçenekleri için bkz: [Data Lake Store veri alma](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

Bu bölümde, veri kopyalamak için bir Azure Data Factory işlem hattı oluşturmak için kullanabileceğiniz JSON tanımlarıyla sunuyoruz. Bu JSON tanımları kullanabilirsiniz [Azure portal](../data-factory/v1/data-factory-copy-activity-tutorial-using-azure-portal.md), veya [Visual Studio](../data-factory/v1/data-factory-copy-activity-tutorial-using-visual-studio.md), veya [Azure PowerShell](../data-factory/v1/data-factory-copy-activity-tutorial-using-powershell.md).

### <a name="source-linked-service-azure-storage-blob"></a>Bağlantılı kaynak hizmeti (Azure Storage blobu)
````
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
````

### <a name="target-linked-service-azure-data-lake-store"></a>Bağlantılı hizmet (Azure Data Lake Store) hedef
````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Giriş veri kümesi
````
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
````
### <a name="output-data-set"></a>Çıktı veri kümesi
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Etkinliğiniz (kopyalama etkinliği)
````
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
````
Daha fazla bilgi için bkz: [veri taşıma Azure Storage blobundan Azure Data Lake Azure Data Factory kullanarak Store'a](../data-factory/connector-azure-data-lake-store.md).

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Azure Data Lake Store'da veri dosyalarını yeniden yapılandırma
319 GB ve böylece Azure içeri/dışarı aktarma hizmetini kullanarak aktarılabilir, aşağı daha küçük boyutu dosyalarına aşan bir dosya ile başlatıldı. Azure Data Lake Store'da verileri değil, biz özgün boyutuna dosyasına yeniden oluşturabilir. Bunu yapmak için aşağıdaki Azure PowerShell cmldts kullanabilirsiniz.

````
# Login to our account
Connect-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
