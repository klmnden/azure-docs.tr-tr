---
title: Azure Cosmos DB için Azure Resource Manager şablonları
description: Azure Resource Manager şablonları oluşturmak ve Azure Cosmos DB yapılandırmak için kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: c907de019b64a6a9d03206514a1147fd43c78106
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65078468"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Resource Manager şablonları

Aşağıdaki tabloda, Azure Cosmos DB için Azure Resource Manager şablonlarının bağlantılarını içerir:

|**API türü** | **örneğe Bağla**| **Açıklama** |
|---|---| ---|
|Core (SQL) API| [(Birden çok ana) bir Azure Cosmos DB hesabı oluşturma](manage-sql-with-resource-manager.md) | Bu şablon bir SQL API hesabı, etkin çok yöneticili iki bölgeleri oluşturur. Azure Cosmos hesabı, veritabanı düzeyinde işleme paylaşan iki kapsayıcı olacaktır. |
| MongoDB API’si | [(Birden çok ana) bir Azure Cosmos DB hesabı oluşturma](manage-mongodb-with-resource-manager.md) | Bu şablon, etkin çok yöneticili iki bölgede MongoDB için Azure Cosmos DB'nin API'sini kullanarak bir hesap oluşturur. Azure Cosmos hesabı, veritabanı düzeyinde işleme paylaşan iki kapsayıcı olacaktır. |
| Cassandra API’si | [(Birden çok ana) bir Azure Cosmos DB hesabı oluşturma](manage-cassandra-with-resource-manager.md) | Bu şablon, etkin çok yöneticili iki bölgede Cassandra API hesabı oluşturur. Azure Cosmos hesabı anahtar alanı düzeyinde işleme paylaşan iki tablo olacaktır. |
| Gremlin API| [(Birden çok ana) bir Azure Cosmos DB hesabı oluşturma](manage-gremlin-with-resource-manager.md) | Bu şablon, iki bölgeleri çok yöneticili etkin bir Gremlin API hesabı oluşturur. Azure Cosmos hesabı, veritabanı düzeyinde işleme paylaşan iki grafik olacaktır. |
| Tablo API’si | [(Birden çok ana) bir Azure Cosmos DB hesabı oluşturma](manage-table-with-resource-manager.md) | Bu şablon, iki bölgeleri çok yöneticili etkin bir tablo API'si hesabı oluşturur. Azure Cosmos hesabı, tek bir tablo gerekir. |

> [!TIP]
> Tablo API'sini kullanarak paylaşılan bir aktarım hızı etkinleştirmek için Azure Portalı'nda hesap düzeyinde aktarım hızı sağlar.

Bkz: [Azure Cosmos DB için ARM başvuru](/azure/templates/microsoft.documentdb/allversions) sayfası için başvuru belgeleri.