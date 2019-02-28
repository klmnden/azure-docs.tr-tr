---
title: Veri alımı için bir arama dizininin - Azure Search veri içeri aktarın
description: Doldurma ve dış veri kaynaklarından Azure Search'te bir dizine veri yükleyin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 85a2810e8ab8de5ad2967aaf17f421d871368063
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56958465"
---
# <a name="indexing-external-data-for-queries-in-azure-search"></a>Azure Search'te sorgular için dış veri dizini oluşturma

Azure Search'te yüklenmiş ve kaydedilmiş içeriğinize sorguları yürütme bir [arama dizini](search-what-is-an-index.md). Bu makalede dizini doldurmak için iki temel yaklaşım inceler: *anında iletme* verileriniz dizine programlı olarak veya bir [Azure Search dizin oluşturucu](search-indexer-overview.md) için bir desteklenen veri kaynağında  *çekme* veri.

Her iki yaklaşım ile amacı sağlamaktır *veri yükleme* Azure Search dizini içine bir dış veri kaynağından. Azure arama, boş bir dizin oluşturma sağlar, ancak anında iletme veya veri içine çekmek kadar sorgulanabilir değil.

## <a name="pushing-data-to-an-index"></a>Verileri dizine gönderme
Verilerinizi programlama yoluyla Azure Search'e göndermek için kullanılan gönderme yöntemi, en esnek yöntemdir. Birincisi, veri kaynağı türüne hiçbir kısıtlama getirmez. Veri kümesindeki her belgede, dizin şemanızda tanımlanan alanlarla eşlenen alanlar bulunduğu varsayımıyla, JSON belgelerinden oluşan tüm veri kümeleri Azure Search dizinine gönderilebilir. İkincisi, yürütme frekansı üzerinde hiçbir kısıtlaması yoktur. Değişiklikleri istediğiniz sıklıkta dizine gönderebilirsiniz. Çok düşük gecikme süresi gereksinimlerine sahip uygulamalar için (örneğin, arama işlemlerinin dinamik stok veritabanlarıyla eşitlenmiş olması gerekiyorsa), tek seçeneğiniz gönderme modelidir.

Belgeleri tek tek veya toplu işlemle karşıya yükleyebileceğinizden (toplu işlem başına en fazla 1000 veya 16 MB sınırlarından hangisi önce gelirse), bu yaklaşım çekme modelinden daha esnektir. Gönderme modeli, verilerinizin nerede olduğuna bakılmaksızın Azure Search'e dosyalarınızı yüklemenizi de sağlar.

### <a name="how-to-push-data-to-an-azure-search-index"></a>Verileri Azure Search dizinine gönderme

Dizin bir tek veya birden çok belge yüklemek için şu API'leri kullanabilirsiniz:

+ [Belge Ekleme, Güncelleştirme veya Silme (REST API)](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents)
+ [indexAction sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction?view=azure-dotnet) veya [indexBatch sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) 

Şu an portal aracılığıyla veri gönderme için hiçbir araç desteği yoktur.

Her yönteme giriş bilgileri için bkz. [REST kullanarak verileri içeri aktarma](search-import-data-rest-api.md) veya [.NET kullanarak verileri içeri aktarma](search-import-data-dotnet.md).


## <a name="pulling-data-into-an-index"></a>Verileri dizine çekme
Çekme modeli, desteklenen veri kaynağında gezinir ve dizininize verileri otomatik olarak yükler. Azure Search'te bu özellik *dizin oluşturucular* aracılığıyla uygulanır ve şu anda aşağıdaki platformlarda kullanılabilir:

+ [Blob depolama](search-howto-indexing-azure-blob-storage.md)
+ [Tablo depolama](search-howto-indexing-azure-tables.md)
+ [Azure Cosmos DB](https://aka.ms/documentdb-search-indexer)
+ [Azure SQL veritabanı ve Azure VM'lerinde SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

Dizin oluşturucular bir dizini bir veri kaynağına (genelde tablo, görünüm veya eşdeğer bir yapı) bağlar ve kaynak alanları dizindeki eşdeğer alanlara eşler. Yürütme sırasında satır kümesi otomatik olarak JSON'a dönüştürülür ve belirtilen dizine yüklenir. Tüm dizin oluşturucular zamanlamayı destekler ve bu sayede verilerin yenilenme sıklığını belirleyebilirsiniz. Çoğu dizin oluşturucular veri kaynağının desteklemesi durumunda değişiklik izleme özelliği sunar. Dizin oluşturucular, var olan belgelerdeki değişiklikleri ve silmeleri takip etmenin yanı sıra yeni belgeleri tanıyarak, dizininizdeki verileri aktif şekilde yönetme ihtiyacını ortadan kaldırır. 


### <a name="how-to-pull-data-into-an-azure-search-index"></a>Verileri Azure Search dizinine çekme

Dizin oluşturucu işlevleri [Azure portalı](search-import-data-portal.md), [REST API'sı](/rest/api/searchservice/Indexer-operations) ve [.NET SDK'sında](/dotnet/api/microsoft.azure.search.indexersoperationsextensions) belirtilmiştir. 

Portalı kullanmanın avantajlarından biri, Azure Search'ün genelde kaynak veri kümesinin meta verilerini okuyarak sizin için varsayılan dizin şeması oluşturabilmesidir. Oluşturulan dizini işlenene kadar değiştirebilirsiniz ancak işlendikten sonra yalnızca dizinin yeniden oluşturulmasını gerektirmeyen şema düzenlemelerine izin verilir. Yapmak istediğiniz değişikliklerin şemayı doğrudan etkilemesi halinde dizini yeniden oluşturmanız gerekir. 

## <a name="verify-data-import-with-search-explorer"></a>Arama Gezgini ile veri alma doğrulayın

Belgede bir ön denetimi gerçekleştirmek için hızlı bir şekilde kullanmaktır **arama Gezgini** portalında. Bu gezgin, bir dizini kod yazmadan sorgulamanızı sağlar. Arama deneyimi [basit söz dizimi](/rest/api/searchservice/simple-query-syntax-in-azure-search) ve varsayılan [searchMode sorgu parametresi](/rest/api/searchservice/search-documents) gibi varsayılan ayarlara bağlıdır. Belgenin tamamını inceleyebilmeniz için sonuçlar JSON biçiminde döndürülür.

> [!TIP]
> Çok sayıda [Azure Search kod örneği](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) tarafından sunulan yerleşik veya hazır veri kümeleri hızlı bir şekilde kullanmaya başlamanızı sağlar. Portalda ayrıca örnek dizin oluşturucu ve küçük bir emlak veri kümesini ("realestate-us-sample" adlı) içeren veri kaynağı mevcuttur. Önceden yapılandırılmış dizin oluşturucuyu örnek veri kaynağında çalıştırdığınızda, bir dizin oluşturulur ve ardından arama Gezgini veya yazdığınız kod tarafından sorgulanabilir belgelerle birlikte yüklenir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
+ [Portal kılavuzu: dizini oluşturma, yükleme, sorgulama](search-get-started-portal.md)
