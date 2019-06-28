---
title: C#Gezinti - Azure Search yardımcı olacak modeller kullanmaya ilişkin öğretici
description: Bu öğreticide model Gezinti eklemek için "Sayfalandırma - Azure Search arama sonuçları" projesi üzerinde oluşturur. Modelleri kolaylıkla bir aramayı daraltmak için kullanılabileceğini öğrenin.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 06/20/2019
ms.openlocfilehash: 62326ad3bc5f2d740ce744819df559bce8658eb7
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443801"
---
# <a name="c-tutorial-use-facets-to-aid-navigation---azure-search"></a>C#öğretici: Gezinti - Azure Search yardımcı olmak için modelleri kullanma

Modelleri, gezinti, kendi arama odaklanmak için kullanılacak bağlantı kümesi ile kullanıcı sağlayarak yardımcı olmak için kullanılır. Modelleri (örneğin, kategori veya belirli bir özelliğini, bir örnek verilerimizi otelden) veri öznitelikleridir.

Bu öğreticide oluşturduğunuz sayfalandırma proje üzerine yapılar [ C# Öğreticisi: Azure Search arama sonuçlarını sayfalandırma -](tutorial-csharp-paging.md) öğretici.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Model özellikleri olarak _IsFacetable_
> * Model Gezinti uygulamanıza ekleme

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

Sahip [ C# Öğreticisi: Azure Search arama sonuçlarını sayfalandırma -](tutorial-csharp-paging.md) proje çalışır. Bu proje kendi sürümü veya Github'dan yükleyin: [İlk uygulamanızı oluşturma](https://github.com/Azure-Samples/azure-search-dotnet-samples).

## <a name="set-model-properties-as-isfacetable"></a>IsFacetable olarak model özelliklerini ayarlama

Bir modeli arama yer aldığı bir model özelliğine için sırada bu ile etiketlenmelidir **IsFacetable**.

1. İnceleme **otel** sınıfı. **Kategori** ve **etiketleri**, örneğin, olarak etiketlenir **IsFacetable**, ancak **HotelName** ve **açıklama** değil. 

    ```cs
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
    ```

2. Biz herhangi bir etiket hotel.cs dosya değiştirilmeden Bu öğretici, bu nedenle Kapat bir parçası olarak değiştirmiş olursunuz değil.

    > [!Note]
    > Bir modeli arama hata bağlanamazsa aramaya istenen bir alan uygun şekilde etiketli değil.


## <a name="add-facet-navigation-to-your-app"></a>Model Gezinti uygulamanıza ekleme

Bu örnekte, sonuçları solunda gösterilen bağlantılar listesinden bir kategori otel ya da bir amenity seçmesini sağlamak için kullanacağız. Arama metni, ardından arama sonuçlarının bir kategoriyi daraltabilirsiniz ve sonuçları başka bir amenity seçerek daraltabilirsiniz bazı girerek kullanıcı başlatır veya amenity seçmeleri ilk (sırası önemlidir değil).

Modellerin listesi görünüme iletmek için denetleyicinin ihtiyacımız var. Arama ilerledikçe kullanıcı seçimlerini sağlamak ihtiyacımız ve yeniden, geçici depolama verileri koruma mekanizması olarak kullanıyoruz.

!["Havuzunun" bir aramayı daraltmak için modeli gezintisini kullanma](./media/tutorial-csharp-create-first-app/azure-search-facet-nav.png)

### <a name="add-filter-strings-to-the-searchdata-model"></a>Filtre dizeleri SearchData modele eklemek

1. SearchData.cs dosyasını açın ve dize özellikleri **SearchData** modeli filtre dizelerinde tutacak sınıf.

    ```cs
        public string categoryFilter { get; set; }
        public string amenityFilter { get; set; }
    ```

### <a name="add-the-facet-action-method"></a>Eylem yöntemi modeli ekleme

Giriş denetleyicisine yeni bir eylem gerekli **modeli**ve mevcut güncelleştirmeleri **dizin** ve **sayfa** güncelleştirmelerinin yanı sıra, Eylemler **RunQueryAsync**  yöntemi.

1. Giriş denetleyicisine dosyasını açın ve eklemek **kullanarak** etkinleştirmek için bildirimi, **listesi&lt;dize&gt;**  oluşturun.

    ```cs
    using System.Collections.Generic;
    ```

2. Değiştirin **dizini (SearchData model)** eylem yöntemi.

    ```cs
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                // Ensure the search string is valid.
                if (model.searchText == null)
                {
                    model.searchText = "";
                }

                // Make the search call for the first page.
                await RunQueryAsync(model, 0, 0, "", "");
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

3. Değiştirin **sayfa (SearchData model)** eylem yöntemi.

    ```cs
        public async Task<ActionResult> Page(SearchData model)
        {
            try
            {
                int page;

                // Calculate the page that should be displayed.
                switch (model.paging)
                {
                    case "prev":
                        page = (int)TempData["page"] - 1;
                        break;

                    case "next":
                        page = (int)TempData["page"] + 1;
                        break;

                    default:
                        page = int.Parse(model.paging);
                        break;
                }

                // Recover the leftMostPage.
                int leftMostPage = (int)TempData["leftMostPage"];

                // Recover the filters.
                string catFilter = TempData["categoryFilter"].ToString();
                string ameFilter = TempData["amenityFilter"].ToString();

                // Recover the search text.
                model.searchText = TempData["searchfor"].ToString();

                // Search for the new page.
                await RunQueryAsync(model, page, leftMostPage, catFilter, ameFilter);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "2" });
            }
            return View("Index", model);
        }
    ```

4. Ekleme bir **Model (model SearchData)** eylem yöntemi, kullanıcının bir modeli bağlantısına tıkladığında etkinleştirilmesi gerekir. Model bir kategori arama filtresi veya bir amenity arama filtresi içerir. Belki de sonra Ekle **sayfa** eylem.

    ```cs
        public async Task<ActionResult> Facet(SearchData model)
        {
            try
            {
                // Filters set by the model override those stored in temporary data.
                string catFilter;
                string ameFilter;
                if (model.categoryFilter != null)
                {
                    catFilter = model.categoryFilter;
                } else
                {
                    catFilter = TempData["categoryFilter"].ToString();
                }

                if (model.amenityFilter != null)
                {
                    ameFilter = model.amenityFilter;
                } else
                {
                    ameFilter = TempData["amenityFilter"].ToString();
                }

                // Recover the search text.
                model.searchText = TempData["searchfor"].ToString();

                // Initiate a new search.
                await RunQueryAsync(model, 0, 0, catFilter, ameFilter);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "2" });
            }
            return View("Index", model);
        }
    ```

### <a name="set-up-the-search-filter"></a>Arama filtre ayarlamak

Bir kullanıcı belirli bir model seçtiğinde, örneğin, bunlar tıklayın **çare ve Spa** kategorisi ve ardından bu kategoriyi sonuçlarda döndürülmelidir gibi belirtilen yalnızca hotels. Bu şekilde bir aramayı daraltmak için ayarlamak ihtiyacımız bir _filtre_.

1. Değiştirin **RunQueryAsync** yöntemini aşağıdaki kod ile. Öncelikle, bir kategori filtre dizesi ve bir amenity filtre dizesi alır ve ayarlar **filtre** parametresinin **kullanılması**.

    ```cs
        private async Task<ActionResult> RunQueryAsync(SearchData model, int page, int leftMostPage, string catFilter, string ameFilter)
        {
            InitSearch();

            string facetFilter = "";

            if (catFilter.Length > 0 && ameFilter.Length > 0)
            {
                // Both facets apply.
                facetFilter = $"{catFilter} and {ameFilter}"; 
            } else
            {
                // One, or zero, facets apply.
                facetFilter = $"{catFilter}{ameFilter}";
            }

            var parameters = new SearchParameters
            {
                Filter = facetFilter,

                // Return information on the text, and number, of facets in the data.
                Facets = new List<string> { "Category,count:20", "Tags,count:20" },

                // Enter Hotel property names into this list, so only these values will be returned.
                Select = new[] { "HotelName", "Description", "Category", "Tags" },

                SearchMode = SearchMode.All,

                // Skip past results that have already been returned.
                Skip = page * GlobalVariables.ResultsPerPage,

                // Take only the next page worth of results.
                Top = GlobalVariables.ResultsPerPage,

                // Include the total number of results.
                IncludeTotalResultCount = true,
            };

            // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
            model.resultList = await _indexClient.Documents.SearchAsync<Hotel>(model.searchText, parameters);

            // This variable communicates the total number of pages to the view.
            model.pageCount = ((int)model.resultList.Count + GlobalVariables.ResultsPerPage - 1) / GlobalVariables.ResultsPerPage;

            // This variable communicates the page number being displayed to the view.
            model.currentPage = page;

            // Calculate the range of page numbers to display.
            if (page == 0)
            {
                leftMostPage = 0;
            }
            else
               if (page <= leftMostPage)
            {
                // Trigger a switch to a lower page range.
                leftMostPage = Math.Max(page - GlobalVariables.PageRangeDelta, 0);
            }
            else
            if (page >= leftMostPage + GlobalVariables.MaxPageRange - 1)
            {
                // Trigger a switch to a higher page range.
                leftMostPage = Math.Min(page - GlobalVariables.PageRangeDelta, model.pageCount - GlobalVariables.MaxPageRange);
            }
            model.leftMostPage = leftMostPage;

            // Calculate the number of page numbers to display.
            model.pageRange = Math.Min(model.pageCount - leftMostPage, GlobalVariables.MaxPageRange);

            // Ensure Temp data is stored for the next call.
            TempData["page"] = page;
            TempData["leftMostPage"] = model.leftMostPage;
            TempData["searchfor"] = model.searchText;
            TempData["categoryFilter"] = catFilter;
            TempData["amenityFilter"] = ameFilter;

            // Return the new view.
            return View("Index", model);
        }
    ```

    Ekledik **kategori** ve **etiketleri** özellikler listesine **seçin** öğeleri döndürmek için. Bu toplama çalışması gezinti modeli için bir gereksinim değildir, ancak biz doğru filtreleme doğrulamak için bu bilgileri kullanırız.

### <a name="add-lists-of-facet-links-to-the-view"></a>Görünüm modeli bağlantı listeleri ekleyin

Görünüm gerektiren bazı önemli değişiklikler yapacaktır. 

1. (Wwwroot/css klasöründe) hotels.css dosyasını açarak başlatın ve aşağıdaki sınıfları ekleyin.

    ```html
    .facetlist {
        list-style: none;
    }

    .facetchecks {
        width: 250px;
        display: normal;
        color: #666;
        margin: 10px;
        padding: 5px;
    }

    .facetheader {
        font-size: 10pt;
        font-weight: bold;
        color: darkgreen;    
    }
    ```

2. Görünüm için şu çıktıyı bir tabloya düzenlemek, sol ve sağ taraftaki sonuçları düzgünce modeli hizalamak için listelenir. Index.cshtml dosyasını açın. HTML öğesinin tüm içeriğini değiştirin &lt;gövdesi&gt; aşağıdaki kod ile etiketler.

    ```cs
    <body>

    @using (Html.BeginForm("Index", "Home", FormMethod.Post))
    {
        <table>
            <tr>
                <td></td>
                <td>
                    <h1 class="sampleTitle">
                        <img src="~/images/azure-logo.png" width="80" />
                        Hotels Search - Facet Navigation
                    </h1>
                </td>
            </tr>

            <tr>
                <td></td>
                <td>
                    <!-- Display the search text box, with the search icon to the right of it.-->
                    <div class="searchBoxForm">
                        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input value="" class="searchBoxSubmit" type="submit">
                    </div>
                </td>
            </tr>

            <tr>
                <td valign="top">
                    <div id="facetplace" class="facetchecks">

                        @if (Model != null && Model.resultList != null)
                        {
                            List<string> categories = Model.resultList.Facets["Category"].Select(x => x.Value.ToString()).ToList();

                            if (categories.Count > 0)
                            {
                                <h5 class="facetheader">Category:</h5>
                                <ul class="facetlist">
                                    @for (var c = 0; c < categories.Count; c++)
                                    {
                                        var facetLink = $"{categories[c]} ({Model.resultList.Facets["Category"][c].Count})";
                                        <li>
                                            @Html.ActionLink(facetLink, "Facet", "Home", new { categoryFilter = $"Category eq '{categories[c]}'" }, null)
                                        </li>
                                    }
                                </ul>
                            }

                            List<string> tags = Model.resultList.Facets["Tags"].Select(x => x.Value.ToString()).ToList();

                            if (tags.Count > 0)
                            {
                                <h5 class="facetheader">Amenities:</h5>
                                <ul class="facetlist">
                                    @for (var c = 0; c < tags.Count; c++)
                                    {
                                        var facetLink = $"{tags[c]} ({Model.resultList.Facets["Tags"][c].Count})";
                                        <li>
                                            @Html.ActionLink(facetLink, "Facet", "Home", new { amenityFilter = $"Tags/any(t: t eq '{tags[c]}')" }, null)
                                        </li>
                                    }
                                </ul>
                            }
                        }
                    </div>
                </td>
                <td valign="top">
                    <div id="resultsplace">
                        @if (Model != null && Model.resultList != null)
                        {
                            // Show the result count.
                            <p class="sampleText">
                                @Html.DisplayFor(m => m.resultList.Count) Results
                            </p>

                            @for (var i = 0; i < Model.resultList.Results.Count; i++)
                            {
                                string amenities = string.Join(", ", Model.resultList.Results[i].Document.Tags);

                                string fullDescription = Model.resultList.Results[i].Document.Description;
                                fullDescription += $"\nCategory: {Model.resultList.Results[i].Document.Category}";
                                fullDescription += $"\nAmenities: {amenities}";

                                // Display the hotel name and description.
                                @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                                @Html.TextArea($"desc{i}", fullDescription, new { @class = "box2" })
                            }
                        }
                    </div>
                </td>
            </tr>

            <tr>
                <td></td>
                <td valign="top">
                    @if (Model != null && Model.pageCount > 1)
                    {
                        // If there is more than one page of results, show the paging buttons.
                        <table>
                            <tr>
                                <td class="tdPage">
                                    @if (Model.currentPage > 0)
                                    {
                                        <p class="pageButton">
                                            @Html.ActionLink("|<", "Page", "Home", new { paging = "0" }, null)
                                        </p>
                                    }
                                    else
                                    {
                                        <p class="pageButtonDisabled">|&lt;</p>
                                    }
                                </td>

                                <td class="tdPage">
                                    @if (Model.currentPage > 0)
                                    {
                                        <p class="pageButton">
                                            @Html.ActionLink("<", "Page", "Home", new { paging = "prev" }, null)
                                        </p>
                                    }
                                    else
                                    {
                                        <p class="pageButtonDisabled">&lt;</p>
                                    }
                                </td>

                                @for (var pn = Model.leftMostPage; pn < Model.leftMostPage + Model.pageRange; pn++)
                                {
                                    <td class="tdPage">
                                        @if (Model.currentPage == pn)
                                        {
                                            // Convert displayed page numbers to 1-based and not 0-based.
                                            <p class="pageSelected">@(pn + 1)</p>
                                        }
                                        else
                                        {
                                            <p class="pageButton">
                                                @Html.ActionLink((pn + 1).ToString(), "Page", "Home", new { paging = @pn }, null)
                                            </p>
                                        }
                                    </td>
                                }

                                <td class="tdPage">
                                    @if (Model.currentPage < Model.pageCount - 1)
                                    {
                                        <p class="pageButton">
                                            @Html.ActionLink(">", "Page", "Home", new { paging = "next" }, null)
                                        </p>
                                    }
                                    else
                                    {
                                        <p class="pageButtonDisabled">&gt;</p>
                                    }
                                </td>

                                <td class="tdPage">
                                    @if (Model.currentPage < Model.pageCount - 1)
                                    {
                                        <p class="pageButton">
                                            @Html.ActionLink(">|", "Page", "Home", new { paging = Model.pageCount - 1 }, null)
                                        </p>
                                    }
                                    else
                                    {
                                        <p class="pageButtonDisabled">&gt;|</p>
                                    }
                                </td>
                            </tr>
                        </table>
                    }
                </td>
            </tr>
        </table>
    }
    </body>
    ```

    Kullanımına dikkat edin **Html.ActionLink** çağırın. Kullanıcı bir model bağlantısına tıkladığında bu çağrı geçerli filtre dizelerinde denetleyicisine iletişim kurar. 

### <a name="run-and-test-the-app"></a>Çalıştırın ve uygulamayı test etme

Kullanıcı gezinti modeli avantajlarından aşağıdaki sırayla göstereceğiz tek bir tıklatmayla aramaları daraltabilirsiniz ' dir.

1. Uygulama, türü "havaalanı" arama metni olarak çalıştırın. Modellerin listesi düzgünce sola göründüğünü doğrulayın. Bu modellerin "ne sıklıkta ortaya havaalanı" metin verilerine içinde sayısına sahip olan, Oteller için geçerli değildir.

    ![Bir arama "havaalanı" daraltmak için modeli gezintisini kullanma](./media/tutorial-csharp-create-first-app/azure-search-facet-airport.png)

2. Tıklayın **çare ve Spa** kategorisi. Bu kategorideki tüm sonuçları olduğundan emin olun.

    !["Çare ve Spa" için arama daraltma](./media/tutorial-csharp-create-first-app/azure-search-facet-airport-ras.png)

3. Tıklayın **Kıta kahvaltı** amenity. Tüm sonuçları "Çare ve Spa" kategorisiyle seçili amenity aşamasında olduğundan emin olun.

    !["Kıta kahvaltı" için arama daraltma](./media/tutorial-csharp-create-first-app/azure-search-facet-airport-ras-cb.png)

4. Tüm diğer kategorilerde birden amenity seçmeyi deneyin ve daraltma sonuçları görüntüleyin. Tersine, bir amenity sonra bir kategori deneyin.

    >[!Note]
    > (Örneğin kategori) modeli listesindeki bir seçim yapıldığında kategori listesi içinde herhangi bir önceki seçimini geçersiz kılar.

## <a name="takeaways"></a>Paketler

Aşağıdaki paketler bu projedeki göz önünde bulundurun:

* Her bir özellik olarak işaretlemek için zorunludur **IsFacetable**, model Gezinti dahil edilecek olmaları durumunda.
* Model Gezinti bir kullanıcı bir arama daraltma, kolay ve sezgisel bir yol sağlar.
* Model gezinti, en uygun bir başlık ile her bölüm (otel kategorisi), bir otel, fiyat aralıkları, derecelendirme aralıkları, vs. kullanılmıyordu bölümlere ayrılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki öğreticide sonuçları sıralama sırasında bakacağız. Bu noktaya kadar sırayla veritabanında bulunan sonuçları sıralanır.

> [!div class="nextstepaction"]
> [C#öğretici: Sipariş sonuçları - Azure Search](tutorial-csharp-orders.md)
