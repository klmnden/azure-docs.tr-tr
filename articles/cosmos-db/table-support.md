---
title: Azure Table Storage destek Azure Cosmos veritabanı | Microsoft Docs
description: Azure Cosmos DB tablo API ve Azure depolama tabloları birlikte nasıl çalıştığını öğrenin.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: 378f00f2-cfd9-4f6b-a9b1-d1e4c70799fd
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 2560c2ee34a83ce86db043e17fb41192c31de398
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Azure Cosmos DB tablo API ve Azure Table storage ile geliştirme

Azure Cosmos DB tablo API Azure Table storage aynı tablo veri modeline paylaşabilir ve aynı oluştur, Sil, güncelleştirme ve sorgu işlemleri kendi SDK aracılığıyla kullanıma sunar. 

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Azure Cosmos DB tablo API ile geliştirme

Şu anda [Azure Cosmos DB tablo API](table-introduction.md) dört SDK geliştirme için kullanılabilir olan: 
- [Microsoft.Azure.CosmosDB.Table](https://aka.ms/tableapinuget) .NET SDK'sı. Bu kitaplık genel olarak aynı sınıfları ve yöntem imzaları sahip [Windows Azure depolama SDK](https://www.nuget.org/packages/WindowsAzure.Storage), ancak tablo API kullanarak Azure Cosmos DB hesaplarına bağlanma özelliği de vardır. 
- [Python SDK](table-sdk-python.md). Yeni Azure Cosmos DB Python SDK'sı Azure Table storage ' Python destekler yalnızca SDK ' dir. Bu SDK, Azure Table depolama ve Azure Cosmos DB tablo API ile bağlanır.
- [Java SDK](table-sdk-java.md). Bu Azure depolama SDK'sını tablo API kullanarak Azure Cosmos DB hesaplarına bağlamak için özelliğine sahiptir.
- [Node.js SDK'sı](table-sdk-nodejs.md). Bu Azure depolama SDK'sını tablo API kullanarak Azure Cosmos DB hesaplarına bağlamak için özelliğine sahiptir.

Tablo API ile çalışma hakkında ek bilgi sağlanmıştır [SSS: Tablo API ile geliştirme](faq.md#develop-with-the-table-api) makalesi.

## <a name="developing-with-the-azure-table-storage"></a>Azure Table storage'ı geliştirme

[Azure Table storage](table-storage-overview.md) birçok SDK'ları kullanılabilir ve öğreticiler kullanılabilir, tümü şimdi kullanılabilir olan [Azure Table depolama](table-storage-overview.md) bölümü. Bu makaleler Azure Table depolama SDK'ları arasında birlikte çalışabilirlik olarak güncelleştirilen ve Azure Cosmos DB tablo API'leri kullanılabilir hale gelir.  





