---
title: 'Öğretici: Azure zaman serisi öngörüleri JavaScript istemci kitaplığını keşfedin | Microsoft Docs'
description: Azure Time Series Insights JavaScript istemci kitaplığını ve ilgili programlama modelini öğrenin.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 05/06/2019
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: 8cb1d06872f7eae04bac934220da9d58982d0f4b
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233746"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Öğretici: Azure Time Series Insights JavaScript istemci kitaplığını keşfetme

Web geliştiricilerin Time Series Insights (TSI) içinde depolanan verileri sorgulamasına ve görselleştirmesine yardımcı olmak için, JavaScript D3 tabanlı İstemci kitaplığı geliştirilmiştir. Bu öğreticide TSI istemci kitaplığı ve barındırılan örnek bir uygulama kullanarak bir programlama modeli aracılığıyla yönlendirecektir.

Öğretici kitaplığıyla birlikte çalışma konusunda ayrıntıları TSI verilere erişmek ve işleme ve verileri görselleştirmek için grafik denetimleri kullanın. Ayrıca farklı türde grafikler veri görselleştirme ile deneme öğreneceksiniz. Öğreticiyi kılavuzumuzun TSI özellikleri kendi web uygulamanıza dahil etmek için istemci kitaplığını kullanmak mümkün olacaktır.

Özellikle, hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * TSI örnek uygulaması.
> * TSI JavaScript istemci kitaplığı.
> * TSI verilerini görselleştirmek için örnek uygulamanın kitaplığı nasıl kullandığı.

