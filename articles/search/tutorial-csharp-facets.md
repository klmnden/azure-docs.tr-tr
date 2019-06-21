---
title: C#Gezinti ve ağ verimliliği için - Azure Search özellikleri kullanmaya ilişkin öğretici
description: Bu öğreticide model aramaları eklemek için "Sayfalandırma - Azure Search arama sonuçları" proje üzerinde oluşturur. Hem gezinti ve otomatik tamamlama özellikleri kullanılabileceğini öğrenin.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 06/20/2019
ms.openlocfilehash: a81042869564533050fef42a983f2f8fb9bc7b23
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304683"
---
# <a name="c-tutorial-use-facets-for-navigation-and-network-efficiency---azure-search"></a>C#öğretici: Gezinti ve ağ verimliliği için - Azure Search modelleri kullanma

Özellikleri, Azure Search'te iki farklı kullanım vardır. Modelleri, gezinti, kullanıcı onay kutuları, kendi arama odaklanmak için kullanılacak bir kümesini sağlayarak yardımcı olmak için kullanılabilir. Ayrıca, bunlar otomatik tamamlama kullanıldığında ağ verimliliği artırmak için kullanılabilir. Yalnızca her sayfa yükleme için bir kez yerine her tuş vuruşu için bir kez dışarı taşınan olduğundan model aramaları verimlidir. 

Modelleri, öznitelikler (örneğin, bir otel bizim örnek veri kategorisini) verilerin ve arama ömrü için güncel.

