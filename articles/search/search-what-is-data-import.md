---
title: "Azure Search'e veri yükleme | Microsoft Belgeleri"
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
ms.date: 05/01/2017
ms.author: ashmaka
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 5a601b75ec67824e72d8736bc3c45f8e1231ca86
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017

---
# <a name="upload-data-to-azure-search"></a>Azure Search'e veri yükleme
> [!div class="op_single_selector"]
> * [Genel Bakış](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Bir dizini verilerinizle doldurmanın iki yolu vardır. İlk seçenek, Azure Search [REST API](search-import-data-rest-api.md)'si veya [.NET SDK](search-import-data-dotnet.md) kullanılarak verilerinizi dizine kendinizin iletmesidir. İkinci seçenek, [desteklenen bir veri kaynağını](search-indexer-overview.md) dizininizin üzerine getirmek ve Azure Search'ün verilerinizi otomatik olarak çekmesini sağlamaktır.

## <a name="push-data-to-an-index"></a>Bir dizine veri gönderme
Bu yaklaşım, aranabilir hale getirmek için verilerinizi Azure Search'e programlı olarak gönderme anlamına gelir. Çok düşük gecikme süresi gereksinimlerine sahip uygulamalar için (örneğin, arama işlemlerinin dinamik stok veritabanlarıyla eşitlenmiş olması gerekiyorsa), tek seçeneğiniz gönderme modelidir.

Bir dizine veri göndermek için [REST API](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) veya [.NET SDK](search-import-data-dotnet.md) kullanabilirsiniz. Şu an portal aracılığıyla veri gönderme için hiçbir araç desteği yoktur.

Belgeleri tek tek veya toplu işlemle karşıya yükleyebileceğinizden (toplu işlem başına en fazla 1000 veya 16 MB sınırlarından hangisi önce gelirse), bu yaklaşım çekme modelinden daha esnektir. Gönderme modeli, verilerinizin nerede olduğuna bakılmaksızın Azure Search'e dosyalarınızı yüklemenizi de sağlar.

Azure Search, JSON biçimindeki verileri işler ve veri kümesindeki tüm belgelerin dizin şemanızda tanımlı alanlara karşılık gelen alanlara sahip olmaları gerekir. 

## <a name="pull-data-into-an-index"></a>Bir dizine veri çekme
Çekme modeli, desteklenen veri kaynağında gezinir ve dizininize verileri otomatik olarak yükler. Azure Search'te bu işlev, şu anda [Blob depolama](search-howto-indexing-azure-blob-storage.md), [Tablo depolama](search-howto-indexing-azure-tables.md), [Azure Cosmos DB](http://aka.ms/documentdb-search-indexer), [Azure SQL Veritabanı ve Azure VM'lerdeki SQL Server](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)'da kullanılabilen *dizin oluşturucular* aracılığıyla uygulanır. 

Dizin oluşturucular bir dizini bir veri kaynağına (genelde tablo, görünüm veya eşdeğer bir yapı) bağlar ve kaynak alanları dizindeki eşdeğer alanlara eşler. Yürütme sırasında satır kümesi otomatik olarak JSON'a dönüştürülür ve belirtilen dizine yüklenir. Tüm dizin oluşturucular zamanlamayı destekler ve bu sayede verilerin yenilenme sıklığını belirleyebilirsiniz. Çoğu dizin oluşturucular veri kaynağının desteklemesi durumunda değişiklik izleme özelliği sunar. Dizin oluşturucular, var olan belgelerdeki değişiklikleri ve silmeleri takip etmenin yanı sıra yeni belgeleri tanıyarak, dizininizdeki verileri aktif şekilde yönetme ihtiyacını ortadan kaldırır. 

Dizin oluşturucu işlevleri [Azure portalı](search-import-data-portal.md), [REST API'sı](/rest/api/searchservice/Indexer-operations) ve [.NET SDK'sında](/dotnet/api/microsoft.azure.search.indexersoperations) belirtilmiştir. 

Portalı kullanmanın avantajlarından biri, Azure Search'ün genelde kaynak veri kümesinin meta verilerini okuyarak sizin için varsayılan dizin şeması oluşturabilmesidir. Oluşturulan dizini işlenene kadar değiştirebilirsiniz ancak işlendikten sonra yalnızca dizinin yeniden oluşturulmasını gerektirmeyen şema düzenlemelerine izin verilir. Yapmak istediğiniz değişikliklerin şemayı doğrudan etkilemesi halinde dizini yeniden oluşturmanız gerekir. 

Dizin doldurulduktan sonra doğrulamak için portal komut çubuğundaki **Arama Gezgini**'ni kullanabilirsiniz.

## <a name="query-an-index-using-search-explorer"></a>Arama Gezgini kullanarak dizin sorgulama

Portalda **Arama Gezgini**'ni kullanarak yüklenen belgede hızlı bir ön denetim gerçekleştirebilirsiniz. Bu gezgin, bir dizini kod yazmadan sorgulamanızı sağlar. Arama deneyimi [basit söz dizimi](/rest/api/searchservice/simple-query-syntax-in-azure-search) ve varsayılan [searchMode sorgu parametresi](/rest/api/searchservice/search-documents) gibi varsayılan ayarlara bağlıdır. Belgenin tamamını inceleyebilmeniz için sonuçlar JSON biçiminde döndürülür.

> [!TIP]
> Çok sayıda [Azure Search kod örneği](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) tarafından sunulan yerleşik veya hazır veri kümeleri hızlı bir şekilde kullanmaya başlamanızı sağlar. Portalda ayrıca örnek dizin oluşturucu ve küçük bir emlak veri kümesini ("realestate-us-sample" adlı) içeren veri kaynağı mevcuttur. Önceden yapılandırılmış dizin oluşturucuyu örnek veri kaynağında çalıştırdığınızda oluşturulan ve belgelerle yüklenen dizini Arama Gezgini veya yazdığınız kod aracılığıyla sorgulayabilirsiniz.
