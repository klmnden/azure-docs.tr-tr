<properties
    pageTitle="Azure Search'te dizin oluşturucular | Microsoft Azure | Barındırılan bulut arama hizmeti"
    description="Aranabilir verileri ayıklamak ve bir Azure Search dizinini doldurmak için Azure SQL Database, DocumentDB veya Azure depolama alanında gezinin."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="04/14/2016"
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

Dizin oluşturucular isteğe bağlı veya her on beş dakikada bir sıklığında çalışan yinelenen bir veri yenileme zamanlamasıyla çalıştırılabilir. Daha sık güncelleştirmeler için hem Azure Search'te hem de dış veri kaynağınızda verileri aynı anda güncelleştiren bir gönderme modeli gerekir.

Dizin oluşturucu, bağlantı dizesi gibi bilgileri içeren bir **veri kaynağından** veri çeker. Şu anda aşağıdaki veri kaynakları desteklenmektedir:

- [Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) (veya bir Azure sanal makinesindeki SQL Server)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Azure Blob depolama alanı](search-howto-indexing-azure-blob-storage.md) (Şu anda önizlemede. PDF, Office belgeleri, HTML, XML'den metin ayıklar.)
- [Azure Table Storage](search-howto-indexing-azure-tables.md) (Şu anda önizlemede)

Veri kaynakları, bunları kullanan dizin oluşturuculardan bağımsız olarak yapılandırılır ve yönetilir. Bu da bir veri kaynağının, bir seferde birden çok dizin yüklemek amacıyla birden çok dizin oluşturucu tarafından kullanılabileceği anlamına gelir. 

Dizin oluşturucuların ve veri kaynaklarının yönetilmesi hem [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx) hem de [Hizmet REST API](https://msdn.microsoft.com/library/azure/dn946891.aspx)'si tarafından desteklenir. 

Alternatif olarak, **Veri İçeri Aktarma** sihirbazını kullanırken portalda bir dizin oluşturucusunu da yapılandırabilirsiniz. Sihirbazı kullanarak bir dizin oluşturma ve yükleme amacıyla örnek verileri ve DocumentDB dizin oluşturucusunu kullanan hızlı bir öğretici için bkz. [Portalda Azure Search ile çalışmaya başlama](search-get-started-portal).






<!----HONumber=Jun16_HO2-->


