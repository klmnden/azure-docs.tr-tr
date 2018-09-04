---
title: Power BI ile Metin Analizi - Azure Bilişsel Hizmetler | Microsoft Docs
description: Power BI'da depolanan metindeki anahtar ifadeleri ayıklamak için Metin Analizi'ni nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: luiscabrer
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: tutorial
ms.date: 3/07/2018
ms.author: luisca
ms.openlocfilehash: 2cdb93d44218627efdcb0360d8cf4a4eeeca177a
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "42889295"
---
# <a name="text-analytics-with-power-bi"></a>Power BI ile Metin Analizi

Microsoft Power BI, kuruluş verilerini, daha hızlı ve daha ayrıntılı içgörüler için kuruluşunuzda dağıtabileceğiniz etkileyici raporlar haline getirir. Microsoft Azure'daki Bilişsel Hizmetler'in parçası olan Metin Analizi hizmeti, bünyesindeki Anahtar İfade Ayıklama API'si sayesinde en önemli ifadeleri ayıklayabilir. Birlikte bu araçlar, müşterilerinizin ne hakkında konuştuğunu ve bu konuda nasıl hissettiğini hızlıca görmenize yardımcı olabilir.

Bu öğreticide, özel bir Power Query işlevi kullanarak müşteri geri bildirimindeki en önemli ifadeleri ayıklamak için Power BI Desktop ile Anahtar İfade Ayıklama API'sini nasıl tümleştirebileceğinizi görebilirsiniz. Ayrıca bu ifadelerden oluşan bir Kelime Bulutu da oluşturacağız.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi gerçekleştirmek için şunlar gerekir:

