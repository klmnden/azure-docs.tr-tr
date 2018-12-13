---
title: Dizin oluşturucular dizin oluşturma sırasında-gezinme veri kaynakları için Azure arama
description: Aranabilir verileri ayıklamak ve bir Azure Search dizinini doldurmak için Azure SQL Database, Azure Cosmos DB veya Azure depolama alanında gezinin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: b763da25fb5a707e6a0c0cee1aabbe3eda03ee4c
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53312383"
---
# <a name="indexers-in-azure-search"></a>Azure Search'te dizin oluşturucular

Bir *dizin oluşturucu* Azure Search'te dizin ile veri kaynağınız arasında alanın alan eşlemeleri, bir Azure dış veri kaynağından aranabilir verileri ve meta verileri ayıklar ve bir dizini dolduran bir Gezgin dayanır. Bir dizine veri gönderen herhangi bir kodun yazılması gerekmeden hizmetin verileri çekmesi nedeniyle bu yaklaşıma bazen "çekme modeli" de denir.

Dizin oluşturucular veri kaynağı türlerini veya platformlarını temel alır ve Azure'da SQL Server, Cosmos DB, Azure Tablo Depolama ve Blob Depolama gibi ürünler için özel dizin oluşturucular mevcuttur.

Bir dizin oluşturucusunu yalnızca veri alımı amacıyla kullanabilir veya dizininize alanların yalnızca bazılarını yüklemek için bir dizin oluşturucu kullanımını içeren bir teknikler birleşimini kullanabilirsiniz.

Dizin oluşturucuları isteğe bağlı olarak veya her on beş dakikada bir çalıştırılan bir yinelenen veri yenileme zamanlamasıyla çalıştırabilirsiniz. Daha sık güncelleştirmeler için hem Azure Search'te hem de dış veri kaynağınızda verileri aynı anda güncelleştiren bir gönderme modeli gerekir.

## <a name="approaches-for-creating-and-managing-indexers"></a>Dizin oluşturucular oluşturma ve yönetme yaklaşımları

Dizin oluşturucuları aşağıdaki yaklaşımlarla oluşturabilir ve yönetebilirsiniz:

* [Portal > Veri Alma Sihirbazı](search-import-data-portal.md)
* [Hizmet REST API'si](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations)
* [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.iindexersoperations)

Başlangıçta, yeni bir dizin oluşturucu bir önizleme özelliği olarak duyurulur. Önizleme özellikleri API'lere (REST ve .NET) eklenir ve ardından genel kullanıma açık hale geldiklerinde portala entegre edilir. Yeni bir dizin oluşturucuyu değerlendiriyorsanız kod yazmayı planlamanız gerekir.


<a name="supported-data-sources"></a>

## <a name="supported-data-sources"></a>Desteklenen veri kaynakları

Dizin oluşturucular veri depoları Azure'da gezinin.

* [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)
* [Azure Tablo Depolama](search-howto-indexing-azure-tables.md)


## <a name="basic-configuration-steps"></a>Temel yapılandırma adımları
Dizin oluşturucular veri kaynağına özgü özellikler sunabilir. Bu bakımdan, dizin oluşturucu veya veri kaynağı yapılandırmasının bazı boyutları dizin oluşturucu türüne göre farklılık gösterir. Bununla birlikte, tüm dizin oluşturucuların temel birleşimi ve gereksinimleri aynıdır. Tüm dizin oluşturucularda ortak olan adımlar aşağıda ele alınmıştır.

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma
Dizin oluşturucu, bağlantı dizesi ve muhtemelen kimlik bilgileri gibi bilgileri içeren bir *veri kaynağından* veri çeker. Çağrı [veri kaynağı oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-data-source) REST API veya [veri kaynağı sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource) kaynak oluşturmak için.

Veri kaynakları, bunları kullanan dizin oluşturuculardan bağımsız olarak yapılandırılır ve yönetilir. Bu da bir veri kaynağının, bir seferde birden çok dizin yüklemek amacıyla birden çok dizin oluşturucu tarafından kullanılabileceği anlamına gelir.

### <a name="step-2-create-an-index"></a>2. Adım: Dizin oluşturma
Dizin oluşturucu veri alımıyla ilgili bazı görevleri otomatikleştirir, ancak dizin oluşturma genellikle bu görevlerden biri değildir. Bir önkoşul olarak dış veri kaynağınızdaki alanlarla eşleşen alanlara sahip önceden tanımlı bir dizininiz olmalıdır. Dizin yapısı hakkında daha fazla bilgi için bkz. [(Azure Search REST API'si) dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/Create-Index) veya [dizin sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index). Alan ilişkilendirme konusunda yardım için bkz. [Azure Search dizin oluşturucularında alan eşlemeleri](search-indexer-field-mappings.md).

> [!Tip]
> Dizin oluşturucular sizin için dizin oluşturamasa da, portaldaki **Verileri içeri aktarma** sihirbazı bu işlem için size yardımcı olabilir. Çoğu durumda, sihirbaz, kaynaktaki mevcut meta verilerden dizin şeması çıkarsayarak, sihirbaz etkin olduğunda satır içinde düzenleyebileceğiniz geçici bir dizin şeması sunar. Hizmet için sihirbaz oluşturulduğunda, portalda yapılabilecek ayrıntılı düzenlemeler, genellikle yeni alanlar eklemeyle sınırlıdır. Sihirbaz dizin oluşturmak için uygun olsa da, düzenlemek için uygun değildir. Uygulama yaparak öğrenmek için, [portal kılavuzundaki](search-get-started-portal.md) adımları izleyin.

### <a name="step-3-create-and-schedule-the-indexer"></a>3. adım: Oluşturma ve zamanlama dizin oluşturucu
Dizin oluşturucu tanımı dizini, veri kaynağını ve bir zamanlamayı belirten bir yapıdır. Bir dizin oluşturucu, aynı abonelikten olduğu sürece başka bir hizmetteki bir veri kaynağına başvurabilir. Bir dizin oluşturucuyu yapılandırma konusunda daha fazla bilgi için bkz. [Dizin Oluşturucu Oluşturma (Azure Search REST API’si)](https://docs.microsoft.com/rest/api/searchservice/Create-Indexer).

## <a name="next-steps"></a>Sonraki adımlar
Artık temel fikri anladığınıza göre, atmanız gereken bir sonraki adım her bir veri kaynağı türüne özgü gereksinimleri ve görevleri incelemektir.

* [Bir Azure sanal makinesinde Azure SQL Veritabanı veya SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)
* [Azure Tablo Depolama](search-howto-indexing-azure-tables.md)
* [Azure Search Blob dizin oluşturucu kullanarak CSV bloblarını dizine ekleme](search-howto-index-csv-blobs.md)
* [Azure Search Blob dizin oluşturucu ile JSON bloblarını dizine ekleme](search-howto-index-json-blobs.md)
