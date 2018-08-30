---
title: Azure Cosmos DB'de Azure Tablo Depolama desteği | Microsoft Docs
description: Azure Cosmos DB Tablo API'si ile Azure Depolama Tabloları'nın birlikte nasıl çalıştığını öğrenin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: na
ms.topic: overview
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 114286b45df5f47e81bd2b990c8b50c8b7b7a482
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43185375"
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Azure Cosmos DB Tablo API'si ve Azure Tablo depolaması ile geliştirme

Azure Cosmos DB Tablo API'si ve Azure Tablo depolaması aynı tablo veri modelini paylaşır ve SDK'ları aracılığıyla aynı oluşturma, silme, güncelleştirme ve sorgulama işlemlerini gösterir. 

[!INCLUDE [storage-table-cosmos-comparison](../../includes/storage-table-cosmos-comparison.md)]

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Azure Cosmos DB Tablo API'siyle geliştirme

Şu anda [Azure Cosmos DB Tablo API'sinin](table-introduction.md) geliştirme için kullanılabilen dört SDK'sı vardır: 
- [Microsoft.Azure.CosmosDB.Table](https://aka.ms/tableapinuget) .NET SDK. Bu kitaplık genel [Windows Azure Depolama SDK'sı](https://www.nuget.org/packages/WindowsAzure.Storage) ile aynı sınıf ve yöntem imzalarına sahip olmasının yanı sıra Tablo API'sini kullanarak Azure Cosmos DB hesaplarına da bağlanabilir. `Microsoft.Azure.CosmosDB.Table` kitaplığının şu an için yalnızca .NET Standard ile kullanılabileceğini, henüz .NET Core için mevcut olmadığını unutmayın.
- [Python SDK’sı](table-sdk-python.md). Yeni Azure Cosmos DB Python SDK'sı, Python'da Azure Tablo depolamasını destekleyen tek SDK'sıdır. Bu SDK hem Azure Tablo depolaması hem de Azure Cosmos DB Tablo API'sine bağlanır.
- [Java SDK'sı](table-sdk-java.md). Bu Azure Depolama SDK'sı, Tablo API'sini kullanarak Azure Cosmos DB hesaplarına bağlanabilir.
- [Node.js SDK’sı](table-sdk-nodejs.md). Bu Azure Depolama SDK'sı, Tablo API'sini kullanarak Azure Cosmos DB hesaplarına bağlanabilir.

Tablo API'siyle çalışma hakkındaki ek bilgileri [SSS: Tablo API'siyle geliştirme](faq.md#table) makalesinde bulabilirsiniz.

## <a name="developing-with-azure-table-storage"></a>Azure Tablo depolamasıyla geliştirme

Azure Tablo depolamasının geliştirme için kullanılabilen SDK'ları vardır:

- [WindowsAzure.Storage .NET SDK'sı](https://www.nuget.org/packages/WindowsAzure.Storage/). Bu kitaplık, depolama Tablo hizmetiyle çalışmanıza olanak tanır.
- [Python SDK’sı](table-sdk-python.md). Python için Azure Cosmos DB Tablo SDK'sı depolama Tablo hizmetini de destekler.
- [Java için Azure Depolama SDK'sı](https://github.com/azure/azure-storage-java). Bu Azure Depolama SDK'sı, Azure Tablo depolamasını kullanmak için Java'da bir istemci kitaplığı sağlar.
- [Node.js SDK’sı](table-sdk-nodejs.md). Bu SDK, depolama Tablo hizmetini kullanmak için bir Node.js paketi ve tarayıcıyla uyumlu bir JavaScript istemci kitaplığı sağlar.
- [AzureRmStorageTable PowerShell modülü](https://www.powershellgallery.com/packages/AzureRmStorageTable/1.0.0.7). Bu PowerShell modülünün depolama Tablolarıyla çalışmak için cmdlet'leri vardır.
- [C++ için Azure Depolama İstemcisi Kitaplığı](https://github.com/Azure/azure-storage-cpp/). Bu kitaplık Azure Depolama için uygulamalar oluşturmanıza olanak tanır.
- [Ruby için Azure Depolama Tablosu İstemci Kitaplığı](https://github.com/azure/azure-storage-ruby/tree/master/table). Bu proje, Azure depolaması Tablo hizmetlerine erişmeyi kolaylaştıran bir Ruby paketi sağlar.
- [Azure Depolama Tablosu PHP İstemci Kitaplığı](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table). Bu proje, Azure depolaması Tablo hizmetlerine erişmeyi kolaylaştıran bir PHP istemci kitaplığı sağlar.


   