> [!div class="checklist"]
> * Microsoft Power BI Desktop. [Ücretsiz olarak indirin](https://powerbi.microsoft.com/get-started/).
> * Bir Microsoft Azure hesabı. [Ücretsiz bir deneme başlatın](https://azure.microsoft.com/free/) veya [oturum açın](https://portal.azure.com/).
> * Metin Analizi için erişim anahtarı. [Kaydolun](../../cognitive-services-apis-create-account.md) ve ardından, [anahtarınızı edinin](../how-tos/text-analytics-how-to-access-key.md).
> * Müşteri yorumları. [Örnek verilerimizden yararlanın](https://aka.ms/cogsvc/ta) veya kendi verilerinizi kullanın.

## <a name="loading-customer-data"></a>Müşteri verilerini yükleme

Başlamak için Power BI Desktop'ı açın ve **FabrikamComments.csv** adlı Virgülle Ayrılmış Değer (CSV) dosyasını yükleyin. Bu dosyada, kurgusal olarak oluşturulmuş küçük bir şirketin destek forumundaki bir günlük varsayımsal etkinlik yer almaktadır.

> [!NOTE]
> Power BI ile Facebook veya bir SQL veritabanı gibi çok çeşitli kaynaklarda yer alan veriler kullanılabilir. [Power BI ile Facebook tümleştirmesi](https://powerbi.microsoft.com/integrations/facebook/) ve [Power BI ile SQL Server tümleştirmesi](https://powerbi.microsoft.com/integrations/sql-server/) başlıklı makalelerde daha fazla bilgiye ulaşın.

Ana Power BI Desktop penceresinin Giriş şeridindeki Dış Veri grubunu bulun. **Veri Al** açılan menüsündeki bu grupta yer alan **Metin/CSV** seçeneğini belirleyin.

![[Veri Al düğmesi]](../media/tutorials/power-bi/get-data-button.png)

Aç iletişim kutusu görünür. İndirmeler klasörünüze (veya örnek veri dosyanızın bulunduğu klasöre) gidin. `FabrikamComments.csv` dosyasına ve ardından, **Aç** düğmesine tıklayın. CSV içeri aktarımı iletişim kutusu görünür.

![[CSV İçeri Aktarımı iletişim kutusu]](../media/tutorials/power-bi/csv-import.png)

CSV içeri aktarımı iletişim kutusu, Power BI Desktop'ın karakter kümesini, sınırlayıcıyı, üst bilgi satırlarını ve sütun türlerini doğru bir şekilde algıladığını doğrulamanıza olanak sağlar. Tüm bilgiler doğru olduğuna göre **Yükle**'ye tıklarız.

Yüklenen verilere görmek için, Power BI çalışma alanının sol kenarındaki Veri Görünümü düğmesini kullanarak Veri görünümüne geçin. Microsoft Excel'de olduğu gibi verilerimizi içeren bir tablo açılır.

![[İçeri aktarılan verilerin başlangıç görünümü]](../media/tutorials/power-bi/initial-data-view.png)

## <a name="preparing-the-data"></a>Verileri hazırlama

Anahtar İfade Ayıklama hizmeti tarafından işlenmeye hazır hale gelebilmesi için verilerinizi Power BI Desktop'ta dönüştürmeniz gerekebilir.

Örneğin, verilerimizde hem `subject` hem de `comment` alanı bulunuyor. Anahtar ifadeleri ayıklarken yalnızca `comment` alanını değil, söz konusu alanların her ikisini de dikkate almamız gerekir. Power BI Desktop'taki Sütunları Birleştir işlevi bu görevi kolay hale getirir.

Giriş şeridindeki Dış Veri gurubunda **Sorguları Düzenle** seçeneğine tıklayarak ana Power BI Desktop penceresinde Sorgu Düzenleyicisi'ni açın. 

![[Giriş şeridindeki Dış Veri grubu]](../media/tutorials/power-bi/edit-queries.png)

Henüz seçili değilse, pencerenin sol tarafındaki Sorgular listesinde "FabrikamComments" seçeneğini belirleyin.

Şimdi de tablodaki `subject` ve `comment` sütunlarının her ikisini de seçin. Bu sütunları görmek için yatay olarak kaydırma yapmanız gerekebilir. Öncelikle, `subject` sütun üst bilgisine tıklayın, ardından, Control tuşunu basılı tutarak `comment` sütun üst bilgisine tıklayın.

![[Birleştirilecek alanları seçme]](../media/tutorials/power-bi/select-columns.png)

Dönüştür şeridinin Metin Sütunları grubunda, **Sütunları Birleştir** seçeneğine tıklayın. Sütunları Birleştir iletişim kutusu görünür.

![[Sütunları Birleştir iletişim kutusunu kullanarak alanları birleştirme]](../media/tutorials/power-bi/merge-columns.png)

Sütunları Birleştir iletişim kutusunda, ayırıcı olarak Sekme seçeneğini belirleyip **Tamam**'a tıklayın. Bitti!

Boş Olanı Kaldır filtresinden yararlanarak veya yazdırılamayan karakterleri Temizle dönüşümü ile kaldırarak boş iletileri de filtreleyebilirsiniz. Verileriniz örnek dosyamızdaki `spamscore` sütunu gibi bir sütun içeriyorsa Sayı Filtresi kullanarak "istenmeyen" yorumları atlayabilirsiniz.

## <a name="understanding-the-api"></a>API'yi anlama

Anahtar İfade Ayıklama API'si, HTTP isteği başına bine kadar metin belgesini işleyebilir. Ancak Power BI'da kayıtlar tek tek işlenir. Bu nedenle, API'ye yönelik çağrılarımız yalnızca tek bir belge içerir. [API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6), işlenen her belge için aşağıdaki alanları gerektirir.

| | |
| - | - |
| `id`  | İstekte bu belge için yer alan benzersiz tanımlayıcı. Yanıtta da bu alan bulunur. Böylece, birden fazla belge işlemeniz halinde, ayıklanan anahtar ifadeleri bu ifadelerin kaynağı olan belge ile kolayca eşleyebilirsiniz. İstek başına tek bir belge işlediğimizden, yanıtın hangi belge ile ilişkili olduğunu zaten biliyoruz ve `id`, her bir istekte aynı olabilir.|
| `text`  | İşlenecek metin. Oluşturduğumuz, birleştirilen konu satırı ve yorum metnini içeren `Merged` sütunundan elde ederiz. Anahtar İfade Ayıklama API'si bu verilerin 5.000 karakter uzunluğunu aşmamasını gerektirir.|
| `language` | Belgenin yazıldığı dili temsil eden kod. Tüm iletilerimiz İngilizce dilindedir; bu nedenle, `en` dilini sorgumuza doğrudan yazabiliriz.|

Anahtar İfade Ayıklama API'sine göndermek için söz konusu alanları bir JSON (JavaScript Nesne Gösterimi) belgesi oluşturacak şekilde birleştirmemiz gerekir. Bu isteğe ilişkin yanıt da bir JSON belgesidir ve anahtar ifadeleri bulmak için söz konusu yanıtı ayrıştırmamız gerekir.

## <a name="creating-a-custom-function"></a>Özel bir işlev oluşturma

Artık Power BI ve Metin Analizi'ni tümleştirecek özel işlevi oluşturmaya hazırız. Power BI Desktop tablodaki her bir satır için işlevi çağırır ve sonuçları içeren yeni bir sütun oluşturur.

İşlev, işlenecek metni bir parametre olarak alır. Gerekli JavaScript Nesne Gösterimi'ne (JSON) gönderilen ve buradan alınan verileri dönüştürür ve Anahtar İfade Ayıklama API'si uç noktasına yönelik HTTP isteğini gerçekleştirir. Yanıt ayrıştırıldıktan sonra işlev, ayıklanan anahtar ifadelerin virgülle ayrılmış listesini içeren bir dize döndürür.

> [!NOTE]
> Power BI Desktop özel işlevleri [Power Query M formül dilinde](https://msdn.microsoft.com/library/mt211003.aspx) (veya kısaca "M") yazılır. M, [F#](https://docs.microsoft.com/dotnet/fsharp/) temelindeki işlevsel bir programlama dilidir. Bu öğreticiyi tamamlamanız için programcı olmanız gerekmese de gerekli kod aşağıda verilmiştir.

Hâlâ Sorgu Düzenleyicisi penceresinde olmanız gerekir. Giriş şeridinde, **Yeni Kaynak**'a (Yeni Sorgu grubunda) tıklayın ve açılan menüde **Boş Sorgu** seçeneğini belirleyin. 

Sorgular listesinde, başlangıçta Sorgu1 olarak adlandırılan yeni bir sorgu görünür. Söz konusu girişe çift tıklayın ve girişi `KeyPhrases` olarak adlandırın.

Şimdi de Giriş şeridindeki Sorgu grubunda yer alan **Gelişmiş Düzenleyici**'ye tıklayarak Gelişmiş Düzenleyici penceresini açın. Pencerede bulunan kodu silin ve aşağıdaki kodu yapıştırın. 

> [!NOTE]
> Aşağıdaki örneklerde uç noktanın https://westus.api.cognitive.microsoft.com adresinde olduğu varsayılmıştır.  Metin Analizi, 13 farklı bölgede abonelik oluşturmayı destekler. Hizmete farklı bir bölgede kaydolduysanız lütfen seçtiğiniz bölgeye yönelik uç noktayı kullandığınızdan emin olun. Bu bilginin, Metin Analizi aboneliğinizi seçtiğinizde Azure portal'daki Genel Bakış sayfasında gösterilmesi gerekir.

```fsharp
// Returns key phrases from the text in a comma-separated list
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = Text.Lower(Text.Combine(jsonresp[documents]{0}[keyPhrases], ", "))
in  keyphrases
```

Ayrıca Microsoft Azure panosundan elde ettiğiniz Metin Analizi API'si anahtarını da `apikey` ile başlayan satıra yapıştırın. (Anahtarın önündeki ve arkasındaki tırnak işaretlerini kaldırmadığınızdan emin olun.) Ardından, **Bitti** seçeneğine tıklayın.

## <a name="using-the-function"></a>İşlevi kullanma

Artık müşteri yorumlarımızın her birinde yer alan anahtar ifadeleri edinmek ve tablodaki yeni bir sütunda depolamak için özel işlevi kullanabiliriz. 

Sorgu Düzenleyicisi'nden ayrılmayıp FabrikamComments sorgusuna geri dönün, Sütun Ekle şeridine geçin ve Genel grubundaki **Özel İşlev Çağır** düğmesine tıklayın.

![[Özel İşlev Çağır düğmesi]](../media/tutorials/power-bi/invoke-custom-function-button.png)<br><br>

Özel işlev Çağır iletişim kutusunda, yeni sütunun adı olarak `keyphrases` değerini girin. İşlev Sorgusu olarak özel işlevimiz olan `KeyPhrases` seçeneğini belirleyin. 

İletişim kutusunda, `text` parametresi olarak işlevimize hangi alanı geçirmek istediğimizin sorulduğu yeni bir alan görünür. Açılan menüden `Merged` (daha önce konu ve ileti alanlarını birleştirerek oluşturduğumuz sütun) seçeneğini belirleyin.

![[Özel işlev çağırma]](../media/tutorials/power-bi/invoke-custom-function.png)

Son olarak, **Tamam**'a tıklayın.

Her şey hazırsa Power BI, Anahtar İfade Ayıklama sorguları gerçekleştirip yeni sütunu tabloya ekleyerek tablomuzdaki her bir satır için işlevimizi bir kez çağırır. Ancak bundan önce kimlik doğrulaması ve gizlilik ayarlarını belirtmeniz gerekebilir.

## <a name="authentication-and-privacy"></a>Kimlik doğrulaması gizlilik

Özel İşlev Çağır iletişim kutusunu kapattıktan sonra, Anahtar İfade Ayıklama uç noktasına nasıl bağlanılacağını belirtmenizin istendiği bir bant görüntülenebilir.

![[kimlik bilgileri bandı]](../media/tutorials/power-bi/credentials-banner.png)

**Kimlik Bilgilerini Düzenle**'ye tıklayın, iletişim kutusunda Anonim'in seçildiğinden emin olun ve ardından, **Bağlan**'a tıklayın. 

> [!NOTE]
> Neden anonim? Metin Analizi hizmeti kimliğinizi API anahtarını kullanarak doğruladığından, Power BI'ın HTTP isteği için kimlik bilgileri sağlaması gerekmez.

![[kimlik doğrulamasını anonim olarak ayarlama]](../media/tutorials/power-bi/access-web-content.png)

Anonim erişimi seçmenize rağmen Kimlik Bilgilerini Düzenle bandını görüyorsanız API anahtarınızı yapıştırmayı unutmuş olabilirsiniz. Yapıştırma işlemini gerçekleştirdiğinizden emin olmak için Gelişmiş Düzenleyici'deki `KeyPhrases` özel işlevini denetleyin.

Ardından, veri kaynaklarınızın gizliliği hakkında bilgiler sunmanızın istendiği bir bant görüntülenebilir. 

![[gizlilik bandı]](../media/tutorials/power-bi/privacy-banner.png)

**Devam Et**'e tıklayın ve iletişim kutusundaki her bir veri kaynağı için Genel'i seçin. Ardından, **Kaydet**'e tıklayın.

![[veri kaynağı gizliliğini ayarlama]](../media/tutorials/power-bi/privacy-dialog.png)

## <a name="creating-the-word-cloud"></a>Kelime bulutunu oluşturma

Görüntülenen tüm bantlarla ilgili gerekli işlemleri yaptıktan sonra, Sorgu Düzenleyicisi'ni kapatmak için Giriş şeridindeki **Kapat ve Uygula** seçeneğine tıklayın.

Power BI Desktop kısa bir süre içinde gerekli HTTP isteklerini yapar. Tablodaki her bir satır için yeni `keyphrases` sütunu, Anahtar İfade Ayıklama API'si tarafından metinde algılanan anahtar ifadeleri içerir. 

Bu sütunu kelime bulutu oluşturmak için kullanalım. Başlamak için, ana Power BI Desktop penceresinde çalışma alanının sol tarafında yer alan Rapor düğmesine tıklayın.

> [!NOTE]
> Kelime bulutu oluşturmak için her bir yorumun tam metni yerine neden ayıklanan anahtar ifadeleri kullanmalı? Anahtar ifadeler bize yalnızca müşteri yorumlarında yer alan *en sık kullanılan* sözcükleri değil, *önemli* sözcükleri de sunar. Ayrıca sonuçta elde edilen buluttaki sözcük boyutlandırması, bir kelimenin nispeten az sayıda yorumda sık olarak kullanılmasına göre şekillenmez.

Henüz yapmadıysanız, Word Cloud özel görselini yükleyin. Çalışma alanının sağ tarafındaki Görsel Öğeler bölmesinde, üç nokta (**...**) simgesine tıklayın ve **Marketten içe aktarın** seçeneğini belirleyin. Ardından, "cloud" araması yapın ve Word Cloud görselinin yanındaki **Ekle** düğmesine tıklayın. Power BI, Word Cloud görselini yükler ve işlemin başarıyla gerçekleştirildiğini size bildirir.

![[özel görsel ekleme]](../media/tutorials/power-bi/add-custom-visuals.png)<br><br>

Şimdi de kelime bulutumuzu oluşturalım!

![[Görsel Öğeler bölmesindeki Word Cloud simgesi]](../media/tutorials/power-bi/visualizations-panel.png)

İlk olarak, Görsel Öğeler bölmesindeki Word Cloud simgesine tıklayın. Çalışma alanında yeni bir rapor görünür. Alanlar bölmesindeki `keyphrases` alanını Görsel Öğeler bölmesindeki Kategori alanına sürükleyin. Kelime bulutu raporda görünür.

Şimdi de Görsel Öğeler bölmesinin Biçim sayfasına geçin. Durdurma Sözcükleri kategorisinde, "of" gibi kısa ve sık kullanılan sözcükleri buluttan kaldırmak için **Varsayılan Durdurma Sözcükleri**'ni etkinleştirin. 

![[varsayılan durdurma sözcüklerini etkinleştirme]](../media/tutorials/power-bi/default-stop-words.png)

Bu bölmenin biraz daha alt kısmında, **Metni Döndür** ve **Başlık**'ı kapatın.

![[odak modunu etkinleştirme]](../media/tutorials/power-bi/word-cloud-focus-mode.png)

Kelime bulutumuzu daha yakından incelemek için raporda Odak Modu aracına tıklayın. Araç, aşağıda gösterildiği gibi çalışma alanının tamamını dolduracak şekilde kelime bulutunu genişletir.

![[Bir Kelime Bulutu]](../media/tutorials/power-bi/word-cloud.png)

## <a name="more-text-analytics-services"></a>Diğer Metin Analizi hizmetleri

Microsoft Azure tarafından sunulan Bilişsel Hizmetler'den biri olan Metin Analizi hizmeti, yaklaşım analizi ve dil algılama özelliklerini de sunar. Dil algılama özelliğin, özellikle de müşteri geri bildirimi tamamen İngilizce dilinde değilse faydalıdır.

Bahsi geçen diğer API'lerin her ikisi de Anahtar İfade Ayıklama API'sine oldukça benzerdir. Bu nedenle, söz konusu API'leri Power BI Desktop ile tümleştirmek için neredeyse aynı olan özel işevler kullanılabilir. Boş bir sorgu oluşturup aşağıdaki uygun kodu daha önce yaptığınız gibi Gelişmiş Düzenleyici'ye yapıştırmanız yeterlidir. (Erişim anahtarınızı unutmayın!) Ardından, daha önce olduğu gibi, tabloya yeni sütun eklemek için işlevi kullanın.

Aşağıdaki Yaklaşım Analizi işlevi, metinde ifade edilen yaklaşımın ne ölçüde olumlu olduğunu gösteren bir puan döndürür.

```fsharp
// Returns the sentiment score of the text, from 0.0 (least favorable) to 1.0 (most favorable)
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    sentiment   = jsonresp[documents]{0}[score]
in  sentiment
```

Dil Algılama işlevine ilişkin iki sürüm aşağıda verilmiştir. İlk sürüm ISO dil kodunu (İngilizce için `en`), ikincisi ise "kolay" adı (`English`) döndürür. Bu iki sürüm arasında yalnızca gövdenin son satırının değişiklik gösterdiğine dikkat edin.

```fsharp
// Returns the two-letter language code (e.g. en for English) of the text
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[iso6391Name]
in  language
```
```fsharp
// Returns the name (e.g. English) of the language in which the text is written
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[name]
in  language
```

Son olarak, sunulmuş olan Anahtar İfade Ayıklama işlevinin, virgülle ayrılan ifadelerden oluşan tek bir dize yerine ifadeleri liste nesnesi olarak döndüren bir değişkeni aşağıda verilmiştir. 

> [!NOTE]
> Tek bir dize döndürülmesi kelime bulutu örneğimizi kolaylaştırmıştır. Diğer yandan listeler, Power BI'da döndürülen ifadelerle çalışmak için daha esnek bir biçimdir. Sorgu Düzenleyicisi'nin Dönüştür şeridindeki Yapılandırılmış Sütun grubunu kullanarak Power BI Desktop'taki liste nesnelerini denetleyebilirsiniz.

```fsharp
// Returns key phrases from the text as a list object
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = jsonresp[documents]{0}[keyPhrases]
in  keyphrases
```

## <a name="next-steps"></a>Sonraki adımlar

Metin Analizi hizmeti, Power Query M formül dili veya Power BI hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Metin Analizi API'si başvurusu](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)

> [!div class="nextstepaction"]
> [Power Query M başvurusu](//msdn.microsoft.com/library/mt211003.aspx)

> [!div class="nextstepaction"]
> [Power BI belgeleri](//powerbi.microsoft.com/documentation/powerbi-landing-page/)
