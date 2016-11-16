---
title: ".NET SDK kullanarak Azure Search dizini oluşturma | Microsoft Belgeleri"
description: "Azure Search .NET SDK&quot;sını kullanarak kod içinde bir dizin oluşturun."
services: search
documentationcenter: 
author: brjohnstmsft
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 3a851647-fc7b-4fb6-8506-6aaa519e77cd
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: brjohnst
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 87757a16f1fa31be97f6f8a0e39c6adbf2513828


---
# <a name="create-an-azure-search-index-using-the-net-sdk"></a>.NET SDK'yı kullanarak Azure Search dizini oluşturma
> [!div class="op_single_selector"]
> * [Genel Bakış](search-what-is-an-index.md)
> * [Portal](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

Bu makale, [Azure Search .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)'sını kullanarak Azure Search [dizini](https://msdn.microsoft.com/library/azure/dn798941.aspx) oluşturma işlemi konusunda size yol gösterecektir.

Bu kılavuzu izlemeden ve dizin oluşturmadan önce, [Azure Search hizmeti oluşturmuş](search-create-service-portal.md) olmanız gerekir.

Bu makaledeki örnek kodun tamamının C# dilinde yazıldığını unutmayın. Tam kaynak kodunu [GitHub](http://aka.ms/search-dotnet-howto)'da bulabilirsiniz.

## <a name="i-identify-your-azure-search-services-admin-apikey"></a>I. Azure Search hizmet yöneticinizin api anahtarını tanımlama
Şimdi bir Azure Search hizmeti sağlamış olduğunuza göre, .NET SDK'yı kullanarak hizmet uç noktanıza istek göndermeye neredeyse hazırsınız. Öncelikle, sağladığınız arama hizmeti için oluşturulan yönetici api anahtarlarından birini edinmeniz gerekir. .NET SDK, hizmetinize yönelik her istek için bu api anahtarını gönderir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

1. Hizmetinizin api anahtarlarını bulmak için [Azure Portal](https://portal.azure.com/)'da oturum açmanız gerekir
2. Azure Search hizmetinizin dikey penceresine gidin
3. "Anahtarlar" simgesine tıklayın

Hizmetiniz, *yönetici anahtarlarına* ve *sorgu anahtarlarına* sahiptir.

* Birincil ve ikincil *yönetici anahtarlarınız*; hizmeti yönetme, dizinler, dizin oluşturucular ve veri kaynakları ekleme ve silme de dahil olmak üzere her türlü işlem için tüm hakları verir. Birincil anahtarı yeniden oluşturmaya karar verirseniz ikincil anahtarı kullanmaya devam edebilmeniz ve tam tersini yapabilmeniz için iki anahtar vardır.
* *Sorgu anahtarları*, dizinler ve belgeler için salt okunur erişim verir ve genellikle, arama istekleri gönderen istemci uygulamalarına dağıtılır.

Dizin oluşturma amacıyla, birincil ya da ikincil yönetici anahtarınızı kullanabilirsiniz.

<a name="CreateSearchServiceClient"></a>

## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. SearchServiceClient sınıfının örneğini oluşturma
Azure Search .NET SDK'sını kullanmaya başlamak için `SearchServiceClient` sınıfının bir örneğini oluşturmanız gerekir. Bu sınıfın birkaç oluşturucusu vardır. İstediğiniz oluşturucu, arama hizmeti adınızı ve `SearchCredentials` nesnesini parametre olarak alır. `SearchCredentials`, api anahtarınızı sarmalar.

Aşağıdaki kod, uygulamanın yapılandırma dosyasında (`app.config` veya `web.config`) depolanan arama hizmeti adı ve api anahtarı için değerler kullanarak yeni bir `SearchServiceClient` oluşturur:

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`, `Indexes` özelliğine sahiptir. Bu özellik Azure Search dizinlerini oluşturmanız, listelemeniz, güncelleştirmeniz veya silmeniz için gereken tüm yöntemleri sağlar.

> [!NOTE]
> `SearchServiceClient` sınıfı, arama hizmetinize yönelik bağlantıları yönetir. Çok fazla bağlantı açmayı önlemek için, mümkünse uygulamanızda tek bir `SearchServiceClient` örneği paylaşmaya çalışmanız gerekir. Yöntemlerinin iş parçacığı bu tür paylaşımları etkinleştirmek için güvenlidir.
> 
> 

<a name="DefineIndex"></a>

## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. `Index` sınıfını kullanarak Azure Search dizininizi tanımlama
`Indexes.Create` yöntemine yönelik tek bir çağrı dizininizi oluşturur. Bu yöntem, Azure Search dizininizi tanımlayan bir `Index` nesnesini parametre olarak alır. Bir `Index` nesnesi oluşturmanız ve bunu aşağıdaki gibi başlatmanız gerekir:

1. `Name` nesnesinin `Index` özelliğini dizin adınız olarak ayarlayın.
2. `Fields` nesnesinin `Index` özelliğini `Field` nesnelerinin dizisi olarak ayarlayın. `Field` nesnelerinin her biri, dizininizdeki bir alanın davranışını tanımlar. Oluşturucuya alan adını veri türü (veya dize alanları çözümleyicisi) ile birlikte sağlayabilirsiniz. `IsSearchable`, `IsFilterable` vb. gibi başka özellikler de ayarlayabilirsiniz.

Her bir `Field` için [uygun özellikler](https://msdn.microsoft.com/library/azure/dn798941.aspx) atanması gerektiğinden, dizininizi tasarlarken arama kullanıcı deneyiminizi ve iş gereksinimlerinizi göz önünde bulundurmanız önemlidir. Bu özellikler, hangi alanlar için hangi arama özelliklerinin (filtreleme, modelleme, tam metin araması sıralama vb.) geçerli olduğunu denetler. Açıkça ayarlamadığınız her özellik, ilgili arama özelliğini özellikle etkinleştirmediğiniz sürece `Field` sınıfı tarafından devre dışı varsayılır.

Bizim örneğimizde, dizinimizi "oteller" olarak adlandırdık ve alanlarımızı aşağıdaki şekilde tanımladık:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

Her bir `Field` için özellik değerlerini, bunların bir uygulamada nasıl kullanılacağını düşünerek dikkatle seçtik. Örneğin, oteller için arama yapan kişiler büyük olasılıkla `description` alanındaki anahtar sözcük eşleşmeleri ile ilgilenecektir. Bu nedenle, `IsSearchable` değerini `true` olarak ayarlayarak bu alan için tam metin aramasını etkinleştiririz.

Lütfen `IsKey` değerini `true` olarak ayarladığınızda, `DataType.String` türündeki dizininizde yalnızca bir alanın *anahtar alanı* olarak belirlenmesi gerektiğini unutmayın (yukarıdaki örnekte bkz. `hotelId` ).

Yukarıdaki dizin tanımı Fransızca metin depolamaya yönelik tasarlandığından, `description_fr` alanı için özel bir dil çözümleyicisi kullanır. Dil çözümleyicileri hakkında daha fazla bilgi için ilgili [blog yazısının](https://msdn.microsoft.com/library/azure/dn879793.aspx) yanı sıra [MSDN'de Dil desteği konu başlığına](https://azure.microsoft.com/blog/language-support-in-azure-search/) bakın.

> [!NOTE]
> Oluşturucuya `AnalyzerName.FrLucene` geçirdiğinizde, `Field` türünün otomatik olarak `DataType.String` olacağını ve `IsSearchable` değerinin `true` olarak ayarlanacağını unutmayın.
> 
> 

## <a name="iv-create-the-index"></a>IV. Dizini oluşturma
Şimdi, başlatılan bir `Index` nesneniz olduğuna göre `Indexes.Create` nesneniz üzerinden `SearchServiceClient` çağrısı yaparak dizininizi oluşturabilirsiniz:

```csharp
serviceClient.Indexes.Create(definition);
```

Başarılı bir istek için yöntem normal olarak döndürür. İstekle ilgili bir sorun varsa (geçersiz bir parametre gibi) yöntem `CloudException` atar.

Dizin ile işiniz bittiğinde ve bunu silmek istediğinizde nesneniz `SearchServiceClient` üzerinden `Indexes.Delete` yöntemini çağırın. Örneğin, "oteller" dizinini aşağıdaki şekilde sileriz:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [!NOTE]
> Bu makaledeki örnek kod, kolaylık amacıyla Azure Search .NET SDK'sının zaman uyumlu yöntemlerini kullanır. Kendi uygulamalarınızda zaman uyumsuz yöntemler kullanarak bunları ölçeklenebilir ve esnek tutmanızı öneririz. Örneğin, yukarıdaki örneklerde `Create` ve `Delete` yerine `CreateAsync` ve `DeleteAsync` kullanabilirsiniz.
> 
> 

## <a name="next"></a>Sonraki
Azure Search dizini oluşturduktan sonra, [içeriğinizi dizine yüklemek](search-what-is-data-import.md) için hazır olursunuz. Böylece, verilerinizi aramaya başlayabilirsiniz.




<!--HONumber=Nov16_HO2-->


