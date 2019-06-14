---
title: Veri bilimi sanal makinesi veri alma araçları - Azure | Microsoft Docs
description: Veri bilimi sanal makinesi, önceden yüklenmiş yardımcı programları ve veri alma araçları hakkında bilgi edinin.
keywords: veri bilimi araçları, veri bilimi sanal makinesi, veri bilimi için araçlar, linux veri bilimi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: 92ff5d21fc30d8fcafe97a2b452ff157a2cd5f86
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60502243"
---
# <a name="data-science-virtual-machine-data-ingestion-tools"></a>Veri bilimi sanal makinesi veri alma araçları

Bir veri bilimi veya AI proje teknik ilk adımlar ve bunları analiz ortamınıza Getir veri kümeleri belirlemektir. Araçlar ve kitaplıklar Bulut veya şirket içi bir analitik veri deposu DSVM üzerinde yerel olarak veya bir veri platformu farklı kaynaklardan verileri getirmek için veri bilimi sanal makinesi (DSVM) sağlar. 

DSVM'nin sağladık bazı veri taşıma araçları şunlardır. 

## <a name="adlcopy"></a>AdlCopy

|    |           |
| ------------- | ------------- |
| Nedir?   | Azure Data Lake Store ile Azure depolama bloblarından veri kopyalamak için bir araç. Ayrıca, verileri iki Azure Data Lake Store hesapları arasında da kopyalayabilirsiniz.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Birden çok BLOB'ları, Azure Data Lake Store ile Azure depolama alanından içeri aktarılıyor.      |
|  Kullanma / çalıştırın nasıl?    |   Bir komut istemi açın yazın `adlcopy` Yardım almak için.    |
| Örneklere bağlantılar      | [AdlCopy’yi kullanma](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob)      |
| DSVM ilgili araçları      | AzCopy, Azure komut satırı     |

## <a name="azure-command-line"></a>Azure komut satırı

|    |           |
| ------------- | ------------- |
| Nedir?   | Azure için bir yönetim aracı. Ayrıca, verileri Azure depolama blobları, Azure Data Lake depolama gibi Azure veri platformları taşıma için komut fiilleri içerir     |
| Desteklenen DSVM sürümleri      | Windows, Linux     |
| Tipik kullanımları      | Azure depolama, Azure Data Lake Store gelen ve giden verileri dışarı aktarma, alma      |
|  Kullanma / çalıştırın nasıl?    |   Bir komut istemi açın yazın `az` Yardım almak için.    |
| Örneklere bağlantılar      | [Azure CLI’yi kullanma](https://docs.microsoft.com/cli/azure)     |
| DSVM ilgili araçları      | AzCopy, AdlCopy      |


## <a name="azcopy"></a>AzCopy

|    |           |
| ------------- | ------------- |
| Nedir?   | Yerel dosyaları, Azure depolama blobları, dosyalar ve tablolar ve veri kopyalamak için bir araç.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Dosyaları blob depolama alanına kopyalama, hesapları arasında BLOB'ları kopyalanıyor.      |
|  Kullanma / çalıştırın nasıl?    |   Bir komut istemi açın yazın `azcopy` Yardım almak için.    |
| Örneklere bağlantılar      | [Windows üzerinde AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy)      |
| DSVM ilgili araçları      | AdlCopy     |


## <a name="azure-cosmos-db-data-migration-tool"></a>Azure Cosmos DB veri geçiş aracı

|    |           |
| ------------- | ------------- |
| Nedir?   | JSON dosyaları, CSV dosyaları, Azure Cosmos DB SQL, MongoDB, Azure tablo depolama, Amazon DynamoDB ve Azure Cosmos DB SQL API koleksiyonlara dahil olmak üzere çeşitli kaynaklardan veri almak için aracı.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Dosya adı CosmosDB olarak Azure tablo Depolama'dan veri aktarma veya adı CosmosDB olarak bir SQL Server veritabanından veri almayı CosmosDB için bir sanal makineden içeri aktarılıyor.     |
|  Kullanma / çalıştırın nasıl?    |   Komut satırında kullanmak için açık bir komut istemi sürümünü yazın `dt`. GUI aracını kullanmak için bir komut istemi açın yazın `dtui`.    |
| Örneklere bağlantılar      | [Verileri CosmosDB İçeri Aktar](https://docs.microsoft.com/azure/cosmos-db/import-data)      |
| DSVM ilgili araçları      | AzCopy, AdlCopy      |


## <a name="bcp"></a>bcp

|    |           |
| ------------- | ------------- |
| Nedir?   | SQL Server ve bir veri dosyası arasında veri kopyalamak için SQL Server Aracı'nı tıklatın.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | SQL Server tablosunu bir dosyaya dışarı aktarma, bir SQL Server tabloya bir CSV dosyasını içeri.      |
|  Kullanma / çalıştırın nasıl?    |   Bir komut istemi açın yazın `bcp` Yardım almak için.    |
| Örneklere bağlantılar      | [Toplu kopyalama yardımcı](https://docs.microsoft.com/sql/tools/bcp-utility)      |
| DSVM ilgili araçları      | SQL Server'ı sqlcmd      |

## <a name="blobfuse"></a>Blobfuse

|    |           |
| ------------- | ------------- |
| Nedir?   | Linux dosya sisteminde bir Azure blob kapsayıcısı bağlamak için bir araç.      |
| Desteklenen DSVM sürümleri      | Linux      |
| Tipik kullanımları      | Bir kapsayıcıdaki blobları yazma ve okuma      |
|  Kullanma / çalıştırın nasıl?    |   Çalıştırma _blobfuse_ bir terminal konumunda.    |
| Örneklere bağlantılar      | [github'da blobfuse](https://github.com/Azure/azure-storage-fuse)      |
| DSVM ilgili araçları      | Azure komut satırı      |


## <a name="microsoft-data-management-gateway"></a>Microsoft Veri Yönetimi ağ geçidi

|    |           |
| ------------- | ------------- |
| Nedir?   | Bağlanmak için bir aracı veri kaynakları, bulut hizmetlerine tüketim için şirket içi.      |
| Desteklenen DSVM sürümleri      | Windows      |
| Tipik kullanımları      | Bir VM, bir şirket içi veri kaynağına bağlanma.      |
|  Kullanma / çalıştırın nasıl?    |   "Microsoft Veri Yönetimi ağ geçidi" Başlat Menüsü'nden başlatın.    |
| Örneklere bağlantılar      | [Veri Yönetimi Ağ Geçidi](https://msdn.microsoft.com/library/dn879362.aspx)      |
| DSVM ilgili araçları      | AzCopy, AdlCopy, bcp    |
