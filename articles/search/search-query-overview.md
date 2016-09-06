<properties
    pageTitle="Azure Search Dizininizi sorgulama | Microsoft Azure | Barındırılan bulut arama hizmeti"
    description="Azure Search'te bir arama sorgusu oluşturun ve arama sonuçlarını filtrelemek ve sıralamak için arama parametrelerini kullanın."
    services="search"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# Azure Search dizininizi sorgulama
> [AZURE.SELECTOR]
- [Genel Bakış](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Azure Search'e arama istekleri gönderirken, uygulamanızın arama kutusuna yazılan gerçek sözcüklerin yanı sıra belirtilebilecek birkaç parametre bulunur. Bu sorgu parametreleri, tam metin arama deneyiminde biraz daha derin denetim elde etmenizi sağlar.

Azure Search'te sorgu parametrelerinin ortak kullanımlarını kısaca açıklayan bir liste aşağıda bulunmaktadır. Sorgu parametrelerinin ve bunların davranışlarının tam kapsamı için lütfen [REST API](https://msdn.microsoft.com/library/azure/dn798927.aspx) ve [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx) için ayrıntılı bilgilerin yer aldığı sayfalara bakın.

## Sorgu türleri

Azure Search, son derece güçlü sorgular oluşturmak için birçok seçenek sunar. Kullanacağınız iki ana sorgu türü `search` ve `filter` sorgularıdır. `search` sorgusu, dizininizdeki tüm _aranabilir_ alanlarda bir veya daha çok terimi arar ve Google veya Bing gibi bir arama alt yapısının çalışmasını beklediğiniz şekilde çalışır. `filter` sorgusu, bir dizindeki tüm _filtrelenebilir_ alanlarda bir boole ifadesini değerlendirir. `search` sorgularının aksine, `filter` sorguları bir alanın tam içeriğini eşleştirir; bu da dize alanları için büyük/küçük harfe duyarlı olduklarını gösterir.

Aramaları ve filtreleri birlikte veya ayrı olarak kullanabilirsiniz. Bunları birlikte kullandığınızda filtre öncelikle tüm dizine uygulanır ve ardından filtrenin sonuçlarında arama gerçekleştirilir. Filtreler arama sorgusunun işlemesi gereken belge kümesini azalttığından, sorgu performansını iyileştirmeye yönelik kullanışlı bir teknik olabilir.

Filtre ifadeleri için söz dizimi, [OData filtre dilinin](https://msdn.microsoft.com/library/azure/dn798921.aspx) bir alt kümesidir. Arama sorguları için aşağıda açıklanan [basitleştirilmiş söz dizimini](https://msdn.microsoft.com/library/azure/dn798920.aspx) veya [Lucene sorgu söz dizimini](https://msdn.microsoft.com/library/azure/mt589323.aspx) kullanabilirsiniz.

### Basit sorgu söz dizimi
[Basit sorgu söz dizimi](https://msdn.microsoft.com/library/azure/dn798920.aspx), Azure Search'te kullanılan varsayılan sorgu dildir. Basit sorgu söz dizimi AND, OR, NOT, tümcecik, sonek ve öncelik işleçleri dahil olmak üzere birkaç ortak arama işleçlerini destekler.

### Lucene sorgu söz dizimi
[Lucene sorgu söz dizimi](https://msdn.microsoft.com/library/azure/mt589323.aspx), [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)'in parçası olarak geliştirilen yaygın şekilde benimsenmiş ve ifade gücüne sahip sorgu dili kullanmanızı sağlar.

Bu sorgu söz diziminin kullanılması, şu işlevleri kolayca elde etmenizi sağlar: [Alan kapsamlı sorgular](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields), [belirsiz arama](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy), [yakınlık araması](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), [terim artırma](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [normal ifade araması](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [joker karakterle arama](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [temel söz dizimi bilgileri](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax) ve [boole işleçlerini kullanan sorgular](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).



## Sonuçları sıralama
Bir arama sorgusunun sonuçları alınırken, Azure Search'ün sonuçları belirli bir alandaki değerlere göre sıralayarak sunmasını isteyebilirsiniz. Varsayılan olarak Azure Search, her bir belgenin [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)'den türetilen arama puanı sıralamasını temel alarak arama sonuçlarını sıralar.

Azure Search'ün sonuçlarınızı arama puanı dışında bir değere göre sıralayarak döndürmesini istiyorsanız `orderby` arama parametresini kullanabilirsiniz. `orderby` parametresinin değerini, alan adlarını ve jeo-uzamsal değerler için [`geo.distance()` işlevine](https://msdn.microsoft.com/library/azure/dn798921.aspx) çağrıları içerecek şekilde belirtebilirsiniz. Her bir ifadenin ardından, sonuçların artan sıralamada istendiğini belirtmek için `asc`, sonuçların azalan sıralamada istendiğini belirtmek için ise `desc` gelebilir. Artan sıralama varsayılandır.

## Sayfalama
Azure Search, arama sonuçlarının sayfalanması uygulamasını kolaylaştırır. `top` ve `skip` parametrelerini kullanarak, tüm arama sonuçları kümesini, iyi arama kullanıcı arabirimi uygulamalarını kolayca etkinleştiren yönetilebilir ve sıralı alt kümeler halinde almanızı sağlayan arama isteklerini sorunsuz bir şekilde gönderebilirsiniz. Bu daha küçük sonuç alt kümelerini alırken, tüm arama sonuçları kümesindeki belge sayısını da alabilirsiniz.

[Azure Search'te arama sonuçlarını numaralandırma](search-pagination-page-layout.md) makalesinde arama sonuçlarının numaralanması hakkında daha fazla bilgi alabilirsiniz.


## İsabet vurgulama
Azure Search'te arama sonuçlarının arama sorgusuyla tam olarak eşleşen kısmının vurgulanması `highlight`, `highlightPreTag` ve `highlightPostTag` parametreleri kullanılarak kolaylaştırılır. Hangi _aranabilir_ alanların eşleşen metninin vurgulanacağının yanı sıra Azure Search'ün döndürdüğü eşleşen metnin başına ve sonuna eklenecek dize etiketlerini tam olarak belirtebilirsiniz.



<!--HONumber=ago16_HO5-->