> [!NOTE]
> * Ücretsiz bir öğreticide, barındırılan [Time Series Insights web tanıtım](https://insights.timeseries.azure.com/clientsample).
> * Time Series Insights örnek uygulama kaynak dosyaları sağlanan [GitHub örnek deposundan](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * Okuma [TSI istemci başvuru belgeleri](https://github.com/microsoft/tsiclient/blob/master/docs/API.md).

## <a name="video"></a>Video

### <a name="in-this-video-we-introduce-the-open-source-time-series-insights-javascript-sdkbr"></a>Bu videoda, açık kaynak Time Series Insights JavaScript SDK’sı sunulmaktadır.</br>

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, tarayıcınızın **Geliştirici Araçları** özelliği. Modern web tarayıcıları ([Microsoft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [FireFox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/)ve diğerleri) genellikle erişmeyi**Web denetçisi görünümü** aracılığıyla `F12` kısayol tuşu. Aksi takdirde, bir Web sayfasında sağ tıklatıp seçerek erişilebileceğini **İnceleme öğesi**.

## <a name="time-series-insights-sample-application"></a>Time Series Insights örnek uygulaması

Bu öğretici boyunca ücretsiz, barındırılan bir zaman serisi görüşleri örnek uygulama, uygulama ve TSI JavaScript istemci kitaplığı kaynak kodunu incelemek için kullanılır. Grafikler ve graflar aracılığıyla verileri görselleştirmek ve JavaScript dilinde TSI etkileşim nasıl öğreneceksiniz.

1. [Time Series Insights örnek uygulamasına](https://insights.timeseries.azure.com/clientsample) gidin. Aşağıdaki oturum açma istemi görünür:

   [![TSI istemci örnek oturum açma istemine](media/tutorial-explore-js-client-lib/tcs-sign-in.png)](media/tutorial-explore-js-client-lib/tcs-sign-in.png#lightbox)

1. Seçin **oturum** veya kimlik bilgilerinizi girin. Bir kurumsal kuruluş hesabı (Azure Active Directory) veya kişisel hesap (Microsoft Account veya MSA) kullanın.

   [![TSI istemci örnek kimlik bilgileri istemi](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png#lightbox)

1. Oturum açma işleminden sonra çeşitli türlerde grafikler TSI doldurulmuş bir sayfa görürsünüz. Kullanıcı hesabınız ve **Oturumu kapat** seçeneği, sağ üst köşede görünür:

   [![Oturum açma sonrasında TSI istemci örnek ana sayfası](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png#lightbox)

### <a name="page-source-and-structure"></a>Sayfa kaynağı ve yapısı

İlk olarak, şimdi görüntüleyin [HTML ve JavaScript kaynak kodu](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html) disk belleğine alınan işlenmiş Web:

1. Tarayıcınızda **Geliştirici Araçları**’nı açın. Geçerli sayfayı oluşturan HTML öğelerini inceleyin (HTML veya DOM ağacı olarak da bilinir).

1. Genişletin `<head>` ve `<body>` öğeleri ve aşağıdaki bölümleri inceleyin:

   * Altında `<head>` öğe, sayfa meta verileri ve çalıştırmak uygulamayı etkinleştirme bağımlılıkları bulabilirsiniz:
     * Azure Active Directory Authentication Library **adal.min.js** (aynı zamanda ADAL olarak bilinir) başvurusu için kullanılan bir `<script>` öğesi. ADAL, API’lere erişim için OAuth 2.0 kimlik doğrulaması (oturum açma) ve belirteç alımı sağlayan bir JavaScript kitaplığıdır.
     * **sampleStyles.css** ve **tsiclient.css** gibi stil sayfaları (aynı zamanda CSS olarak bilinir) için birden fazla `<link>` öğesi. Stil sayfaları renk, yazı tipi, boşluk vb görsel sayfası stil ayrıntılarını denetlemek için kullanılır.
     * TSI İstemcisi JavaScript kitaplığı **tsiclient.js** başvurusu için kullanılan bir `<script>` öğesi. Kitaplık, TSI hizmet API’lerini çağırmak ve sayfa üzerinde grafik denetimleri oluşturmak için sayfa tarafından kullanılır.

     >[!NOTE]
     > * ADAL JavaScript kitaplığının kaynak kodu [azure-activedirectory-library-for-js repository](https://github.com/AzureAD/azure-activedirectory-library-for-js) konumunda bulunabilir.
     > * TSI İstemci JavaScript kitaplığı [tsiclient deposunda](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial) bulunabilir.

   * Altında `<body>` öğesini bulabileceğiniz `<div>` sayfası ve diğer öğelerin düzeni tanımlamaya yardımcı olacak öğeleri `<script>` öğesi:
     * İlk `<div>` öğesi, **Log In** iletişim kutusunu (`id="loginModal"`) belirtir.
     * İkinci `<div>` öğesi şunlar için üst öğe işlevi görür:
       * Sayfanın üst kısmında (`class="header"`) doğru durum iletileri ve oturum açma bilgileri için kullanılan üst bilgi `<div>` öğesi.
       * Grafiklerin tümü (`class="chartsWrapper"`) dahil olmak üzere kalan sayfa gövdesi öğeleri için `<div>` öğesi.
       * Sayfayı denetlemek için kullanılan tüm JavaScript'leri içeren `<script>` bölümü.

   [![Geliştirici Araçları ile TSI İstemcisi örneği](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

1. Genişletin `<div class="chartsWrapper">` öğesi ve daha fazla alt bulabileceğiniz `<div>` öğeleri. Bu öğeler her bir grafik denetimi örneğini konumlandırmak için kullanılır. Her grafik örneği için bir tane olmak üzere birkaç `<div>` öğesi çiftine dikkat edin:

   * İlk (`class="rowOfCardsTitle"`) öğe, grafiğin neyi gösterdiğini özetleyen açıklayıcı bir başlık içerir. Örneğin, `Static Line Charts With Full-Size Legends.`
   * İkinci (`class="rowOfCards"`) öğe, bir üst öğedir ve bir satırın içindeki gerçek grafik denetimlerini konumlandıran ek alt `<div>` öğelerini içerir.

   [![Gövde div öğeleri](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png#lightbox)

1. Şimdi, doğrudan `<div class="chartsWrapper">` öğesinin altındaki `<script type="text/javascript">` öğesini genişletin. Tüm sayfa mantığı (TSI hizmetinde grafik denetimleri ve daha fazla işleme API'lerini çağırma kimlik doğrulaması) işlemek için kullanılan sayfa düzeyi JavaScript bölüm başına dikkat edin:

   [![Betik gövdesi](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png#lightbox)

## <a name="tsi-javascript-client-library-concepts"></a>TSI JavaScript istemci kitaplığı kavramları

TSI istemci kitaplığı (**tsclient.js**) iki önemli JavaScript işlevleri için soyutlama sağlar:

* **TSI sorgu API'leri çağırmak için sarmalayıcı yöntemleri**: Sorgu olanak tanıyan TSI verilerini toplama ifadesi kullanarak REST API'ler. Metotlar, kitaplığın `TsiClient.Server` ad alanı altında düzenlenmiştir.

* **Oluşturma ve doldurma denetimleri grafik türleri çeşitli yöntemleri**: TSI çizmek için kullanılan yöntemler bir web sayfasında veri toplama. Metotlar, kitaplığın `TsiClient.UX` ad alanı altında düzenlenmiştir.

Bu basitleştirme geliştiricilerin TSI verilerle daha kolay desteklenir UI grafiği ve grafik bileşenler oluşturabilirsiniz.

### <a name="authentication"></a>Kimlik Doğrulaması

[Time Series Insights örnek uygulama](https://insights.timeseries.azure.com/clientsample) ADAL OAuth 2.0 kullanıcı kimlik doğrulaması desteği ile bir tek sayfalı uygulama:

1. ADAL kimlik doğrulaması için kullanırken, istemci uygulamasının Azure Active Directory'de kayıtlı olması gerekir. Aslında, tek sayfalı uygulamayı kullanmak için kayıtlı [OAuth 2.0 örtülü izin akışı](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-implicit-grant-flow).
1. Bu nedenle, uygulamanın çalışma zamanında kayıt özelliklerden bazılarını belirtmeniz gerekir. İstemci GUID'sini bunlar (`clientId`) ve yeniden yönlendirme URI'si (`postLogoutRedirectUri`).
1. Daha sonra uygulama isteklerini bir **erişim belirteci** Azure Active Directory'den. Belirli hizmet/API tanımlayıcısına izinlerini sınırlı bir dizi için erişim belirteci verilir (`https://api.timeseries.azure.com`). Belirteç izinleri, oturum açmış kullanıcı adına verilir. Hizmet/API'sini uygulamanın Azure Active Directory kaydında bulunan başka bir özellik tanımlayıcısıdır.
1. ADAL erişim belirteci uygulamasına döndükten sonra olarak geçirilir bir **taşıyıcı belirteç** zaman TSI erişme izin ver hizmet API'leri.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-204&highlight=3-7,34-37)]

> [!TIP]
> Microsoft tarafından desteklenen ADAL kitaplıkları hakkında daha fazla bilgi için bkz. [ADAL başvuru belgeleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries#microsoft-supported-client-libraries).

### <a name="control-identification"></a>Denetim tanımlama

Sağlanan örnekte `<div>` öğeleri üst düzenlenir `<body>` tüm sayfada işlenen grafik denetimleri için duyarlı Düzen sağlamak için öğesi.

Her `<div>` öğesi yerleştirme ve görsel öznitelikleri grafik denetimi özelliklerini belirtir. HTML öğesi `id` özellikleri oluşturma ve görselleştirilmiş verileri güncelleştirme için belirli denetimler bağlamak için benzersiz tanımlayıcıları olarak hizmet verir.

### <a name="aggregate-expressions"></a>Toplama ifadeleri

TSI istemci kitaplığı API'lerini Toplama ifadesi kullanın:

* Bir toplama ifadesindeki bir veya daha fazla oluşturabilme olanağı sağlayan **arama terimlerini**.

* İstemci API'leri, başka bir sağlama demo uygulamasına benzer bir işlevsellik sağlamak için tasarlanmıştır ( [Time Series Insights gezgininin](https://insights.timeseries.azure.com/demo)), arama aralığının kullanır, where koşulu, ölçüler ve bölünmüş tarafından değeri.

* İstemci Kitaplığı API'lerini çoğunu TSI veri sorgusu oluşturmak için hizmet tarafından kullanılan Toplama ifadesi dizisini alır.

### <a name="call-pattern"></a>Çağrı deseni

Grafik denetimleri oluşturma ve doldurma genel bir desen izler. Bu genel model, örnek uygulama boyunca gözlemlenen ve istemci Kitaplığı kullanıldığında yardımcı olur:

1. Bir veya birden çok TSI toplama ifadesini barındıracak bir `array` bildirin:

   ```javascript
   var aes =  [];
   ```

1. Derleme *1* için *n* toplama ifade nesneleri. Ardından, bunları Toplama ifadesi diziye ekleyin:

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```

   **aggregateExpression parametreleri**

   | Parametre | Açıklama | Örnek |
   | --------- | ----------- | ------- |
   | `predicateObject` | Veri filtreleme ifadesi. |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | Kullanılan ölçümün özellik adı. | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | Ölçü özelliği için istenen toplamalar. | `['avg', 'min']` |
   | `searchSpan`      | Toplama ifadesinin süresi ve aralık boyutu. | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | Bölme ölçütü olarak kullanmak istediğiniz dize özelliği (isteğe bağlı; null olabilir). | `{property: 'Station', type: 'String'}` |
   | `color`         | İşlemek istediğiniz nesnelerin rengi. | `'pink'` |
   | `alias`           | Toplama ifadesinin kolay adı. | `'Factory3Temperature'` |
   | `contextMenuActions` | Görselleştirmede zaman serisi nesnelerine bağlı olacak eylem dizisi (isteğe bağlı). | Daha fazla bilgi için bkz [açılan bağlam menüleri](#pop-up-context-menus) |

1. Toplama verilerini istemek için `TsiClient.Server` API'lerini kullanarak bir TSI sorgusu çağırın:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **getAggregates parametreleri**

   | Parametre | Açıklama | Örnek |
   | --------- | ----------- | ------- |
   | `token`     | TSI API'si için erişim belirteci. |  `authContext.getTsiToken()` Daha fazla bilgi için [kimlik doğrulama bölümüne](#authentication) bakın. |
   | `envFQDN`   | TSI ortamı için tam etki alanı adı (FQDN). | Azure portalından, örneğin: `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | TSI sorgu ifadeleri dizisi. | Daha önce açıklandığı gibi `aes` değişkenini kullanın: `aes.map(function(ae){return ae.toTsx()}`. |

1. TSI sorgusundan döndürülen sıkıştırılmış sonucu görselleştirme amacıyla JSON nesnelerine dönüştürün:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

1. `TsiClient.UX` API'lerini kullanarak bir grafik denetimi oluşturun ve bunu sayfadaki `<div>` öğelerinden birine bağlayın:

   ```javascript
   var barChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

1. Grafik denetimini dönüştürülmüş JSON veri nesneleriyle doldurun ve sayfadaki denetimi işleyin:

   ```javascript
   barChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="rendering-controls"></a>Denetimleri işleme

TSI istemci kitaplığı, sekiz benzersiz, çıkış,-hazır, analiz denetimlerini sağlar:

* **Çizgi grafik**
* **Pasta grafiği**
* **Çubuk grafik**
* **Isı Haritası**
* **Hiyerarşi denetimleri**
* **erişilebilir bir kılavuz**
* **ayrık olay zaman çizelgeleri**
* **Durum Geçişi zaman çizelgeleri**

### <a name="line-bar-pie-chart-examples"></a>Çizgi, çubuk, pasta grafik örnekleri

Bazı standart grafik denetimi işlemek için kullanılan tanıtım koduna bakar. Programlama modeli ve bu denetimleri oluşturmak için desen dikkat edin. Özellikle, HTML altında bölümünü inceleyin `// Example 3/4/5` HTML denetimleri işleyen açıklama `id` değerleri `chart3`, `chart4`, ve `chart5`.

Geri çekilemedi **3. adım** , [sayfasında kaynak ve yapı bölüm](#page-source-and-structure) grafik denetimleri satırların her biri bir açıklayıcı bir başlık satırı sahip sayfasında düzenlenir. Bu örnekte, üç grafikleri başlığı doldurulur `"Multiple Chart Types From the Same Data"` `<div>` öğesi ve bağlı için üç `<div>` başlığı olan öğeler:

[!code-html[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

Aşağıdaki JavaScript kodu bölümünde, TSI toplama ifadelerini oluşturmak, bunları kullanarak TSI verilerini sorgulamak ve üç grafiği işlemek için daha önce belirtilen desen kullanılır. İlgili grafikleri oluşturmak için `tsiClient.ux` ad alanından kullanılan üç türe (`LineChart`, `BarChart` ve `PieChart`) dikkat edin. Ayrıca, grafiklerin üçünün de aynı toplama ifadesi verilerini (`transformedResult`) kullanabildiğine de dikkat edin:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

Üç grafik işlendiğinde aşağıdaki gibi görünür:

[![Aynı verilerle birden çok grafik türü](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Gelişmiş özellikler

TSI istemci kitaplığı veri görselleştirmeleri yaratıcı bir şekilde uygulamak için kullanabileceğiniz çeşitli ek özellikler vardır.

### <a name="states-and-events"></a>Durumlar ve olaylar

Bir Gelişmiş durumu geçişleri ve ayrık olayları için grafik ekleme olanağı işlevdir. Bu özellik, vurgulama olaylar, uyarılar ve durum anahtarlar (açık/kapalı örneğin anahtarlar) oluşturmak için yararlıdır.

Konum çevreleyen kod `// Example 10` açıklaması. Kod bir satır denetimi altındaki başlığı işler `"Line Charts with Multiple Series Types"`ve kendisine bağlar `<div>` HTML öğesiyle `id` değer `chart10`.

1. İlk olarak, bir yapı adlı `events4` izlemek için durum değişikliği öğeleri tutmak için tanımlanır. Yapı şunları içerir:

   * `Component States` adlı bir dize anahtarı.
   * Durumları temsil eden bir değer nesneleri dizisi. Her nesne şunları içerir:
     * JavaScript ISO zaman damgası içeren bir dize anahtarı.
     * Durumun özelliklerini içeren bir dizi: bir renk ve bir açıklama.

2. İleri `events5` yapısı için tanımlanmış `Incidents`, olay öğeleri izlemek için bir dizi tutar. Dizi yapısı, `events4` için belirtilen yapıyla aynı şekildedir.

3. Son olarak çizgi grafiği işlenir ve grafik seçenekleri parametreleriyle iki yapıdan geçer: `events:` ve `states:`. `tooltip:`, `theme:` ve `grid:` belirtmeye yönelik diğer seçenek parametrelerine dikkat edin.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

Görsel olarak, olayları belirtmek için baklava şeklinde işaretçiler/açılan pencereler kullanılır ve zaman ölçeğiyle birlikte renkli çubuklar/açılan pencereler de durum değişikliklerini belirtir:

[![Birden Çok Seri Türü İçeren Çizgi Grafikler](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Açılır menüler

Başka bir Gelişmiş özel bağlam menüleri (sağ tıklama açılır menüler) oluşturma olanağı işlevdir. Özel açılır menüler, uygulamanızın kapsamı dahilinde sıradaki eylemleri ve mantıksal adımları etkinleştirmek için yararlıdır.

Kod araştırın `// Example 13/14/15` açıklaması. Bu kod, başlangıçta bir çizgi grafik başlığı altında işler `"Line Chart with Context Menu to Create Pie/Bar Chart"` ve grafik bağlı `<div>` HTML öğesiyle `id` değer `chart13`.

Bağlam menülerini kullanarak, çizgi grafik dinamik olarak `chart14` ve `chart15` kimlikleriyle `<div>` öğelerine bağlı bir pasta ve çubuk grafik oluşturma olanağı sağlar. Buna ek olarak, hem pasta hem de çubuk grafikler de kendi özelliklerini etkinleştirmek için bağlam menülerini kullanır: sırasıyla pasta grafikten çubuk grafiğe veri kopyalama ve grafik verilerini tarayıcıya yazdırma özellikleri.

1. İlk olarak bir dizi özel eylem tanımlanır. Her eylem bir veya daha fazla öğe ile bir dizi içerir. Her öğe tek bir bağlam menüsü öğesini tanımlar:

   * `barChartActions`: Bu eylem, tek bir öğeyi tanımlamak için bir öğe içeriyorsa pasta grafiği için bağlam menüsünü tanımlar:
     * `name`: Menü öğesi için kullanılan metin: "Konsol parametreleri yazdırın."
     * `action`: Menü öğesi ile ilişkili eylem. Eylem her zaman anonim bir işlevdir ve grafiği oluşturmak için kullanılan toplama ifadesi temelinde üç bağımsız değişken alır. Bu örnekte, bağımsız değişkenler tarayıcı konsol penceresine yazılır:
       * `ae`: Toplama ifadesi dizi.
       * `splitBy`: `splitBy` Değeri.
       * `timestamp`: Zaman damgası.

   * `pieChartActions`: Bu eylem, tek bir öğeyi tanımlamak için bir öğe içeriyorsa çubuk grafik için bağlam menüsünü tanımlar. Şekil ve şema önceki `barChartActions` öğesi ile aynıdır, ama `action` işlevinin önemli ölçüde farklı olduğuna dikkat edin çünkü bu işlev çubuk grafiği başlatır ve işler. Ayrıca çalışma zamanında menü öğesi açıldığında geçirilen toplama ifadesi dizisini belirtmeye yönelik `ae` bağımsız değişkeninin kullanıldığına dikkat edin. İşlev, `barChartActions` bağlam menüsüyle birlikte `ae.contextMenu` özelliğini de ayarlar.
   * `contextMenuActions`: Bu eylem, üç menü öğeleri tanımlamak için üç öğeleri içeren çizgi grafik için bağlam menüsünü tanımlar. Her öğenin şekli ve şeması, açıklanan önceki öğelerle aynıdır. Aynı `barChartActions` öğesi gibi, ilk öğe tarayıcı konsol penceresine üç işlev bağımsız değişkenini yazar. `pieChartActions` öğesine benzer biçimde, ikinci iki öğe sırasıyla pasta ve çubuk grafiklerini başlatır ve işler. İkinci iki öğe sırasıyla `pieChartActions` ve `barChartActions` bağlam menüleriyle `ae.contextMenu` özelliklerini de ayarlar.

2. Sonra, iki toplama ifadesi `aes` toplama ifadesi dizisinin içine gönderilir ve her öğe için `contextMenuActions` dizisi belirtilir. Bu ifadeler, çizgi grafik denetimiyle kullanılır.

3. Son olarak, başlangıçta yalnızca çizgi grafik işlenir; çalışma zamanında hem pasta hem de çubuk grafik bu çizgi grafikten işlenebilir.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

Ekran görüntüsünde ilgili açılan bağlam menüleriyle birlikte grafikler gösterilir. Pasta ve çubuk grafikler, çizgi grafiğin bağlam menüsü seçeneklerinin kullanılmasıyla dinamik olarak oluşturulmuştur.

[![Pasta/Çubuk Grafik Oluşturmak için Bağlam Menüsüyle Çizgi Grafik](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Fırçalar

Fırçalar, yakınlaştırma ve keşfetme gibi eylemleri tanımlamak üzere zaman aralığının kapsamını belirlemek için kullanılır.

Açılan bağlam menüleri açıklayan önceki "Çizgi grafik ile bağlam menüsü için oluşturma Pasta/Çubuk Grafik" örnekte Fırçalar göstermek için kullanılan kod gösterilmektedir.

1. Fırça eylemleri, fırça için bir dizi özel eylem tanımlaması bakımından bağlam menüsüne benzer. Her eylem bir veya daha fazla öğe ile bir dizi içerir. Her öğe tek bir bağlam menüsü öğesini tanımlar:
   * `name`: Menü öğesi için kullanılan metin: "Konsol parametreleri yazdırın."
   * `action`: Her zaman iki bağımsız değişken alan anonim bir işlevdir menü öğesi ile ilişkili eylem. Bu örnekte, bağımsız değişkenler tarayıcı konsol penceresine yazılır:
      * `fromTime`: `from` Fırça seçimin zaman damgası.
      * `toTime`: `to` Fırça seçimin zaman damgası.

2. Fırça eylemleri başka bir grafik seçeneği özelliği olarak eklenir. `brushContextMenuActions: brushActions` özelliğinin `linechart.Render` çağrısına geçirildiğine dikkat edin.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

[![Fırçalar ile Pasta/Çubuk grafik oluşturma için bağlam menüsü ile çizgi grafik](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Oturum açın ve TSI örnek uygulamasıyla onun kaynağını keşfedin.
> * TSI JavaScript istemci kitaplığında API'leri kullanın.
> * TSI verileriyle grafik denetimlerini oluşturmak ve doldurmak için JavaScript kullanın.

Görüldüğü gibi TSI örnek uygulama bir tanıtım veri kümesi kullanır. Kendi TSI ortamınızı ve veri kümenizi nasıl oluşturabileceğiniz hakkında bilgi edinmek için, şu makaleye ilerleyin:

> [!div class="nextstepaction"]
> [Öğretici: Azure zaman serisi görüşleri ortamı oluşturma](tutorial-create-populate-tsi-environment.md)

Veya TSI örnek uygulama kaynak dosyaları görüntüleyin:

> [!div class="nextstepaction"]
> [TSI örnek uygulama deposunu](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)

TSI istemci API başvuru belgeleri okuyun:

> [!div class="nextstepaction"]
> [TSI API başvuru belgeleri](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)
