---
title: C#sıralama sonuçları - Azure Search ile ilgili öğretici
description: Bu öğreticide, arama sonuçlarını sıralamasını eklemek için "Sayfalandırma - Azure Search arama sonuçları" projesi üzerinde oluşturur. Birincil bir özelliği ve sonuçlar için aynı birincil özelliğine sahip sonuçlarını sıralama öğrenin ikincil bir özellikte sonuçların nasıl. Son olarak, sonuçların bir Puanlama profilini temel alan nasıl öğrenin.
services: search
ms.service: search
ms.topic: tutorial
ms.author: v-pettur
author: PeterTurcan
ms.date: 06/21/2019
ms.openlocfilehash: 32e253b4e131d753ab6937d0aa2a49bda471e091
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466508"
---
# <a name="c-tutorial-order-the-results---azure-search"></a>C#öğretici: Azure Search - sonuçlarını sıralama

Bu noktası öğreticiler gönderinizi serimizin, sonuçlarını döndürdü ve varsayılan sırada görüntülenir. Bu verilerin bulunduğu sırada veya büyük olasılıkla bir varsayılan olabilir _Puanlama profili_ tanımlı doğrulayacak herhangi bir sıralama parametre belirtildiğinde. Bu öğreticide, birincil bir özellikte ve sonuçlar için aynı birincil özelliğine sahip göre sonuçları sıralamak nasıl ele alacağız nasıl ikincil bir özellikte, seçimi. Sayısal değerlerine göre sıralama alternatif olarak, Son örnekte sipariş edileceğini gösterir. özel bir Puanlama profilini temel alan. Ayrıca ekranını biraz daha ayrıntılı ele alacağız _karmaşık türler_.

