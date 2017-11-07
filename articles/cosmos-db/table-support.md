---
title: "Azure Table Storage destek Azure Cosmos veritabanı | Microsoft Docs"
description: "Azure Cosmos DB tablo API ve Azure depolama tabloları birlikte nasıl çalıştığını öğrenin."
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: 
ms.assetid: 378f00f2-cfd9-4f6b-a9b1-d1e4c70799fd
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 1513fc53501f1cfec93134841fbef9a8552dd43c
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Azure Cosmos DB tablo API ve Azure Table storage ile geliştirme

Azure Cosmos DB tablo API Azure Table storage aynı tablo veri modeline paylaşabilir ve aynı oluştur, Sil, güncelleştirme ve sorgu işlemleri kendi SDK aracılığıyla kullanıma sunar. 

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Azure Cosmos DB tablo API ile geliştirme

Şu anda [Azure Cosmos DB tablo API](table-introduction.md) bir .NET SDK kullanılabilir olan [Windows Azure depolama Premium tablosu (Önizleme)](https://aka.ms/premiumtablenuget). Bu kitaplık genel olarak aynı sınıfları ve yöntem imzaları sahip [Windows Azure depolama SDK](https://www.nuget.org/packages/WindowsAzure.Storage), ancak tablo API (Önizleme) kullanarak Azure Cosmos DB hesaplarına bağlanma özelliği de vardır. Hızlı Başlangıç ve Azure Cosmos DB tablo API'sini kullanarak öğretici için aşağıdaki makalelere bakın:
- Hızlı Başlangıç: [Azure Cosmos DB: Tablo API kullanarak bir .NET uygulaması oluşturma](create-table-dotnet.md)
- Öğretici: [Azure Cosmos DB: .NET API tabloda ile geliştirme](tutorial-develop-table-dotnet.md)

Tablo API ile çalışma hakkında ek bilgi sağlanmıştır [SSS: Tablo API ile geliştirme](faq.md#develop-with-the-table-api-preview) makalesi.

## <a name="developing-with-the-azure-table-storage"></a>Azure Table storage'ı geliştirme

[Azure Table storage](table-storage-overview.md) birçok SDK'ları kullanılabilir ve öğreticiler kullanılabilir, tümü şimdi kullanılabilir olan [Azure Table depolama](table-storage-overview.md) bölümü. Bu makaleler Azure Table depolama SDK'ları arasında birlikte çalışabilirlik olarak güncelleştirilen ve Azure Cosmos DB tablo API'leri kullanılabilir hale gelir.  





