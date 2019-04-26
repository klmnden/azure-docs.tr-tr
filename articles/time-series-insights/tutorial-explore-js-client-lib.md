---
title: 'Öğretici: Azure zaman serisi öngörüleri JavaScript istemci kitaplığını keşfedin | Microsoft Docs'
description: Azure Time Series Insights JavaScript istemci kitaplığını ve ilgili programlama modelini öğrenin.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 06/05/2018
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: 5e3005eb8f548e562e037431ae5fd89f82ec2100
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60330848"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Öğretici: Azure Time Series Insights JavaScript istemci kitaplığını keşfetme

Web geliştiricilerin Time Series Insights (TSI) içinde depolanan verileri sorgulamasına ve görselleştirmesine yardımcı olmak için, JavaScript D3 tabanlı İstemci kitaplığı geliştirilmiştir.  Bu öğretici, örnek bir web uygulaması kullanarak TSI İstemci kitaplığının ve ilgili programlama modelinin keşfinde size yol gösterir.

Bu öğreticideki konular, kitaplıkla deneme yapma, TSI verilerine erişimi anlama ve verileri işlemek ve görselleştirmek üzere grafik denetimlerini kullanma fırsatı sağlar. Hedef, kitaplığı kendi web uygulamanızda kullanabilmeniz için size gerekli ayrıntıları sağlamaktır.

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> * TSI örnek uygulaması.
> * TSI JavaScript istemci kitaplığı.
> * TSI verilerini görselleştirmek için örnek uygulamanın kitaplığı nasıl kullandığı.

> [!NOTE]
> Time Series Insights örnek uygulama kaynak dosyaları bulunabilir, sağlanan [GitHub örnek deposundan](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).

## <a name="video"></a>Video: 

