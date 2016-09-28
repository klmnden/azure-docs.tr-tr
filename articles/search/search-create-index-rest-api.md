<properties
    pageTitle="REST API'yi kullanarak Azure Search dizini oluşturma | Microsoft Azure | Barındırılan bulut arama hizmeti"
    description="Azure Search HTTP REST API'sini kullanarak kod içinde bir dizin oluşturun."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager=""
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>


# REST API'yi kullanarak Azure Search dizini oluşturma
> [AZURE.SELECTOR]
- [Genel Bakış](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)


Bu makale, Azure Search REST API'sini kullanarak Azure Search [dizini](https://msdn.microsoft.com/library/azure/dn798941.aspx) oluşturma işlemi konusunda size yol gösterecektir.

Bu kılavuzu izlemeden ve dizin oluşturmadan önce, [Azure Search hizmeti oluşturmuş](search-create-service-portal.md) olmanız gerekir.

REST API'yi kullanan bir Azure Search dizini oluşturmak için, Azure Search hizmetinizin URL uç noktasına tek bir HTTP POST isteği göndereceksiniz. Dizin tanımınız, doğru biçimlendirilmiş JSON içeriği olarak istek gövdesinde yer alır.


## I. Azure Search hizmet yöneticinizin api anahtarını tanımlama
Şimdi bir Azure Search hizmeti sağlamış olduğunuza göre, .REST API'yi kullanarak hizmetinizin URL uç noktasına HTTP istekleri gönderebilirsiniz. Ancak *tüm* API isteklerinin sağladığınız Search hizmeti için oluşturulan API anahtarını içermesi gerekir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

1. Hizmetinizin api anahtarlarını bulmak için [Azure Portal](https://portal.azure.com/)'da oturum açmanız gerekir
2. Azure Search hizmetinizin dikey penceresine gidin
3. "Anahtarlar" simgesine tıklayın

Hizmetiniz, *yönetici anahtarlarına* ve *sorgu anahtarlarına* sahiptir.

 - Birincil ve ikincil *yönetici anahtarlarınız*; hizmeti yönetme, dizinler, dizin oluşturucular ve veri kaynakları ekleme ve silme de dahil olmak üzere her türlü işlem için tüm hakları verir. Birincil anahtarı yeniden oluşturmaya karar verirseniz ikincil anahtarı kullanmaya devam edebilmeniz ve tam tersini yapabilmeniz için iki anahtar vardır.
 - *Sorgu anahtarları*, dizinler ve belgeler için salt okunur erişim verir ve genellikle, arama istekleri gönderen istemci uygulamalarına dağıtılır.

Dizin oluşturma amacıyla, birincil ya da ikincil yönetici anahtarınızı kullanabilirsiniz.

## II. Doğru biçimlendirilmiş JSON kullanarak Azure Search dizininizi tanımlama
Hizmetinize yönelik tek bir HTTP POST isteği dizininizi oluşturur. HTTP POST isteğinizin gövdesi, Azure Search dizininizi tanımlayan tek bir JSON nesnesi içerir.

1. Bu JSON nesnesinin ilk özelliği dizininizin adıdır.
2. Bu JSON nesnesinin ikinci özelliği, dizininizdeki her bir alan için ayrı bir JSON nesnesi içeren `fields` adlı bir JSON dizisidir. Bu JSON nesnelerinin her biri, her bir alan özniteliği için "ad", "tür" vb. de dahil olmak üzere birden çok ad/değer çifti içerir.

Her bir alan için [uygun öznitelikler](https://msdn.microsoft.com/library/azure/dn798941.aspx) atanması gerektiğinden, dizininizi tasarlarken arama kullanıcı deneyiminizi ve iş gereksinimlerinizi göz önünde bulundurmanız önemlidir. Bu öznitelikler, hangi alanlar için hangi arama özelliklerinin (filtreleme, modelleme, tam metin araması sıralama vb.) geçerli olduğunu denetler. Belirtmediğiniz her öznitelik için ilgili arama özelliği, özellikle devre dışı bırakmadığınız sürece varsayılan olarak etkinleştirilir.

Bizim örneğimizde, dizinimizi "oteller" olarak adlandırdık ve alanlarımızı aşağıdaki şekilde tanımladık:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Her bir alan için dizin özniteliklerini, bunların bir uygulamada nasıl kullanılacağını düşünerek dikkatle seçtik. Örneğin; `hotelId`, oteller için arama yapan kişilerin büyük olasılıkla bilmediği benzersiz bir anahtardır. Bu nedenle, `searchable` değerini `false` olarak ayarlayarak bu alan için tam metin aramasını devre dışı bırakırız. Bu, dizinde yer kazandırır.

Lütfen `Edm.String` türündeki dizininizde yalnızca bir alanın "anahtar" alanı olarak belirlenmesi gerektiğini unutmayın 

Yukarıdaki dizin tanımı Fransızca metin depolamaya yönelik tasarlandığından, `description_fr` alanı için özel bir dil çözümleyicisi kullanır. Dil çözümleyicileri hakkında daha fazla bilgi için ilgili [blog yazısının](https://msdn.microsoft.com/library/azure/dn879793.aspx) yanı sıra [MSDN'de Dil desteği konu başlığına](https://azure.microsoft.com/blog/language-support-in-azure-search/) bakın.

## III. HTTP isteği gönderme
1. Dizin tanımınızı istek gövdesi olarak kullanarak Azure Search hizmeti uç nokta URL'nize HTTP POST isteği gönderin. URL'de hizmet adınızı ana bilgisayar adı olarak kullandığınızdan emin olun ve sorgu dizesi parametresi olarak uygun `api-version` öğesini kullanın (Bu belge yayımlandığı sırada, `2015-02-28` geçerli API sürümüdür).
2. İstek üst bilgilerinde, `Content-Type` öğesini `application/json` olarak belirtin. Ayrıca `api-key` üst bilgisinde, 1. Adımda tanımladığınız hizmet yöneticisi anahtarınızı sağlamanız gerekir.


Aşağıdaki isteği göndermek için kendi hizmet adınızı ve API anahtarınızı sağlamanız gerekir:


    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [api-key]


Başarılı bir istek için 201 durum kodunu (Oluşturuldu) görmeniz gerekir. REST API aracılığıyla dizin oluşturma hakkında daha fazla bilgi için lütfen [MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx)'de API başvurusuna bakın. Hata durumunda döndürülebilen diğer HTTP durum kodları hakkında daha fazla bilgi için bkz. [HTTP durum kodları (Azure Search)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

Dizin ile işiniz bittiğinde ve bunu silmek istediğinizde HTTP DELETE isteği gönderin. Örneğin, "oteller" dizinini aşağıdaki şekilde sileriz:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2015-02-28
    api-key: [api-key]


## Sonraki
Azure Search dizini oluşturduktan sonra, [içeriğinizi dizine yüklemek](search-what-is-data-import.md) için hazır olursunuz. Böylece, verilerinizi aramaya başlayabilirsiniz.



<!--HONumber=Sep16_HO3-->


