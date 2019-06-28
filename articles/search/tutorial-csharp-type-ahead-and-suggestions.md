---
title: C#Otomatik Tamamlama ve öneriler - Azure Search Öğreticisi
description: Bu öğreticide, otomatik tamamlama ve öneriler eklemek için "Sayfalandırma - Azure Search arama sonuçları" projesi üzerinde oluşturur. Daha zengin bir kullanıcı deneyimi hedeftir. Satır içi Otomatik Tamamlama önerileri, aşağı açılan listesini birleştirmeyi öğrenin.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 05/01/2019
ms.openlocfilehash: 01c0819fd0bf525739675ad756031cafc1a51673
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67434746"
---
# <a name="c-tutorial-add-autocompletion-and-suggestions---azure-search"></a>C#öğretici: Otomatik Tamamlama ve öneriler - Azure Search Ekle

Otomatik Tamamlama (yazarken tamamlanan ve öneriler) uygulamayı öğrenin kullanıcı başladığında, arama kutusuna yazın. Bu öğreticide, biz yazarken tamamlanan sonuçları ve öneri sonuçları ayrı olarak göster ve daha zengin bir kullanıcı deneyimi oluşturmak için bunları birleştirerek bir yöntemi gösterir. Yalnızca bir kullanıcı, iki veya üç anahtarlar kullanılabilir olan tüm sonuçları bulmak için yazın zorunda kalabilirsiniz. Bu öğreticide oluşturduğunuz sayfalandırma proje üzerine yapılar [ C# Öğreticisi: Azure Search arama sonuçlarını sayfalandırma -](tutorial-csharp-paging.md) öğretici.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Önerileri ekleme
> * Öneriler için vurgulama ekleme
> * Otomatik Tamamlama Ekle
> * Otomatik Tamamlama ve öneriler birleştirin

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

Sahip [ C# Öğreticisi: Azure Search arama sonuçlarını sayfalandırma -](tutorial-csharp-paging.md) proje çalışır. Bu proje, önceki öğreticide, tamamlanan kendi sürümü ya da Github'dan yükleyin: [İlk uygulamanızı oluşturma](https://github.com/Azure-Samples/azure-search-dotnet-samples).

## <a name="add-suggestions"></a>Önerileri ekleme

Kullanıcı alternatifleri'kurmak sunarak, en basit durumu ile başlayalım: öneriler açılan listesi.

1. Index.cshtml dosyasında değiştirmek **TextBoxFor** aşağıdaki deyimi.

    ```cs
     @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautosuggest" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

    Burada arama kutusuna kimliği ayarladığınız anahtardır **azureautosuggest**.

2. Kapatma sonrasında bu deyiminin sonrasındaki  **&lt;/div&gt;** , bu betiği girin.

    ```javascript
    <script>
        $("#azureautosuggest").autocomplete({
            source: "/Home/Suggest?highlights=false&fuzzy=false",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            }
        });
    </script>
    ```

    Biz bu betik ile aynı kimliği arama kutusuna bağladınız Ayrıca, en az iki karakter arama tetiklemek için gereklidir ve diyoruz **Öner** iki sorgu parametreleri ile giriş denetleyicideki eylem: **vurgular** ve **belirsiz**, her ikisi de bu örnekte false olarak ayarlayın.

### <a name="add-references-to-jquery-scripts-to-the-view"></a>Jquery betikleri başvuruları görünümüne ekleyin.

Yukarıdaki komut Otomatik Tamamlama işlevi biz jquery Kitaplığı'nda kullanılabilir olarak kendimize yazmanız gereken bir şey değil. 

1. Jquery kitaplığınıza erişmesine izin değiştirme &lt;baş&gt; görünümü dosyasına aşağıdaki kod bölümü.

    ```cs
    <head>
        <meta charset="utf-8">
        <title>Typeahead</title>
        <link href="https://code.jquery.com/ui/1.12.1/themes/start/jquery-ui.css"
              rel="stylesheet">
        <script src="https://code.jquery.com/jquery-1.10.2.js"></script>
        <script src="https://code.jquery.com/ui/1.12.1/jquery-ui.js"></script>

        <link rel="stylesheet" href="~/css/hotels.css" />
    </head>
    ```

2. Ayrıca, jquery _Layout.cshtml dosyasına başvuran bir satırı açıklama satırı yapın veya kaldırmak ihtiyacımız (içinde **görünümler/paylaşılan** klasörü). Aşağıdaki satırları bulun ve gösterildiği gibi ilk komut satırı açıklama satırı yapın. Bu değişiklik, jquery başvuruları çakışan önler.

    ```html
    <environment include="Development">
        <!-- <script src="~/lib/jquery/dist/jquery.js"></script> -->
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>
    </environment>
    ```

    Önceden tanımlanmış otomatik tamamlama jquery işlevleri artık kullanabiliriz.

### <a name="add-the-suggest-action-to-the-controller"></a>Denetleyiciye Öner Eylem Ekle

1. Giriş denetleyicisi ekleme **Öner** eylem (varsayalım, sonra **sayfa** eylem).

    ```cs
        public async Task<ActionResult> Suggest(bool highlights, bool fuzzy, string term)
        {
            InitSearch();

            // Setup the suggest parameters.
            var parameters = new SuggestParameters()
            {
                UseFuzzyMatching = fuzzy,
                Top = 8,
            };

            if (highlights)
            {
                parameters.HighlightPreTag = "<b>";
                parameters.HighlightPostTag = "</b>";
            }

            // Only one suggester can be specified per index. The name of the suggester is set when the suggester is specified by other API calls.
            // The suggester for the hotel database is called "sg", and simply searches the hotel name.
            DocumentSuggestResult<Hotel> suggestResult = await _indexClient.Documents.SuggestAsync<Hotel>(term, "sg", parameters);

            // Convert the suggest query results to a list that can be displayed in the client.
            List<string> suggestions = suggestResult.Results.Select(x => x.Text).ToList();

            // Return the list of suggestions.
            return new JsonResult(suggestions);
        }
    ```

    **Üst** parametresi, döndürülecek kaç sonuçları belirtir (belirtilmezse, varsayılan değer 5'tir). A _öneri aracı_ veri yedekleme ve Bu öğreticide gibi bir istemci uygulama tarafından değil olarak ayarlandığında yapılır Azure dizininde belirtilir. Bu durumda, öneri aracı "gg" olarak adlandırılır ve aradığı **HotelName** alan - başka bir şey. 

    İzin verir "yakın isabetsiz" benzer öğe eşleştirmesi çıktısında dahil edilecek. Varsa **vurgular** parametrenin ayarlanmış true olarak sonra kalın HTML etiketleri için çıkış eklenir. Sonraki bölümde bu iki parametre true olarak ayarlayacağız.

2. Bazı sözdizimi hatalarıyla karşılaşabilirsiniz. Bu durumda, aşağıdaki iki ekleme **kullanarak** deyimlerini dosyanın üstüne.

    ```cs
    using System.Collections.Generic;
    using System.Linq;
    ```

3. Uygulamayı çalıştırın. Örneğin "ss" girdiğinizde, çeşitli seçenekleri elde ederim? "Pa"'ı şimdi deneyin.

    !["Ss" yazarak iki önerileri gösterir](./media/tutorial-csharp-create-first-app/azure-search-suggest-po.png)

    Harfleri girdiğiniz fark _gerekir_ bir Word'ü başlatın ve yalnızca Word'de dahil.

4. Görünüm komut dosyasında **& belirsiz** true ve uygulamayı yeniden çalıştırın. Artık "ss" girin. Arama, bir harf yanlış aradığınızı varsaydığını unutmayın!
 
    ![TRUE belirsiz kümesiyle "pa" yazma](./media/tutorial-csharp-create-first-app/azure-search-suggest-fuzzy.png)

    İlgileniyorsanız, [Lucene sorgu söz dizimi Azure Search'te](https://docs.microsoft.com/azure/search/query-lucene-syntax) ayrıntılı belirsiz arama dizesinde mantığı açıklar.

## <a name="add-highlighting-to-the-suggestions"></a>Öneriler için vurgulama ekleme

Öneriler kullanıcı görünümünü bir bit ayarlayarak geliştirebiliriz **vurgular** parametresi true. Ancak, ilk kalın metin görüntülenecek görünüm için bazı kod eklemek ihtiyacımız var.

1. Görünümü (Index.cshtml) sonra aşağıdaki betiği ekleyin **azureautosuggest** Yukarıda girilen komut dosyası.

    ```javascript
    <script>
        var updateTextbox = function (event, ui) {
            var result = ui.item.value.replace(/<\/?[^>]+(>|$)/g, "");
            $("#azuresuggesthighlights").val(result);
            return false;
        };

        $("#azuresuggesthighlights").autocomplete({
            html: true,
            source: "/home/suggest?highlights=true&fuzzy=false&",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            },
            select: updateTextbox,
            focus: updateTextbox
        }).data("ui-autocomplete")._renderItem = function (ul, item) {
            return $("<li></li>")
                .data("item.autocomplete", item)
                .append("<a>" + item.label + "</a>")
                .appendTo(ul);
        };
    </script>
    ```

2. Artık şu şekilde okunacak şekilde metin kutusunun Kimliğini değiştirin.

    ```cs
    @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azuresuggesthighlights" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

