---
title: C#Arama sonuçlarını sayfalandırma - Azure Search Öğreticisi
description: Bu öğreticide, "Azure Search - ilk uygulamanızı oluşturma" projede iki tür disk belleği tercih ettiğiniz oluşturur. İlk sayfa numarası düğmelerini yanı sıra ilk olarak, daha önceki bir dizi kullanır ve düğmeler son sayfa. İkinci sayfalama sistem sonsuz kaydırma, dikey kaydırma çubuğu alt sınırına taşıyarak tetiklenen kullanır.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 05/01/2019
ms.openlocfilehash: fc2f358921380803e89c7a8ed5c2ef0fc8e1e467
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304367"
---
# <a name="c-tutorial-search-results-pagination---azure-search"></a>C#öğretici: Arama sonuçlarını sayfalandırma - Azure Search

İki farklı disk belleği sistemleri, uygulama hakkında bilgi edinmek ilk temel sayfa numaralarını ve sonsuz kayan üzerinde ikinci alan. Her iki sistem disk belleği, yaygın olarak kullanılan ve sonuçları ile istediğiniz kullanıcı deneyimini doğru olanı seçerek bağlıdır. Bu öğreticide oluşturulan projesine sayfalama sistemleri yapıları [ C# Öğreticisi: Azure Search - ilk uygulamanızı oluşturma](tutorial-csharp-create-first-app.md) öğretici.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Numaralı disk belleği ile uygulamanızı genişletin
> * Sonsuz kayan ile uygulamanızı genişletin

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

Sahip [ C# Öğreticisi: Azure Search - ilk uygulamanızı oluşturma](tutorial-csharp-create-first-app.md) proje çalışır. Bu proje kendi sürümü veya Github'dan yükleyin: [İlk uygulamanızı oluşturma](https://github.com/Azure-Samples/azure-search-dotnet-samples).

## <a name="extend-your-app-with-numbered-paging"></a>Numaralı disk belleği ile uygulamanızı genişletin

Numaralı disk belleği ana internet arama motorları ve diğer arama Web sitelerinin çoğu tercih ettiğiniz sayfalama sistemidir. Numaralı disk belleği, genellikle bir dizi gerçek sayfa numarası yanı sıra, "İleri" ve "önceki" bir seçenek içerir. Ayrıca bir "ilk sayfası" ve "son sayfa" seçeneği de kullanılabilir. Bu seçenekler, sayfa tabanlı sonuçları arasında gezinme boyunca bir kullanıcı denetimi kesinlikle verin.

İlk, önceki, sonraki içerir ve en son, 1 ile başlamayan sayfa numaralarını birlikte seçenekleri sistem ekleyeceğiz ancak bunun yerine saran bir sayfayı kullanıcı (örneğin kullanıcı, 10, belki de sayfa numaralarını 8 sayfanın arıyor bu nedenle, açıktır 9, 10, 11 ve 12 görüntülenir).

Sistemin genel bir değişkende ayarlanacak görünür sayfası numaraları izin vermek için esnek olacaktır.

Sistem görüntülenen sayfa numaraları aralığını değiştirme tetikleyecek anlamı en sol ve sağ en sayfa numarası düğmelerini özel olarak değerlendirir. Örneğin, sayfa numarası 8, 9, 10, 11 ve 12 görüntülenir ve 8'de kullanıcı tıkladığında, ardından bir dizi sayfa numarası 6, 7, 8, 9 ve 10 görüntülenen değişir. Ve benzer bir sağa kaydırma 12'i seçtiyseniz.

### <a name="add-paging-fields-to-the-model"></a>Modeli için disk belleği alanları ekleme

Açık temel arama sayfası çözümünüz varsa.

1. SearchData.cs model dosyasını açın.

2. Öncelikle bazı genel değişkenleri ekleyin. MVC'de genel değişkenleri kendi statik sınıfta bildirilir. **ResultsPerPage** sayfa başına sonuç sayısını ayarlar. **MaxPageRange** görünümünde görünür sayfa numaralarını sayısını belirler. **PageRangeDelta** kaç sayfaları sola veya sağa sayfa aralığı kaydırılacağı uzaklık, en sol veya en sağ bir sayfa numarası seçildiğinde belirler. Genellikle bu ikinci etrafında sayıdır yarısını **MaxPageRange**. Aşağıdaki kod ad alanına ekleyin.

    ```cs
    public static class GlobalVariables
    {
        public static int ResultsPerPage
        {
            get
            {
                return 3;
            }
        }
        public static int MaxPageRange
        {
            get
            {
                return 5;
            }
        }

        public static int PageRangeDelta
        {
            get
            {
                return 2;
            }
        }
    }
    ```

    >[!Tip]
    >Bu proje, bir dizüstü bilgisayar gibi daha küçük bir ekrana sahip bir cihazda çalıştırıyorsanız değiştirmeyi göz önünde bulundurun **ResultsPerPage** 2.

3. Disk belleği özellikleri **SearchData** , örneğin sonra sınıf **Aramametni** özelliği.

    ```cs
        // The current page being displayed.
        public int currentPage { get; set; }

        // The total number of pages of results.
        public int pageCount { get; set; }

        // The left-most page number to display.
        public int leftMostPage { get; set; }

        // The number of page numbers to display - which can be less than MaxPageRange towards the end of the results.
        public int pageRange { get; set; }

        // Used when page numbers, or next or prev buttons, have been selected.
        public string paging { get; set; }
    ```

### <a name="add-a-table-of-paging-options-to-the-view"></a>Disk belleği seçenekleri, tablo görünümü ekleme

1. Index.cshtml dosyasını açıp kapatma hemen önce aşağıdaki kodu ekleyin &lt;/body&gt; etiketi. Bu yeni kod bir tablosu disk belleği seçenekleri sunar: ilk, önceki, 1, 2, 3, 4, 5, ardından, en son.

    ```cs
    @if (Model != null && Model.pageCount > 1)
    {
    // If there is more than one page of results, show the paging buttons.
    <table>
        <tr>
            <td>
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

            <td>
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
                <td>
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

            <td>
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

            <td>
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
    ```

    Bir HTML tablosu şeyler düzgünce hizalamak için kullanırız. Tüm eylem geldiği ancak @Html.ActionLink her denetleyiciyle çağırma deyimleri, bir **yeni** farklı girişleri ile oluşturulan model **sayfalama** daha önce eklediğimiz özelliği.

    İlk ve son sayfa seçenekleri gibi "first" ve "son" dizelerini gönderme, ancak doğru sayfa numaralarını gönderin.

2. Bazı sayfalama sınıfları HTML stillerini hotels.css dosya listesine ekleyin. **PageSelected** sınıfı oradadır kullanıcı şu anda görüntülemekte (sayı kalın kapatarak) sayfası tanımlamak için sayfa numaralarını listesinde.

    ```html
        .pageButton {
            border: none;
            color: darkblue;
            font-weight: normal;
            width: 50px;
        }

        .pageSelected {
            border: none;
            color: black;
            font-weight: bold;
            width: 50px;
        }

        .pageButtonDisabled {
            border: none;
            color: lightgray;
            font-weight: bold;
            width: 50px;
        }
    ```

### <a name="add-a-page-action-to-the-controller"></a>Denetleyici için bir sayfa Eylem Ekle

1. HomeController.cs dosyasını açın ve eklemek **sayfa** eylem. Bu eylem, seçili sayfa seçeneklerden herhangi biri olarak yanıt verir.

    ```cs
        public async Task<ActionResult> Page(SearchData model)
        {
            try
            {
                int page;

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

                // Recover the search text and search for the data for the new page.
                model.searchText = TempData["searchfor"].ToString();

                await RunQueryAsync(model, page, leftMostPage);

                // Ensure Temp data is stored for next call, as TempData only stored for one call.
                TempData["page"] = (object)page;
                TempData["searchfor"] = model.searchText;
                TempData["leftMostPage"] = model.leftMostPage;
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "2" });
            }
            return View("Index", model);
        }
    ```

    **RunQueryAsync** yöntemi bir söz dizimi hatası, şimdi biz de biraz gelir için üçüncü parametresi nedeniyle gösterilir.

    > [!Note]
    > **TempData** çağrıları depolamak bir değer (bir **nesne**) geçici depolama biriminde, ancak bu depolama devam ederse için _yalnızca_ çağrı. Geçici verileri depolarız bir şey, bir denetleyici eylemi sonraki çağrı için kullanılabilir, ancak yapmamanız çağrı tarafından bundan sonra silinecekler! Bu kısa kullanım ömrü nedeniyle depolarız arama metnini ve disk belleği özellikleri her çağrı için geçici depolamayı geri **sayfa**.

2. **Index(model)** eylem gereksinimlerini en soldaki sayfa parametresi eklenecek ve geçici değişkenlerin depolamak için güncelleştirilmiş **RunQueryAsync** çağırın.

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
                await RunQueryAsync(model, 0, 0);

                // Ensure temporary data is stored for the next call.
                TempData["page"] = 0;
                TempData["leftMostPage"] = 0;
                TempData["searchfor"] = model.searchText;
            }

            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View(model);
        }
    ```

3. **RunQueryAsync** yöntemi gereksinimlerini önemli ölçüde güncelleştirildi. Kullandığımız **atla**, **üst**, ve **IncludeTotalResultCount** alanlarının **kullanılması** yalnızca bir sayfa değerinde istemek için sınıfı Sonuçlar, başlayan **atla** ayarı. Ayrıca, görünüm için disk belleği değişkenleri hesaplamak ihtiyacımız var. Tüm yöntemini aşağıdaki kodla değiştirin.

    ```cs
        private async Task<ActionResult> RunQueryAsync(SearchData model, int page, int leftMostPage)
        {
            InitSearch();

            var parameters = new SearchParameters
            {
                   // Enter Hotel property names into this list so only these values will be returned.
                   // If Select is empty, all values will be returned, which can be inefficient.
                   Select = new[] { "HotelName", "Description" },
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
            model.pageCount = ( (int)model.resultList.Count + GlobalVariables.ResultsPerPage - 1) / GlobalVariables.ResultsPerPage;

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
                leftMostPage = Math.Min(leftMostPage + GlobalVariables.PageRangeDelta, model.pageCount - GlobalVariables.MaxPageRange);
            }
            model.leftMostPage = leftMostPage;

            // Calculate the number of page numbers to display.
            model.pageRange = Math.Min(model.pageCount - leftMostPage, GlobalVariables.MaxPageRange);

            return View("Index", model);
        }
    ```

4. Son olarak, görünümde küçük bir değişiklik yapmak ihtiyacımız var. Değişken **resultsList.Results.Count** artık toplam sayı bir sayfa (Bizim örneğimizde 3), döndürülen sonuç sayısını içerir. Biz ayarlandığından **IncludeTotalResultCount** true olarak değişkeni **resultsList.Count** artık sonuçlarının toplam sayısını içerir. Bu nedenle sonuç sayısı Görünümü'nde görüntüleyen bulun ve aşağıdaki kodla değiştirin.

    ```cs
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Count) Results
            </p>
    ```

    > [!Note]
    > Genellikle çok ayarlayarak bir rağmen bir performans düşüşüne olan **IncludeTotalResultCount** true, bu toplam Azure Search tarafından hesaplanması gerektiği için. Döndürülen değer bir uyarı var. karmaşık veri kümeleri ile bir _yaklaşık_. Otel verilerimiz için doğru olacaktır.

### <a name="compile-and-run-the-app"></a>Derleme ve uygulamayı çalıştırma

Şimdi seçtiğiniz **hata ayıklama olmadan Başlat** (veya F5 tuşuna basın).

1. Sonuçlar (örneğin, "wifi") bolca sağlayacak bazı metin arama yapın. Düzgünce sonuçlarını sayfasında?

    !["Havuzu" sonuçları numaralı sayfalama](./media/tutorial-csharp-create-first-app/azure-search-numbered-paging.png)

2. En sağdaki ve daha sonra tıklayarak en soldaki sayfa numarası deneyin. Sayfa numaralarını Orta olan sayfa için uygun şekilde ayarlama?

3. "First" ve "son" seçenekleri büyük/küçük harf yararlıdır? Başkalarının bunu ve bazı popüler web aramaları bu seçenekleri kullanın.

4. Son sonuçları sayfasına gidin. Son sayfa içerebilen tek sayfasıdır küçüktür **ResultsPerPage** sonuçları.

    ![Son sayfasında "wifi" İnceleme](./media/tutorial-csharp-create-first-app/azure-search-pool-last-page.png)

5. "Piyasada" yazın ve arama'yı tıklatın. Sonuçlar birden az standart sayfa değerinde ise disk belleği seçenekleri görüntülenir.

    !["Şehir" için arama](./media/tutorial-csharp-create-first-app/azure-search-town.png)

Artık bu projeyi kaydedin ve bu tür bir disk belleği alternatif deneyelim.

## <a name="extend-your-app-with-infinite-scrolling"></a>Sonsuz kayan ile uygulamanızı genişletin

Bir kullanıcı, son görüntülenen sonuçların bir dikey kaydırma çubuğu kaydırdığında sonsuz kayan tetiklenir. Bu durumda, sonraki sonuç sayfasını için sunucuya bir çağrı yapılır. Daha fazla sonuç varsa, hiçbir şey döndürülür ve dikey kaydırma çubuğunun değişmez. Daha fazla sonuç varsa, geçerli sayfa ve daha fazla sonuç kullanılabilir olduğunu göstermek için kaydırma çubuğu değişiklikleri eklenir.

Burada önemli olan nokta, görüntülenen sayfa, ancak eklenmiş yeni sonuçlarla yerini aldığını değil ' dir. Bir kullanıcı her zaman geri arama ilk sonuçlarını kadar kaydırma yapabilir.

Herhangi bir sayfa numarası kaydırma öğeleri eklenmeden önceki sonsuz kayan uygulamak için proje ile başlayalım. Bu nedenle, gerekiyorsa, başka bir kopya temel arama sayfasının Github'dan olun: [İlk uygulamanızı oluşturma](https://github.com/Azure-Samples/azure-search-dotnet-samples).

### <a name="add-paging-fields-to-the-model"></a>Modeli için disk belleği alanları ekleme

1. İlk olarak, ekleme bir **sayfalama** özelliğini **SearchData** sınıfta (SearchData.cs model dosyası).

    ```cs
        // Record if the next page is requested.
        public string paging { get; set; }
    ```

    Bu değişken sonraki sonuç sayfasını gönderilmesi gerektiğini veya boş bir arama'nın ilk sayfasında için "İleri" tutan bir dize ise.

2. Aynı dosyada ve ad alanı içinde bir özelliği olan bir genel değişken sınıfına ekleyin. MVC'de genel değişkenleri kendi statik sınıfta bildirilir. **ResultsPerPage** sayfa başına sonuç sayısını ayarlar. 

    ```cs
    public static class GlobalVariables
    {
        public static int ResultsPerPage
        {
            get
            {
                return 3;
            }
        }
    }
    ```

### <a name="add-a-vertical-scroll-bar-to-the-view"></a>Dikey kaydırma çubuğu görünümü Ekle

1. Sonuçları görüntüler Index.cshtml dosyasının bölümünü bulun (ile başlayan  **@if (Model! = null)** ).

2. Bölümü, aşağıdaki kodla değiştirin. Yeni **&lt;div&gt;** bölümdür kaydırılabilir olmalıdır ve her ikisi de ekler etrafına bir **overflow-y** özniteliğini ve bir çağrı bir **onscroll**"scrolled()", çağrılan işlev sözlüğüdür.

    ```cs
        @if (Model != null)
        {
            // Show the result count.
            <p class="sampleText">
                @Html.DisplayFor(m => m.resultList.Count) Results
            </p>

            <div id="myDiv" style="width: 800px; height: 450px; overflow-y: scroll;" onscroll="scrolled()">

                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    // Display the hotel name and description.
                    @Html.TextAreaFor(m => Model.resultList.Results[i].Document.HotelName, new { @class = "box1" })
                    @Html.TextArea("desc", Model.resultList.Results[i].Document.Description, new { @class = "box2" })
                }
            </div>
        }
    ```

3. Döngü hemen altındaki sonra &lt;/div&gt; etiketinde, ekleyin **kaydırılan** işlevi.

    ```javascript
        <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 2) {
                                div.innerHTML += '\n<textarea class="box1">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2">' + data[i + 1] + '</textarea>';
                            }
                        });
                    }
                }
        </script>
    ```

    **Varsa** ifade yukarıda kullanıcının dikey kaydırma çubuğunun altına kaydırılan olmadığını görmek için sınar. Aksi halde, bir çağrı **giriş** denetleyicisi çağrılan bir eylem yapılan **sonraki**. Denetleyici tarafından başka hiçbir bilgi gereklidir, veri sonraki sayfasına dönersiniz. Bu verileri aynı HTML stil özgün sayfa kullanılarak biçimlendirilir. Hiçbir sonuç döndürmezse, hiçbir şey eklenir ve oldukları gibi şeyler kalın.

### <a name="handle-the-next-action"></a>Tanıtıcı sonraki eylem

Denetleyiciye gönderilmesi gereken yalnızca üç eylem vardır: çağıran uygulama, ilk çalıştırma **İNDİS()** , çağırır kullanıcı tarafından ilk arama **Index(model)** ve ardından sonraki Daha fazla sonuç için çağıran **Next(model)** .

1. Giriş denetleyicisine dosyasını açın ve silmek **RunQueryAsync** özgün öğreticiden yöntemi.

2. Değiştirin **Index(model)** aşağıdaki kodla eylem. Artık işleme **sayfalama** alan null ya da "sonraki" ayarlayın ve Azure Search çağrısını işler.

    ```cs
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                InitSearch();

                int page;

                if (model.paging != null && model.paging == "next")
                {
                    // Increment the page.
                    page = (int)TempData["page"] + 1;

                    // Recover the search text.
                    model.searchText = TempData["searchfor"].ToString();
                }
                else
                {
                    // First call. Check for valid text input.
                    if (model.searchText == null)
                    {
                        model.searchText = "";
                    }
                    page = 0;
                }

                // Setup the search parameters.
                var parameters = new SearchParameters
                {
                    // Enter Hotel property names into this list so only these values will be returned.
                    // If Select is empty, all values will be returned, which can be inefficient.
                    Select = new[] { "HotelName", "Description" },
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

                // Ensure TempData is stored for the next call.
                TempData["page"] = page;
                TempData["searchfor"] = model.searchText;
            }
            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View("Index", model);
        }
    ```

    Kullandığımız numaralı disk belleği yöntemine benzer **atla** ve **üst** ihtiyacımız olan veriler istemek için arama ayarları döndürülür.

3. Ekleme **sonraki** giriş denetleyicisine eylem. Nasıl bu iki öğeyi listeye eklemeyi her otel listesini döndürür. Not: bir otel adı ve Otel açıklama. Bu biçim eşleşecek şekilde ayarlanan **kaydırılan** döndürülen veri görünümünde işlevin kullanın.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel name, then description, to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

4. Bir söz dizimi hatası alıyorsanız **listesi&lt;dize&gt;** , ardından aşağıdakileri ekleyin **kullanarak** denetleyici dosyasının başındaki yönergesini.

    ```cs
    using System.Collections.Generic;
    ```

### <a name="compile-and-run-your-project"></a>Projeyi derlemek ve çalıştırmak

Şimdi seçtiğiniz **hata ayıklama olmadan Başlat** (veya F5 tuşuna basın).

1. Sonuçları bolca ("havuzu") gibi verin ve ardından dikey kaydırma çubuğunu test bir terim girin. Bu, yeni bir sayfa sonuçlarının işlemine neden?

    ![Sonsuz "havuzu" sonuçları arasında kaydırma](./media/tutorial-csharp-create-first-app/azure-search-infinite-scroll.png)

    > [!Tip]
    > Bir kaydırma çubuğu'nın ilk sayfasında görünmesini sağlamak için ilk sayfasını biraz içinde görüntüleniyor alanının yüksekliğini aşması gerekir. Bizim örneğimizde **.box1** 30 piksel cinsinden yüksekliği **.box2** 100 piksel yüksekliği _ve_ 24 piksel bir alt kenar boşluğu. Bu nedenle her giriş 154 piksel kullanır. Üç girdi, 3 x 154 = 462 ' alacağınız piksel. Dikey kaydırma çubuğunun görünmesi, yükseklik görüntüleme alanına ayarlanmalıdır 462 pikselden bile 461 works daha küçük sağlamaktır. Bu sorun yalnızca ilk sayfasında oluşur, daha sonra bir kaydırma çubuğunun görünmesi emin. Güncelleştirilecek satırın:  **&lt;div kimliği "myDiv" style = "width: 800 piksel; Yükseklik: 450px; overflow-y: scroll;" onscroll="scrolled()"&gt;** .

2. Tüm sonuçları alt kısmına kaydırın. Bildirim nasıl bir görünüm sayfasında tüm bilgiler sunulmuştur. Tüm geri dön sunucu çağrıları tetiklemeden gezinebilirsiniz.

Daha karmaşık sonsuz kayan sistemleri, fare tekerleğini kullanıyor olabilir veya benzer sonuçlar yeni bir sayfa yüklenmesini tetiklemek için diğer mekanizması. Biz Bu öğretici, daha fazla tüm kaydırma sonsuz sürüyor değil, ancak ek fare tıklamasına önler ve diğer seçenekleri daha fazla araştırmak isteyebileceğiniz ona belirli bir düğmesine sahip!

## <a name="takeaways"></a>Paketler

Aşağıdaki paketler bu projedeki göz önünde bulundurun:

* Numaralı disk belleği, arama sonuçlarını sıralamasını biraz rastgele, seçeneğidir; yani orada Mayıs iyi olduğu bir sonraki sayfalarında, kullanıcılarınızın ilgisini için uygundur.
* Sonsuz kayan sonuçlarını sıralamasını özellikle önemli olduğu durumlarda geçerlidir. Örneğin bir hedef şehri merkezine uzaklığı üzerinde sıralı sonuçlar.
* Numaralı disk belleği daha iyi bazı gezinme sağlar. Örneğin, bu tür bir kolayca başvurmak sonsuz kayan var ancak bir kullanıcı bir ilgi çekici sonuç 6, sayfada olduğunu hatırlayabileceğiniz.
* Sonsuz kayan tıklayın fussy sayfa numarası yok ile yukarı ve aşağı kaydırma bir kolayca geçirmeye itraz et, sahiptir.
* Sonsuz kayan bir anahtar özelliği, sonuçları verimlidir, sayfa değiştirerek değil var olan bir sayfasına eklenir.
* Geçici depolama için yalnızca bir çağrı devam ediyorsa ve ek çağrıları varlığını sürdürmesi için sıfırlanması gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Disk belleği internet aramaları için temeldir. Kapsanan de disk belleği ile yazarken tamamlanan aramaları ekleyerek kullanıcı deneyimini daha da geliştirmek için sonraki adım olacaktır.

> [!div class="nextstepaction"]
> [C#Öğretici: Otomatik Tamamlama ve öneriler - Azure Search Ekle](tutorial-csharp-type-ahead-and-suggestions.md)
