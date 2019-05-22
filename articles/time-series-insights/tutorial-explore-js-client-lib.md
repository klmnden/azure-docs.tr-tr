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
ms.openlocfilehash: 7208c0d42cba0e7f04fc6876bf3ada9fa65a00d9
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991448"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Öğretici: Azure Time Series Insights JavaScript istemci kitaplığını keşfetme

Azure Time Series Insights JavaScript D3 tabanlı istemci kitaplığı, web geliştiricilerin sorgu yardımcı olmak ve Time Series Insights içinde depolanan verileri görselleştirmek için geliştirilmiştir. Bu öğreticide, zaman serisi görüşleri istemci kitaplığı ve programlama modeli barındırılan örnek uygulaması kullanarak size yol gösterir.

Öğretici kitaplığı ile çalışma konusunda, işleme ve verileri görselleştirmek için grafik denetimleri kullanmayı ve zaman serisi öngörüleri verilerini nasıl ayrıntılı olarak açıklanmaktadır. Ayrıca farklı türde grafikler veri görselleştirme ile deneme öğreneceksiniz. Öğreticinin sonunda, zaman serisi görüşleri özellikleri kendi web uygulamanıza dahil etmek için istemci kitaplığını kullanmak mümkün olacaktır.

Özellikle, hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Time Series Insights örnek uygulaması
> * Zaman serisi öngörüleri JavaScript istemci kitaplığı
> * Örnek uygulama kitaplığı zaman serisi öngörüleri verilerini görselleştirmek için kullanma