3. Uygulamayı yeniden çalıştırın ve öneriler de, girilen metni kalın görmeniz gerekir. Örneğin, "pa" yazmayı deneyin.
 
    ![Vurgu "pa" yazma](./media/tutorial-csharp-create-first-app/azure-search-suggest-highlight.png)

4. Yukarıdaki vurgulama betikte kullanılan mantığa kusursuz değil. Aynı adlı iki kez görünür bir terim girin, kalın sonuçları oldukça istediklerinizi değildir. "Ay" yazmayı deneyin.

    Bir geliştirici yanıtlaması gereken soruları biridir, ne zaman olduğunu "da yeterli" çalışan bir betik ve ne zaman kendi quirks gereken olması ele. Biz bu öğreticide daha fazla herhangi vurgulama, ancak kesin bir algoritma vurgulama daha da sürüyor durumunda dikkate alınması gereken bir şeydir bulma sürüyor değil.

## <a name="add-autocompletion"></a>Otomatik Tamamlama Ekle

Önerilerini biraz daha farklı olan başka bir değişim (bazen "yazarken tamamlanan" denir) otomatik tamamlama ' dir. Yine, kullanıcı deneyimini geliştiriyor üzerine geçmeden önce basit uygulama ile başlayacağız.

1. Önceki komut aşağıdaki görünüme aşağıdaki betiği girin.

    ```javascript
    <script>
        $("#azureautocompletebasic").autocomplete({
            source: "/Home/Autocomplete",
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            }
        });
    </script>
    ```

