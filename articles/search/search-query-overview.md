---
title: Azure Search dizininizi sorgulama | Microsoft Belgeleri
description: "Azure Search'te bir arama sorgusu oluşturun ve arama sonuçlarını filtrelemek ve sıralamak için arama parametrelerini kullanın."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 69205d7a-363f-4b92-a53f-6ca818a3d2c7
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 04/26/2017
ms.author: ashmaka
ms.openlocfilehash: 01be1b14e838c4f1b6f2498111fb8369c2bbb92a
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="query-your-azure-search-index"></a>Azure Search dizininizi sorgulama
> [!div class="op_single_selector"]
> * [Genel Bakış](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Azure Search'e arama istekleri gönderirken, uygulamanızın arama kutusuna yazılan gerçek sözcüklerin yanı sıra belirtilebilecek birkaç parametre bulunur. Bu sorgu parametreleri, [tam metin arama deneyiminde](search-lucene-query-architecture.md) biraz daha derin denetim elde etmenizi sağlar.

Azure Search'te sorgu parametrelerinin ortak kullanımlarını kısaca açıklayan bir liste aşağıda bulunmaktadır. Sorgu parametrelerinin ve bunların davranışlarının tam kapsamı için lütfen [REST API](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) ve [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters#microsoft_azure_search_models_searchparameters#properties_summary) için ayrıntılı bilgilerin yer aldığı sayfalara bakın.

## <a name="types-of-queries"></a>Sorgu türleri
Azure Search, son derece güçlü sorgular oluşturmak için birçok seçenek sunar. Kullanacağınız iki ana sorgu türü `search` ve `filter` sorgularıdır. `search` sorgusu, dizininizdeki tüm *aranabilir* alanlarda bir veya daha çok terimi arar ve Google veya Bing gibi bir arama alt yapısının çalışmasını beklediğiniz şekilde çalışır. `filter` sorgusu, bir dizindeki tüm *filtrelenebilir* alanlarda bir boole ifadesini değerlendirir. `search` sorgularının aksine, `filter` sorguları bir alanın tam içeriğini eşleştirir; bu da dize alanları için büyük/küçük harfe duyarlı olduklarını gösterir.

Aramaları ve filtreleri birlikte veya ayrı olarak kullanabilirsiniz. Bunları birlikte kullandığınızda filtre öncelikle tüm dizine uygulanır ve ardından filtrenin sonuçlarında arama gerçekleştirilir. Filtreler arama sorgusunun işlemesi gereken belge kümesini azalttığından, sorgu performansını iyileştirmeye yönelik kullanışlı bir teknik olabilir.

Filtre ifadeleri için söz dizimi, [OData filtre dilinin](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) bir alt kümesidir. Arama sorguları için aşağıda açıklanan [basitleştirilmiş söz dizimini](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search) veya [Lucene sorgu söz dizimini](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search) kullanabilirsiniz.

### <a name="simple-query-syntax"></a>Basit sorgu söz dizimi
[Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search), Azure Search'te kullanılan varsayılan sorgu dildir. Basit sorgu söz dizimi AND, OR, NOT, tümcecik, sonek ve öncelik işleçleri dahil olmak üzere birkaç ortak arama işleçlerini destekler.

### <a name="lucene-query-syntax"></a>Lucene sorgu söz dizimi
[Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search), [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)'in parçası olarak geliştirilen yaygın şekilde benimsenmiş ve ifade gücüne sahip sorgu dili kullanmanızı sağlar.

Bu sorgu söz diziminin kullanılması, şu işlevleri kolayca elde etmenizi sağlar: [Alan kapsamlı sorgular](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fields), [belirsiz arama](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_fuzzy), [yakınlık araması](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_proximity), [terim artırma](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_termboost), [normal ifade araması](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_regex), [joker karakterle arama](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_wildcard), [temel söz dizimi bilgileri](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_syntax) ve [boole işleçlerini kullanan sorgular](https://docs.microsoft.com/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search#bkmk_boolean).

## <a name="ordering-results"></a>Sonuçları sıralama
Bir arama sorgusunun sonuçları alınırken, Azure Search'ün sonuçları belirli bir alandaki değerlere göre sıralayarak sunmasını isteyebilirsiniz. Varsayılan olarak Azure Search, her bir belgenin [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)'den türetilen arama puanı sıralamasını temel alarak arama sonuçlarını sıralar.

Azure Search'ün sonuçlarınızı arama puanı dışında bir değere göre sıralayarak döndürmesini istiyorsanız `orderby` arama parametresini kullanabilirsiniz. `orderby` parametresinin değerini, alan adlarını ve jeo-uzamsal değerler için [`geo.distance()` işlevine](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search) çağrıları içerecek şekilde belirtebilirsiniz. Her bir ifadenin ardından, sonuçların artan sıralamada istendiğini belirtmek için `asc`, sonuçların azalan sıralamada istendiğini belirtmek için ise `desc` gelebilir. Artan sıralama varsayılandır.

## <a name="paging"></a>Sayfalama
Azure Search, arama sonuçlarının sayfalanması uygulamasını kolaylaştırır. `top` ve `skip` parametrelerini kullanarak, tüm arama sonuçları kümesini, iyi arama kullanıcı arabirimi uygulamalarını kolayca etkinleştiren yönetilebilir ve sıralı alt kümeler halinde almanızı sağlayan arama isteklerini sorunsuz bir şekilde gönderebilirsiniz. Bu daha küçük sonuç alt kümelerini alırken, tüm arama sonuçları kümesindeki belge sayısını da alabilirsiniz.

[Azure Search'te arama sonuçlarını numaralandırma](search-pagination-page-layout.md) makalesinde arama sonuçlarının numaralanması hakkında daha fazla bilgi alabilirsiniz.

## <a name="hit-highlighting"></a>İsabet vurgulama
Azure Search'te arama sonuçlarının arama sorgusuyla tam olarak eşleşen kısmının vurgulanması `highlight`, `highlightPreTag` ve `highlightPostTag` parametreleri kullanılarak kolaylaştırılır. Hangi *aranabilir* alanların eşleşen metninin vurgulanacağının yanı sıra Azure Search'ün döndürdüğü eşleşen metnin başına ve sonuna eklenecek dize etiketlerini tam olarak belirtebilirsiniz.

## <a name="try-out-query-syntax"></a>Sorgu söz dizimini deneme

Söz dizimi farklılıklarını anlamak için en iyi yol, sorgu göndermek ve sonuçları gözden geçirmektir.

+ Azure portalında [Arama Gezgini](search-explorer.md)'ni kullanın. [Örnek dizini](search-get-started-portal.md) dağıttıktan sonra portaldaki araçları kullanarak dakikalar içinde dizini sorgulayabilirsiniz.

+ Arama hizmetinize yüklediğiniz bir dizine sorgu göndermek için Telerik Fiddler'ı veya Chrome Postman'i kullanın. Her iki araç da HTTP uç noktalarına yönelik REST çağrılarını destekler. 