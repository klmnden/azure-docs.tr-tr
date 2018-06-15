---
title: Dizin Sorgulama (portal - Azure Search) | Microsoft Docs
description: Azure Portal'ın Arama Gezgini'ninde arama sorgusu gönderin.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: quickstart
ms.date: 07/10/2017
ms.author: heidist
ms.openlocfilehash: a3592bd0c304dfb78374eeba432c0d28203980c9
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31790517"
---
# <a name="query-an-azure-search-index-using-search-explorer-in-the-azure-portal"></a>Azure Portal’ın Arama Gezgini’ni kullanarak Azure Search dizinini sorgulama
> [!div class="op_single_selector"]
> * [Genel Bakış](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Bu makalede, Azure Portal’ın **Arama Gezgini**’ni kullanarak bir Azure Search dizininin nasıl sorgulanacağı gösterilir. Hizmetinizde var olan herhangi bir dizine basit veya tam Lucene sorgu dizeleri göndermek için Arama Gezgini’ni kullanabilirsiniz.

## <a name="open-the-service-dashboard"></a>Hizmet panosunu açma
1. [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)'ın sol tarafındaki atlama çubuğunda **Tüm kaynaklar**’a tıklayın.
2. Azure Search hizmetinizi seçin.

## <a name="select-an-index"></a>Dizin seçme

**Dizinler** kutucuğunda aramak istediğiniz dizini seçin.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Arama Gezgini’ni açma

Arama çubuğunu ve sonuçlar bölmesini kaydırarak açmak için Arama Gezgini kutucuğuna tıklayın.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Aramayı başlatma

Arama Gezgini'ni kullanırken sorguyu formüle etmek için [sorgu parametreleri](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) belirtebilirsiniz.

1. **Sorgu dizesi**’nde sorguyu yazın ve **Ara**’ya basın. 

   Azure Search REST API'sine HTTP isteği göndermek için, sorgu dizesi uygun istek URL'si içine otomatik olarak ayrıştırılır.   
   
   İsteği oluşturmak için herhangi bir geçerli basit veya tam Lucene sorgu söz dizimini kullanabilirsiniz. `*` karakteri, tüm belgelerin belirli bir sırada olmaksızın döndürüldüğü boş veya belirtilmemiş aramaya eşdeğerdir.

2. **Sonuçlar**’da sorgu sonuçları ham JSON olarak verilir; bu, HTTP Yanıt Gövdesinde programlama aracılığıyla istek verdiğinizde döndürülen yükle özdeştir.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki kaynaklar ek sorgu söz dizimi bilgileri ve örnekler içerir.

 + [Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Lucene sorgu söz dizimi örnekleri](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [OData Filtre ifadesinin söz dizimi](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 