Döndürülen sonuçlar bir kolayca karşılaştırmak için oluşturulan sonsuz kayan projenin üzerine bu projeyi oluşturur [ C# Öğreticisi: Azure Search arama sonuçlarını sayfalandırma -](tutorial-csharp-paging.md) öğretici.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Bir özelliğe dayalı düzeni sonuçları
> * Birden fazla özelliğe göre düzeni sonuçları
> * Sonuçları bir uzaklık bir coğrafi noktasından göre filtrele
> * Bir Puanlama profilini temel alan düzeni sonuçları

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

Sonsuz kayan sürümüne sahip [ C# Öğreticisi: Azure Search arama sonuçlarını sayfalandırma -](tutorial-csharp-paging.md) proje çalışır. Bu proje kendi sürümü veya Github'dan yükleyin: [İlk uygulamanızı oluşturma](https://github.com/Azure-Samples/azure-search-dotnet-samples).

## <a name="order-results-based-on-one-property"></a>Bir özelliğe dayalı düzeni sonuçları

Biz bir özelliğine göre deyin derecelendirme, otel sonuçlarını sıralama yalnızca bir sıralı sonuçlar istiyoruz, onay sırasını doğru olduğunu da istiyoruz. Diğer bir deyişle, biz derecelendirme sipariş, biz Görünümü'nde derecelendirme görüntülemelidir.

Bu öğreticide ayrıca biraz daha sonuçları, ucuz odası oranı ve en pahalı odası hızı, her otel için görüntülenecek ekleyeceğiz. Ayrıca sıralama, biz ne biz üzerinde sipariş emin olmak için değerleri eklenecektir içine araştırırken Görünümü'nde görüntülenir.

Modeller sıralamayı etkinleştirmek için değişiklik gerek yoktur. Görünüm ve denetleyici gereken güncelleştirildi. Giriş denetleyicisine açarak işleme başlayın.

### <a name="add-the-orderby-property-to-the-search-parameters"></a>Vlastnost OrderBy için arama parametrelerini Ekle

1. Tek bir sayısal özelliğini temel alarak düzeni sonuçları için gereken tek şey ayarlamak için **OrderBy** parametre olarak özellik adıdır. İçinde **dizini (SearchData model)** yöntemi için arama parametrelerini aşağıdaki satırı ekleyin.

    ```cs
        OrderBy = new[] { "Rating desc" },
    ```

    >[!Note]
    > Ekleyebilirsiniz ancak varsayılan sırası artan **asc** bu netleştirmek için özellikte. Azalan belirtilen ekleyerek **desc**.

2. Şimdi uygulamayı çalıştırın ve ortak bir arama terimi girin. Sonuçları olabilir veya birinin diğerinden kullanıcıya değil, geliştirici olarak size doğrulama sonuçları herhangi bir kolayca şekilde olduğu gibi doğru sırayla olabilir!

3. Bunu olalım Temizle sonuçları Derecelendirmeye göre sıralanır. İlk olarak, yerini **Kutu1** ve **kutusu 2** hotels.css dosya sınıfları (tüm yeni vm'lere Bu öğretici için ihtiyacımız olan bu sınıflar) aşağıdaki sınıflar.

    ```html
    textarea.box1A {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        padding-left: 5px;
        text-align: left;
    }

    textarea.box1B {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        text-align: right;
        padding-right: 5px;
    }

    textarea.box2A {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        color: blue;
        padding-left: 5px;
        text-align: left;
    }

    textarea.box2B {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        color: blue;
        text-align: right;
        padding-right: 5px;
    }

    textarea.box3 {
        width: 648px;
        height: 100px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        padding-left: 5px;
        margin-bottom: 24px;
    }
    ```

    >[!Tip]
    >Tarayıcılar genelde css dosyaları önbelleğe ve bu kullanılan eski bir css dosyası ve göz ardı düzenlemeleriniz neden olabilir. Round bu en iyi yolu, bir sürüm parametresi olan bir sorgu dizesi bağlantısını eklemektir. Örneğin:
    >
    >```html
    >   <link rel="stylesheet" href="~/css/hotels.css?v1.1" />
    >```
    >
    >Tarayıcınız tarafından kullanılan eski bir css dosyası düşünüyorsanız, sürüm numarasını güncelleştirin.

4. Ekleme **derecelendirme** özelliğini **seçin** parametresi, **dizini (SearchData model)** yöntemi.

    ```cs
    Select = new[] { "HotelName", "Description", "Rating"},
    ```

5. Görünümü (Index.cshtml) açın ve işleme döngüsü değiştirin ( **&lt;!--otel veri göster.--&gt;** ) aşağıdaki kodla.

    ```cs
                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                    // Display the hotel details.
                    @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                    @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box3" })
                }
    ```

6. Derecelendirme ilk görüntülenen sayfa hem de sonsuz kaydırma çağrılan sonraki sayfalarda kullanılabilir olması gerekir. İkincisi bu iki durumda, her ikisi de güncelleştirmek ihtiyacımız **sonraki** denetleyici, eylem ve **kaydırılan** görünümünde işlevi. Denetleyici ile başlayarak, değiştirme **sonraki** yöntemine aşağıdaki kodu. Bu kod oluşturur ve derecelendirme metin iletişim kurar.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel details to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                var ratingText = $"Rating: {model.resultList.Results[n].Document.Rating}";

                // Add three strings to the list.
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(ratingText);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

7. Şimdi Güncelleştir **kaydırılan** Derecelendirme metni görüntülemek için bu görünümünde işlevi.

    ```javascript
            <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 3) {
                                div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box3">' + data[i + 2] + '</textarea>';
                            }
                        });
                    }
                }
            </script>

    ```

8. Şimdi uygulamayı yeniden çalıştırın. "Wifi" gibi ortak bir terim üzerinde arama ve sonuçlar, otel sıralama, azalan olarak sıralanır doğrulayın.

    ![Derecelendirmesine göre sıralama](./media/tutorial-csharp-create-first-app/azure-search-orders-rating.png)

    Birkaç hotels özdeş bir derecelendirme sahip ve görünümlerini görüntüdeki yeniden verilerin bulunduğu sırada olacak şekilde, isteğe bağlı olduğu fark edeceksiniz.

    İkinci düzeyde bir sıralama ekleme uygulamasına göz atmadan önce odası oranları aralığını görüntülemek için bazı kod ekleyelim. Bu kod içinden veri ayıklama iki gösterilecek ekliyoruz bir _karmaşık tür_, ve ayrıca tartışabiliriz sıralama sonuçlar sağlar. böylece göre fiyat (ucuz ilk belki de).

### <a name="add-the-range-of-room-rates-to-the-view"></a>Oda oranları aralığını görünümüne ekleyin.

1. Hotel.cs modeline ucuz ve en pahalı odası oranı içeren özellikler ekleyin.

    ```cs
        // Room rate range
        public double cheapest { get; set; }
        public double expensive { get; set; }
    ```

2. Sonunda yer ücretleri hesaplamak **dizini (SearchData model)** giriş denetleyicideki eylem. Sonra geçici verileri depolamak, hesaplama ekleyin.

    ```cs
                // Ensure TempData is stored for the next call.
                TempData["page"] = page;
                TempData["searchfor"] = model.searchText;

                // Calculate the room rate ranges.
                for (int n = 0; n < model.resultList.Results.Count; n++)
                {
                    // Calculate room rates.
                    var cheapest = 0d;
                    var expensive = 0d;

                    for (var r = 0; r < model.resultList.Results[n].Document.Rooms.Length; r++)
                    {
                        var rate = model.resultList.Results[n].Document.Rooms[r].BaseRate;
                        if (rate < cheapest || cheapest == 0)
                        {
                            cheapest = (double)rate;
                        }
                        if (rate > expensive)
                        {
                            expensive = (double)rate;
                        }
                    }
                    model.resultList.Results[n].Document.cheapest = cheapest;
                    model.resultList.Results[n].Document.expensive = expensive;
                }
    ```

3. Ekleme **odaları** özelliğini **seçin** parametresinde **dizini (SearchData model)** denetleyici eylem yöntemi.

    ```cs
     Select = new[] { "HotelName", "Description", "Rating", "Rooms" },
    ```

4. İşleme döngü görünümünde oranı aralığının ilk sayfasını görüntülemek için değiştirin.

    ```cs
                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    var rateText = $"Rates from ${Model.resultList.Results[i].Document.cheapest} to ${Model.resultList.Results[i].Document.expensive}";
                    var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                    // Display the hotel details.
                    @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                    @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                    @Html.TextArea($"rates{i}" , rateText, new { @class = "box2A" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box3" })
                }
    ```

5. Değişiklik **sonraki** oranı aralığının sonuçları sonraki sayfa iletişim kurmak için giriş denetleyicisini yöntemi.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel details to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                var ratingText = $"Rating: {model.resultList.Results[n].Document.Rating}";
                var rateText = $"Rates from ${model.resultList.Results[n].Document.cheapest} to ${model.resultList.Results[n].Document.expensive}";

                // Add strings to the list.
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(ratingText);
                nextHotels.Add(rateText);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

6. Güncelleştirme **kaydırılan** oda işlemek için bu görünümünde işlevi metin derecelendirir.

    ```javascript
            <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 4) {
                                div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                            }
                        });
                    }
                }
            </script>
    ```

7. Uygulamayı çalıştırın ve odası oranı aralıkları gösterilmediğini doğrulayın.

    ![Oda oranı aralıkları görüntüleme](./media/tutorial-csharp-create-first-app/azure-search-orders-rooms.png)

**OrderBy** arama parametrelerini özelliği değil kabul edeceği bir giriş gibi **Rooms.BaseRate** odaları zaten (Bu bunlar değil) oranı sıralandığı olsa bile ucuz odası oranını sağlamak için. Otel odası oranına sıralı örnek veri kümesini görüntülemek için giriş denetleyicisine sonuçları sıralamak ve görüntülemek istediğiniz sırada bu sonuçları göndermek gerekir.

## <a name="order-results-based-on-multiple-values"></a>Birden fazla değerlerine göre düzeni sonuçları

Sorunun artık, aynı derecelendirmesi hotels birbirinden ayırt şeklidir. Bir en iyi yolu, otel renovated en son ne zaman göndermemeniz sipariş olacaktır. Diğer bir deyişle, otel daha yakın zamanda renovated, daha yüksek otel sonuçları görüntülenir.

1. İkinci düzeyde bir sıralama eklemek, değiştirmek **OrderBy** ve **seçin** özelliklerinde **dizini (SearchData model)** içerecek şekilde yöntemi  **İn LastRenovationDate** özelliği.

    ```cs
    OrderBy = new[] { "Rating desc", "LastRenovationDate desc" },
    Select = new[] { "HotelName", "Description", "Rating", "Rooms", "LastRenovationDate" },
    ```

    >[!Tip]
    >Herhangi bir sayıda özellikleri girilebilir **OrderBy** listesi. Hotels Yenileme tarihi ve aynı derecelendirme olsaydı, üçüncü bir özellik birbirinden ayırmak için girilen.

2. Yeniden sıralama doğru olduğundan emin olmak için yenileme tarihi görünümünde görmek ihtiyacımız var. Böyle bir şey için bir yenileme, büyük olasılıkla yalnızca yıl gereklidir. İşleme döngü görünümünde aşağıdaki kodla değiştirin.

    ```cs
                <!-- Show the hotel data. -->
                @for (var i = 0; i < Model.resultList.Results.Count; i++)
                {
                    var rateText = $"Rates from ${Model.resultList.Results[i].Document.cheapest} to ${Model.resultList.Results[i].Document.expensive}";
                    var lastRenovatedText = $"Last renovated: { Model.resultList.Results[i].Document.LastRenovationDate.Value.Year}";
                    var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                    // Display the hotel details.
                    @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                    @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                    @Html.TextArea($"rates{i}" , rateText, new { @class = "box2A" })
                    @Html.TextArea($"renovation{i}", lastRenovatedText, new { @class = "box2B" })
                    @Html.TextArea($"desc{i}", Model.resultList.Results[i].Document.Description, new { @class = "box3" })
                }
    ```

3. Değişiklik **sonraki** son Yenileme tarihi yıl bileşenini iletmek için giriş denetleyicisini yöntemi.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel details to the list.
            for (int n = 0; n < model.resultList.Results.Count; n++)
            {
                var ratingText = $"Rating: {model.resultList.Results[n].Document.Rating}";
                var rateText = $"Rates from ${model.resultList.Results[n].Document.cheapest} to ${model.resultList.Results[n].Document.expensive}";
                var lastRenovatedText = $"Last renovated: {model.resultList.Results[n].Document.LastRenovationDate.Value.Year}";

                // Add strings to the list.
                nextHotels.Add(model.resultList.Results[n].Document.HotelName);
                nextHotels.Add(ratingText);
                nextHotels.Add(rateText);
                nextHotels.Add(lastRenovatedText);
                nextHotels.Add(model.resultList.Results[n].Document.Description);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

4. Değişiklik **kaydırılan** görünümünde yenileme metni görüntülemek için işlev.

    ```javascript
            <script>
                function scrolled() {
                    if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                        $.getJSON("/Home/Next", function (data) {
                            var div = document.getElementById('myDiv');

                            // Append the returned data to the current list of hotels.
                            for (var i = 0; i < data.length; i += 5) {
                                div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box2B">' + data[i + 3] + '</textarea>';
                                div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                            }
                        });
                    }
                }
            </script>
    ```

5. Uygulamayı çalıştırın. "Havuzu" veya "Görünüm" gibi genel bir terim üzerinde arama yapın ve aynı derecelendirmesi hotels artık Yenileme tarihi azalan sırada görüntülendiğini doğrulayın.

    ![Yenileme tarihi sıralama](./media/tutorial-csharp-create-first-app/azure-search-orders-renovation.png)

## <a name="filter-results-based-on-a-distance-from-a-geographical-point"></a>Sonuçları bir uzaklık bir coğrafi noktasından göre filtrele

Derecelendirme ve Yenileme tarihi en iyi şekilde azalan düzende görüntülenen özellikler örnekleridir. Bir alfabetik liste artan düzende iyi bir kullanım örneği olacaktır (örneğin, varsa yalnızca **OrderBy** özellik ve ayarlandığı **HotelName** bir alfabetik görüntülenmekteydi sonra ). Ancak, bizim örnek veriler için bir coğrafi noktasından uzaklığı daha uygun olacaktır.

Coğrafi uzaklıktan üzerinde göre sonuçları görüntülemek için birkaç adım gereklidir.

1. Boylam, enlem ve RADIUS parametreleri olan bir filtre girerek belirli bir noktadan belirtilen bir yarıçap dışında tüm hotels filtreleyin. Boylam ilk noktası işleve verilir. RADIUS kilometre ' dir.

    ```cs
        // "Location" must match the field name in the Hotel class.
        // Distance (the radius) is in kilometers.
        // Point order is Longitude then Latitude.
        Filter = $"geo.distance(Location, geography'POINT({model.lon} {model.lat})') le {model.radius}",
    ```

2. Yukarıdaki filtresinden _değil_ hakkında daha fazla mesafe göre sonuçları sipariş, yalnızca aykırı değerleri kaldırır. Sonuçları sıralamak için girin bir **OrderBy** ayarı geoDistance yöntemi belirtir.

    ```cs
    OrderBy = new[] { $"geo.distance(Location, geography'POINT({model.lon} {model.lat})') asc" },
    ```

3. Uzaklık filtresini kullanarak Azure Search tarafından döndürülen sonuçları olsa da, hesaplanan uzaklıktır veriler ile belirtilen nokta arasında _değil_ döndürdü. Sonuçları görüntülemek istiyorsanız bu değeri görünümünde veya denetleyicisi yeniden hesaplayın.

    Aşağıdaki kod, iki lat/lon nokta arasındaki uzaklığı hesaplar.

    ```cs
        const double EarthRadius = 6371;

        public static double Degrees2Radians(double deg)
        {
            return deg * Math.PI / 180;
        }

        public static double DistanceInKm( double lat1,  double lon1, double lat2, double lon2)
        {
            double dlon = Degrees2Radians(lon2 - lon1);
            double dlat = Degrees2Radians(lat2 - lat1);

            double a = (Math.Sin(dlat / 2) * Math.Sin(dlat / 2)) + Math.Cos(Degrees2Radians(lat1)) * Math.Cos(Degrees2Radians(lat2)) * (Math.Sin(dlon / 2) * Math.Sin(dlon / 2));
            double angle = 2 * Math.Atan2(Math.Sqrt(a), Math.Sqrt(1 - a));
            return angle * EarthRadius;
        }
    ```

4. Şimdi, bu kavramları pekiştirip gerekir. Öğreticimize oluşturmaya gideceğini kadar harita tabanlı bir uygulama için okuyucu bir alıştırma olarak kalır ancak bu kod parçacıkları verilmiştir. Bu örnek daha fazla yararlanmak için ya da bir RADIUS ile bir şehir adı girerek veya bir nokta bir harita üzerindeki bulma ve bir RADIUS seçerek göz önünde bulundurun. Bu seçenekler daha fazla araştırmak için aşağıdaki kaynaklara bakın:

* [Azure Haritalar Belgeleri](https://docs.microsoft.com/azure/azure-maps/)
* [Azure haritalar arama hizmetini kullanarak bir adres bulma](https://docs.microsoft.com/azure/azure-maps/how-to-search-for-address)

## <a name="order-results-based-on-a-scoring-profile"></a>Bir Puanlama profilini temel alan düzeni sonuçları

Öğreticide şu ana kadar verilen sayısal (, derecelendirme Yenileme tarihi, coğrafi uzaklıktan), sağlama değerlerini sıralamak nasıl örnekler bir _tam_ sipariş işleme. Ancak, bazı arama ve bazı veriler kendilerini iki veri öğeleri arasındaki bu tür bir kolayca karşılaştırma ödünç vermek değil. Azure Search'ü içerir kavramını _Puanlama_. _Puanlama profilleri_ ne zaman, örneğin olacağı karar vermek için metin tabanlı verileri karşılaştırma ilk görüntülenen en değerli olmalıdır daha karmaşık ve nitel karşılaştırmalar, sağlamak için kullanılan veri kümesi için belirtilebilir.

Puanlama profilleri kullanıcılar tarafından ancak genellikle bir veri kümesinin yöneticiler tarafından tanımlı değil. Birkaç Puanlama profilleri, Oteller veriler üzerinde ayarlanan. Şimdi bir Puanlama profili nasıl tanımlandığını arayın ve arama için kod yazma deneyin.

### <a name="how-scoring-profiles-are-defined"></a>Tanımlanmış nasıl Puanlama profilleri

Şimdi Puanlama profillerini üç örneklere göz atacağız ve nasıl her göz önünde bulundurun _gereken_ sonuçları sırasını etkiler. Bir uygulama geliştiricisi olarak, bu profillerin yazma, veri Yöneticisi tarafından yazılan, ancak sözdizimine göz yardımcı olur.

1. Puanlama profili hotels herhangi belirtmediğinizde kullanılan için veri kümesi, varsayılan **OrderBy** veya **ScoringProfile** parametresi. Bu profili artırıyor _puanı_ arama metnini otel adı, açıklama veya etiketleri (s) bir listesi varsa, bir otel için. Puanlamaların ağırlıkları belirli alanların nasıl favor dikkat edin. Arama metni başka bir alanda aşağıda listelenen değil ağırlık olarak 1 olacaktır görünüyorsa. Kuşkusuz, daha yüksek puan önceki bir sonuç görünümünde görüntülenir.

     ```cs
    {
            "name": "boostByField",
            "text": {
                "weights": {
                    "HotelName": 2,
                    "Description": 1.5,
                    "Description_fr": 1.5,
                    "Tags": 3
                }
            }
        }

    ```

2. Sağlanan parametre bir veya daha fazla (Bu, "s" çağıran) etiketlerinin listesini içeriyorsa, aşağıdaki Puanlama profili puanı önemli ölçüde artırır. Bu profilin Buradaki önemli nokta, bir parametredir _gerekir_ sağlanması, metni içeren. Parametre boş ya da sağlanmazsa, bir hata oluşturulur.
 
    ```cs
            {
            "name": "boostAmenities",
            "functions": [
                {
                    "type": "tag",
                    "fieldName": "Tags",
                    "boost": 5,
                    "tag": {
                        "tagsParameter": "amenities"
                    }
                }
            ]
        }
    ```

3. Üçüncü Bu örnekte, derecelendirme önemli boost puanına sağlar. Son renovated tarihi, geçerli tarihi yalnızca verilerin 730 gün içinde (2 yıl) kalırsa ancak puanı da artırır.

    ```cs
            {
            "name": "renovatedAndHighlyRated",
            "functions": [
                {
                    "type": "magnitude",
                    "fieldName": "Rating",
                    "boost": 20,
                    "interpolation": "linear",
                    "magnitude": {
                        "boostingRangeStart": 0,
                        "boostingRangeEnd": 5,
                        "constantBoostBeyondRange": false
                    }
                },
                {
                    "type": "freshness",
                    "fieldName": "LastRenovationDate",
                    "boost": 10,
                    "interpolation": "quadratic",
                    "freshness": {
                        "boostingDuration": "P730D"
                    }
                }
            ]
        }

    ```

    Şimdi, bu profillerin yapmaları gerektiğini düşündüğünüz çalışıyorsanız bize görün!

### <a name="add-code-to-the-view-to-compare-profiles"></a>Kod görünümüne profilleri Karşılaştırılacak ekleme

1. Index.cshtml dosyasını açın ve Değiştir &lt;gövdesi&gt; bölümü aşağıdaki kod ile.

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
                        Hotels Search - Order Results
                    </h1>
                </td>
            </tr>
            <tr>
                <td></td>
                <td>
                    <!-- Display the search text box, with the search icon to the right of it. -->
                    <div class="searchBoxForm">
                        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input class="searchBoxSubmit" type="submit" value="">
                    </div>

                    <div class="searchBoxForm">
                        <b>&nbsp;Order:&nbsp;</b>
                        @Html.RadioButtonFor(m => m.scoring, "Default") Default&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "RatingRenovation") By numerical Rating&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "boostAmenities") By Amenities&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "renovatedAndHighlyRated") By Renovated date/Rating profile&nbsp;&nbsp;
                    </div>
                </td>
            </tr>

            <tr>
                <td valign="top">
                    <div id="facetplace" class="facetchecks">

                        @if (Model != null && Model.facetText != null)
                        {
                            <h5 class="facetheader">Amenities:</h5>
                            <ul class="facetlist">
                                @for (var c = 0; c < Model.facetText.Length; c++)
                                {
                                    <li> @Html.CheckBoxFor(m => m.facetOn[c], new { @id = "check" + c.ToString() }) @Model.facetText[c] </li>
                                }

                            </ul>
                        }
                    </div>
                </td>
                <td>
                    @if (Model != null && Model.resultList != null)
                    {
                        // Show the total result count.
                        <p class="sampleText">
                            @Html.DisplayFor(m => m.resultList.Count) Results <br />
                        </p>

                        <div id="myDiv" style="width: 800px; height: 450px; overflow-y: scroll;" onscroll="scrolled()">

                            <!-- Show the hotel data. -->
                            @for (var i = 0; i < Model.resultList.Results.Count; i++)
                            {
                                var rateText = $"Rates from ${Model.resultList.Results[i].Document.cheapest} to ${Model.resultList.Results[i].Document.expensive}";
                                var lastRenovatedText = $"Last renovated: { Model.resultList.Results[i].Document.LastRenovationDate.Value.Year}";
                                var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                                string amenities = string.Join(", ", Model.resultList.Results[i].Document.Tags);
                                string fullDescription = Model.resultList.Results[i].Document.Description;
                                fullDescription += $"\nAmenities: {amenities}";

                                // Display the hotel details.
                                @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                                @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                                @Html.TextArea($"rates{i}", rateText, new { @class = "box2A" })
                                @Html.TextArea($"renovation{i}", lastRenovatedText, new { @class = "box2B" })
                                @Html.TextArea($"desc{i}", fullDescription, new { @class = "box3" })
                            }
                        </div>

                        <script>
                            function scrolled() {
                                if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                                    $.getJSON("/Home/Next", function (data) {
                                        var div = document.getElementById('myDiv');

                                        // Append the returned data to the current list of hotels.
                                        for (var i = 0; i < data.length; i += 5) {
                                            div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                            div.innerHTML += '<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                            div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                                            div.innerHTML += '<textarea class="box2B">' + data[i + 3] + '</textarea>';
                                            div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                                        }
                                    });
                                }
                            }
                        </script>
                    }
                </td>
            </tr>
        </table>
    }
    </body>
    ```

2. SearchData.cs dosyasını açın ve Değiştir **SearchData** aşağıdaki kodla sınıfı.

    ```cs
    public class SearchData
    {
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

        // Array to hold the text for each amenity.
        public string[] facetText { get; set; }

        // Array to hold the setting for each amenitity.
        public bool[] facetOn { get; set; }

        // The text to search for.
        public string searchText { get; set; }

        // Record if the next page is requested.
        public string paging { get; set; }

        // The list of results.
        public DocumentSearchResult<Hotel> resultList;

        public string scoring { get; set; }       
    }
    ```

3. Hotels.css dosyasını açın ve aşağıdaki HTML sınıfları ekleyin.

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

### <a name="add-code-to-the-controller-to-specify-a-scoring-profile"></a>Denetleyiciye bir Puanlama profilini belirtmek için kod ekleyin

1. Giriş denetleyicisine dosyasını açın. Aşağıdaki **kullanarak** deyimi (listeleri oluşturmaya yardımcı olmak için).

    ```cs
    using System.Linq;
    ```

2.  Bu örnekte, ilk çağrı ihtiyacımız **dizin** başlangıç görünümü biraz daha fazlasını döndürmek için. Yöntemi artık görüntülemek en fazla 20 kullanılmıyordu arar.

    ```cs
        public async Task<ActionResult> Index()
        {
            InitSearch();

            // Set up the facets call in the search parameters.
            SearchParameters sp = new SearchParameters()
            {
                // Search for up to 20 amenities.
                Facets = new List<string> { "Tags,count:20" },
            };

            DocumentSearchResult<Hotel> searchResult = await _indexClient.Documents.SearchAsync<Hotel>("*", sp);

            // Convert the results to a list that can be displayed in the client.
            List<string> facets = searchResult.Facets["Tags"].Select(x => x.Value.ToString()).ToList();

            // Initiate a model with a list of facets for the first view.
            SearchData model = new SearchData(facets);

            // Save the facet text for the next view.
            SaveFacets(model, false);

            // Render the view including the facets.
            return View(model);
        }
    ```

3. Modelleri geçici depolama alanına kaydedin ve geçici depolama alanından kurtarmayı ve bir modeli doldurmak için iki özel yöntem ihtiyacımız var.

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

4. Ayarlanacak ihtiyacımız **OrderBy** ve **ScoringProfile** parametrelerini gerektiği şekilde. Varolan **dizini (SearchData model)** aşağıdaki yöntemi.

    ```cs
        public async Task<ActionResult> Index(SearchData model)
        {
            try
            {
                InitSearch();

                int page;

                if (model.paging != null && model.paging == "next")
                {
                    // Recover the facet text, and the facet check box settings.
                    RecoverFacets(model, true);

                    // Increment the page.
                    page = (int)TempData["page"] + 1;

                    // Recover the search text.
                    model.searchText = TempData["searchfor"].ToString();
                }
                else
                {
                    // First search with text. 
                    // Recover the facet text, but ignore the check box settings, and use the current model settings.
                    RecoverFacets(model,false);

                    // First call. Check for valid text input, and valid scoring profile.
                    if (model.searchText == null)
                    {
                        model.searchText = "";
                    }
                    if (model.scoring == null)
                    {
                        model.scoring = "Default";
                    }
                    page = 0;
                }

                // Set empty defaults for ordering and scoring parameters.
                var orderby = new List<string>();
                string profile = "";
                var scoringParams = new List<ScoringParameter>();

                // Set the ordering based on the user's radio button selection.
                switch (model.scoring)
                {
                    case "RatingRenovation":
                        orderby.Add("Rating desc");
                        orderby.Add("LastRenovationDate desc");
                        break;

                    case "boostAmenities":
                        {
                            profile = model.scoring;
                            var setAmenities = new List<string>();

                            // Create a string list of amenities that have been clicked.
                            for (int a = 0; a < model.facetOn.Length; a++)
                            {
                                if (model.facetOn[a])
                                {
                                    setAmenities.Add(model.facetText[a]);
                                }
                            }
                            if (setAmenities.Count > 0)
                            {
                                // Only set scoring parameters if there are any.
                                var sp = new ScoringParameter("amenities", setAmenities);
                                scoringParams.Add(sp);
                            }
                            else
                            {
                                // No amenities selected, so set profile back to default.
                                profile = "";
                            }
                        }
                        break;

                    case "renovatedAndHighlyRated":
                        profile = model.scoring;
                        break;

                    default:
                        break;
                }

                // Setup the search parameters.
                var parameters = new SearchParameters
                {
                    // Set the ordering/scoring parameters.
                    OrderBy = orderby,
                    ScoringProfile = profile,
                    ScoringParameters = scoringParams,

                    // Select the data properties to be returned.
                    Select = new[] { "HotelName", "Description", "Tags", "Rooms", "Rating", "LastRenovationDate" },
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
                TempData["scoring"] = model.scoring;
                SaveFacets(model,true);

                // Calculate the room rate ranges.
                for (int n = 0; n < model.resultList.Results.Count; n++)
                {
                    var cheapest = 0d;
                    var expensive = 0d;

                    for (var r = 0; r < model.resultList.Results[n].Document.Rooms.Length; r++)
                    {
                        var rate = model.resultList.Results[n].Document.Rooms[r].BaseRate;
                        if (rate < cheapest || cheapest == 0)
                        {
                            cheapest = (double)rate;
                        }
                        if (rate > expensive)
                        {
                            expensive = (double)rate;
                        }
                    }
                    model.resultList.Results[n].Document.cheapest = cheapest;
                    model.resultList.Results[n].Document.expensive = expensive;
                }
            }
            catch
            {
                return View("Error", new ErrorViewModel { RequestId = "1" });
            }
            return View("Index", model);
        }
    ```

    Her biri için yorumları okuyun **geçiş** seçimleri.

5. Biz değişiklik gerekmez **sonraki** önceki bölümde üzerinde birden fazla özelliğe göre sıralama için ek kod tamamladıysanız eylem.

### <a name="run-and-test-the-app"></a>Çalıştırın ve uygulamayı test etme

1. Uygulamayı çalıştırın. Bir kümesini kullanılmıyordu görünümünde görmeniz gerekir.

2. Sıralama için "sayısal derecelendirmesine göre" seçerek size arasında eşit derecelendirme, Oteller karar Yenileme tarihi olan Bu öğreticide zaten uyguladıysanız sayısal sıralama sunar.

!["Derecelendirmesine göre Plaj" sıralama](./media/tutorial-csharp-create-first-app/azure-search-orders-beach.png)

3. "Tarafından kullanılmıyordu" profile'ı şimdi deneyin. Kullanılmıyordu, çeşitli seçimleri yapın ve bu kullanılmıyordu ile otel sonuç listesini yükseltilir doğrulayın.

!["Plaj profilini temel alan" sıralama](./media/tutorial-csharp-create-first-app/azure-search-orders-beach-profile.png)

4. Deneyin "tarafından profil tarih/derecelendirme Renovated" beklediğiniz alıp alamayacağınızı öğrenin. Yalnızca son renovated hotels almanız gerekir bir _güncellik_ artırın.

### <a name="resources"></a>Kaynaklar

Daha fazla bilgi için aşağıdakilere bakın [Azure Search dizini için Puanlama profilleri ekleme](https://docs.microsoft.com/azure/search/index-add-scoring-profiles).

## <a name="takeaways"></a>Paketler

Aşağıdaki paketler bu projedeki göz önünde bulundurun:

* Kullanıcıların arama sonuçları, ilk ilgilendiren sıralanmalıdır beklediğiniz.
* Böylece sıralama ihtiyaçlarını yapılandırılmış verilerin kolaydır. Veri ek kod yapılması sıralamayı etkinleştirmek için yapılandırılmamış gibi "üzerinde ucuz" kolayca sıralamanız açamadık.
* Birçok sıralama, sıralama, daha yüksek bir düzeyde aynı değere sahip sonuçları birbirinden ayırmak için düzey olabilir.
* Bazı sonuçlar sırası (örneğin, uzaktaki bir noktaya uzaklık) ve bazı da azalan artan düzende sıralanmalıdır için doğal (örneğin, Konuk derecelendirme).
* Sayısal karşılaştırma kullanılabilir değil ya da bir veri kümesi için yeteri kadar akıllı değildir Puanlama profilleri tanımlanabilir. Her sonuç Puanlama sıralamak için Yardım ve akıllı bir şekilde sonuçları görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu dizi tamamladınız C# - öğreticiler, kazanılan Azure arama API'lerinin değerli bilgi.

Daha fazla başvuru ve öğreticiler için gözatma göz önünde bulundurun [Microsoft Learn](https://docs.microsoft.com/learn/browse/?products=azure), ya da diğer öğreticileri, [Azure arama belgeleri](https://docs.microsoft.com/azure/search/).
