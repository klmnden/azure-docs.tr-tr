---
title: Azure Search'te dizin oluşturucular | Microsoft Belgeleri
description: Aranabilir verileri ayıklamak ve bir Azure Search dizinini doldurmak için Azure SQL Database, Azure Cosmos DB veya Azure depolama alanında gezinin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2017
ms.author: heidist
ms.openlocfilehash: 8def65c15d631909c69428a1cb5f100beb1f9b08
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="indexers-in-azure-search"></a>Azure Search'te dizin oluşturucular
> [!div class="op_single_selector"]
>
> * [Genel Bakış](search-indexer-overview.md)
> * [Portal](search-import-data-portal.md)
> * [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
> * [Azure Cosmos DB](search-howto-index-cosmosdb.md)
> * [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)
> * [Azure Tablo Depolama](search-howto-indexing-azure-tables.md)
>

Azure Search'te *dizin oluşturucu*, dış bir veri kaynağından aranabilir verileri ve meta verileri ayıklayan ve dizin ile veri kaynağınız arasında alandan alana eşlemeleri temel alan bir dizini dolduran gezgindir. Bir dizine veri gönderen herhangi bir kodun yazılması gerekmeden hizmetin verileri çekmesi nedeniyle bu yaklaşıma bazen "çekme modeli" de denir.

Dizin oluşturucular veri kaynağı türlerini veya platformlarını temel alır ve Azure'da SQL Server, Cosmos DB, Azure Tablo Depolama ve Blob Depolama gibi ürünler için özel dizin oluşturucular mevcuttur.

Bir dizin oluşturucusunu yalnızca veri alımı amacıyla kullanabilir veya dizininize alanların yalnızca bazılarını yüklemek için bir dizin oluşturucu kullanımını içeren bir teknikler birleşimini kullanabilirsiniz.

Dizin oluşturucuları isteğe bağlı olarak veya her on beş dakikada bir çalıştırılan bir yinelenen veri yenileme zamanlamasıyla çalıştırabilirsiniz. Daha sık güncelleştirmeler için hem Azure Search'te hem de dış veri kaynağınızda verileri aynı anda güncelleştiren bir gönderme modeli gerekir.

## <a name="approaches-for-creating-and-managing-indexers"></a>Dizin oluşturucular oluşturma ve yönetme yaklaşımları

Dizin oluşturucuları aşağıdaki yaklaşımlarla oluşturabilir ve yönetebilirsiniz:

* [Portal > Veri Alma Sihirbazı](search-get-started-portal.md)
* [Hizmet REST API'si](https://msdn.microsoft.com/library/azure/dn946891.aspx)
* [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

Başlangıçta, yeni bir dizin oluşturucu bir önizleme özelliği olarak duyurulur. Önizleme özellikleri API'lere (REST ve .NET) eklenir ve ardından genel kullanıma açık hale geldiklerinde portala entegre edilir. Yeni bir dizin oluşturucuyu değerlendiriyorsanız kod yazmayı planlamanız gerekir.

## <a name="basic-configuration-steps"></a>Temel yapılandırma adımları
Dizin oluşturucular veri kaynağına özgü özellikler sunabilir. Bu bakımdan, dizin oluşturucu veya veri kaynağı yapılandırmasının bazı boyutları dizin oluşturucu türüne göre farklılık gösterir. Bununla birlikte, tüm dizin oluşturucuların temel birleşimi ve gereksinimleri aynıdır. Tüm dizin oluşturucularda ortak olan adımlar aşağıda ele alınmıştır.

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma
Dizin oluşturucu, bağlantı dizesi ve muhtemelen kimlik bilgileri gibi bilgileri içeren bir *veri kaynağından* veri çeker. Şu anda aşağıdaki veri kaynakları desteklenmektedir:

* [Bir Azure sanal makinesinde Azure SQL Veritabanı veya SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* Seçili içerik türleri için [Azure Blob depolama](search-howto-indexing-azure-blob-storage.md)
* [Azure Tablo Depolama](search-howto-indexing-azure-tables.md)

Veri kaynakları, bunları kullanan dizin oluşturuculardan bağımsız olarak yapılandırılır ve yönetilir. Bu da bir veri kaynağının, bir seferde birden çok dizin yüklemek amacıyla birden çok dizin oluşturucu tarafından kullanılabileceği anlamına gelir.

### <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma
Dizin oluşturucu veri alımıyla ilgili bazı görevleri otomatikleştirir, ancak dizin oluşturma genellikle bu görevlerden biri değildir. Bir önkoşul olarak dış veri kaynağınızdaki alanlarla eşleşen alanlara sahip önceden tanımlı bir dizininiz olmalıdır. Bir dizini yapılandırma konusunda daha fazla bilgi için bkz. [Dizin oluşturma (Azure Search REST API’si)](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Alan ilişkilendirme konusunda yardım için bkz. [Azure Search dizin oluşturucularında alan eşlemeleri](search-indexer-field-mappings.md).

> [!Tip]
> Dizin oluşturucular sizin için dizin oluşturamasa da, portaldaki **Verileri içeri aktarma** sihirbazı bu işlem için size yardımcı olabilir. Çoğu durumda, sihirbaz, kaynaktaki mevcut meta verilerden dizin şeması çıkarsayarak, sihirbaz etkin olduğunda satır içinde düzenleyebileceğiniz geçici bir dizin şeması sunar. Hizmet için sihirbaz oluşturulduğunda, portalda yapılabilecek ayrıntılı düzenlemeler, genellikle yeni alanlar eklemeyle sınırlıdır. Sihirbaz dizin oluşturmak için uygun olsa da, düzenlemek için uygun değildir. Uygulama yaparak öğrenmek için, [portal kılavuzundaki](search-get-started-portal.md) adımları izleyin.

### <a name="step-3-create-and-schedule-the-indexer"></a>3. Adım: Dizin oluşturucuyu oluşturma ve zamanlama
Dizin oluşturucu tanımı dizini, veri kaynağını ve bir zamanlamayı belirten bir yapıdır. Bir dizin oluşturucu, aynı abonelikten olduğu sürece başka bir hizmetteki bir veri kaynağına başvurabilir. Bir dizin oluşturucuyu yapılandırma konusunda daha fazla bilgi için bkz. [Dizin Oluşturucu Oluşturma (Azure Search REST API’si)](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

## <a name="next-steps"></a>Sonraki adımlar
Artık temel fikri anladığınıza göre, atmanız gereken bir sonraki adım her bir veri kaynağı türüne özgü gereksinimleri ve görevleri incelemektir.

* [Bir Azure sanal makinesinde Azure SQL Veritabanı veya SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)
* [Azure Tablo Depolama](search-howto-indexing-azure-tables.md)
* [Azure Search Blob dizin oluşturucu kullanarak CSV bloblarını dizine ekleme](search-howto-index-csv-blobs.md)
* [Azure Search Blob dizin oluşturucu ile JSON bloblarını dizine ekleme](search-howto-index-json-blobs.md)
