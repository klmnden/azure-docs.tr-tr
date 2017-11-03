---
title: "Veri bilimi sanal makine veri alım araçları - Azure | Microsoft Docs"
description: "Veri bilimi sanal makine veri alım araçları"
keywords: "Veri bilimi araçları, veri bilimi sanal makine, veri bilimi, linux veri bilimi için Araçlar"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.openlocfilehash: 564c06c5017a77431b7d6fed7b43c47141b12252
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="data-science-virtual-machine-data-ingestion-tools"></a>Veri bilimi sanal makine veri alım araçları

Bir veri bilimi veya AI projesini ilk teknik adımlarını kullanılması ve bunları analytics ortamınıza hale getirmek için veri kümeleri belirlemektir. Veri bilimi sanal makine (DSVM) araçları ve Bulut veya şirket içi DSVM yerel olarak veya bir veri platformu analitik veri depolama alanına farklı kaynaklardan veri getirmek için kitaplıkları sağlar. 

Burada, üzerinde DSVM sağladık bazı veri taşıma araçları bulunmaktadır. 

## <a name="adlcopy"></a>AdlCopy

|    |           |
| ------------- | ------------- |
| Nedir?   | Verileri Azure storage bloblarından Azure Data Lake Store kopyalamak için bir araç. Ayrıca, iki Azure Data Lake Store hesapları arasında verileri de kopyalayabilirsiniz.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanır      | Birden çok BLOB'ları Azure depolama biriminden Azure Data Lake Store aktarma.      |
|  Kullanın / çalıştırmak için nasıl?    |   Bir komut istemi açın yazın `adlcopy` Yardım almak için.    |
| Örnekleri bağlantılar      | [AdlCopy kullanarak] https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob)      |
| DSVM ilgili araçları      | AzCopy, Azure komut satırı     |

## <a name="azure-command-line"></a>Azure komut satırı

|    |           |
| ------------- | ------------- |
| Nedir?   | Azure için bir yönetim aracı. Ayrıca Azure storage bloblarında, Azure Data Lake Storage gibi Azure veri platformlarda sunucudan veri taşımak için komutu fiiller içerir     |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Tipik kullanır      | İçeri aktarma, verileri Azure storage, Azure Data Lake Store gelen ve dışarı aktarma      |
|  Kullanın / çalıştırmak için nasıl?    |   Bir komut istemi açın yazın `az` Yardım almak için.    |
| Örnekleri bağlantılar      | [Azure CLI’yi kullanma](https://docs.microsoft.com/cli/azure/?viee-cli-latest)     |
| DSVM ilgili araçları      | AzCopy, AdlCopy      |


## <a name="azcopy"></a>AzCopy

|    |           |
| ------------- | ------------- |
| Nedir?   | Ve yerel dosyaları, Azure storage bloblarında, dosyaları ve tablolarından veri kopyalamak için bir araç.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanır      | Blob depolama alanına dosyaları kopyalama, hesapları arasında BLOB kopyalama.      |
|  Kullanın / çalıştırmak için nasıl?    |   Bir komut istemi açın yazın `azcopy` Yardım almak için.    |
| Örnekleri bağlantılar      | [Windows üzerinde AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy)      |
| DSVM ilgili araçları      | AdlCopy     |


## <a name="azure-cosmos-db-documentdb-api-data-migration-tool"></a>Azure Cosmos DB: DocumentDB API veri geçiş aracı

|    |           |
| ------------- | ------------- |
| Nedir?   | JSON dosyaları, CSV dosyaları, SQL, MongoDB, Azure Table storage, Amazon DynamoDB ve Azure Cosmos DB DocumentDB API koleksiyonlara Azure Cosmos DB veya Azure DocumentDB dahil olmak üzere çeşitli kaynaklardan veri almak için aracı.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanır      | Dosyaları bir VM'den veri CosmosDB için Azure tablo depolamasından içeri aktarma veya CosmosDB için bir SQL Server veritabanından veri alma CosmosDB içeri aktarılıyor.     |
|  Kullanın / çalıştırmak için nasıl?    |   Komut satırını kullanmak için sürüm, açık bir komut istemi yazın `dt`. GUI aracı'nı kullanmak için bir komut istemi açın yazın `dtui`.    |
| Örnekleri bağlantılar      | [Veri CosmosDB Al](https://docs.microsoft.com/en-us/azure/cosmos-db/import-data)      |
| DSVM ilgili araçları      | AzCopy, AdlCopy      |


## <a name="bcp"></a>bcp

|    |           |
| ------------- | ------------- |
| Nedir?   | SQL Server ve bir veri dosyası arasında veri kopyalamak için SQL Server Aracı'nı tıklatın.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanır      | Bir SQL Server tablosuna bir dosyaya dışa aktarma, bir SQL Server tabloya bir CSV dosyası alınıyor.      |
|  Kullanın / çalıştırmak için nasıl?    |   Bir komut istemi açın yazın `bcp` Yardım almak için.    |
| Örnekleri bağlantılar      | [Toplu kopyalama yardımcı programı](https://docs.microsoft.com/en-us/sql/tools/bcp-utility)      |
| DSVM ilgili araçları      | SQL Server'ı sqlcmd      |


## <a name="microsoft-data-management-gateway"></a>Microsoft Veri Yönetimi ağ geçidi

|    |           |
| ------------- | ------------- |
| Nedir?   | Bağlanmak için bir aracı bulut hizmetlerine tüketimi için veri kaynakları şirket içi.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanır      | VM bir şirket içi veri kaynağına bağlanma.      |
|  Kullanın / çalıştırmak için nasıl?    |   "Microsoft Veri Yönetimi ağ geçidi" Başlat menüsünden başlatın.    |
| Örnekleri bağlantılar      | [Veri Yönetimi Ağ Geçidi](https://msdn.microsoft.com/library/dn879362.aspx)      |
| DSVM ilgili araçları      | AzCopy, AdlCopy, bcp    |
