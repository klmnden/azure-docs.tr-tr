---
title: "Azure Cosmos DB tablo API ile kullanmak için içeri aktarma verileri | Microsoft Docs"
description: "Bilgi nasıl Azure Cosmos DB tablo API ile kullanmak için veri alabilirsiniz."
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: 
ms.assetid: b60743e2-0227-43ab-965a-0ae3ebacd917
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: mimig
ms.openlocfilehash: 1c53be736ad65a53767626033be27f0891de06ba
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="import-data-for-use-with-the-azure-cosmos-db-table-api"></a>Azure Cosmos DB tablo API ile kullanmak için veri alma

Bu öğretici Azure Cosmos DB ile kullanmak için veri alma hakkında yönergeler sağlar. [tablo API](table-introduction.md). Azure Table depolama alanında depolanan verileri varsa, verilerinizi almak için veri geçiş aracı veya AzCopy kullanabilirsiniz. Bir Azure Cosmos DB tablo API (Önizleme) hesabında depolanan verileriniz varsa, verileri geçirmek için veri geçiş aracı kullanmanız gerekir. Verilerinizi alındıktan sonra anahtar teslimi genel dağıtım, ayrılmış işleme, en yüksek oranda kullanılabilirlik garanti 99 tek basamaklı milisaniyelik gecikme gibi Azure Cosmos DB teklifleri premium özelliklerinden yararlanmak kullanabileceksiniz, ve otomatik ikincil dizin oluşturma.

Bu öğretici, aşağıdaki görevleri içerir:

> [!div class="checklist"]
> * Veri Geçiş Aracı ile veri alma
> * AzCopy ile veri alma
> * Tablo API için tablo API (Önizleme) geçirme 

## <a name="data-migration-tool"></a>Veri Geçiş Aracı

Komut satırı Azure Cosmos DB veri Geçiş Aracı (dt.exe) bir tablo API GA hesabına, var olan Azure Table depolama veri almak için kullanılan veya verileri bir tabloda API (Önizleme) hesabından bir tablo API GA hesabına geçirmek. Diğer kaynakları şu anda desteklenmemektedir. Temelli UI veri Geçiş Aracı (dtui.exe) tablo API hesapları için şu anda desteklenmiyor. 

Tablo verisi geçişini gerçekleştirmek için aşağıdaki görevleri tamamlayın:

1. Geçiş aracından karşıdan [GitHub](https://github.com/azure/azure-documentdb-datamigrationtool).
2. Çalıştırma `dt.exe` senaryonuz için komut satırı bağımsız değişkenleri kullanma.

dt.exe aşağıdaki biçimde bir komut alır:

    dt.exe [/<option>:<value>] /s:<source-name> [/s.<source-option>:<value>] /t:<target-name> [/t.<target-option>:<value>] 

Komutu için Seçenekler şunlardır:

    /ErrorLog: Optional. Name of the CSV file to redirect data transfer failures
    /OverwriteErrorLog: Optional. Overwrite error log file
    /ProgressUpdateInterval: Optional, default is 00:00:01. Time interval to refresh on-screen data transfer progress
    /ErrorDetails: Optional, default is None. Specifies that detailed error information should be displayed for the following errors: None, Critical, All

### <a name="command-line-source-settings"></a>Komut satırı kaynak ayarları

Azure Table Storage veya tablo API Önizleme geçiş kaynağı olarak tanımlarken aşağıdaki kaynak seçenekleri kullanın.

    /s:AzureTable: Reads data from Azure Table storage
    /s.ConnectionString: Connection string for the table endpoint. This can be retrieved from the Azure portal
    /s.LocationMode: Optional, default is PrimaryOnly. Specifies which location mode to use when connecting to Azure Table storage: PrimaryOnly, PrimaryThenSecondary, SecondaryOnly, SecondaryThenPrimary
    /s.Table: Name of the Azure Table
    /s.InternalFields: Set to All for table migration as RowKey and PartitionKey are required for import.
    /s.Filter: Optional. Filter string to apply
    /s.Projection: Optional. List of columns to select

Azure tablo depolamasından içeri aktarırken kaynak bağlantı dizesini almak için Azure Portalı'nı açın ve **depolama hesapları** > **hesap**  >   **Erişim tuşları**ve kopyalamak için Kopyala düğmesini kullanın **bağlantı dizesi**.

![Ekran görüntüsü, HBase kaynak seçenekleri](./media/table-import/storage-table-access-key.png)

Bir Azure Cosmos DB tablo API (Önizleme) hesabından alırken kaynak bağlantı dizesini almak için Azure portal, açmak **Azure Cosmos DB** > **hesap**  >  **Bağlantı dizesi** ve kopyalamak için Kopyala düğmesini kullanın **bağlantı dizesi**.

![Ekran görüntüsü, HBase kaynak seçenekleri](./media/table-import/cosmos-connection-string.png)

[Azure Table Storage komut örneği](#azure-table-storage)

[Örnek Azure Cosmos DB tablo API (Önizleme) komutu](#table-api-preview)

### <a name="command-line-target-settings"></a>Komut satırı hedef ayarları

Geçiş hedefi olarak Azure Cosmos DB tablo API tanımlarken, aşağıdaki hedef seçenekleri kullanın.

    /t:TableAPIBulk: Uploads data into Azure CosmosDB Table in batches
    /t.ConnectionString: Connection string for the table endpoint
    /t.TableName: Specifies the name of the table to write to
    /t.Overwrite: Optional, default is false. Specifies if existing values should be overwritten
    /t.MaxInputBufferSize: Optional, default is 1GB. Approximate estimate of input bytes to buffer before flushing data to sink
    /t.Throughput: Optional, service defaults if not specified. Specifies throughput to configure for table
    /t.MaxBatchSize: Optional, default is 2MB. Specify the batch size in bytes

<a id="azure-table-storage"></a>
### <a name="sample-command-source-is-azure-table-storage"></a>Örnek komut: kaynağıdır Azure tablo depolaması

Tablo API için Azure tablo depolamasından içeri aktarma gösteren bir komut satırı örnek aşağıda verilmiştir:

```
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Table storage account name>;AccountKey=<Account Key>;EndpointSuffix=core.windows.net /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```
<a id="table-api-preview"></a>
### <a name="sample-command-source-is-azure-cosmos-db-table-api-preview"></a>Örnek komut: kaynağıdır Azure Cosmos DB tablo API (Önizleme)

Tablo API Önizlemesi'nden tablo API GA almak için komut satırı bir örnek şudur:

```
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Table API preview account name>;AccountKey=<Table API preview account key>;TableEndpoint=https://<Account Name>.documents.azure.com; /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```

## <a name="azcopy-command"></a>AzCopy komut

AzCopy komut satırı yardımcı programını kullanarak geçirmek için diğer seçenek olan Azure Table depolama biriminden verileri Azure Cosmos DB tablo API. AzCopy kullanmak için önce verilerinizi açıklandığı gibi dışa [veri tablosu depodan dışarı aktarma](../storage/common/storage-use-azcopy.md#export-data-from-table-storage), sonra verileri açıklandığı gibi Azure Cosmos DB almanız [Azure Cosmos DB tablo API](../storage/common/storage-use-azcopy.md#import-data-into-table-storage).

Azure Cosmos DB içe gerçekleştirirken, aşağıdaki örneğe bakın. / Dest değeri cosmosdb, çekirdek değildir kullandığını unutmayın.

Örnek alma komutu:

```
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.cosmosdb.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

## <a name="migrate-from-table-api-preview-to-table-api"></a>Tablo API (Önizleme) tablo API'sine geçirin

> [!WARNING]
> Genel olarak kullanılabilir tabloların hemen avantajlarından sonra mevcut Önizleme belirtildiği gibi bu bölümdeki tablolarda, lütfen geçirmek istiyorsanız, aksi takdirde biz otomatik geçişler gerçekleştirecekseniz haftalarda mevcut Önizleme müşteriler için unutmayın Otomatik geçirilen ancak Önizleme tablolar yeni tablolar oluşturulmamış bazı kısıtlamalar bunlara sahip olur.
> 

Tablo artık genel olarak kullanılabilir (GA) API'dir. Önizleme ve tabloları hem de istemcide çalışan kod yanı sıra bulutta çalışan bir kod GA sürümleri arasındaki farklar vardır. Bu nedenle, bir GA tablo API hesabıyla Önizleme SDK istemcisi karışımı denemek için önerilmez ve tersi. Varolan tablolarıyla kullanmaya devam ancak bir üretim ortamında Önizlemesi'nden GA ortamına geçirin veya otomatik geçişi için beklemeniz gerekir istediğiniz API Önizleme Müşteriler tablosu. Otomatik geçişi için beklerseniz, geçirilen tablolarda kısıtlama bildirilir. Geçişten sonra (geçirilen tabloları kısıtlamaları yalnızca) yeni tablolar var olan hesabınızı kısıtlama olmadan oluşturmak mümkün olacaktır.

Genel olarak kullanılabilir tablo API (Önizleme) tablo API'SİNDEN geçirmek için:

1. Yeni bir Azure Cosmos DB hesabı oluşturun ve açıklandığı gibi Azure tablosuna API türünü ayarlayın [bir veritabanı hesabı oluşturma](create-table-dotnet.md#create-a-database-account).

2. Değiştirme GA sürümünü kullanmak için istemcileri [tablo API SDK'ları](table-sdk-dotnet.md).

3. İstemci verilerini Önizleme tablolarından veri geçiş aracı kullanarak GA tablolara geçirin. Bu amaç için veri geçiş aracı kullanma yönergeleri açıklanmıştır [veri geçiş aracı](#data-migration-tool). 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Veri Geçiş Aracı ile veri alma
> * AzCopy ile verileri içeri aktar
> * Tablo API (Önizleme) tablo API'sine geçirin

Şimdi, sonraki öğretici devam etmek ve Azure Cosmos DB tablo API'sini kullanarak veri sorgu öğrenin. 

> [!div class="nextstepaction"]
>[Sorgu verileri nasıl?](../cosmos-db/tutorial-query-table.md)