### <a name="in-this-video-we-introduce-the-open-source-time-series-insights-javascript-sdkbr"></a>Bu videoda, açık kaynak Time Series Insights JavaScript SDK’sı sunulmaktadır.</br>

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide dahil olmak üzere Çoğu modern web tarayıcılarında bulunamadı "Geliştirici Araçları" özelliğini (DevTools veya F12 olarak da bilinir) [Microsoft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [FireFox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/)ve diğerleri. Bu özelliği henüz tanımıyorsanız, devam etmeden önce tarayıcınızda incelemek isteyebilirsiniz.

## <a name="time-series-insights-sample-application"></a>Time Series Insights örnek uygulaması

Bu öğreticide, TSI JavaScript istemci kitaplığının kullanımı da dahil olmak üzere uygulamanın ardındaki kaynak kodu incelemek için Time Series Insights örnek uygulaması kullanılır. Örnek, kitaplığın nasıl kullanıldığını gösteren bir tek sayfalı uygulamadır (SPA). Örnekte, örnek bir TSI ortamındaki verileri sorgulama ve görselleştirme işlemi gösterilmektedir.

1. [Time Series Insights örnek uygulamasına](https://insights.timeseries.azure.com/clientsample) göz atın. Oturum açma istemi ile birlikte aşağıdaki görüntüye benzer bir sayfa görürsünüz:

   ![TSI İstemcisi örnek oturum açma istemi](media/tutorial-explore-js-client-lib/tcs-sign-in.png)

2. **Oturum Aç**’ı seçin ve kimlik bilgilerinizi girin ya da seçin. Kurumsal hesap veya kuruluş hesabı (Azure Active Directory) veya kişisel hesap (Microsoft Hesabı veya MSA) kullanın.

   ![TSI İstemci örneği kimlik bilgileri istemi](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)

3. Başarıyla oturum açtıktan sonra aşağıdaki görüntüye benzer bir sayfa görürsünüz. Sayfada TSI verileriyle doldurulan örnek grafiklerin çeşitli stilleri gösterilmektedir. Kullanıcı hesabınız ve **Oturumu kapat** seçeneği, sağ üst köşede görünür:

   ![Oturum açtıktan sonra TSI İstemci örneği ana sayfası](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)

### <a name="page-source-and-structure"></a>Sayfa kaynağı ve yapısı

İlk olarak, şimdi görüntüleyin [HTML ve JavaScript kaynak kodu](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html) tarayıcınızda işlenen sayfanın arkasına olmasıdır. Öğelerin tümüne değinmeyeceğiz, ama önemli bölümleri öğrenebilir ve sayfanın nasıl çalıştığı hakkında fikir edinebilirsiniz:

1. Tarayıcınızda **Geliştirici Araçları**’nı açın. Geçerli sayfayı oluşturan HTML öğelerini inceleyin (HTML veya DOM ağacı olarak da bilinir).

2. `<head>` ve `<body>` öğelerini genişletip aşağıdaki bölümlere dikkat edin:

   - `<head>` öğesi altında, sayfanın işlevine yardımcı olacak ek dosyaları çeken öğeler bulursunuz:
     - Azure Active Directory Authentication Library **adal.min.js** (aynı zamanda ADAL olarak bilinir) başvurusu için kullanılan bir `<script>` öğesi. ADAL, API’lere erişim için OAuth 2.0 kimlik doğrulaması (oturum açma) ve belirteç alımı sağlayan bir JavaScript kitaplığıdır.
     - **sampleStyles.css** ve **tsiclient.css** gibi stil sayfaları (aynı zamanda CSS olarak bilinir) için birden fazla `<link>` öğesi. Stil sayfaları renk, yazı tipi, boşluk vb görsel sayfası stil ayrıntılarını denetlemek için kullanılır.
     - TSI İstemcisi JavaScript kitaplığı **tsiclient.js** başvurusu için kullanılan bir `<script>` öğesi. Kitaplık, TSI hizmet API’lerini çağırmak ve sayfa üzerinde grafik denetimleri oluşturmak için sayfa tarafından kullanılır.

     >[!NOTE]
     > ADAL JavaScript kitaplığının kaynak kodu [azure-activedirectory-library-for-js repository](https://github.com/AzureAD/azure-activedirectory-library-for-js) konumunda bulunabilir.
     > TSI İstemci JavaScript kitaplığı [tsiclient deposunda](https://github.com/Microsoft/tsiclient) bulunabilir.

   - `<body>` öğesi altında, sayfadaki öğelerin düzenini tanımlayan kapsayıcı işlevi gören `<div>` öğelerini ve başka bir `<script>` öğesi bulursunuz:
     - İlk `<div>` öğesi, **Log In** iletişim kutusunu (`id="loginModal"`) belirtir.
     - İkinci `<div>` öğesi şunlar için üst öğe işlevi görür:
       - Sayfanın üst kısmında (`class="header"`) doğru durum iletileri ve oturum açma bilgileri için kullanılan üst bilgi `<div>` öğesi.
       - Grafiklerin tümü (`class="chartsWrapper"`) dahil olmak üzere kalan sayfa gövdesi öğeleri için `<div>` öğesi.
       - Sayfayı denetlemek için kullanılan tüm JavaScript'leri içeren `<script>` bölümü.

   [![Geliştirici Araçları ile TSI İstemcisi örneği](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

3. `<div class="chartsWrapper">` öğesini genişlettiğinizde daha fazla `<div>` öğesi bulabilirsiniz. Bu öğeler her bir grafik denetimi örneğini konumlandırmak için kullanılır. Her grafik örneği için bir tane olmak üzere birkaç `<div>` öğesi çiftine dikkat edin:

   - İlk (`class="rowOfCardsTitle"`) öğe, grafiğin neyi gösterdiğini özetleyen açıklayıcı bir başlık içerir. Örneğin: "Tam boyutlu göstergeleri içeren statik çizgi grafikler."
   - İkinci (`class="rowOfCards"`) öğe, bir üst öğedir ve bir satırın içindeki gerçek grafik denetimlerini konumlandıran ek alt `<div>` öğelerini içerir.

   ![Gövde div öğeleri](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)

4. Şimdi, doğrudan `<div class="chartsWrapper">` öğesinin altındaki `<script type="text/javascript">` öğesini genişletin. Kimlik doğrulama, TSI hizmet API'lerini çağırma ve grafik denetimlerini işleme gibi daha bir çok şey için tüm sayfa mantığını işlemek amacıyla kullanılan sayfa düzeyi JavaScript bölümünün başına dikkat edin:

   ![Gövde betiği](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)

## <a name="tsi-javascript-client-library-concepts"></a>TSI JavaScript istemci kitaplığı kavramları

Ayrıntılı olarak gözden geçirmesek de, temelde TSI İstemci kitaplığı **tsclient.js** iki önemli kategorinin soyutlamasını sağlar:

- **TSI sorgu API'leri çağırmak için sarmalayıcı yöntemleri**: Sorgu olanak tanıyan TSI verilerini toplama ifadesi kullanarak REST API'ler. Metotlar, kitaplığın `TsiClient.Server` ad alanı altında düzenlenmiştir.
- **Oluşturma ve doldurma denetimleri grafik türleri çeşitli yöntemleri**: TSI çizmek için kullanılan yöntemler bir web sayfasında veri toplama. Metotlar, kitaplığın `TsiClient.UX` ad alanı altında düzenlenmiştir.

Aşağıdaki kavramlar evrenseldir ve genel olarak TSI İstemci kitaplığı API'lerine uygulanabilir.

### <a name="authentication"></a>Kimlik Doğrulaması

Daha önce de belirtildiği gibi, bu örnek bir SPA’dır ve kullanıcı kimlik doğrulaması için ADAL'deki OAuth 2.0 desteğini kullanır. Betiğin bu bölümünde ilgi çekici bazı noktalar:

1. Kimlik doğrulamasında ADAL kullanırken istemci uygulamasının kendisini Azure Active Directory (Azure AD) uygulama kayıt defterine kaydetmesi gerekir. Bir SPA olarak bu uygulama "örtülü" OAuth 2.0 kimlik doğrulaması yapma akışı kullanacak şekilde kaydedilir. Buna bağlı olarak, uygulama akışa katılmak için istemci kimliği GUID'si (`clientId`) ve yeniden yönlendirme URI'si (`postLogoutRedirectUri`) gibi bazı kayıt özelliklerini çalışma zamanında belirtir.

2. Daha sonra, uygulama Azure AD'den bir "erişim belirteci" ister. Erişim belirteci sınırlı bir izin kümesi ve belirli bir hizmet/API tanımlayıcısı https://api.timeseries.azure.com için verilir. Hizmet/API tanımlayıcısı aynı zamanda belirteç "hedef kitlesi" olarak bilinir. Belirteç izinleri, oturum açmış kullanıcı adına verilir. Hizmet/API için tanımlayıcı, uygulamanın Azure AD kaydında yer alan bir diğer özelliktir. ADAL erişim belirtecini uygulamaya döndürdükten sonra, bu TSI hizmet API'lerine erişim sırasında "taşıyıcı belirteç" olarak kullanılır.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-204&highlight=4-9,36-39)]

### <a name="control-identification"></a>Denetim tanımlama

Daha önce de açıklandığı gibi, `<body>` öğesi içindeki `<div>` öğeleri sayfada gösterilen tüm grafik denetimlerinin düzenini sağlar. Her `<div>` öğesi, bir `id` özelliği de dahil olmak üzere grafik denetiminin yerleşimi ve görsel özniteliklerine yönelik özellikleri belirtir. `id` özelliği benzersiz tanımlayıcı sağlar. Bu benzersiz tanımlayıcı JavaScript kodunda işleme ve güncelleştirme için her denetimi tanımlamak ve bağlamak amacıyla kullanılır.

### <a name="aggregate-expressions"></a>Toplama ifadeleri

TSI İstemci kitaplığı API'leri toplama ifadelerini yoğun olarak kullanır. Toplama ifadesi bir veya birden çok "arama terimini" oluşturma becerisi sağlar. API'ler, [Time Series Insights gezgininin](https://insights.timeseries.azure.com/demo) arama aralığını, where koşulunu, ölçümleri ve split-by değerini kullanma şekline benzer biçimde tasarlanmıştır. Çoğu kitaplık API'si, hizmet tarafından TSI veri sorgusunu oluşturmak için kullanılan bir toplama ifadeleri dizisi alır.

### <a name="call-pattern"></a>Çağrı deseni

Grafik denetimlerinin doldurulması ve işlenmesi genel bir desene uyar. TSI örnek uygulama denetimlerini başlatan ve yükleyen sayfadaki JavaScript'in her yerinde bu desenin kullanıldığını bulabilirsiniz:

1. Bir veya birden çok TSI toplama ifadesini barındıracak bir `array` bildirin:

   ```javascript
   var aes =  [];
   ```

2. 1 ile n arası toplama ifadesi nesnelerini oluşturun ve bunları toplama ifadesi dizisine ekleyin:

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```

   **aggregateExpression parametreleri**

   | Parametre | Açıklama | Örnek |
   | --------- | ----------- | ------- |
   | `predicateObject` | Veri Filtreleme ifadesi. |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | Kullanılan ölçümün özellik adı. | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | Ölçü özelliği için istenen toplamalar. | `['avg', 'min']` |
   | `searchSpan`      | Toplama ifadesinin süresi ve aralık boyutu. | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | Bölme ölçütü olarak kullanmak istediğiniz dize özelliği (isteğe bağlı; null olabilir). | `{property: 'Station', type: 'String'}` |
   | `color`         | İşlemek istediğiniz nesnelerin rengi. | `'pink'` |
   | `alias`           | Toplama ifadesinin kolay adı. | `'Factory3Temperature'` |
   | `contextMenuActions` | Görselleştirmede zaman serisi nesnelerine bağlı olacak eylem dizisi (isteğe bağlı). | Daha fazla bilgi için açılan bağlam menüleri Gelişmiş özellikleri bölümünde bakın. |

3. Toplama verilerini istemek için `TsiClient.Server` API'lerini kullanarak bir TSI sorgusu çağırın:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **getAggregates parametreleri**

   | Parametre | Açıklama | Örnek |
   | --------- | ----------- | ------- |
   | `token`     | TSI API'si için erişim belirteci. |  `authContext.getTsiToken()` Daha fazla bilgi için [kimlik doğrulama bölümüne](#authentication) bakın. |
   | `envFQDN`   | TSI ortamı için tam etki alanı adı (FQDN). | Azure portalından, örneğin: `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | TSI sorgu ifadeleri dizisi. | Daha önce açıklandığı gibi `aes` değişkenini kullanın: `aes.map(function(ae){return ae.toTsx()}`. |

4. TSI sorgusundan döndürülen sıkıştırılmış sonucu görselleştirme amacıyla JSON nesnelerine dönüştürün:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

5. `TsiClient.UX` API'lerini kullanarak bir grafik denetimi oluşturun ve bunu sayfadaki `<div>` öğelerinden birine bağlayın:

   ```javascript
   var lineChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

6. Grafik denetimini dönüştürülmüş JSON veri nesneleriyle doldurun ve sayfadaki denetimi işleyin:

   ```javascript
   lineChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="rendering-controls"></a>Denetimleri işleme

TSI İstemci kitaplığı şu anda sekiz benzersiz analiz denetimi kullanıma sunar: çizgi grafik, pasta grafik, çubuk grafik, ısı haritası, hiyerarşi denetimleri, erişilebilir kılavuz, bağımsız olay zaman çizelgeleri ve durum geçişi zaman çizelgeleri.

### <a name="line-bar-pie-chart-examples"></a>Çizgi, çubuk, pasta grafik örnekleri

Uygulamada gösterilen bazı standart grafik denetimlerinin ardındaki koda ve denetimleri oluşturmak için kullanılan programlama modeline/desenlerine bakın. Özel olarak, `chart3`, `chart4` ve `chart5` kimlik değerlerine sahip denetimleri işleyen `// Example 3/4/5` açıklamasının altındaki HTML bölümünü inceleyin.

[Sayfa kaynağı ve yapısı bölümünün](#page-source-and-structure) 3. adımından anımsayacağınız gibi, grafik denetimleri sayfada satırlar halinde yerleştirilir ve her birinin açıklayıcı bir başlık satırı vardır. Bu örnekte, doldurulan üç grafik "Multiple Chart Types From the Same Data" başlığı `<div>` öğesi altında yer alır ve başlığın altındaki üç `<div>` öğesine bağlıdır:

[!code-html[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

Aşağıdaki JavaScript kodu bölümünde, TSI toplama ifadelerini oluşturmak, bunları kullanarak TSI verilerini sorgulamak ve üç grafiği işlemek için daha önce belirtilen desen kullanılır. İlgili grafikleri oluşturmak için `tsiClient.ux` ad alanından kullanılan üç türe (`LineChart`, `BarChart` ve `PieChart`) dikkat edin. Ayrıca, grafiklerin üçünün de aynı toplama ifadesi verilerini (`transformedResult`) kullanabildiğine de dikkat edin:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

Üç grafik işlendiğinde aşağıdaki gibi görünür:

[![Aynı verilerle birden çok grafik türü](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Gelişmiş özellikler

TSI İstemcisi kitaplığı, yararlanmak isteyebileceğiniz bazı isteğe bağlı gelişmiş özellikler de ortaya koyar.

### <a name="states-and-events"></a>Durumlar ve olaylar

Sağlanan gelişmiş özelliklerin bir örneği, grafiklere durum geçişleri ve bağımsız olaylar ekleyebilme özelliğidir. Bu özellik olayları vurgulamak, uyarıda bulunmak ve açık/kapalı gibi durum geçişleri yapmak için yararlı olur.

`// Example 10` açıklamasının altında yer alan HTML bölümünün ardındaki koda bakın. Kod, "Line Charts with Multiple Series Types" başlığı altındaki bir çizgi denetimini işler ve `chart10` kimlik değeriyle `<div>` öğesine bağlar.

1. İlk olarak, izlenecek durum değişikliği öğelerini barındırmak üzere `events4` adlı bir yapı tanımlanır. Yapı şunları içerir:

   - `Component States` adlı bir dize anahtarı.
   - Durumları temsil eden bir değer nesneleri dizisi. Her nesne şunları içerir:
     - JavaScript ISO zaman damgası içeren bir dize anahtarı.
     - Durumun özelliklerini içeren bir dizi: bir renk ve bir açıklama.

2. Sonra, izlenecek olay öğeleri dizisini barındıran "Olaylar" için `events5` yapısı tanımlanır. Dizi yapısı, `events4` için belirtilen yapıyla aynı şekildedir.

3. Son olarak çizgi grafiği işlenir ve grafik seçenekleri parametreleriyle iki yapıdan geçer: `events:` ve `states:`. `tooltip:`, `theme:` ve `grid:` belirtmeye yönelik diğer seçenek parametrelerine dikkat edin.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

Görsel olarak, olayları belirtmek için baklava şeklinde işaretçiler/açılan pencereler kullanılır ve zaman ölçeğiyle birlikte renkli çubuklar/açılan pencereler de durum değişikliklerini belirtir:

[![Birden Çok Seri Türü İçeren Çizgi Grafikler](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Açılır menüler

Gelişmiş işlevselliğin bir başka örneği ise özel açılır menülerdir (sağ tıklanan açılır menüler). Özel açılır menüler, uygulamanızın kapsamı dahilinde sıradaki eylemleri ve mantıksal adımları etkinleştirmek için yararlıdır.

`// Example 13/14/15` açıklamasının altında yer alan HTML bölümünün ardındaki koda bakın. Bu kod başlangıçta "Line Chart with Context Menu to Create Pie/Bar Chart" (Pasta/Çubuk Grafik Oluşturmak için Bağlam Menüsüyle Çizgi Grafik) başlığı altında bir çizgi grafik işler ve grafik, `chart13` kimlik değeriyle `<div>` öğesine bağlıdır. Bağlam menülerini kullanarak, çizgi grafik dinamik olarak `chart14` ve `chart15` kimlikleriyle `<div>` öğelerine bağlı bir pasta ve çubuk grafik oluşturma olanağı sağlar. Buna ek olarak, hem pasta hem de çubuk grafikler de kendi özelliklerini etkinleştirmek için bağlam menülerini kullanır: sırasıyla pasta grafikten çubuk grafiğe veri kopyalama ve grafik verilerini tarayıcıya yazdırma özellikleri.

1. İlk olarak bir dizi özel eylem tanımlanır. Her eylem bir veya daha fazla öğe ile bir dizi içerir. Her öğe tek bir bağlam menüsü öğesini tanımlar:

   - `barChartActions`: Bu eylem, tek bir öğeyi tanımlamak için bir öğe içeriyorsa pasta grafiği için bağlam menüsünü tanımlar:
     - `name`: Menü öğesi için kullanılan metin: "Konsol parametreleri yazdırın."
     - `action`: Menü öğesi ile ilişkili eylem. Eylem her zaman anonim bir işlevdir ve grafiği oluşturmak için kullanılan toplama ifadesi temelinde üç bağımsız değişken alır. Bu örnekte, bağımsız değişkenler tarayıcı konsol penceresine yazılır:
       - `ae`: Toplama ifadesi dizi.
       - `splitBy`: SplitBy değeri.
       - `timestamp`: Zaman damgası.

   - `pieChartActions`: Bu eylem, tek bir öğeyi tanımlamak için bir öğe içeriyorsa çubuk grafik için bağlam menüsünü tanımlar. Şekil ve şema önceki `barChartActions` öğesi ile aynıdır, ama `action` işlevinin önemli ölçüde farklı olduğuna dikkat edin çünkü bu işlev çubuk grafiği başlatır ve işler. Ayrıca çalışma zamanında menü öğesi açıldığında geçirilen toplama ifadesi dizisini belirtmeye yönelik `ae` bağımsız değişkeninin kullanıldığına dikkat edin. İşlev, `barChartActions` bağlam menüsüyle birlikte `ae.contextMenu` özelliğini de ayarlar.
   - `contextMenuActions`: Bu eylem, üç menü öğeleri tanımlamak için üç öğeleri içeren çizgi grafik için bağlam menüsünü tanımlar. Her öğenin şekli ve şeması, açıklanan önceki öğelerle aynıdır. Aynı `barChartActions` öğesi gibi, ilk öğe tarayıcı konsol penceresine üç işlev bağımsız değişkenini yazar. `pieChartActions` öğesine benzer biçimde, ikinci iki öğe sırasıyla pasta ve çubuk grafiklerini başlatır ve işler. İkinci iki öğe sırasıyla `pieChartActions` ve `barChartActions` bağlam menüleriyle `ae.contextMenu` özelliklerini de ayarlar.

2. Sonra, iki toplama ifadesi `aes` toplama ifadesi dizisinin içine gönderilir ve her öğe için `contextMenuActions` dizisi belirtilir. Bu ifadeler, çizgi grafik denetimiyle kullanılır.

3. Son olarak, başlangıçta yalnızca çizgi grafik işlenir; çalışma zamanında hem pasta hem de çubuk grafik bu çizgi grafikten işlenebilir.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

Ekran görüntüsünde ilgili açılan bağlam menüleriyle birlikte grafikler gösterilir. Pasta ve çubuk grafikler, çizgi grafiğin bağlam menüsü seçeneklerinin kullanılmasıyla dinamik olarak oluşturulmuştur.

[![Pasta/Çubuk Grafik Oluşturmak için Bağlam Menüsüyle Çizgi Grafik](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Fırçalar

Fırçalar, yakınlaştırma ve keşfetme gibi eylemleri tanımlamak üzere zaman aralığının kapsamını belirlemek için kullanılır.

Açılan bağlam menüleri açıklayan önceki "Çizgi grafik ile bağlam menüsü için oluşturma Pasta/Çubuk Grafik" örnekte Fırçalar göstermek için kullanılan kod gösterilmektedir.

1. Fırça eylemleri, fırça için bir dizi özel eylem tanımlaması bakımından bağlam menüsüne benzer. Her eylem bir veya daha fazla öğe ile bir dizi içerir. Her öğe tek bir bağlam menüsü öğesini tanımlar:
   - `name`: Menü öğesi için kullanılan metin: "Konsol parametreleri yazdırın."
   - `action`: Her zaman iki bağımsız değişken alan anonim bir işlevdir menü öğesi ile ilişkili eylem. Bu örnekte, bağımsız değişkenler tarayıcı konsol penceresine yazılır:
      - `fromTime`: "Kimden" zaman damgası fırça seçimi.
      - `toTime`: "Hedef" zaman damgası fırça seçimi.

2. Fırça eylemleri başka bir grafik seçeneği özelliği olarak eklenir. `brushContextMenuActions: brushActions` özelliğinin `linechart.Render` çağrısına geçirildiğine dikkat edin.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

![Fırçalarla pasta/çubuk grafik oluşturmak için bağlam menüsüyle çizgi grafik](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)

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