---
title: C#Azure Search - ilk uygulamanızı oluşturmak için öğretici
description: Bu öğretici, Azure arama için ilk uygulamanızı oluşturma adım adım yönergeler sağlar. Öğretici, sıfırdan uygulama oluşturma için GitHub ve tam işlem hem de çalışan bir uygulamayı bağlantı sağlar. Azure Search'ün temel bileşenleri hakkında bilgi edinin.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 05/01/2019
ms.openlocfilehash: 5ca01e8077eb0651dff57be4c7681995764f6992
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67166900"
---
# <a name="c-tutorial-create-your-first-app---azure-search"></a>C#Öğretici: Azure Search - ilk uygulamanızı oluşturma

Kullanarak Azure Search dizini sorgulama ve arama sonuçları mevcut bir web arabirimi oluşturmayı öğrenin. Bu öğreticide, arama sayfası oluşturmaya odaklanabilmeniz için var olan ve barındırılan dizin ile başlar. Dizini, kurgusal bir Konaklama veri içerir. Temel sayfa aldıktan sonra disk belleği, özellikleri ve yazarken tamamlama deneyimi içerecek şekilde sonraki derslerde geliştirebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir geliştirme ortamı Kurulumu
> * Model veri yapıları
> * Bir web sayfası oluşturma
> * Definovat metody
> * Uygulamayı test etme

Ayrıca, nasıl basit bir arama çağrıdır öğreneceksiniz. Aşağıdaki kodla anahtar ifadeler Kapsüllenen birkaç satırı.

```cs
var parameters = new SearchParameters
{
    // Enter Hotel property names into this list, so only these values will be returned.
    Select = new[] { "HotelName", "Description" }
};

DocumentSearchResult<Hotel> results  = await _indexClient.Documents.SearchAsync<Hotel>("search text", parameters);
```

Bu çağrı, Azure veri bir arama başlatır ve sonuçları döndürür.

!["Havuzu" için arama](./media/tutorial-csharp-create-first-app/azure-search-pool.png)


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

