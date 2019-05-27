---
title: Azure Cosmos DB için Azure Resource Manager şablonları
description: Azure Resource Manager şablonları oluşturmak ve Azure Cosmos DB yapılandırmak için kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 4a32b0497d2457a740e9c082f990bb9112208bfd
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969172"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Azure Cosmos DB için Azure Resource Manager şablonları

Aşağıdaki tablolarda Azure Cosmos DB için Azure Resource Manager şablonlarının bağlantılarını içerir:

## <a name="sql-core-api"></a>SQL (Core) API

|**Şablon**|**Açıklama**|
|---| ---|
|[Bir Azure Cosmos hesabı, veritabanı, kapsayıcı oluşturma](manage-sql-with-resource-manager.md#create-resource) | Bu şablon, etkin çok yöneticili iki bölgeleri (çekirdek) SQL API hesabı oluşturur. Azure Cosmos hesabı, veritabanı düzeyinde işleme paylaşan iki kapsayıcı olacaktır. |
|[İşleme (RU/s) bir veritabanı için güncelleştirme](manage-sql-with-resource-manager.md#database-ru-update) | Bu şablon, bir veritabanı (Temel) SQL API hesabı için aktarım hızı güncelleştirir. |
|[İşleme (RU/s) bir kapsayıcı için güncelleştirme](manage-sql-with-resource-manager.md#container-ru-update) | Bu şablon, aktarım hızı (çekirdek) SQL API hesabı, kapsayıcı için güncelleştirir. |

## <a name="mongodb-api"></a>MongoDB API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Bir Azure Cosmos hesabı, veritabanı, koleksiyon oluşturma](manage-mongodb-with-resource-manager.md#create-resource) | Bu şablon, etkin çok yöneticili kullanarak iki bölgede MongoDB için Azure Cosmos DB API hesabı oluşturur. Azure Cosmos hesabı, veritabanı düzeyinde işleme paylaşan iki kapsayıcı olacaktır. |
|[İşleme (RU/s) bir veritabanı için güncelleştirme](manage-mongodb-with-resource-manager.md#database-ru-update) | Bu şablon, bir veritabanında bir MongoDB API hesabı için aktarım hızı güncelleştirir. |
|[İşleme (RU/s) bir koleksiyon için güncelleştirme](manage-mongodb-with-resource-manager.md#collection-ru-update) | Bu şablon bir MongoDB API hesabı, kapsayıcı için aktarım hızı güncelleştirir. |

## <a name="cassandra-api"></a>Cassandra API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Bir Azure Cosmos hesabı, anahtar alanı, tablo oluşturma](manage-cassandra-with-resource-manager.md#create-resource) | Bu şablon, etkin çok yöneticili iki bölgede Cassandra API hesabı oluşturur. Azure Cosmos hesabı anahtar alanı düzeyinde işleme paylaşan iki tablo olacaktır. |
|[İşleme (RU/s) için bir anahtar alanı güncelleştirme](manage-cassandra-with-resource-manager.md#keyspace-ru-update) | Bu şablon için bir anahtar alanı Cassandra API hesabı, aktarım hızı güncelleştirir. |
|[İşleme (RU/s) bir tablo için güncelleştirme](manage-cassandra-with-resource-manager.md#table-ru-update) | Bu şablon, bir tablodaki bir Cassandra API hesabı için aktarım hızı güncelleştirir. |

## <a name="gremlin-api"></a>Gremlin API

|**Şablon**|**Açıklama**|
|---| ---|
|[Bir Azure Cosmos hesabı, veritabanı, grafik oluşturma](manage-gremlin-with-resource-manager.md#create-resource) | Bu şablon, iki bölgeleri çok yöneticili etkin bir Gremlin API hesabı oluşturur. Azure Cosmos hesabı, veritabanı düzeyinde işleme paylaşan iki grafik olacaktır. |
|[İşleme (RU/s) bir veritabanı için güncelleştirme](manage-gremlin-with-resource-manager.md#database-ru-update) | Bu şablon, bir veritabanında bir Gremlin API hesabı için aktarım hızı güncelleştirir. |
|[Bir grafik işleme (RU/s) güncelleştirme](manage-gremlin-with-resource-manager.md#graph-ru-update) | Bu şablon, bir grafikte bir Gremlin API hesabı için aktarım hızı güncelleştirir. |

## <a name="table-api"></a>Tablo API’si

|**Şablon**|**Açıklama**|
|---| ---|
|[Azure Cosmos hesap oluşturma, tablo](manage-table-with-resource-manager.md#create-resource) | Bu şablon, iki bölgeleri çok yöneticili etkin bir tablo API'si hesabı oluşturur. Azure Cosmos hesabı, tek bir tablo gerekir. |
|[İşleme (RU/s) bir tablo için güncelleştirme](manage-table-with-resource-manager.md#table-ru-update) | Bu şablon, bir tablodaki bir tablo API'si hesabı için aktarım hızı güncelleştirir. |

> [!TIP]
> Tablo API'sini kullanarak paylaşılan bir aktarım hızı etkinleştirmek için Azure Portalı'nda hesap düzeyinde aktarım hızı sağlar.

Bkz: [Azure Cosmos DB için ARM başvuru](/azure/templates/microsoft.documentdb/allversions) sayfası için başvuru belgeleri.