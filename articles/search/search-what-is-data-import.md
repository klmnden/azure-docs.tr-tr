<properties
    pageTitle="Azure Search'e veri yükleme | Microsoft Azure | Barındırılan bulut arama hizmeti"
    description="Azure Search'te bir dizine nasıl veri yükleneceğini öğrenin."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager=""
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="05/31/2016"
    ms.author="ashmaka"/>

# Azure Search'e veri yükleme
> [AZURE.SELECTOR]
- [Genel Bakış](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)


## Azure Search'te veri yükleme modelleri
Azure Search dizininizi verilerinizle doldurmanın iki yolu vardır. İlk seçenek, Azure Search [REST API](search-import-data-rest-api.md)'si veya [.NET SDK](search-import-data-dotnet.md) kullanılarak verilerinizi dizine kendinizin iletmesidir. İkinci seçenek, [desteklenen bir veri kaynağını](search-indexer-overview.md) Azure Search dizininizin üzerine getirmek ve Azure Search'ün verilerinizi arama hizmetine otomatik olarak çekmesini sağlamaktır.

Bu kılavuz, yalnızca verileri karşıya yüklemenin gönderme modelinin kullanımı (yalnızca [REST API](search-import-data-rest-api.md) ve [.NET SDK](search-import-data-dotnet.md)'da desteklenir) hakkında yönergeleri kapsar. Ancak aşağıda çekme modeliyle ilgili daha fazla bilgi edinebilirsiniz.

### Bir dizine veri gönderme

Bu yaklaşım, aranabilir hale getirmek için verilerinizi Azure Search'e programlı olarak gönderme anlamına gelir. Çok düşük gecikme süresi gereksinimlerine sahip uygulamalar için (ör. arama işlemlerinin dinamik stok veritabanlarıyla eşitlenmiş olması gerekiyorsa), tek seçeneğiniz gönderme modelidir.

Bir dizine veri göndermek için [REST API](https://msdn.microsoft.com/library/azure/dn798930.aspx) veya [.NET SDK](search-import-data-dotnet.md) kullanabilirsiniz. Şu an portal aracılığıyla veri gönderme için hiçbir araç desteği yoktur.

Belgeleri tek tek veya toplu işlemle karşıya yükleyebileceğinizden (toplu işlem başına en fazla 1000 veya 16 MB sınırlarından hangisi önce gelirse), bu yaklaşım çekme modelinden daha esnektir. Gönderme modeli, verilerinizin nerede olduğuna bakılmaksızın Azure Search'e dosyalarınızı yüklemenizi de sağlar.

### Bir dizine veri çekme

Çekme modeli, desteklenen veri kaynağında gezinir ve Azure Search dizinine verileri sizin için otomatik olarak yükler. Dizin oluşturucular, var olan belgelerdeki değişiklikleri ve silmeleri takip etmenin yanı sıra yeni belgeleri tanıyarak, dizininizdeki verileri aktif şekilde yönetme ihtiyacını ortadan kaldırır.

Azure Search'te bu işlev, şu anda [Blob depolama alanı (önizleme)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer), [Azure SQL Database ve Azure VM'lerdeki SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)'da kullanılabilen *dizin oluşturucular* aracılığıyla uygulanır.

Dizin oluşturucu işlevi, [Azure Portal](search-import-data-portal.md)'ın yanı sıra [REST API](https://msdn.microsoft.com/library/azure/dn946891.aspx)'de de sağlanmaktadır.



<!---HONumber=Jun16_HO2-->


