---
title: Azure Search dizini yeniden oluşturmanız veya aranabilir içeriği - Azure Search Yenile
description: Yeni öğeler eklemek, var olan öğeleri veya belgeleri güncelleştirin veya tam yeniden derleme veya kısmi artımlı bir Azure Search dizini yenilemek için dizin geçersiz belgelerde silme.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 9c9af69e45af6a70c5327393a1c10385ba2c2aed
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316905"
---
# <a name="how-to-rebuild-an-azure-search-index"></a>Nasıl bir Azure Search dizini yeniden oluşturma

Bir dizin değişiklikleri Azure Search hizmetinizde dizin fiziksel ifade değiştirme yapısını yeniden. Buna karşılık, bir dizin yenileme katkıda bulunan bir dış veri kaynağından en son değişiklikleri alması için bir yalnızca içerik güncelleştirmesidir. Bu makalede, yapısal olarak ve vermeyiz dizinleri güncelleştirme nasıl yönü sağlar.

Dizin güncelleştirmeleri hizmet düzeyinde okuma / yazma izinleri gereklidir. Programlı olarak tam yeniden oluşturma veya güncelleştirme seçenekleri belirterek parametrelerle dizin içeriğini, artımlı REST veya .NET API'leri çağırabilirsiniz. 

Genel olarak, güncelleştirmeleri bir dizin için isteğe bağlı. Ancak, kaynak özgü kullanarak doldurulmuş dizinler için [dizin oluşturucular](search-indexer-overview.md), yerleşik bir Zamanlayıcı'yı kullanabilirsiniz. Zamanlayıcı, belge yenileme hangi aralığı ve düzeni ihtiyaç duyduğunuz kadar her 15 dakikada sıklıkta destekler. Daha hızlı bir yenileme hızı dizin güncelleştirmelerini el ile belki de aynı anda hem dış veri kaynağı hem de Azure Search dizini güncelleştiren işlemleri, bir çift-yazma aracılığıyla gönderilmesi gerekir.

## <a name="full-rebuilds"></a>Tam yeniden oluşturulması

Birçok türden güncelleştirmeler için tam yeniden derleme gereklidir. Tam yeniden derleme hem verileri hem de dış veri kaynaklarından dizini yeniden tarafından meta veriler, bir dizinin silinmesini ifade eder. Programlı olarak [Sil](https://docs.microsoft.com/rest/api/searchservice/delete-index), [oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index), ve [yeniden](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) yeniden oluşturmak için dizini. 

Sonrası yeniden oluşturma, sorgu desenlerine, test ve temel alınan içeriği değiştiyse Puanlama profilleri, değişim sorgu sonuçlarında bekleyebileceğiniz unutmayın.

## <a name="when-to-rebuild"></a>Ne zaman yeniden oluştur

Dizin şemaları flux bir durumda olduğunda sık, tam planında etkin geliştirme sırasında yeniden oluşturur.

| Değiştirme | Durum yeniden oluşturun|
|--------------|---------------|
| Veri türü, alan adı, değiştirme veya kendi [dizin öznitelikleri](https://docs.microsoft.com/rest/api/searchservice/create-index) | Bir alan tanımı değiştirilmesi genellikle bunlar dışında bir yeniden derleme cezası doğurur [dizin öznitelikleridir](https://docs.microsoft.com/rest/api/searchservice/create-index): Alınabilir, SearchAnalyzer, SynonymMaps. Dizinini yeniden derlemeye gerek kalmadan, mevcut bir alanda alınabilir ve SearchAnalyzer SynonymMaps öznitelikler ekleyebilirsiniz.|
| Bir alan ekleme | Yeniden derleme üzerinde katı gereksinimi yoktur. Var olan dizini oluşturulan belgeler, yeni alan için bir null değer verilir. Gelecekteki bir reindex, Azure Search tarafından eklenen null değerlere kaynak veri değerlerini değiştirin. |
| Bir alanı silme | Bu gibi durumlarda, bir alan doğrudan bir Azure Search dizini silemezsiniz. Bunun yerine, uygulamanızın yoksay, kullanmaktan kaçınmak için "silindi" alanı olmalıdır. Fiziksel alan tanımı ve içeriği dizine dizininizi söz konusu alanı atlar bir şema kullanılarak yeniden açana kadar kalır.|

> [!Note]
> Katmanları geçerseniz yeniden de gereklidir. Belirli bir noktada hakkında daha fazla kapasite karar verirseniz, yerinde yükseltme yoktur. Yeni bir hizmet, yeni bir kapasite noktada oluşturulmalı ve sıfırdan yeni hizmette dizinler oluşturulmalıdır. 

## <a name="partial-or-incremental-indexing"></a>Kısmi veya artımlı dizin oluşturma

Dizin üretime girdiğinde, artımlı, genellikle hiçbir ölçek hizmet kesintilerinden ile dizin oluşturma için kaydırır. Kısmi veya artımlı dizin oluşturma içerik katkıda bulunan veri kaynağındaki durumunu yansıtacak şekilde bir arama dizininin içeriğini eşitleyen bir yalnızca içerik iş yüküdür. Eklenen veya kaynak silinmiş bir belge eklendiğinde ya dizini silindi. Kod içinde çağrı [ekleme, güncelleştirme veya silme belgeleri](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) .NET öğreticisini veya işlemi.

> [!Note]
> Dış veri kaynaklarında gezinen dizin oluşturucuları kullanarak, artımlı dizin oluşturma için değişiklik izleme mekanizması kaynak sistemlerde yararlanılabilir. İçin [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection), `lastModified` alanı kullanılır. Üzerinde [Azure tablo depolama](search-howto-indexing-azure-tables.md#incremental-indexing-and-deletion-detection), `timestamp` aynı amaca hizmet eder. Benzer şekilde, her ikisi de [Azure SQL veritabanı dizin oluşturucu](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) ve [Azure Cosmos DB dizinleyici](search-howto-index-cosmosdb.md#indexing-changed-documents) satır güncelleştirmeleri bayrak eklemek için alanlar içerir. Dizin oluşturucular hakkında daha fazla bilgi için bkz: [Dizin oluşturucuya genel bakış](search-indexer-overview.md).


## <a name="see-also"></a>Ayrıca bkz.

+ [Dizin Oluşturucu’ya genel bakış](search-indexer-overview.md)
+ [Büyük veri kümelerinde uygun ölçekte dizini](search-howto-large-index.md)
+ [Portalda dizin oluşturma](search-import-data-portal.md)
+ [Azure SQL veritabanı dizin oluşturucu](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Azure Cosmos DB dizinleyici](search-howto-index-cosmosdb.md)
+ [Azure Blob Depolama dizin oluşturucu](search-howto-indexing-azure-blob-storage.md)
+ [Azure Tablo Depolama dizin oluşturucu](search-howto-indexing-azure-tables.md)
+ [Azure Search'te güvenlik](search-security-overview.md)