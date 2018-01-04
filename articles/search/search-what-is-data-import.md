---
title: "Azure Search'te verileri içeri aktarma | Microsoft Docs"
description: "Azure Search'te bir dizine nasıl veri yükleneceğini öğrenin."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: aa8d47c1-4ae6-4209-a8ce-48d5a9474707
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 11/01/2017
ms.author: ashmaka
ms.openlocfilehash: ebf7319f0017b4adef25fe5840864e002c88fea7
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="data-import-in-azure-search"></a>Azure Search'te verileri içeri aktarma
> [!div class="op_single_selector"]
> * [Genel Bakış](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Azure Search'te sorgular, [arama dizinine](search-what-is-an-index.md) yüklenmiş olan içeriğiniz üzerinde yürütülür. Bu makalede içeriği dizine yüklemeye yönelik iki temel yaklaşım incelenir: verilerinizi programlama yoluyla dizine *gönderme* veya desteklenen veri kaynağında verileri *çekmek* için bir [Azure Search dizin oluşturucuya](search-indexer-overview.md) işaret etme.

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
+ [Azure Cosmos DB](http://aka.ms/documentdb-search-indexer)
+ [Azure SQL veritabanı ve Azure VM'lerinde SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

Dizin oluşturucular bir dizini bir veri kaynağına (genelde tablo, görünüm veya eşdeğer bir yapı) bağlar ve kaynak alanları dizindeki eşdeğer alanlara eşler. Yürütme sırasında satır kümesi otomatik olarak JSON'a dönüştürülür ve belirtilen dizine yüklenir. Tüm dizin oluşturucular zamanlamayı destekler ve bu sayede verilerin yenilenme sıklığını belirleyebilirsiniz. Çoğu dizin oluşturucular veri kaynağının desteklemesi durumunda değişiklik izleme özelliği sunar. Dizin oluşturucular, var olan belgelerdeki değişiklikleri ve silmeleri takip etmenin yanı sıra yeni belgeleri tanıyarak, dizininizdeki verileri aktif şekilde yönetme ihtiyacını ortadan kaldırır. 


### <a name="how-to-pull-data-into-an-azure-search-index"></a>Verileri Azure Search dizinine çekme

Dizin oluşturucu işlevleri [Azure portalı](search-import-data-portal.md), [REST API'sı](/rest/api/searchservice/Indexer-operations) ve [.NET SDK'sında](/dotnet/api/microsoft.azure.search.indexersoperations) belirtilmiştir. 

Portalı kullanmanın avantajlarından biri, Azure Search'ün genelde kaynak veri kümesinin meta verilerini okuyarak sizin için varsayılan dizin şeması oluşturabilmesidir. Oluşturulan dizini işlenene kadar değiştirebilirsiniz ancak işlendikten sonra yalnızca dizinin yeniden oluşturulmasını gerektirmeyen şema düzenlemelerine izin verilir. Yapmak istediğiniz değişikliklerin şemayı doğrudan etkilemesi halinde dizini yeniden oluşturmanız gerekir. 

## <a name="verify-data-import-with-search-explorer"></a>Arama Gezgini ile veri içeri aktarma işlemini doğrulama

Portalda **Arama Gezgini**'ni kullanarak yüklenen belgede hızlı bir ön denetim gerçekleştirebilirsiniz. Bu gezgin, bir dizini kod yazmadan sorgulamanızı sağlar. Arama deneyimi [basit söz dizimi](/rest/api/searchservice/simple-query-syntax-in-azure-search) ve varsayılan [searchMode sorgu parametresi](/rest/api/searchservice/search-documents) gibi varsayılan ayarlara bağlıdır. Belgenin tamamını inceleyebilmeniz için sonuçlar JSON biçiminde döndürülür.

> [!TIP]
> Çok sayıda [Azure Search kod örneği](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) tarafından sunulan yerleşik veya hazır veri kümeleri hızlı bir şekilde kullanmaya başlamanızı sağlar. Portalda ayrıca örnek dizin oluşturucu ve küçük bir emlak veri kümesini ("realestate-us-sample" adlı) içeren veri kaynağı mevcuttur. Önceden yapılandırılmış dizin oluşturucuyu örnek veri kaynağında çalıştırdığınızda oluşturulan ve belgelerle yüklenen dizini Arama Gezgini veya yazdığınız kod aracılığıyla sorgulayabilirsiniz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
+ [Portal kılavuzu: dizini oluşturma, yükleme, sorgulama](search-get-started-portal.md)