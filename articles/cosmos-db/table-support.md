---
title: Azure Cosmos DB, Azure tablo depolama desteği
description: Azure Cosmos DB Tablo API'si ile Azure Depolama Tabloları'nın birlikte nasıl çalıştığını öğrenin.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: overview
ms.date: 11/15/2017
author: wmengmsft
ms.author: wmeng
ms.reviewer: sngun
ms.openlocfilehash: c7c9caad6c454b90a6d50932450922a03bbc1944
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65604166"
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Azure Cosmos DB Tablo API'si ve Azure Tablo depolaması ile geliştirme

Azure Cosmos DB Tablo API'si ve Azure Tablo depolaması aynı tablo veri modelini paylaşır ve SDK'ları aracılığıyla aynı oluşturma, silme, güncelleştirme ve sorgulama işlemlerini gösterir. 

[!INCLUDE [storage-table-cosmos-comparison](../../includes/storage-table-cosmos-comparison.md)]

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Azure Cosmos DB Tablo API'siyle geliştirme

Şu anda [Azure Cosmos DB Tablo API'sinin](table-introduction.md) geliştirme için kullanılabilen dört SDK'sı vardır: 

* [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table): .NET SDK'sı. Bu kitaplık .NET standart hedefler ve genel aynı sınıf ve yöntem imzalarına sahip [Windows Azure depolama SDK'sı](https://www.nuget.org/packages/WindowsAzure.Storage), ancak tablo API'sini kullanarak Azure Cosmos DB hesaplarına nasıl bağlanacağınızı olanağı da vardır. .NET Framework kitaplığı kullanıcıları [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table/) yükseltmek için önerilen [Microsoft.Azure.Cosmos.Table](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) bakım modunda ve yakında kullanımdan kaldırılacak.

* [Python SDK'sı](table-sdk-python.md): Yeni Azure Cosmos DB Python SDK'sı, Python'da Azure Tablo depolamasını destekleyen tek SDK'sıdır. Bu SDK hem Azure Tablo depolaması hem de Azure Cosmos DB Tablo API'sine bağlanır.

* [Java SDK](table-sdk-java.md): Bu Azure Depolama SDK'sı, Tablo API'sini kullanarak Azure Cosmos DB hesaplarına bağlanabilir.

* [Node.js SDK'sı](table-sdk-nodejs.md): Bu Azure Depolama SDK'sı, Tablo API'sini kullanarak Azure Cosmos DB hesaplarına bağlanabilir.


Tablo API'si ile çalışma hakkında ek bilgi sağlanmıştır [SSS: Tablo API'si ile geliştirme](faq.md#table) makalesi.

## <a name="developing-with-azure-table-storage"></a>Azure Tablo depolamasıyla geliştirme

Azure Tablo depolamasının geliştirme için kullanılabilen SDK'ları vardır:

- [WindowsAzure.Storage .NET SDK'sı](https://www.nuget.org/packages/WindowsAzure.Storage/). Bu kitaplık, depolama Tablo hizmetiyle çalışmanıza olanak tanır.
- [Python SDK’sı](table-sdk-python.md). Python için Azure Cosmos DB Tablo SDK'sı depolama Tablo hizmetini de destekler.
- [Java için Azure Depolama SDK'sı](https://github.com/azure/azure-storage-java). Bu Azure Depolama SDK'sı, Azure Tablo depolamasını kullanmak için Java'da bir istemci kitaplığı sağlar.
- [Node.js SDK’sı](table-sdk-nodejs.md). Bu SDK, depolama Tablo hizmetini kullanmak için bir Node.js paketi ve tarayıcıyla uyumlu bir JavaScript istemci kitaplığı sağlar.
- [AzureRmStorageTable PowerShell modülü](https://www.powershellgallery.com/packages/AzureRmStorageTable). Bu PowerShell modülünün depolama Tablolarıyla çalışmak için cmdlet'leri vardır.
- [C++ için Azure Depolama İstemcisi Kitaplığı](https://github.com/Azure/azure-storage-cpp/). Bu kitaplık Azure Depolama için uygulamalar oluşturmanıza olanak tanır.
- [Ruby için Azure Depolama Tablosu İstemci Kitaplığı](https://github.com/azure/azure-storage-ruby/tree/master/table). Bu proje, Azure depolaması Tablo hizmetlerine erişmeyi kolaylaştıran bir Ruby paketi sağlar.
- [Azure Depolama Tablosu PHP İstemci Kitaplığı](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table). Bu proje, Azure depolaması Tablo hizmetlerine erişmeyi kolaylaştıran bir PHP istemci kitaplığı sağlar.


   