[Visual Studio yükleme](https://visualstudio.microsoft.com/) IDE kullanılacak.

### <a name="install-and-run-the-project-from-github"></a>Yükleme ve projeyi Github'dan çalıştırın

1. GitHub üzerinde örnek bulun: [İlk uygulamanızı oluşturma](https://github.com/Azure-Samples/azure-search-dotnet-samples).
1. Seçin **Kopyala veya indir** ve proje, özel yerel kopyasını oluşturun.
1. Visual Studio kullanarak, gidin ve çözüm için temel arama sayfasını açın ve seçin **ayıklamadan Başlat** (veya F5 tuşuna basın).
1. Bazı sözcükler (örneğin "wifi", "Görünüm", "çubuğu", "park") yazın ve sonuçları inceleyin!

    !["Wi-Fi" için arama](./media/tutorial-csharp-create-first-app/azure-search-wifi.png)

Umarım bu proje sorunsuz çalışmasını ve sahip olduğunuz Azure uygulaması çalışıyor. İyi bir fikirdir üzerinden gidin ve adım adım yeniden, bu nedenle çok daha karmaşık aramaları için gerekli bileşenler bu bir uygulamada dahil edilir.

Bu proje sıfırdan oluşturun ve bu nedenle Azure Search bileşenlerde fikrinizi güçlendirmek yardımcı olması için aşağıdaki adımları izleyerek gidin.

## <a name="set-up-a-development-environment"></a>Bir geliştirme ortamı ayarlama

1. Visual Studio 2017 veya sonraki sürümlerde, seçin **yeni/proje** ardından **ASP.NET Core Web uygulaması**. Proje, "FirstAzureSearchApp" gibi bir ad verin.
    ![Bir bulut projesi oluşturma](./media/tutorial-csharp-create-first-app/azure-search-project1.png)

2. Tıklattıktan sonra **Tamam** bu proje türü için bu proje için geçerli seçenekleri ikinci bir set sunulur. Seçin **Web uygulaması (Model-View-Controller)** .

    ![Bir MVC projesi oluşturma](./media/tutorial-csharp-create-first-app/azure-search-project2.png)

3. Ardından **Araçları** menüsünde **NuGet Paket Yöneticisi** ardından **çözüm için NuGet paketlerini Yönet...** . Bir paket yüklemek için ihtiyacımız yoktur. Seçin **Gözat** sekmesinde ve arama kutusuna "Azure Search" yazın. Yükleme **Microsoft.Azure.Search** listesinde göründüğünde (9.0.1, sürümü veya üzeri). Yüklemenin tamamlanması için birkaç ek iletişim kutularının tıklamanız gerekir.

    ![NuGet kullanarak Azure kitaplıkları eklemek](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

### <a name="initialize-azure-search"></a>Azure Search'ü başlatın

Bu örnek, genel kullanıma açık otel veri kullanıyoruz. Bu veri 50 kurgusal bir Konaklama adları ve açıklamaları, yalnızca tanıtım verileri sağlama amacıyla oluşturulan rastgele bir koleksiyonudur. Bu verilere erişmek için bir ad belirtin ve bunun için anahtar gerekir.

1. Yeni projenize appsettings.json dosyasını açın ve varsayılan satırları aşağıdaki adı ve anahtarı ile değiştirin. Burada örnek bir anahtar olmadığı API anahtarı olduğu _tam olarak_ otel verilere erişmek için gereksinim duyduğunuz anahtarı. Appsettings.json dosyanız şu şekilde görünmelidir.

```cs
{
  "SearchServiceName": "azs-playground",
  "SearchServiceQueryApiKey": "EA4510A6219E14888741FCFC19BFBB82"
}
```

2. Biz bu dosyayla yapılmadı henüz bu dosya için Özellikler'i seçin ve değiştirme **çıkış dizinine Kopyala** ayarını **yeniyse Kopyala**.

    ![Uygulama ayarları çıkışı kopyalama](./media/tutorial-csharp-create-first-app/azure-search-copy-if-newer.png)

## <a name="model-data-structures"></a>Model veri yapıları

Modelleri (C# sınıflar) (Görünüm) istemci, sunucunun (denetleyicisi) ve ayrıca MVC (model, görünüm, denetleyici) mimarisi kullanarak Azure bulut arasında veri iletişim kurmak için kullanılır. Genellikle, bu modeller erişiliyor verilerin yapısını ücreti yansıtılır. Ayrıca, işlemek ve görünüme/denetleyiciye iletişimler için bir model oluşturmamız gerekir.

1. Açık yukarı **modelleri** klasör ve Çözüm Gezgini kullanarak projenizin bir varsayılan model orada görürsünüz: **ErrorViewModel.cs**.

2. Sağ **modelleri** klasörü ve select **Ekle** ardından **yeni öğe**. Görüntülenen iletişim kutusunda, ardından **ASP.NET Core** sonra ilk seçenek **sınıfı**. Hotel.cs için .cs dosyayı yeniden adlandırın ve tıklayın **Ekle**. Hotel.cs tüm içeriğini aşağıdaki kodla değiştirin. Bildirim **adresi** ve **odası** sınıf üyeleri, bu alanlar sınıflardır kendilerini modelleri için çok ihtiyacımız şekilde.

```cs
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

namespace FirstAzureSearchApp.Models
{
    public partial class Hotel
    {
        [System.ComponentModel.DataAnnotations.Key]
        [IsFilterable]
        public string HotelId { get; set; }

        [IsSearchable, IsSortable]
        public string HotelName { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.EnLucene)]
        public string Description { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.FrLucene)]
        [JsonProperty("Description_fr")]
        public string DescriptionFr { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string Category { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string[] Tags { get; set; }

        [IsFilterable, IsSortable, IsFacetable]
        public bool? ParkingIncluded { get; set; }

        [IsFilterable, IsSortable, IsFacetable]
        public DateTimeOffset? LastRenovationDate { get; set; }

        [IsFilterable, IsSortable, IsFacetable]
        public double? Rating { get; set; }

        public Address Address { get; set; }

        [IsFilterable, IsSortable]
        public GeographyPoint Location { get; set; }

        public Room[] Rooms { get; set; }
    }
}
```

3. Modeli oluşturma aynı süreci izleyin **adresi** Address.cs dosya adı dışında sınıf. İçeriğini aşağıdakiyle değiştirin.

```cs
using Microsoft.Azure.Search;

namespace FirstAzureSearchApp.Models
{
    public partial class Address
    {
        [IsSearchable]
        public string StreetAddress { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string City { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string StateProvince { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string PostalCode { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string Country { get; set; }
    }
}
```

4. Yeniden oluşturmak için aynı süreci izleyin **odası** Room.cs dosya adlandırma sınıfı. Yeniden içeriğini aşağıdakiyle değiştirin.

```cs
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Newtonsoft.Json;

namespace FirstAzureSearchApp.Models
{
    public partial class Room
    {
        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.EnMicrosoft)]

        public string Description { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.FrMicrosoft)]
        [JsonProperty("Description_fr")]
        public string DescriptionFr { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string Type { get; set; }

        [IsFilterable, IsFacetable]
        public double? BaseRate { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string BedOptions { get; set; }

        [IsFilterable, IsFacetable]

        public int SleepsCount { get; set; }

        [IsFilterable, IsFacetable]
        public bool? SmokingAllowed { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string[] Tags { get; set; }
    }
}
```

5. Dizi **otel**, **adresi**, ve **odası** sınıflardır Azure bilinen [ _karmaşık türler_ ](search-howto-complex-data-types.md), Azure Search'ün önemli bir özelliği. Karmaşık türler sınıflar ve alt sınıfların derin düzeyleri olması ve kullanmaktan gösterilemeyecek kadar çok daha karmaşık veri yapılarını etkinleştir _basit türler_ (yalnızca ilkel üyeleri içeren bir sınıf). Bir daha fazla model yapmamız gerekir, böylece yeni bir model sınıfı yeniden oluşturma sürecinden geçer, ancak bu süre çağrı SearchData.cs sınıfı ve varsayılan kodu aşağıdakiyle değiştirin.

```cs
using Microsoft.Azure.Search.Models;

namespace FirstAzureSearchApp.Models
{
    public class SearchData
    {
        // The text to search for.
        public string searchText { get; set; }

        // The list of results.
        public DocumentSearchResult<Hotel> resultList;
    }
}
```

Bu sınıf, kullanıcının giriş içerir (**Aramametni**), ve arama çıktı (**resultList**). Çıkış türü önemlidir **DocumentSearchResult&lt;otel&gt;** gibi bu tür Arama sonuçlarından tam olarak eşleşir ve görünüm üzerinden bu başvuruyu geçirilecek gerekiyor.



## <a name="create-a-web-page"></a>Bir web sayfası oluşturma

Oluşturduğunuz proje varsayılan olarak, istemci sayısı görünümler oluşturur. Tam görünümleri çekirdek kullanmakta olduğunuz .NET sürümüne bağlıdır (2.1 Bu örnekte kullanıyoruz). Tüm içinde bulundukları **görünümleri** proje klasörü. Yalnızca Index.cshtml dosyasını değiştirmeniz gerekir (içinde **görünümler/giriş** klasörü).

Index.cshtml içeriği tamamen silin ve aşağıdaki adımlarda dosyasını yeniden oluşturun.

1. Görünümü'nde iki küçük resimler kullanırız. Kendi kullanın veya GitHub projesinden görüntüler arasında kopyalayın: azure-logo.png ve search.png. Bu iki görüntü yerleştirilmelidir **wwwroot/görüntülerinden** klasör.

2. Index.cshtml ilk satırının şu kullanacağınız veri olan (denetleyicisi) sunucu ile istemci (Görünüm) arasında iletişim kurmak için model başvurmalıdır **SearchData** modeli oluşturduk. Bu satırı Index.cshtml dosyaya ekleyin.

```cs
@model FirstAzureSearchApp.Models.SearchData
```

3. Sonraki satırları bu nedenle, görünüm için bir başlık girin standart bir uygulamadır:

```cs
@{
    ViewData["Title"] = "Home Page";
}
```

4. Başlık kısa bir süre içinde oluşturacağız ve HTML stil, başvuru girin.

```cs
<head>
    <link rel="stylesheet" href="~/css/hotels.css" />
</head>
```

5. Artık et görünümün için. Anımsanması anahtar bir şey görünüm iki durum işlemek sahip olur. İlk olarak, bu görünen uygulama ilk kez başlatılmadan ve kullanıcı henüz herhangi bir arama metni girmedi işlemesi gerekir. İkincisi, arama metin kutusuna, kullanıcı tarafından tekrarlanan kullanmak için ek olarak, sonuçların görüntülenmesini işlemesi gerekir. Bu durumlardan işlemek için sağlanan görünüm modeli null olup olmadığını denetlemek ihtiyacımız var. Null bir model, iki durumda (uygulamanın ilk çalıştırma) ilk duyuyoruz gösterir. Index.cshtml dosyasına aşağıdakileri ekleyin ve yorumları okuyun.

```cs
<body>
    <h1 class="sampleTitle">
        <img src="~/images/azure-logo.png" width="80" />
        Hotels Search
    </h1>

    @using (Html.BeginForm("Index", "Home", FormMethod.Post))
    {
        // Display the search text box, with the search icon to the right of it.
        <div class="searchBoxForm">
            @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input class="searchBoxSubmit" type="submit" value="">
        </div>

        @if (Model != null)
        {
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Results.Count) Results
            </p>

            @for (var i = 0; i < Model.resultList.Results.Count; i++)
            {
                // Display the hotel name and description.
                @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                @Html.TextArea("desc", Model.resultList.Results[i].Document.Description, new { @class = "box2" })
            }
        }
    }
</body>
```

6. Son olarak, stil sayfası ekleyin. Visual Studio içinde **dosya** menüsünü seçin **yeni/dosya** ardından **stil sayfası** (ile **genel** vurgulanan). Varsayılan kodu aşağıdakiyle değiştirin. Bu dosyaya herhangi ayrıntılı kullanacağız değil, standart HTML stillerdir.

```cs
   textarea.box1 {
    width: 648px;
    height: 30px;
    border: none;
    background-color: azure;
    font-size: 14pt;
    color: blue;
    padding-left: 5px;
}

textarea.box2 {
    width: 648px;
    height: 100px;
    border: none;
    background-color: azure;
    font-size: 12pt;
    padding-left: 5px;
    margin-bottom: 24px;
}

.sampleTitle {
    font: 32px/normal 'Segoe UI Light',Arial,Helvetica,Sans-Serif;
    margin: 20px 0;
    font-size: 32px;
    text-align: left;
}

.sampleText {
    font: 16px/bold 'Segoe UI Light',Arial,Helvetica,Sans-Serif;
    margin: 20px 0;
    font-size: 14px;
    text-align: left;
    height: 30px;
}

.searchBoxForm {
    width: 648px;
    box-shadow: 0 0 0 1px rgba(0,0,0,.1), 0 2px 4px 0 rgba(0,0,0,.16);
    background-color: #fff;
    display: inline-block;
    border-collapse: collapse;
    border-spacing: 0;
    list-style: none;
    color: #666;
}

.searchBox {
    width: 568px;
    font-size: 16px;
    margin: 5px 0 1px 20px;
    padding: 0 10px 0 0;
    border: 0;
    max-height: 30px;
    outline: none;
    box-sizing: content-box;
    height: 35px;
    vertical-align: top;
}

.searchBoxSubmit {
    background-color: #fff;
    border-color: #fff;
    background-image: url(/images/search.png);
    background-repeat: no-repeat;
    height: 20px;
    width: 20px;
    text-indent: -99em;
    border-width: 0;
    border-style: solid;
    margin: 10px;
    outline: 0;
}
```

7. Stil sayfası dosyayı hotels.css varsayılan site.css dosyasında yanı sıra wwwroot/css klasörüne kaydedin.

Bu, bizim görünümü tamamlar. İyi bir gelişme yapıyoruz. Modelleri ve görünümleri tamamlandı, her şeyi birbirine bağlamak için yalnızca denetleyici bırakılır.

## <a name="define-methods"></a>Definovat metody

Bir denetleyici içeriğini değiştirmek ihtiyacımız (**giriş denetleyicisine**), varsayılan olarak oluşturulur.

1. HomeController.cs dosyasını açın ve Değiştir **kullanarak** deyimlerini aşağıdaki.

```cs
using System;
using System.Diagnostics;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using FirstAzureSearchApp.Models;
using Microsoft.Extensions.Configuration;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
```

### <a name="add-index-methods"></a>Dizin yöntemleri ekleyin

İki ihtiyacımız **dizin** yöntemler, bir parametre (durum) uygulamayı ilk kez açıldığında için alma ve bir modeli (kullanıcı, arama metni zaman geçtiğini için) bir parametre olarak alan bir. Bu yöntemlerin ilki olan varsayılan olarak oluşturulur. 

1. Varsayılan sonra aşağıdaki yöntemi ekleyin **İNDİS()** yöntemi.

```cs
        [HttpPost]
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                // Ensure the search string is valid.
                if (model.searchText == null)
                {
                    model.searchText = "";
                }

                // Make the Azure Search call.
                await RunQueryAsync(model);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
```

Bildirim **zaman uyumsuz** yöntem bildiriminden ve **await** çağrısı **RunQueryAsync**. Bu anahtar sözcükler bizim çağrılar zaman uyumsuz olma ilgileniriz ve böylece engelleyici iş parçacıkları sunucuda kaçının.

**Catch** bloğu bizim için varsayılan olarak oluşturulan hata modelini kullanır.

### <a name="note-the-error-handling-and-other-default-views-and-methods"></a>Hata işleme ve diğer varsayılan görünümler ve yöntemleri unutmayın

.NET Core kullanıyorsanız, varsayılan biraz farklı bir dizi sürümüne bağlı olarak, varsayılan olarak görünüm oluşturulur. .NET Core 2.1 için dizin, hakkında kişi, gizlilik ve hata varsayılan görünümleridir. .NET Core 2.2 için örneğin, varsayılan dizin, gizlilik ve hata görünümleridir. Her iki durumda da, bu varsayılan sayfalar uygulamayı çalıştırırken görüntüleyebilir ve denetleyicide nasıl işleneceğini inceleyin.

Biz hata görünümünün daha sonra Bu öğreticide test.

GitHub Bu örnekte biz kullanılmayan görünümleri ve ilişkili eylemlerinin sildiniz.

### <a name="add-the-runqueryasync-method"></a>RunQueryAsync yöntemi ekleme

Azure Search arama içinde kapsüllenir bizim **RunQueryAsync** yöntemi.

1. İlk olarak Azure hizmet ve bunları başlatmak için bir çağrı ayarlamak için bazı statik değişkenler ekleyin.

```cs
        private static SearchServiceClient _serviceClient;
        private static ISearchIndexClient _indexClient;
        private static IConfigurationBuilder _builder;
        private static IConfigurationRoot _configuration;

        private void InitSearch()
        {
            // Create a configuration using the appsettings file.
            _builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
            _configuration = _builder.Build();

            // Pull the values from the appsettings.json file.
            string searchServiceName = _configuration["SearchServiceName"];
            string queryApiKey = _configuration["SearchServiceQueryApiKey"];

            // Create a service and index client.
            _serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(queryApiKey));
            _indexClient = _serviceClient.Indexes.GetClient("hotels");
        }
```

2. Şimdi ekleyin **RunQueryAsync** yöntemi.

```cs
        private async Task<ActionResult> RunQueryAsync(SearchData model)
        {
            InitSearch();

            var parameters = new SearchParameters
            {
                // Enter Hotel property names into this list so only these values will be returned.
                // If Select is empty, all values will be returned, which can be inefficient.
                Select = new[] { "HotelName", "Description" }
            };

            // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
            model.resultList = await _indexClient.Documents.SearchAsync<Hotel>(model.searchText, parameters);

            // Display the results.
            return View("Index", model);
        }
```

Bu yöntemde, ilk Azure bizim yapılandırması başlatılan ve ardından bazı arama parametrelerini ayarla emin oluruz. Alanların adlarını **seçin** parametreyle eşleşen özellik adlarını tam olarak **otel** sınıfı. Dışlamayı mümkündür **seçin** parametresi bu durumda tüm özellikleri döndürülür. Hayır ancak ayarlarsanız **seçin** biz yalnızca verilerin bir alt kümesi ilgileniyorsanız verimsiz parametreleri. İlgileniriz özellikleri belirterek bu özellikler yalnızca döndürülür.

Aramak için zaman uyumsuz çağrı (**model.resultList = _indexClient.Documents.SearchAsync await&lt;otel&gt;(model.searchText, Parametreler);** ) ne Bu öğretici ve uygulama hakkında vardır. **DocumentSearchResult** ilgi çekici bir sınıftır ve burada bir kesme noktası ayarlayın ve hata ayıklayıcı içeriğini incelemek için (uygulama çalışırken) bir fikir olabilir **model.resultList**. Sezgisel, sorulan verilerle sağlama ve çok başka olduğunu bulmanız gerekir.

Artık süre sonuna doğru.

### <a name="test-the-app"></a>Uygulamayı test etme

Şimdi, şimdi uygulama çalıştırmaları doğru denetleyin.

1. Seçin **hata ayıklama/başlangıç hata ayıklama olmadan** veya F5 tuşuna basın. Şeyler düzgün şekilde kodlanmış ilk Index görünümünü alırsınız.

     ![Uygulamayı açma](./media/tutorial-csharp-create-first-app/azure-search-index.png)

2. "Plaj" (veya aklınıza gelen herhangi bir metin) gibi bir metin girin ve arama simgesine tıklayın. Bazı sonuçlar almanız gerekir.

     !["Plaj" için arama](./media/tutorial-csharp-create-first-app/azure-search-beach.png)

3. "Beş yıldızlı" girmeyi deneyin. Sonuç nasıl ulaşabileceğinizi unutmayın. Daha karmaşık bir arama "beş yıldızlı", "lüks" eşanlamlısı olarak kabul et ve ancak bu sonuçlar döndürebilir. Biz bunu ilk öğreticilerde kapsayan değil ancak Azure Search'te eş anlamlıları kullanımını kullanılabilir.
 
4. "Arama metni olarak" Sık erişimli girmeyi deneyin. Mevcut _değil_ döndürür "otel" sözcüğünü içeren girişleri bunları. Birkaç sonuç döndürdü ancak arama özelliğimizi yalnızca tam sözcükleri bulunuyor.

5. Başka bir deyişle deneyin: "havuz", "güneşli", "Görünüm" ve ne olursa olsun. Azure çalışan en basit haliyle, ancak yine de düzeyi ikna arama görürsünüz.

## <a name="test-edge-conditions-and-errors"></a>Test edge koşullar ve hataları

Hata işleme özelliklerimiz bile şey mükemmel çalışırken, olması gerektiği gibi çalıştığını doğrulamak önemlidir. 

1. İçinde **dizin** yöntemi, sonra **deneyin {** çağrı, satırı girin **yeni Exception() Throw**. Bu özel durum, şu metni ararken bir hata zorlar.

2. Uygulamayı çalıştırın, "olarak metin arama ve arama simgesine tıklayın çubuğu" girin. Özel durum hatası Görünümü'nde neden olur.

     ![Bir hata zorla](./media/tutorial-csharp-create-first-app/azure-search-error.png)

> [!Important]
> İç hata numaralarını hata sayfalarında döndürülecek bir güvenlik riski olarak kabul edilir. Uygulamanızı genel kullanıma yöneliktir, güvenli ve en iyi yöntemleri bazı araştırmasını ne bir hata oluştuğunda döndürülecek yapın.

3. Kaldırma **yeni Exception() Throw** memnun olduğunuzda olması gerektiği gibi çalıştığını işleme hatası.

## <a name="takeaways"></a>Paketler

Aşağıdaki paketler bu projedeki göz önünde bulundurun:

* Bir Azure Search arama kısa olabilir ve sonuçları yorumlamanıza kolaydır.
* Zaman uyumsuz çağrılar, az miktarda bir karmaşıklık denetleyiciye ekleyebilirsiniz, ancak kaliteli uygulamaları geliştirmek istiyorsanız, en iyi uygulamalardan biridir.
* Bu uygulamanın ne ayarlanır tarafından tanımlanan, bir basit metin araması gerçekleştirilen **kullanılması**. Ancak, bu bir sınıf, bir arama için Gelişmiş algoritmaların eklemek çok sayıda üye içeren doldurulabilir. Bu uygulamanın önemli ölçüde daha güçlü olmasını değil kadar ek çalışma gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure arama'yı kullanarak en iyi kullanıcı deneyimi sağlamak için daha fazla özellik eklemek özellikle sayfalama gerekiyor (ya da sayfa numarası veya sonsuz kayan) ve otomatik tamamlama/öneriler. Biz de daha karmaşık arama parametrelerini (örneğin, coğrafi aramaları belirli bir noktaya ve arama sonuçlarını sıralamasını belirtilen bir yarıçap içindeki Oteller) dikkate almalısınız.

Aşağıdaki adımlarda bir öğretici serisinde ele alınır. Disk belleği ile başlayalım.

> [!div class="nextstepaction"]
> [C#Öğretici: Arama sonuçlarını sayfalandırma - Azure Search](tutorial-csharp-paging.md)


