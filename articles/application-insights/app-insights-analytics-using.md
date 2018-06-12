---
title: Analizi - Azure Application Insights güçlü arama aracını kullanarak | Microsoft Docs
description: 'Analizi, Application Insights, güçlü tanılama arama aracını kullanma. '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: c3b34430-f592-4c32-b900-e9f50ca096b3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2017
ms.reviewer: danha
ms.author: mbullwin
ms.openlocfilehash: 7f8f49cf88bda8e485d2365281c13680ef796196
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295528"
---
# <a name="using-analytics-in-application-insights"></a>Application Insights'ta Analytics kullanma
[Analytics](app-insights-analytics.md) güçlü arama özelliğidir [Application Insights](app-insights-overview.md). Bu sayfaları günlük analizi sorgu dili açıklanmaktadır.

* **[Tanıtım videosunu izleyin](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Benzetimli verilerimizi Analytics sürücüde test](https://analytics.applicationinsights.io/demo)**  uygulamanızı veriler Application Insights'a henüz değil gönderiliyorsa.

## <a name="open-analytics"></a>Açık analizi
Giriş kaynağının, uygulamanızın Application ınsights'ta, Analytics'ı tıklatın.

![Portal.Azure.com açın, Application Insights kaynağınıza açın ve analizi'ı tıklatın.](./media/app-insights-analytics-using/001.png)

Satır içi öğretici neler yapabileceğiniz hakkında fikir edinmek sağlar.

Var olan bir [burada daha kapsamlı Turu](app-insights-analytics-tour.md).

## <a name="query-your-telemetry"></a>Telemetrinizi sorgu
### <a name="write-a-query"></a>Bir sorgu yazın
![Şema görüntüleme](./media/app-insights-analytics-using/150.png)

Sol tarafta listelenen tablolardan herhangi birini adları ile başlar (veya [aralığı](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/range-operator) veya [UNION](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/union-operator) işleçleri). Kullanım `|` bir kanalı oluşturmak için [işleçleri](https://docs.loganalytics.io/docs/Learn/References/Useful-operators). 

IntelliSense işleçler ve kullanabileceğiniz ifade öğeleri ister. Bilgi simgesini tıklatın (veya CTRL + ARA ÇUBUĞU tuşlarına basın) daha uzun bir açıklama ve her öğesinin nasıl kullanılacağını örnekleri alınamadı.

Bkz: [Analytics dil Turu](app-insights-analytics-tour.md) ve [dil başvurusu](app-insights-analytics-reference.md).

### <a name="run-a-query"></a>Sorgu çalıştırma
![Bir sorgu çalıştırılarak](./media/app-insights-analytics-using/130.png)

1. Sorguda tek satır sonları kullanabilirsiniz.
2. İmleci içinde veya sorguyu çalıştırmak istediğiniz sonuna yerleştirin.
3. Sorgunuz zaman aralığını denetleyin. (Bunu değiştirmek veya kendi dahil ederek geçersiz kılma [ `where...timestamp...` ](https://docs.loganalytics.io/docs/Learn/Tutorials/Date-and-time-operations) yan tümcesinde, sorgunuzu.)
3. Sorguyu çalıştırmak için Git'i tıklatın.
4. Boş satırlar sorgunuzda koymayın. Boş satırlar ile ayırarak bir sorgu sekmede birkaç ayrılmış sorguları kullanmaya devam edebilir. İmleç olan sorguyu çalıştırır.

### <a name="save-a-query"></a>Bir sorguyu kaydetme
![Bir sorguyu kaydetme](./media/app-insights-analytics-using/140.png)

1. Geçerli sorgu dosyasını kaydedin.
2. Kaydedilmiş sorgu dosyasını açın.
3. Yeni bir sorgu dosyası oluşturun.

## <a name="see-the-details"></a>Ayrıntıları görün
Herhangi bir satırın özelliklerini tam listesini görmek için sonuçları genişletin. Daha fazla yapılandırılmış değer - örneğin olan herhangi bir özelliği, özel boyutları veya listeleyen bir özel durum yığını genişletebilirsiniz.

![Bir satır genişletin](./media/app-insights-analytics-using/070.png)

## <a name="arrange-the-results"></a>Sonuçları düzenleyin
Sıralama, filtre, sayfalara bölme ve Grup sorgudan döndürülen sonuçları.

> [!NOTE]
> Sıralama, gruplandırma ve tarayıcıda filtreleme sorgunuzu yeniden çalıştırmayın. Bunlar yalnızca son sorgu tarafından döndürülen sonuçları yeniden düzenleyin. 
> 
> Sonuçların döndürülmesini önce sunucuda bu görevleri gerçekleştirmek için sorgunuzu ile yazma [sıralama](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator), [özetlemek](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) ve [burada](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) işleçler.
> 
> 

Bkz, onları yeniden sıralamak için sütun üst bilgileri sürükleyin ve bunların kenarlıklarını sürükleyerek sütunları yeniden boyutlandırmak için istediğiniz sütunları seçin.

![Sütunları düzenleme](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a>Sıralama ve filtreleme öğeleri
Sonuçları head bir sütunun tıklayarak sıralayın. Başka bir şekilde sıralamak için tekrar tıklatın ve üçüncü tıklatın özgün sorgu tarafından döndürülen sıralama için dönmek için zaman.

Aramanızı daraltmak için filtre simgesini kullanın.

![Sütunları sıralama ve filtreleme](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a>Öğeleri gruplandırma
Birden fazla sütuna göre sıralamak için gruplandırma kullanın. İlk olarak etkinleştirmeniz ve sütun üst bilgileri tablonun üstündeki alanına sürükleyin.

![Grup](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a>Bazı sonuçları eksik mi?

Beklediğiniz tüm sonuçları görmüyorsanız düşünüyorsanız, birkaç olası nedeni vardır.

* **Zaman aralığı filtresi**. Varsayılan olarak, yalnızca son 24 saat sonuçları görürsünüz. Kaynak tablolardan alınan sonuçları aralığını sınırlar otomatik bir filtre yok. 

    Bununla birlikte, zaman aralığını değiştirebilirsiniz açılan menüsünü kullanarak filtre.

    Ya da kendi ekleyerek otomatik aralık geçersiz kılabilirsiniz [ `where  ... timestamp ...` yan tümcesi](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) sorgunuzu içine. Örneğin:

    `requests | where timestamp > ago('2d')`

* **Sonuç sınırı**. Yaklaşık 10 k satır portaldan döndürülen sonuçlar üzerinde bir sınırı yoktur. Sınırın gidin, bir uyarı gösterir. Bu durumda, sonuçlarınızı tablosundaki sıralama her zaman, tüm gerçek ilk veya son sonuçları göstermeyecektir. 

    Sınır basarsa önlemek için iyi bir uygulamadır. Zaman aralığı filtresi veya işleçleri gibi kullanın:

  * [zaman damgası tarafından ilk 100](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-operator) 
  * [100 alın](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/take-operator)
  * [Özetle ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) 
  * [Burada zaman damgası > ago(3d)](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator)

(10'dan fazla k satırları istiyorsunuz? Kullanmayı [sürekli verme](app-insights-export-telemetry.md) yerine. Analytics alınırken ham verileri yerine analiz için tasarlanmıştır.)

## <a name="diagrams"></a>Diyagramlar
İstediğiniz diyagram türünü seçin:

![Bir diyagram türü seçin](./media/app-insights-analytics-using/230.png)

Sağ türlerinin birkaç sütun varsa, x ve y eksenleri ve bir sütun sonuçlarına göre bölme boyutlarının seçebilirsiniz.

Varsayılan olarak, sonuçlar başlangıçta tablo olarak görüntülenir ve diyagram el ile seçin. Ancak kullanabilirsiniz [yönergesi işleme](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) bir diyagram seçmek için bir sorgu sonunda.

### <a name="analytics-diagnostics"></a>Analytics tanılama


Bir timechart üzerinde ani ani veya verilerinizi adımda ise satır vurgulanan noktasında görebilirsiniz. Bu Analytics tanılama ani değişiklik filtre özellikleri bileşimini belirledi gösterir. Üzerine filtre daha fazla ayrıntı almak ve filtrelenmiş sürümü görmek için tıklatın. Bu değişikliği neyin neden olduğunu belirlemenize yardımcı olabilir. 

[Analytics Tanılama hakkında daha fazla bilgi edinin](app-insights-analytics-diagnostics.md)


![Analytics tanılama](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-to-dashboard"></a>Panoya sabitle
Bir diyagram veya tablo birine sabitleyebilirsiniz, [panolar paylaşılan](app-insights-dashboards.md) -PIN tıklamanız yeterlidir. 

![PIN'ı tıklatın](./media/app-insights-analytics-using/pin-01.png)

Bu, performans veya web hizmetlerinizi kullanımını izlemenize yardımcı olması için bir Pano birlikte geçirdiğinizde, diğer ölçümleri yanında oldukça karmaşık bir analiz dahil edebilirsiniz, anlamına gelir. 

Dört veya daha az sütun varsa Pano tabloya sabitleyebilirsiniz. Yalnızca üst yedi satır görüntülenir.

### <a name="dashboard-refresh"></a>Pano yenileme
Panosuna sabitlediğiniz grafik sorgu yaklaşık olarak saatte yeniden çalıştırarak otomatik olarak yenilenir. Yenile düğmesini tıklatabilirsiniz.

### <a name="automatic-simplifications"></a>Otomatik basitleştirme

Belirli bir panoya Sabitle basitleştirme grafiğe uygulanır.

**Kısıtlama süresi:** sorgular son 14 gün için otomatik olarak sınırlı. Sorgunuz içeriyorsa gibi aynı etkisidir `where timestamp > ago(14d)`.

**Bin sayısı kısıtlaması:** çok ayrık depo (genellikle bir çubuk grafiği) sahip bir grafik görüntüler, daha az doldurulan depo otomatik olarak gruplandırılır "diğer" tek bir depo. Örneğin, bu sorgu:

    requests | summarize count_search = count() by client_CountryOrRegion

analytics'te şöyle görünür:

![İle uzun tail grafik](./media/app-insights-analytics-using/pin-07.png)

Ancak bir panoya Sabitle, şöyle görünür:

![İle sınırlı depo grafik](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-to-excel"></a>Excel'e Aktar
Bir sorgu çalıştırdıktan sonra bir .csv dosyası indirebilirsiniz. Tıklatın **Excel'e,**.

## <a name="export-to-power-bi"></a>Power BI’a aktarma
İmleç sorguda koyun ve seçin **verme, Power BI**.

![Power BI Analytics'ten dışarı aktarma](./media/app-insights-analytics-using/240.png)

Power BI'da sorgu çalıştırın. Bir zamanlamaya göre yenilemek için ayarlayabilirsiniz.

Power BI sayesinde, çok çeşitli kaynaklardan verileri bir araya getirme panolar oluşturabilirsiniz.

[Power BI için dışarı aktarma hakkında daha fazla bilgi edinin](app-insights-export-power-bi.md)

## <a name="deep-link"></a>Ayrıntılı bağlantı

Bir bağlantı altında **verme, paylaşım bağlantı** , başka bir kullanıcıya gönderebilirsiniz. Kullanıcının sağlanan [, kaynak grubuna erişim](app-insights-resources-roles-access-control.md), sorgu Analytics Arabiriminde açılır.

(Sonra sorgu metnini bağlantıdaki görünür "? q =" gzip sıkıştırılmış ve base-64 kodlamalı. Kullanıcılara sağlamak derin bağlantılar oluşturmak üzere kod yazabilirsiniz. Bununla birlikte, Kod Analizi çalıştırmak için önerilen yöntem kullanmaktır [REST API](https://dev.applicationinsights.io/).)


## <a name="automation"></a>Otomasyon

Kullanım [veri erişim REST API](https://dev.applicationinsights.io/) analitik sorguları çalıştırmak için. [Örneğin](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (PowerShell kullanarak):

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

Analytics UI, REST API otomatik olarak herhangi bir zaman damgası sınırlama sorgularınızı için eklemez. Kendi where-büyük yanıtları alma önlemek için yan tümcesi eklemeyi unutmayın.



## <a name="import-data"></a>Veri içeri aktarma

Bir CSV dosyasından veri içeri aktarabilirsiniz. Tablolarla, telemetrisinden katılabilirsiniz statik verileri içeri tipik bir kullanımdır. 

Örneğin, kimliği doğrulanmış kullanıcılar, telemetri bir diğer ad veya karıştırılmış kimliğe göre belirlenirse, diğer adlar için gerçek adlarını eşleyen bir tablo içe aktarılamadı. Bir birleştirme isteği telemetriyi gerçekleştirerek Analytics raporlarda gerçek adlarına göre kullanıcılar tanımlayabilirsiniz.

### <a name="define-your-data-schema"></a>Veri şemanızı tanımlayın

1. Tıklatın **ayarları** (sol üst) ve ardından **veri kaynakları**. 
2. Yönergeleri izleyerek bir veri kaynağı ekleyin. En az 10 satır içermelidir veri örneği sağlamanız istenir. Sonra şema de düzeltin.

Bundan sonra tek tek tablolar almak için kullanabileceğiniz bir veri kaynağı tanımlar.

### <a name="import-a-table"></a>Tablo alma

1. Veri kaynağı tanımınız listeden açın.
2. "Karşıya Yükle" seçeneğini tıklatın ve tablo karşıya yüklemek için yönergeleri izleyin. Bu nedenle otomatikleştirmek kolay ve bu bir REST API çağrısı içerir. 

Tablonuz analitik sorguları kullanmak için kullanıma sunulmuştur. Analizleri görünür 

### <a name="use-the-table"></a>Tabloyu kullanın

Şimdi veri kaynağı tanımınız varsayın `usermap`, ve iki alan olduğunu `realName` ve `user_AuthenticatedId`. `requests` Da tablolu adında bir alan `user_AuthenticatedId`, bunları katılmaya kolaydır:

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
Ortaya çıkan tabloda istekleri başka bir sütuna sahip `realName`.

### <a name="import-from-logstash"></a>LogStash alma

Kullanırsanız [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), günlüklerinizi sorgulamak için Analytics kullanabilirsiniz. Kullanım [veri analizi kanallar eklentisi](https://github.com/Microsoft/logstash-output-application-insights). 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