Bu öğreticide, iki proje, bir modeli gezinme ve modeli otomatik tamamlama için diğer oluşturur. Her iki proje oluşturduğunuz sayfalandırma proje üzerine yapı [ C# Öğreticisi: Azure Search arama sonuçlarını sayfalandırma -](tutorial-csharp-paging.md) öğretici.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Model özellikleri olarak _IsFacetable_
> * Model Gezinti uygulamanıza ekleme
> * Model otomatik tamamlama uygulamanıza ekleme
> * Model otomatik tamamlama kullanma zamanı karar verin

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

Sahip [ C# Öğreticisi: Azure Search arama sonuçlarını sayfalandırma -](tutorial-csharp-paging.md) proje çalışır. Bu, kendi sürümü veya Github'dan yükleyin: [İlk uygulamanızı oluşturma](https://github.com/Azure-Samples/azure-search-dotnet-samples).

## <a name="set-model-fields-as-isfacetable"></a>Küme modeli alanlarını IsFacetable olarak

Model özelliğine bir modeli arama (Gezinti veya Otomatik Tamamlama) yer alması için sırada bu ile etiketlenmelidir **IsFacetable**.

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

2. Biz herhangi bir etiket Bu öğreticide değiştirmiş olursunuz değil. Bir modeli arama hata bağlanamazsa aramaya istenen bir alan uygun şekilde etiketli değil.


## <a name="add-facet-navigation-to-your-app"></a>Model Gezinti uygulamanıza ekleme

Bu örnekte, kullanıcı tarafından gösterilen sonuçları solunda bir listedeki otel, bir veya daha fazla kategorileri seçmek sağlamak için kullanacağız. Uygulamayı ilk kez çalıştırdığınızda, kategori listesi bilmeniz ve bu listeyi ilk ekran oluşturulduğunda görüntülenecek görünüme iletmek için denetleyicinin ihtiyacımız var. Her bir sayfa olarak size hem özellikleri listesini ve sonraki sayfalara boyunca geçirilecek geçerli kullanıcı seçimleri, tutulan olduğundan emin olun gerekir. Yine, geçici depolama verileri koruma mekanizması olarak kullanırız.

!["Havuzunun" bir aramayı daraltmak için modeli gezintisini kullanma](./media/tutorial-csharp-create-first-app/azure-search-facet-nav.png)

### <a name="modify-the-searchdata-model"></a>SearchData modeli Değiştir

1. SearchData.cs dosyasını açın ve bu ek ekleyin **kullanarak** deyimi. Etkinleştirmek ihtiyacımız **listesi&lt;dize&gt;**  oluşturun.

    ```cs
    using System.Collections.Generic;
    ```

2. Aynı dosyada, aşağıdaki satırları ekleyin **SearchData** sınıfı. Var olan sınıf özellikleri silmeyin, ancak aşağıdaki Oluşturucu yöntemleri ve özellikleri dizileri ekleyin.

    ```cs
        public SearchData()
        {
        }

        // Constructor to initialize the list of facets sent from the controller.
        public SearchData(List<string> facets)
        {
            facetText = new string[facets.Count];

            for (int i = 0; i < facets.Count; i++)
            {
                facetText[i] = facets[i];
            }
        }

        // Array to hold the text for each facet.
        public string[] facetText { get; set; }

        // Array to hold the check box setting.
        public bool[] facetOn { get; set; }
    ```


### <a name="search-for-facets-on-the-first-index-call"></a>Arama dizini ilk çağrıda modelleri için

Giriş denetleyicisine önemli bir değişiklik gerekir. İlk çağrıda **İNDİS()** artık başka bir işlem olan bir görünümü verir. Görünüm modelleri tam bir listesini sağlamak istiyoruz ve bu amaç için doğru olanı ilk çağrıdır.

1. Giriş denetleyicisine dosyasını açın ve iki ekleme **kullanarak** deyimleri.

    ```cs
    using System.Collections.Generic;
    using System.Linq;
    ```

2. Şimdi geçerli birkaç satırı değiştirin **İNDİS()** yöntemi bir yöntemle otel kategorileri için bir model arama yapar. Arama zaman uyumsuz olarak yürütülmesi gereken şekilde size bildirmelidir **dizin** yöntemi olarak **zaman uyumsuz**.

    ```cs
        public async Task<ActionResult> Index()
        {
            InitSearch();

            // Set up the facets call in the search parameters.
            SearchParameters sp = new SearchParameters()
            {
                // Search for up to 20 categories.
                // Field names specified here must be marked as "IsFacetable" in the model, or the search call will throw an exception.
                Facets = new List<string> { "Category,count:20" },
            };

            DocumentSearchResult<Hotel> searchResult = await _indexClient.Documents.SearchAsync<Hotel>("*", sp);

            // Convert the results to a list that can be displayed in the client.
            List<string> categories = searchResult.Facets["Category"].Select(x => x.Value.ToString()).ToList();

            // Initiate a model with a list of facets for the first view.
            SearchData model = new SearchData(categories);

            // Save the facet text for the next view.
            SaveFacets(model);

            // Render the view including the facets.
            return View(model);
        }
    ```

    Burada dikkat edilecek noktaları birkaç. Biz bir dize listesi için arama çağrının sonuçlarını dönüştürmesi, ardından bu modeli dizeler eklenen bir **SearchData** iletişimi görünüm için model. Ayrıca biz Bu dizelerin geçici depolama birimine görünümü son işlemeden önce kaydedin. Bu kaydetme, böylece bu liste bir denetleyici eylemi sonraki çağrı için kullanılabilir yapılır.

3. Kaydet ve modelleri modeline ve geçici depolama birimine geri yüklemek için iki özel yöntem ekleyelim.

    ```cs
        // Save the facet text to temporary storage, optionally saving the state of the check boxes.
        private void SaveFacets(SearchData model, bool saveChecks = false)
        {
            for (int i = 0; i < model.facetText.Length; i++)
            {
                TempData["facet" + i.ToString()] = model.facetText[i];
                if (saveChecks)
                {
                    TempData["faceton" + i.ToString()] = model.facetOn[i];
                }
            }
            TempData["facetcount"] = model.facetText.Length;
        }

        // Recover the facet text to a model, optionally recoving the state of the check boxes.
        private void RecoverFacets(SearchData model, bool recoverChecks = false)
        {
            // Create arrays of the appropriate length.
            model.facetText = new string[(int)TempData["facetcount"]];
            if (recoverChecks)
            {
                model.facetOn = new bool[(int)TempData["facetcount"]];
            }

            for (int i = 0; i < (int)TempData["facetcount"]; i++)
            {
                model.facetText[i] = TempData["facet" + i.ToString()].ToString();
                if (recoverChecks)
                {
                    model.facetOn[i] = (bool)TempData["faceton" + i.ToString()];
                }
            }
        }
    ```

### <a name="save-and-restore-facet-text-on-all-calls"></a>Kaydet ve modeli metin tüm çağrılarda geri yükleme

1. İki diğer eylemleri giriş denetleyicisinin **dizini (SearchData model)** ve **sayfa (SearchData model)** , arama çağrısından önce modelleri kurtarmak ve bunları tekrar arama çağrısından sonra kaydetmek her ikisine birden ihtiyacınız. Değişiklik **dizini (SearchData model)** bu iki çağrıları yapma.

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

                // Recover the facet text.
                RecoverFacets(model);

                // Make the search call for the first page.
                await RunQueryAsync(model, 0, 0);

                // Ensure temporary data is stored for the next call.
                TempData["page"] = 0;
                TempData["leftMostPage"] = 0;
                TempData["searchfor"] = model.searchText;

                // Facets
                SaveFacets(model, true);
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

2. Artık bunu **sayfa (SearchData model)** yöntemi. Yalnızca aşağıdaki ilgili kod listeledik. Ekleme **RecoverFacets** ve **SaveFacets** etrafında çağırır **RunQueryAsync** çağırın.

    ```cs
                // Recover facet text and check marks.
                RecoverFacets(model, true);

                await RunQueryAsync(model, page, leftMostPage);

                // Save facets and check marks.
                SaveFacets(model, true);
    ```

### <a name="set-up-a-search-filter"></a>Bir arama filtre ayarlamak

Bir kullanıcı belirli özellikleri seçtiğinde, örneğin, örneğin bunlar tıklayın **bütçe** ve **çare ve Spa** kategorileri, sonra bu iki kategoriden olarak döndürülmelidir, belirtilen hotels Sonuç. Bu şekilde bir arama iyileştirmek için ayarlamak ihtiyacımız bir _filtre_.

1. İçinde **RunQueryAsync** yöntemi, bir filtre dizesi oluşturmak için belirli bir modelin model ayarları döngüsü için kod ekleyin. Ve filtresine **kullanılması**aşağıdaki kodda gösterildiği gibi.

    ```cs
            // Create a filter for selected facets.
            string selectedFacets = "";

            for (int f = 0; f < model.facetText.Length; f++)
            {
                if (model.facetOn[f])
                {
                    if (selectedFacets.Length > 0)
                    {
                        // If there is more than one selected facet, logically OR them together.
                        selectedFacets += " or ";
                    }
                    selectedFacets += "(Category eq \'" + model.facetText[f] + "\')";
                }
            }

            var parameters = new SearchParameters
            {
                // Facets: add the filter.
                Filter = selectedFacets,

                // Enter Hotel property names into this list so only these values will be returned.
                // If Select is empty, all values will be returned, which can be inefficient.
                Select = new[] { "HotelName", "Description", "Category" },
                SearchMode = SearchMode.All,

                // Skip past results that have already been returned.
                Skip = page * GlobalVariables.ResultsPerPage,

                // Take only the next page worth of results.
                Top = GlobalVariables.ResultsPerPage,

                // Include the total number of results.
                IncludeTotalResultCount = true,
            };
    ```

    Ekledik **kategori** özellik listesine **seçin** öğeleri döndürmek için. Bu özelliğe bir gereksinim değildir, ancak bu şekilde biz size doğru filtreleme doğrulayabilirsiniz.

### <a name="define-a-few-additional-html-styles"></a>Birkaç ek HTML stillerini tanımlamak

Görünüm gerektiren bazı önemli değişiklikler yapacaktır. 

1. (Wwwroot/css klasöründe) hotels.css dosyasını açarak başlatın ve aşağıdaki sınıfları ekleyin.

    ```html
    .facetlist {
        list-style: none;
    }

    .facetchecks {
        width: 200px;
        background-color: lightgoldenrodyellow;
        display: normal;
        color: #666;
        margin: 10px;
    }
    ```

### <a name="add-a-list-of-facet-checkboxes-to-the-view"></a>Model onay kutularından oluşan bir liste görünümüne ekleyin.

Görünümü için sol taraftaki modelleri ve sağdaki sonuçları düzgünce hizalamak için bir tabloya çıkış düzenleyin. Index.cshtml dosyasını açın.

1. HTML öğesinin tüm içeriğini değiştirin &lt;gövdesi&gt; aşağıdaki kod ile etiketler.

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
                        <h5>Filter by Category:</h5>
                        <ul class="facetlist">
                            @for (var i = 0; i < Model.facetText.Length; i++)
                            {
                                <li> @Html.CheckBoxFor(m => m.facetOn[i], new { @id = "check" + i.ToString() }) @Model.facetText[i] </li>
                            }
                        </ul>
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
                                // Display the hotel name and description.
                                @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                                @Html.TextArea("desc", Model.resultList.Results[i].Document.Description + "\nCategory:  " +  Model.resultList.Results[i].Document.Category, new { @class = "box2" })
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

    Kullanımına dikkat edin **CheckBoxFor** doldurmak için çağrı **facetOn** kullanıcı seçimlerini dizi. Ayrıca, otel açıklama sonuna otel kategorisini ekledik. Bu metin yalnızca arama özelliğimizi düzgün çalıştığından emin olmaktır. Biz bir tabloya çıkış düzenli dışında önceki öğreticilerden değil daha başka değiştirdi.

### <a name="run-and-test-the-app"></a>Çalıştırın ve uygulamayı test etme

1. Uygulamayı çalıştırın ve modelleri listesinde düzgünce sola göründüğünü doğrulayın.

2. Bir, iki, üç veya daha fazla onay kutusunu seçmeyi deneyin ve sonuçları doğrulayın.

    ![Bir arama "wifi" daraltmak için modeli gezintisini kullanma](./media/tutorial-csharp-create-first-app/azure-search-facet-nav.png)

3. Model Gezinti sahip hafif bir karmaşıklık yoktur. Bir kullanıcı (seçerek veya serbest onay kutularını seçerek) modeli seçimi değiştirir, ancak disk belleği seçenekleri ve arama çubuğunu birine tıklar neler? Aslında bu geçerli sayfa doğru olarak seçimi değiştirme yeni bir arama başlatmak. Alternatif olarak, kullanıcı değişiklikleri gözardı, ve sonraki sonuç sayfasını verilen, özgün modeli seçimlerine göre. Biz bu örnekte ikinci çözüm seçtiniz, ancak belki de önceki çözümü nasıl uygulayabilir göz önünde bulundurun. Yeni bir arama belki de en son seçimi seçilen facets geçici depolama seçimi tam olarak eşleşmiyor tetikleme?

Bu, Örneğimizdeki modeli Gezinti tamamlar. Ancak belki de ayrıca bu uygulamayı nasıl uzayabilir deneyebilirsiniz. Model listesi modeli mümkün diğer alanları içerecek şekilde genişletilmesini (söyleyin **etiketleri**), böylece bir kullanıcı bir havuz, wifi, çubuk, ücretsiz park ve benzeri gibi seçenekleri seçebilirsiniz. 

Kullanıcı gezinti modeli avantajı aynı metin girerek korumak zorunda değildir, modeli seçimleri için uygulamanın geçerli oturumla ömrü korunur olmasıdır. Kategori seçmeleri ve diğer belirli bir metin üzerinde tek bir tıklamayla olabilecek diğer öznitelikler arayın.

Artık farklı bir kullanım facets inceleyelim.

## <a name="add-facet-autocompletion-to-your-app"></a>Model otomatik tamamlama uygulamanıza ekleme

Model otomatik tamamlama, uygulamayı ilk kez çalıştırdığınızda bir ilk araması yaparak çalışır. Bu arama kullanıcı yazarken öneri olarak kullanılacak özellikleri listesini toplar.

![Yazma "Kaldır" üç modelleri ortaya çıkarır.](./media/tutorial-csharp-create-first-app/azure-search-facet-type-re.png)

Bu örnek için bir temel olarak ikinci öğreticide tamamlamış olabilirsiniz numaralı sayfalama uygulama kullanacağız.

Model otomatik tamamlama uygulamak için biz modelleri (veri sınıfları) değiştirmek gerekmez. Görünüm ve denetleyici için eylem bazı komut dosyası eklemeniz gerekir.

### <a name="add-an-autocomplete-script-to-the-view"></a>Otomatik Tamamlama betiği Görünümü Ekle

Bir modeli aramayı başlatmak için bir sorgu göndermek gerekiyor. Eklenen Index.cshtml dosyası için aşağıdaki JavaScript sorgu mantığının ve ihtiyacımız sunu sağlar.

1. Bulun **@Html.TextBoxFor(m = > m.searchText,...)** deyimi, aşağıdakine benzer benzersiz bir kimliği ekleyin.

    ```cs
    <div class="searchBoxForm">
        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azuresearchfacets" }) <input value="" class="searchBoxSubmit" type="submit">
    </div>
    ```

2. Şimdi aşağıdaki JavaScript ekleyin (Kapanıştan sonra **&lt;/div&gt;** ince works gösterilen).

    ```JavaScript
     <script>
            $(function () {
                $.getJSON("/Home/Facets", function (data) {

                    $("#azuresearchfacets").autocomplete({
                        source: data,
                        minLength: 2,
                        position: {
                            my: "left top",
                            at: "left-23 bottom+10"
                        }
                    });
                });
            });
        </script>
    ```

    Bir betik çağıran bildirimi **modelleri** iki yazılan karakterlerin en az şu uzunlukta ulaşıldığında hiçbir parametre olmadan, ev denetleyicideki eylem.

### <a name="add-references-to-jquery-scripts-to-the-view"></a>Jquery betikleri başvuruları görünümüne ekleyin.

Yukarıdaki komut Otomatik Tamamlama işlevi biz jquery Kitaplığı'nda kullanılabilir olarak kendimize yazmanız gereken bir şey değil. 

1. Jquery kitaplığınıza erişmesine izin değiştirin &lt;baş&gt; bölümünü aşağıdaki kodla görünüm dosyası.

    ```cs
    <head>
        <meta charset="utf-8">
        <title>Facets demo</title>
        <link href="https://code.jquery.com/ui/1.10.4/themes/ui-lightness/jquery-ui.css"
              rel="stylesheet">
        <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
        <script src="https://code.jquery.com/ui/1.10.4/jquery-ui.js"></script>

        <link rel="stylesheet" href="~/css/hotels.css" />
    </head>
    ```

2. Ayrıca, jquery _Layout.cshtml dosyasına başvuran bir satırı açıklama satırı yapın veya kaldırmak ihtiyacımız (içinde **görünümler/paylaşılan** klasörü). Aşağıdaki satırları bulun ve gösterildiği gibi ilk komut satırı açıklama satırı yapın. Bu satırı kaldırarak, biz jquery belirsiz başvuru kaçının.

    ```html
     <environment include="Development">
            <!-- <script src="~/lib/jquery/dist/jquery.js"></script> -->
            <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
            <script src="~/js/site.js" asp-append-version="true"></script>
    </environment>
    ```

Önceden tanımlanmış otomatik tamamlama jquery işlevleri artık kullanabiliriz.

### <a name="add-a-facet-action-to-the-controller"></a>Denetleyiciye bir modeli eylemi ekleyin

1. Giriş denetleyicisine açın ve aşağıdaki iki ekleme **kullanarak** deyimlerini dosyanın karşılaştırması.

    ```cs
    using System.Collections.Generic;
    using System.Linq;
    ```

2. Görünümü Tetikleyiciler JavaScript'te **modelleri** eylem denetleyicide, bu eylem giriş denetleyicisine şimdi ekleyin. (örneğin, aşağıdaki **sayfası** eylem).

    ```cs
        public async Task<ActionResult> Facets()
        {
            InitSearch();

            // Set up the facets call in the search parameters.
            SearchParameters sp = new SearchParameters()
            {
                // Search all Tags, but limit the total number to 100, and add up to 20 categories.
                // Field names specified here must be marked as "IsFacetable" in the model, or the search call will throw an exception.
                Facets = new List<string> { "Tags,count:100", "Category,count:20" },
            };

            DocumentSearchResult<Hotel> searchResult = await _indexClient.Documents.SearchAsync<Hotel>("*", sp);

            // Convert the results to two lists that can be displayed in the client.
            List<string> facets = searchResult.Facets["Tags"].Select(x => x.Value.ToString()).ToList();
            List<string> categories = searchResult.Facets["Category"].Select(x => x.Value.ToString()).ToList();

            // Combine and return the lists.
            facets.AddRange(categories);
            return new JsonResult(facets);
        }
    ```

    Biz den en fazla 100 modelleri isteyen dikkat edin **etiketleri** alanlar, 20'den en fazla ve **kategori** alanları. **Sayısı** girişler isteğe bağlı, bir sayı ayarlanırsa, varsayılan 10'dur.

    Biz aranacak iki alan sorulan içerdiği birine, ardından birleştirilir iki liste ihtiyacımız (**etiketleri** ve **kategori**). Şu üç alan aranacak istenirse, biz üç listeleri birine birleştirmek ve benzeri gerekir.

    > [!NOTE]
    > Bir veya daha fazla aşağıdaki parametreleri her bir alan için bir model arama ayarlamak da mümkündür: **sayısı**, **sıralama**, **aralığı**, ve **değerleri**. Daha fazla bilgi için [Azure Arama'da çok yönlü navigasyon uygulamak nasıl](https://docs.microsoft.com/azure/search/search-faceted-navigation).

### <a name="compile-and-run-your-project"></a>Projeyi derlemek ve çalıştırmak

Şimdi, program test edin.

1. Birkaç sonuçlar göstermesi gerekir arama kutusuna "fr" yazmayı deneyin.

    !["Fr" yazarak üç modelleri ortaya çıkarır](./media/tutorial-csharp-create-first-app/azure-search-facet-type-fr.png)

2. Şimdi "" Excel'den"yapıp çeşitli seçenekleri için sınırlı fark o" ekleyin.

3. Diğer iki harf birleşimiyle yazın ve hangi görüntülenir. Sunucuya yazarken olduğunu fark *değil* çağrılan. Modelleri, uygulama başlatıldığında ve kullanıcı bir arama istediğinde artık bir çağrı yalnızca sunucuya yapılır yerel olarak önbelleğe alınır.

## <a name="decide-when-to-use-a-facet-autocompletion-search"></a>Bir modeli otomatik tamamlama araması ne zaman karar

Modeli aramalar ve öneriler ve otomatik tamamlama, gibi diğer aramaları Temizle birbirinden modeli arama olmasıdır _tasarlanmış_ bir sayfa yüklendiğinde yalnızca bir kez gerçekleştirilmesi. Otomatik Tamamlama aramalar _tasarlanmış_ her karakter girildikten sonra çağrılabilir. Modelleri kullanarak bu şekilde potansiyel olarak birçok sunucu çağrıları kaydeder. 

Ancak, ne zaman model otomatik tamamlama kullanılmalıdır?

En iyi modeli otomatik tamamlama kullanılır:
* Her tuş vuruşu çağrı sunucusu diğer arama performansını bir sorun olduğunu birincil nedenidir.
* Bunlar birkaç karakter yazdığınızda döndürülen modeller makul uzunluğu seçeneklerin bir listesi ile kullanıcı sağlar.
* Döndürülen modeller en erişmek için hızlı bir yol veya ideal olarak, kullanılabilir verilerin tümünü sağlar.
* Maksimum sayısı dahil çoğu özellikleri sağlar. En fazla 100 modeller için ayarladık, kodumuz **etiketleri** ve 20 modelleri için **kategori**. Üst sınırlar kümesi de veri kümesi boyutu ile çalışması gerekir. Çok sayıda olası modelleri Kes, ardından belki de arama olması gerektiği gibi yararlı değildir.

> [!NOTE]
> Modeli aramaları sayfa yükleme, bunlar Elbette çok daha sık çağrılabilen bir kez çağrılması için tasarlanmış olsa da,, JavaScript bağlıdır. Otomatik Tamamlama/öneri aramaları tuş vuruşu başına bir kez daha az sıklıkta gerçekleştirilebilme değer eşit şekilde true şeklindedir. Bu yeniden, JavaScript, Azure Search tarafından belirlenir. Ancak, model arama modeller Azure Search tarafından bu Aranan aklınızda belgelerle oluşturulan gibi sayfa başına yalnızca bir kez çağrılması için tasarlanmıştır. Modeli otomatik tamamlama aramaları, kullanıcı Yardım biraz daha az esnek ancak daha verimli ağ form olarak dikkate alınması gereken iyi bir uygulamadır.

## <a name="takeaways"></a>Paketler

Aşağıdaki paketler bu projedeki göz önünde bulundurun:

* Her bir alan olarak işaretlemek için zorunludur **IsFacetable**, modeli gezinme veya otomatik tamamlama dahil edilecek olmaları durumunda.
* Model Gezinti bir kullanıcı bir arama daraltma, kolay ve sezgisel bir yol sağlar.
* Model gezinti, en uygun bir başlık ile her bölüm (otel kategorisi), bir otel, fiyat aralıkları, vs. özellikleri bölümlere ayrılmıştır.
* Model otomatik tamamlama, diğer otomatik tamamlama aramalarınızı yinelenen sunucu çağrıları olmadan bir yardımcı bir kullanıcı deneyimi alma etkili bir yoldur.
* Model otomatik tamamlama olduğu bir _alternatif_ otomatik tamamlama/öneri, bir toplama için.

## <a name="next-steps"></a>Sonraki adımlar

Bu dizi tamamladınız C# - öğreticiler, kazanılan Azure arama API'lerinin değerli bilgi.

Daha fazla başvuru ve öğreticiler için gözatma göz önünde bulundurun [Microsoft Learn](https://docs.microsoft.com/learn/browse/?products=azure), ya da diğer öğreticileri, [Azure arama belgeleri](https://docs.microsoft.com/azure/search/).