2. Artık şu şekilde okunacak şekilde metin kutusunda, Kimliğini değiştirin.

    ```cs
    @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautocompletebasic" }) <input value="" class="searchBoxSubmit" type="submit">
    ```

3. Girmek ihtiyacımız olan giriş denetleyicisi **otomatik tamamlama** eylem, örneğin aşağıda **Öner** eylem.

    ```cs
        public async Task<ActionResult> AutoComplete(string term)
        {
            InitSearch();

            // Setup the autocomplete parameters.
            var ap = new AutocompleteParameters()
            {
                AutocompleteMode = AutocompleteMode.OneTermWithContext,
                Top = 6
            };
            AutocompleteResult autocompleteResult = await _indexClient.Documents.AutocompleteAsync(term, "sg", ap);

            // Convert the results to a list that can be displayed in the client.
            List<string> autocomplete = autocompleteResult.Results.Select(x => x.Text).ToList();

            // Return the list.
            return new JsonResult(autocomplete);
        }
    ```

    Aynı kullanıyoruz bildirimi *öneri aracı* (yalnızca Otomatik Tamamla otel adları çalışıyoruz şekilde) için öneriler yaptığımız gibi "gg" Otomatik Tamamlama aramanın çağrılan işlev,.

    Bir dizi vardır **AutoCompleteMode özelliği** ayarları ve kullandığınız **OneTermWithContext**. Başvurmak [Azure otomatik tamamlama](https://docs.microsoft.com/rest/api/searchservice/autocomplete) için çeşitli seçenekler burada açıklaması.

4. Uygulamayı çalıştırın. Çeşitli seçenekleri aşağı açılan listede görüntülenen tek sözcük nasıl olduğuna dikkat edin. "Kaldır" ile başlayan sözcükleri yazmayı deneyin. Daha fazla harf yazıldığı gibi birçok seçenek nasıl azalttığını dikkat edin.

    ![Temel Otomatik Tamamlama yazma](./media/tutorial-csharp-create-first-app/azure-search-suggest-autocompletebasic.png)

    Anlaşıldığı gibi daha önce çalıştırdığınız önerileri betik büyük olasılıkla bu otomatik tamamlama betik daha yararlı olur. Otomatik Tamamlama daha kolay hale getirmek için bu en iyi öneri aramayı eklenir.

## <a name="combine-autocompletion-and-suggestions"></a>Otomatik Tamamlama ve öneriler birleştirin

Otomatik Tamamlama ve öneriler birleştirme seçeneklerimiz en karmaşık ve büyük olasılıkla en iyi kullanıcı deneyimi sağlar. İstediğimiz olduğu görünen, satır içi yazılmakta olan metin ile ilk Azure arama için ekliyor metin seçimi. Ayrıca, bir dizi öneri aşağı açılan liste olarak istiyoruz.

Bu işlev, genellikle "satır içi Otomatik Tamamlama" ya da benzer bir adla adlı - teklif kitaplıkları vardır. Neler olup bittiğini görmek için bu özellik, yerel olarak uygulamak için ancak kullanacağız. İş denetleyicisinde bu örnekte ilk kez başlatmak için kullanacağız.

1. Belirtilen sayıda öneri birlikte tek bir otomatik tamamlama sonuç döndüren denetleyiciye bir eylem eklemek ihtiyacımız var. Bu eylem diyoruz **AutocompleteAndSuggest**. Giriş denetleyicisine, diğer yeni eylemlerinizi izleyerek aşağıdaki eylemi ekleyin.

    ```cs
        public async Task<ActionResult> AutocompleteAndSuggest(string term)
        {
            InitSearch();

            // Setup the type-ahead search parameters.
            var ap = new AutocompleteParameters()
            {
                AutocompleteMode = AutocompleteMode.OneTermWithContext,
                Top = 1,
            };
            AutocompleteResult autocompleteResult = await _indexClient.Documents.AutocompleteAsync(term, "sg", ap);

            // Setup the suggest search parameters.
            var sp = new SuggestParameters()
            {
                Top = 8,
            };

            // Only one suggester can be specified per index. The name of the suggester is set when the suggester is specified by other API calls.
            // The suggester for the hotel database is called "sg", and it searches only the hotel name.
            DocumentSuggestResult<Hotel> suggestResult = await _indexClient.Documents.SuggestAsync<Hotel>(term, "sg", sp);

            // Create an empty list.
            var results = new List<string>();

            if (autocompleteResult.Results.Count > 0)
            {
                // Add the top result for type-ahead.
                results.Add(autocompleteResult.Results[0].Text);
            }
            else
            {
                // There were no type-ahead suggestions, so add an empty string.
                results.Add("");
            }
            for (int n = 0; n < suggestResult.Results.Count; n++)
            {
                // Now add the suggestions.
                results.Add(suggestResult.Results[n].Text);
            }

            // Return the list.
            return new JsonResult(results);
        }
    ```

    Bir Otomatik Tamamlama seçeneği en üstündeki döndürülen **sonuçları** listesinde, tüm önerileri tarafından izlenen.

2. Böylece kullanıcı tarafından girilen bolder metnin sağ işlenen bir açık gri otomatik tamamlama sözcük Görünümü'nde, biz öncelikle el uygulayın. HTML, bu amaç için göreli konumunu içerir. Değişiklik **TextBoxFor** deyimi (ve bunun çevresinde &lt;div&gt; deyimleri) aşağıdaki ikinci bir arama kutusu olarak tanımlanan dikkate alınarak **altında** hemen altındaki bizim normal arama kutusuna, bu arama kutusu 39 piksel varsayılan konumunda dışına çekerek!

    ```cs
    <div id="underneath" class="searchBox" style="position: relative; left: 0; top: 0">
    </div>

    <div id="searchinput" class="searchBoxForm" style="position: relative; left: 0; top: -39px">
        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox", @id = "azureautocomplete" }) <input value="" class="searchBoxSubmit" type="submit">
    </div>
    ```

    Kimliği değiştirme biz Not yeniden, **azureautocomplete** böyle bir durumda.

3. Ayrıca Görünümü'nde aşağıdaki betiği şu ana kadar girdiğiniz betikleri tüm girin. İşte bir çok bu kadar kolay.

    ```javascript
    <script>
        $('#azureautocomplete').autocomplete({
            delay: 500,
            minLength: 2,
            position: {
                my: "left top",
                at: "left-23 bottom+10"
            },

            // Use Ajax to set up a "success" function.
            source: function (request, response) {
                var controllerUrl = "/Home/AutoCompleteAndSuggest?term=" + $("#azureautocomplete").val();
                $.ajax({
                    url: controllerUrl,
                    dataType: "json",
                    success: function (data) {
                        if (data && data.length > 0) {

                            // Show the autocomplete suggestion.
                            document.getElementById("underneath").innerHTML = data[0];

                            // Remove the top suggestion as it is used for inline autocomplete.
                            var array = new Array();
                            for (var n = 1; n < data.length; n++) {
                                array[n - 1] = data[n];
                            }

                            // Show the drop-down list of suggestions.
                            response(array);
                        } else {

                            // Nothing is returned, so clear the autocomplete suggestion.
                            document.getElementById("underneath").innerHTML = "";
                        }
                    }
                });
            }
        });

        // Complete on TAB.
        // Clear on ESC.
        // Clear if backspace to less than 2 characters.
        // Clear if any arrow key hit as user is navigating the suggestions.
        $("#azureautocomplete").keydown(function (evt) {

            var suggestedText = document.getElementById("underneath").innerHTML;
            if (evt.keyCode === 9 /* TAB */ && suggestedText.length > 0) {
                $("#azureautocomplete").val(suggestedText);
                return false;
            } else if (evt.keyCode === 27 /* ESC */) {
                document.getElementById("underneath").innerHTML = "";
                $("#azureautocomplete").val("");
            } else if (evt.keyCode === 8 /* Backspace */) {
                if ($("#azureautocomplete").val().length < 2) {
                    document.getElementById("underneath").innerHTML = "";
                }
            } else if (evt.keyCode >= 37 && evt.keyCode <= 40 /* Any arrow key */) {
                document.getElementById("underneath").innerHTML = "";
            }
        });

        // Character replace function.
        function setCharAt(str, index, chr) {
            if (index > str.length - 1) return str;
            return str.substr(0, index) + chr + str.substr(index + 1);
        }

        // This function is needed to clear the "underneath" text when the user clicks on a suggestion, and to
        // correct the case of the autocomplete option when it does not match the case of the user input.
        // The interval function is activated with the input, blur, change, or focus events.
        $("#azureautocomplete").on("input blur change focus", function (e) {

            // Set a 2 second interval duration.
            var intervalDuration = 2000, 
                interval = setInterval(function () {

                    // Compare the autocorrect suggestion with the actual typed string.
                    var inputText = document.getElementById("azureautocomplete").value;
                    var autoText = document.getElementById("underneath").innerHTML;

                    // If the typed string is longer than the suggestion, then clear the suggestion.
                    if (inputText.length > autoText.length) {
                        document.getElementById("underneath").innerHTML = "";
                    } else {

                        // If the strings match, change the case of the suggestion to match the case of the typed input.
                        if (autoText.toLowerCase().startsWith(inputText.toLowerCase())) {
                            for (var n = 0; n < inputText.length; n++) {
                                autoText = setCharAt(autoText, n, inputText[n]);
                            }
                            document.getElementById("underneath").innerHTML = autoText;

                        } else {
                            // The strings do not match, so clear the suggestion.
                            document.getElementById("underneath").innerHTML = "";
                        }
                    }

                    // If the element loses focus, stop the interval checking.
                    if (!$input.is(':focus')) clearInterval(interval);

                }, intervalDuration);
        });
    </script>
    ```

    Akıllı kullanımına dikkat edin **aralığı** artık hangi kullanıcının yazıyor eşleşir ve ("pa" eşleştiğinden "PA", "pA", "Pa" olduğunda da aynı durum (üst veya alt) kullanıcı olarak ayarlanacak yazıyor alttaki metin clear hem işlevi Arama), Kaplanmış metin düzgün olmasını sağlayın.

    Daha kapsamlı anlamak için komut açıklamaları inceleyin.

4. Son olarak, şu iki HTML sınıf saydam hale getirmek için küçük bir ayarlama yapmanız gerekir. Aşağıdaki satırı ekleyin **searchBoxForm** ve **searchBox** hotels.css dosyadaki sınıfları.

    ```html
        background: rgba(0,0,0,0);
    ```

5. Şimdi uygulamayı çalıştırın. Arama kutusuna "pa" girin. Birlikte "pa" içeren iki hotels "palace" Otomatik Tamamlama öneri olarak elde ederim?

    ![Otomatik Tamamlama ve öneriler ile yazma](./media/tutorial-csharp-create-first-app/azure-search-suggest-autocomplete.png)

6. Otomatik Tamamlama öneriyi kabul etmek ve ok tuşları ve SEKME tuşunu kullanarak önerileri seçmeyi deneyin sekme ve fare ve tek tıklamayla kullanarak tekrar deneyin. Betik bu durumlar düzgünce işlediğini doğrulayın.

    Sizin için bu özelliği sunan bir kitaplıkta yüklemek kolaydır, ancak artık satır içi Otomatik Tamamlama işe başlamak için en az bir yol biliyorsunuz karar verebilir!

## <a name="takeaways"></a>Paketler

Aşağıdaki paketler bu projedeki göz önünde bulundurun:

* (Ayrıca olarak "yazarken tamamlanan" bilinen) otomatik tamamlama ve öneriler, tam olarak istediklerini bulmak için yalnızca birkaç anahtarları kullanıcıdan etkinleştirebilirsiniz.
* Otomatik Tamamlama ve birlikte çalışan öneriler zengin bir kullanıcı deneyimi sağlar.
* Her zaman tüm giriş biçimlerini ile otomatik tamamlama işlevleri test edin.
* Kullanarak **setInterval** işlevi kullanıcı Arabirimi öğeleri düzeltme ve doğrulama de yararlı olabilir.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki öğreticide, kullanıcı deneyimini geliştiriyor bir başka yolu göz aramaları tek bir tıklamayla daraltmak için modelleri kullanarak sahibiz.

> [!div class="nextstepaction"]
> [C#Öğretici: Gezinti - Azure Search yardımcı olmak için modelleri kullanma](tutorial-csharp-facets.md)


