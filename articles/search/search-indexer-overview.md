<properties
    pageTitle="Azure Search'te dizin oluşturucular | Microsoft Azure | Barındırılan bulut arama hizmeti"
    description="Aranabilir verileri ayıklamak ve bir Azure Search dizinini doldurmak için Azure SQL Database, DocumentDB veya Azure depolama alanında gezinin."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/08/2016"
    ms.author="heidist"/>


# Azure Search'te dizin oluşturucular
> [AZURE.SELECTOR]
- [Genel Bakış](search-indexer-overview.md)
- [Portal](search-import-data-portal.md)
- [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Blob Storage (önizleme)](search-howto-indexing-azure-blob-storage.md)
- [Table Storage (önizleme)](search-howto-indexing-azure-tables.md)

Azure Search'te **dizin oluşturucu**, dış bir veri kaynağından aranabilir verileri ve meta verileri ayıklayan ve dizin ile veri kaynağınız arasında alandan alana eşlemeleri temel alan bir dizini dolduran gezgindir. Bir dizine veri gönderen herhangi bir kodun yazılması gerekmeden hizmetin verileri çekmesi nedeniyle bu yaklaşıma bazen "çekme modeli" de denir.

Bir dizin oluşturucusunu yalnızca veri alımı amacıyla kullanabilir veya dizininize alanların yalnızca bazılarını yüklemek için bir dizin oluşturucu kullanımını içeren bir teknikler birleşimini kullanabilirsiniz.

Dizin oluşturucuları isteğe bağlı olarak veya her on beş dakikada bir çalıştırılan bir yinelenen veri yenileme zamanlamasıyla çalıştırabilirsiniz. Daha sık güncelleştirmeler için hem Azure Search'te hem de dış veri kaynağınızda verileri aynı anda güncelleştiren bir gönderme modeli gerekir.

## Dizin oluşturucular oluşturma ve yönetme yaklaşımları

Azure SQL veya DocumentDB gibi genel kullanıma açık dizin oluşturucular için bu yaklaşımları kullanarak dizin oluşturucular oluşturabilir ve bunları yönetebilirsiniz:

- [Portal > Veri Alma Sihirbazı ](search-get-started-portal.md)
- [Hizmet REST API'si](https://msdn.microsoft.com/library/azure/dn946891.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

Azure Blob veya Tablo depolama gibi önizleme dizin oluşturucuları, [Dizin oluşturucular için Azure Search Önizleme REST API’si](search-api-indexers-2015-02-28-preview.md) gibi önizleme API’leri gerektirir. Önizleme özellikleri için genellikle portal araçları kullanılamaz.

## Temel yapılandırma adımları

Dizin oluşturucular veri kaynağına özgü özellikler sunabilir. Bu bakımdan, dizin oluşturucu veya veri kaynağı yapılandırmasının bazı boyutları dizin oluşturucu türüne göre farklılık gösterir. Bununla birlikte, tüm dizin oluşturucuların temel birleşimi ve gereksinimleri aynıdır. Tüm dizin oluşturucularda ortak olan adımlar aşağıda ele alınmıştır.

### 1. Adım: Dizin oluşturma

Dizin oluşturucu veri alımıyla ilgili bazı görevleri otomatikleştirir, ancak dizin oluşturma bu görevlerden biri değildir. Bir önkoşul olarak dış veri kaynağınızdaki alanlarla eşleşen alanlara sahip önceden tanımlı bir dizininiz olmalıdır. Bir dizini yapılandırma konusunda daha fazla bilgi için bkz. [Dizin oluşturma (Azure Search REST API’si)](https://msdn.microsoft.com/library/azure/dn798941.aspx).

### 2. Adım: Veri kaynağı oluşturma

Dizin oluşturucu, bağlantı dizesi gibi bilgileri içeren bir **veri kaynağından** veri çeker. Şu anda aşağıdaki veri kaynakları desteklenmektedir:

- [Bir Azure sanal makinesinde Azure SQL Veritabanı veya SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Azure Blob depolama (Önizleme)](search-howto-indexing-azure-blob-storage.md): PDF, Office belgeleri, HTML veya XML’den metin ayıklamak için kullanılır
- [Azure Tablo Depolama (Önizleme)](search-howto-indexing-azure-tables.md)

Veri kaynakları, bunları kullanan dizin oluşturuculardan bağımsız olarak yapılandırılır ve yönetilir. Bu da bir veri kaynağının, bir seferde birden çok dizin yüklemek amacıyla birden çok dizin oluşturucu tarafından kullanılabileceği anlamına gelir. 

### 3. Adım: Dizin oluşturucuyu oluşturma ve zamanlama

Dizin oluşturucu tanımı dizini, veri kaynağını ve bir zamanlamayı belirten bir yapıdır. Bir dizin oluşturucu, aynı abonelikten olduğu sürece başka bir hizmetteki bir veri kaynağına başvurabilir. Bir dizin oluşturucuyu yapılandırma konusunda daha fazla bilgi için bkz. [Dizin Oluşturucu Oluşturma (Azure Search REST API’si)](https://msdn.microsoft.com/library/azure/dn946899.aspx).

## Sonraki adımlar

Artık temel fikri anladığınıza göre, atmanız gereken bir sonraki adım her bir veri kaynağı türüne özgü gereksinimleri ve görevleri incelemektir.

- [Bir Azure sanal makinesinde Azure SQL Veritabanı veya SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Azure Blob depolama (Önizleme)](search-howto-indexing-azure-blob-storage.md): PDF, Office belgeleri, HTML veya XML’den metin ayıklamak için kullanılır
- [Azure Tablo Depolama (Önizleme)](search-howto-indexing-azure-tables.md)
- [Azure Search Blob dizin oluşturucu (Önizleme) kullanarak CSV bloblarını dizine ekleme](search-howto-index-csv-blobs.md)
- [Azure Search Blob dizin oluşturucu (Önizleme) ile JSON bloblarını dizine ekleme](search-howto-index-json-blobs.md)




<!--HONumber=Sep16_HO3-->