> [!NOTE]
> * Ücretsiz bir öğreticide, barındırılan [Time Series Insights web tanıtım](https://insights.timeseries.azure.com/clientsample).
> * Time Series Insights örnek uygulama kaynak dosyaları sağlanan [GitHub örnek deposundan](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * Okuma [Time Series Insights istemci başvuru belgeleri](https://github.com/microsoft/tsiclient/blob/master/docs/API.md).

## <a name="video"></a>Video

Bu videoda, size açık kaynak zaman serisi Insights JavaScript SDK'sı sunar:
<br /><br />

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, tarayıcınızın **Geliştirici Araçları** özelliği. Modern web tarayıcıları ([Microsoft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [Firefox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/)ve diğerleri) genellikle erişmeyi**Web denetçisi görünümü** F12 kısayol tuşu klavyenizde aracılığıyla. Bir Web sayfasında sağ tıklayın ve ardından görünümü başka bir yolu budur **İnceleme öğesi**.

## <a name="time-series-insights-sample-application"></a>Time Series Insights örnek uygulaması

Bu öğretici boyunca ücretsiz, barındırılan bir zaman serisi görüşleri örnek uygulaması arkasındaki uygulama kaynak kodu keşfetmenize ve zaman serisi öngörüleri JavaScript istemci kitaplığı keşfetmek için kullanırız. Örnek uygulamayı kullanarak JavaScript dilinde zaman serisi görüşleri ile etkileşimde bulunmak ve grafikler ve graflar aracılığıyla verileri görselleştirmek nasıl öğreneceksiniz.

1. Git [Time Series Insights örnek uygulama](https://insights.timeseries.azure.com/clientsample). Aşağıdaki oturum açma istemi görünür:

   [![Oturum açma, zaman serisi görüşleri istemci örnek istemi](media/tutorial-explore-js-client-lib/tcs-sign-in.png)](media/tutorial-explore-js-client-lib/tcs-sign-in.png#lightbox)

1. Seçin **oturum** veya kimlik bilgilerinizi girin. Bir kurumsal kuruluş hesabı (Azure Active Directory) ya da bir kişisel hesap (Microsoft hesabı) kullanın.

   [![Zaman serisi görüşleri istemci örnek kimlik bilgileri iste](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png#lightbox)

1. Oturum açtıktan sonra zaman serisi görüşleri doldurulmuş grafikleri görüntüleyen bir sayfa gösterilmektedir. Kullanıcı hesabınız ve **Oturumu kapat** seçeneği, sağ üst köşede görünür:

   [![Zaman serisi görüşleri istemci örnek ana sayfadan sonra oturum açma](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png#lightbox)

### <a name="page-source-and-structure"></a>Sayfa kaynağı ve yapısı

İlk olarak, şimdi görüntüleyin [HTML ve JavaScript kaynak kodu](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html) işlenmiş sayfasının:

1. Tarayıcınızda **Geliştirici Araçları**’nı açın. Geçerli sayfayı oluşturan HTML öğelerini inceleyin (HTML veya DOM ağacı olarak da bilinir).

1. Genişletin `<head>` ve `<body>` öğeleri ve aşağıdaki bölümleri inceleyin:

   * Altında `<head>` öğe, sayfa meta verileri ve çalıştırmak uygulamayı etkinleştirme bağımlılıkları bulabilirsiniz:
     * A `<script>` Azure Active Directory kimlik doğrulama kitaplığı (ADAL) dosyasına başvurmak için kullanılan öğe *adal.min.js*. ADAL, API’lere erişim için OAuth 2.0 kimlik doğrulaması (oturum açma) ve belirteç alımı sağlayan bir JavaScript kitaplığıdır.
     * Birden çok `<link>` stil sayfaları için öğeleri (diğer adıyla *CSS*) gibi *sampleStyles.css* ve *tsiclient.css*. Stil sayfaları, renkleri, yazı tipleri ve boşluk gibi görsel sayfanın stil ayrıntıları denetleyin.
     * A `<script>` zaman serisi öngörüleri JavaScript istemci kitaplığı başvurmak için kullanılan öğe *tsiclient.js*. Sayfa kitaplığı Time Series Insights hizmeti API'leri çağırmak için ve grafik denetimleri sayfasında işlemek için kullanır.

     >[!NOTE]
     > * ADAL JavaScript kitaplığı kaynak kodunu kullanılabilir [azure-activedirectory-kitaplığı-için-js depo](https://github.com/AzureAD/azure-activedirectory-library-for-js).
     > * Zaman serisi öngörüleri JavaScript istemci kitaplığı kaynak kodunu kullanılabilir [tsiclient depo](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).

   * Altında `<body>` öğesini bulabileceğiniz `<div>` sayfası ve diğer öğelerin düzeni tanımlamaya yardımcı olacak öğeleri `<script>` öğesi:
     * İlk `<div>` öğesi belirtir **oturum** iletişim kutusu (`id="loginModal"`).
     * İkinci `<div>` öğesi şunlar için üst öğe işlevi görür:
       * Sayfanın üst kısmında (`class="header"`) doğru durum iletileri ve oturum açma bilgileri için kullanılan üst bilgi `<div>` öğesi.
       * A `<div>` öğesi grafikleri de dahil olmak üzere sayfa gövdesi öğelerini, geri kalanında (`class="chartsWrapper"`).
       * A `<script>` sayfa denetlemek için kullanılan JavaScript içeren bölümü.

   [![Geliştirici Araçları ile zaman serisi öngörüleri istemci örneği](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

1. Genişletin `<div class="chartsWrapper">` öğesi ve daha fazla alt bulabileceğiniz `<div>` öğeleri. Bu öğeler her bir grafik denetimi örneğini konumlandırmak için kullanılır. Birden çok çiftlerini vardır `<div>` öğeleri, her grafik örneği:

   * İlk (`class="rowOfCardsTitle"`) öğe, grafiğin neyi gösterdiğini özetleyen açıklayıcı bir başlık içerir. Örneğin, `Static Line Charts With Full-Size Legends.`
   * İkinci (`class="rowOfCards"`) ek alt içeren bir üst öğedir `<div>` satır içindeki gerçek grafik denetimleri konumlandırma öğeleri.

   [![Gövde div öğeleri](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png#lightbox)

1. Genişletin `<script type="text/javascript">` doğrudan aşağıdaki öğesi `<div class="chartsWrapper">` öğesi. Sayfa düzeyi JavaScript bölümünün başında, tüm sayfa mantığı (Time Series Insights hizmeti grafik denetimleri ve daha fazla işleme API'lerini çağırma kimlik doğrulaması) işlemek için kullanılır:

   [![Betik gövdesi](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png#lightbox)

## <a name="time-series-insights-javascript-client-library-concepts"></a>Zaman serisi öngörüleri JavaScript istemci kitaplığı kavramları

Time Series Insights istemci kitaplığı (*tsclient.js*) iki önemli JavaScript işlevleri için soyutlama sağlar:

* **Zaman serisi öngörüleri sorgu API'leri çağırmak için sarmalayıcı yöntemleri**: Zaman serisi öngörüleri için veri toplama ifadesi kullanarak sorgulamak için kullanabileceğiniz REST API'ler. Yöntemleri kitaplığı TsiClient.Server ad alanı altında düzenlenir.

* **Oluşturma ve doldurma denetimleri grafik türleri çeşitli yöntemleri**: Time Series Insights'ı oluşturmak için kullanabileceğiniz yöntemler bir Web sayfasındaki veri toplama. Yöntemleri kitaplığı TsiClient.UX ad alanı altında düzenlenir.

Bu basitleştirme geliştiriciler ile zaman serisi öngörüleri verilerini desteklenen UI grafiği ve grafik bileşenleri daha kolayca oluşturabilir.

### <a name="authentication"></a>Kimlik Doğrulaması

[Time Series Insights örnek uygulama](https://insights.timeseries.azure.com/clientsample) ADAL OAuth 2.0 kullanıcı kimlik doğrulaması desteği olan bir tek sayfalı uygulama:

1. Kimlik doğrulaması için ADAL'ı kullandığınızda, istemci uygulamasının Azure Active Directory (Azure AD) kayıtlı olması gerekir. Aslında, tek sayfalı uygulamayı kullanmak için kayıtlı [OAuth 2.0 örtülü izin akışı](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-implicit-grant-flow).
1. Uygulama çalışma zamanında kayıt özelliklerden bazılarını belirtmeniz gerekir. İstemci GUID'sini özellikleri içerir (`clientId`) ve yeniden yönlendirme URI'si (`postLogoutRedirectUri`).
1. Daha sonra uygulama isteklerini bir *erişim belirteci* Azure AD'den. Sınırlı bir özel hizmet veya API tanımlayıcısına izin kümesi için erişim belirtecini veren (https:\//api.timeseries.azure.com). Belirteç izinleri, oturum açmış kullanıcı adına verilir. Hizmet veya API için uygulamanın Azure AD'ye kaydında bulunan başka bir özellik tanımlayıcısıdır.
1. ADAL erişim belirteci uygulamasına döndükten sonra olarak geçirilir bir *taşıyıcı belirteç* Time Series Insights hizmet API'lerini ne zaman eriştiği.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-204&highlight=3-7,34-37)]

> [!TIP]
> Microsoft tarafından desteklenen Azure AD kimlik doğrulama kitaplıkları hakkında daha fazla bilgi için bkz: [Azure Active Directory Authentication Library başvuru belgeleri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries#microsoft-supported-client-libraries).

### <a name="control-identification"></a>Denetim tanımlama

Sağlanan örnekte `<div>` öğeleri üst düzenlenir `<body>` sayfasında işleyen grafik denetimleri için duyarlı Düzen sağlamak için öğesi.

Her `<div>` öğesi yerleştirme ve görsel öznitelikleri grafik denetimi özelliklerini belirtir. HTML öğesi `id` özellikleri sunmak gibi verileri işlemek ve güncelleştirme için belirli denetimlerine bağlamanıza imkan benzersiz tanımlayıcıları görselleştirilir.

### <a name="aggregate-expressions"></a>Toplama ifadeleri

Time Series Insights istemci kitaplığı API'lerini Toplama ifadesi kullanın:

* Bir toplama ifadesindeki bir veya daha fazla oluşturabilme olanağı sağlayan *arama terimlerini*.

* İstemci API'leri, başka bir demo uygulamasına benzer bir işlevsellik sağlamak için tasarlanmıştır ( [Time Series Insights gezgininin](https://insights.timeseries.azure.com/demo)), aralık, arama kullanır `where` doğrulamaları, ölçüler ve `splitBy` değerleri.

* Çoğu istemci kitaplığı API'lerini Time Series Insights veri sorgusu oluşturmak için hizmetin kullandığı toplama ifadesi dizisini alır.

### <a name="call-pattern"></a>Çağrı deseni

Grafik denetimleri oluşturma ve doldurma genel bir desen izler. Örnek uygulamada genel düzen inceleyebileceğiniz ve istemci kitaplığı kullandığınızda yardımcı olabilir:

1. Bildirme bir `array` bir veya daha fazla zaman serisi görüşleri Toplama ifadesi tutmak için:

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
   | `predicateObject` | Veri filtreleme ifadesi |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | Kullanılan ölçü özellik adı | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | Toplamalar Ölçü özelliğinin istediğiniz | `['avg', 'min']` |
   | `searchSpan`      | Toplama süresi ve aralık boyutu | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | Bölme ölçütü dize özelliği (isteğe bağlı: null olabilir) | `{property: 'Station', type: 'String'}` |
   | `color`         | İşlemek istediğiniz nesnelerin rengi | `'pink'` |
   | `alias`           | Toplama ifadesi için bir kolay ad | `'Factory3Temperature'` |
   | `contextMenuActions` | (İsteğe bağlı) bir görselleştirmede zaman serisi nesnelere bağlı eylemler dizisi | Daha fazla bilgi için [açılan bağlam menüleri](#pop-up-context-menus). |

1. Time Series Insights sorgu toplu veriler istemek için TsiClient.Server API'lerini kullanarak çağırın:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **getAggregates parametreleri**

   | Parametre | Açıklama | Örnek |
   | --------- | ----------- | ------- |
   | `token`     | Zaman serisi öngörüleri API için erişim belirteci |  `authContext.getTsiToken()`<br />Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). |
   | `envFQDN`   | Zaman serisi görüşleri ortamı için tam etki alanı adı (FQDN) | Azure portalından. Örneğin: `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | Bir dizi Time Series Insights sorgu ifadeleri | Kullanım `aes` daha önce açıklandığı gibi değişken: `aes.map(function(ae){return ae.toTsx()}`. |

1. Time Series Insights sorgudan JSON'a görselleştirme için döndürülen sıkıştırılmış sonucu dönüştürün:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

1. Grafik denetimi TsiClient.UX API'lerini kullanarak oluşturun. Birine bağlamanız `<div>` sayfadaki öğeleri:

   ```javascript
   var barChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

1. Grafik denetimi dönüştürülen JSON veri nesneleri ile doldurur ve sayfasındaki denetimi oluşturmak:

   ```javascript
   barChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="render-controls"></a>Denetimleri oluşturma

Time Series Insights istemci kitaplığı, sekiz benzersiz, kullanıma hazır analiz denetimlerini sağlar:

* **Çizgi grafik**
* **Pasta grafiği**
* **Çubuk grafik**
* **Isı Haritası**
* **Hiyerarşi denetimleri**
* **erişilebilir bir kılavuz**
* **ayrık olay zaman çizelgeleri**
* **Durum Geçişi zaman çizelgeleri**

### <a name="line-chart-bar-chart-and-pie-chart-examples"></a>Çizgi grafiği çubuk grafik ve pasta grafiği örnekleri

Bazı standart grafik denetimleri oluşturmak için kullanılan tanıtım koduna bakar. Programlama modeli ve bu denetimleri oluşturmak için desen unutmayın. Özellikle, HTML altında inceleyin `// Example 3/4/5` HTML denetimleri işleyen açıklama `id` değerleri `chart3`, `chart4`, ve `chart5`.

Geri çağırma 3 adımda [sayfasında kaynak ve yapı bölüm](#page-source-and-structure) grafik denetimleri sayfasında satırları düzenlenir. Her grafik denetiminin, açıklayıcı bir başlık satırı vardır. Bu örnekte, üç grafikleri altında doldurulur `Multiple Chart Types From the Same Data` başlık `<div>` ve bu öğe için üç bağlı `<div>` başlığı olan öğeler:

[!code-html[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

JavaScript kodu aşağıdaki bölümde daha önce açıklanan desenleri kullanır: zaman serisi görüşleri toplama ifadeleri oluşturmak, Time Series Insights verileri sorgulamak için bunları kullanın ve ardından üç grafik işleme. Üç grafik türleri tsiClient.ux ad alanından kullanılır: `LineChart`, `BarChart`, ve `PieChart`. Grafik türleri oluşturun ve ilgili grafikler işlemek için kullanılır. Tüm üç grafiklerini aynı Toplama ifadesi verileri kullanabilir `transformedResult`:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

Üç grafik işlendiğinde aşağıdaki gibi görünür:

[![Aynı verilerle birden çok grafik türü](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Gelişmiş özellikler

Time Series Insights istemci kitaplığı, veri görselleştirmeleri yaratıcı bir şekilde uygulamak için kullanabileceğiniz çeşitli ek özellikler vardır.

### <a name="states-and-events"></a>Durumlar ve olaylar

Durumu geçişleri ve ayrık olayları için grafik ekleme olanağı bir Gelişmiş işlevdir. Bu özellik, vurgulama olaylar, uyarılar ve durum anahtarları (/ anahtarlar, örneğin kapalı) oluşturma için kullanışlıdır.

Konum çevreleyen kod `// Example 10` açıklaması. Kod bir satır denetimi altındaki başlığı işler `Line Charts with Multiple Series Types` ve kendisine bağlar `<div>` HTML öğesiyle `id` değer `chart10`.

Aşağıdaki adımları işlemi açıklanmaktadır:

1. Adlı bir yapı `events4` izlemek için durum değişikliği öğeleri tutmak için tanımlanır. Yapı şunları içerir:

   * `Component States` adlı bir dize anahtarı.
   * Durumları temsil eden bir değer nesneleri dizisi. Her nesne şunları içerir:
     * JavaScript ISO zaman damgası içeren bir dize anahtarı.
     * Durumun özelliklerini içeren bir dizi: bir renk ve bir açıklama.

1. `events5` Yapısı için tanımlanmış `Incidents`, olay öğeleri izlemek için bir dizi tutar. Dizi yapısı, `events4` için belirtilen yapıyla aynı şekildedir.

1. Çizgi grafiği oluşturulur ve parametrelerini geçirir grafik seçenekleri ile iki yapılardaki: `events:` ve `states:`. Diğer seçenek parametrelerini belirtmek için Not bir `tooltip:`, `theme:`, veya `grid:`.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

Olayları göstermek için kullanılan elmas işaretçileri/pop-kaydolma windows ve renkli çubuk/pop-kaydolma windows zaman ölçeği boyunca durum değişikliklerini gösterir:

[![Birden fazla seri türleri ile çizgi grafikler](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Açılır menüler

Başka bir Gelişmiş özel bağlam menüleri (sağ tıklama açılır menüler) oluşturma olanağı işlevdir. Özel açılır menüler, uygulamanızın kapsamı dahilinde sıradaki eylemleri ve mantıksal adımları etkinleştirmek için yararlıdır.

Kodda gezinmeye bakın `// Example 13/14/15` açıklaması. Bu kod, başlangıçta bir çizgi grafik başlığı altında işler `Line Chart with Context Menu to Create Pie/Bar Chart`. Grafik bağlı `<div>` HTML öğesiyle `id` değer `chart13`.

Bağlam menülerini kullanarak, çizgi grafik dinamik olarak `chart14` ve `chart15` kimlikleriyle `<div>` öğelerine bağlı bir pasta ve çubuk grafik oluşturma olanağı sağlar. Çubuk grafik ve pasta grafik aynı zamanda bağlam menüleri kendi özelliklerini etkinleştirmek için kullanın: çubuk grafik olarak pasta grafiğin verileri kopyalamak ve tarayıcı konsol penceresine çubuk grafiği verileri sırasıyla yazdırma olanağı.

Aşağıdaki adımları işlemi açıklanmaktadır:

1. Özel Eylemler dizisi tanımlanır. Her eylem bir veya daha fazla öğe ile bir dizi içerir. Her öğe tek bir bağlam menüsü öğesini tanımlar:

   * `barChartActions`: Bu eylem, tek bir öğeyi tanımlamak için bir öğe içeriyorsa pasta grafiği için bağlam menüsünü tanımlar:
     * `name`: Menü öğesi için kullanılan metin: "Konsol parametreleri yazdırın."
     * `action`: Menü öğesi ile ilişkili eylem. Eylem her zaman anonim bir işlevdir ve grafiği oluşturmak için kullanılan toplama ifadesi temelinde üç bağımsız değişken alır. Bu örnekte, bağımsız değişkenler tarayıcı konsol penceresine yazılır:
       * `ae`: Toplama ifadesi dizi.
       * `splitBy`: `splitBy` Değeri.
       * `timestamp`: Zaman damgası.

   * `pieChartActions`: Bu eylem, tek bir öğeyi tanımlamak için bir öğe içeriyorsa çubuk grafik için bağlam menüsünü tanımlar. Şema ve şekli aynı olup `barChartActions` öğe daha önce açıklanan ancak `action` işlevi, önemli ölçüde farklıdır: başlatır ve çubuk grafik oluşturur. `ae` Bağımsız değişkeni, menü öğesi açıldığında, çalışma zamanında geçirilen Toplama ifadesi dizi belirtmek için kullanılır. İşlev, `barChartActions` bağlam menüsüyle birlikte `ae.contextMenu` özelliğini de ayarlar.
   * `contextMenuActions`: Bu eylem, üç menü öğeleri tanımlamak için üç öğeleri içeren çizgi grafik için bağlam menüsünü tanımlar. Her öğe için şema ve şekli ile aynıdır, daha önce açıklanan öğeleri. Aynı `barChartActions` öğesi gibi, ilk öğe tarayıcı konsol penceresine üç işlev bağımsız değişkenini yazar. Benzer şekilde `pieChartActions` öğesi, ikinci iki öğeyi örneklemek ve sırasıyla pasta grafik ve çubuk grafik oluşturma. İkinci iki öğe sırasıyla `pieChartActions` ve `barChartActions` bağlam menüleriyle `ae.contextMenu` özelliklerini de ayarlar.

1. İki Toplama ifadesi üzerine itilir `aes` Toplama ifadesi dizi. Belirlediği `contextMenuActions` her öğe için bir dizi. Bu ifadeler, çizgi grafik denetimiyle kullanılır.

1. Çizgi grafik ilk işlenen, çubuk grafik ve pasta grafik çalışma zamanında işlenebilir.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

Aşağıdaki ekran görüntüsünde, ilgili açılan bağlam menüleri grafiklerle gösterir. Çubuk grafik ve pasta grafiği çizgi grafik bağlam menüsü seçeneklerini kullanarak dinamik olarak oluşturulmuştur.

[![Pasta grafik ve çubuk grafik oluşturma için bağlam menüsü ile çizgi grafik](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Fırçalar

Fırçalar, yakınlaştırma ve keşfetme gibi eylemleri tanımlamak için bir zaman aralığı kapsamını belirlemek için kullanabilirsiniz.

Fırçalar göstermek için kullanılan kod gösterilen `Line Chart with Context Menu to Create Pie/Bar Chart` açılan bağlam menüleri tanımlayan örnek.

* Fırça eylemleri, fırça için bir dizi özel eylem tanımlaması bakımından bağlam menüsüne benzer. Her eylem bir veya daha fazla öğelere sahip bir dizi içeriyor. Her öğe tek bir bağlam menüsü öğesini tanımlar:
   * `name`: Menü öğesi için kullanılan metin: "Konsol parametreleri yazdırın."
   * `action`: Her zaman iki bağımsız değişken alan anonim bir işlevdir menü öğesi ile ilişkili eylem. Bu örnekte, bağımsız değişkenler tarayıcı konsol penceresine yazılır:
     * `fromTime`: `from` Fırça seçimin zaman damgası.
     * `toTime`: `to` Fırça seçimin zaman damgası.

* Fırça eylemleri başka bir grafik seçeneği özelliği olarak eklenir. `brushContextMenuActions: brushActions` Özelliği geçirildiğinde `linechart.Render` çağırın.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

[![Pasta grafik ve çubuk grafik Fırçalar kullanarak oluşturmak için bağlam menüsü ile çizgi grafik](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Oturum açın ve Time Series Insights örnek uygulamayı ve onun kaynağı keşfedin
> * Zaman serisi öngörüleri JavaScript istemci Kitaplığı'ndaki API'lerini kullanma
> * Oluşturma ve grafik denetimleri ile zaman serisi öngörüleri verilerini doldurmak için JavaScript kullanın

Time Series Insights örnek uygulama bir tanıtım veri kümesini kullanır. Time Series Insights ortamınızı ve veri kümesi nasıl oluşturabileceğinize dair bilgi için şu makaleyi okuyun:

> [!div class="nextstepaction"]
> [Öğretici: Azure zaman serisi görüşleri ortamı oluşturma](tutorial-create-populate-tsi-environment.md)

Ya da zaman serisi görüşleri örnek uygulama kaynak dosyaları görüntüleyin:

> [!div class="nextstepaction"]
> [Zaman serisi görüşleri örnek uygulama deposunu](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)

Time Series Insights istemci API başvuru belgeleri okuyun:

> [!div class="nextstepaction"]
> [Zaman serisi öngörüleri API başvuru belgeleri](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)
