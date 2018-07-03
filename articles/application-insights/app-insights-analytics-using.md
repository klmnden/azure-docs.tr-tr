---
title: Analytics - Azure Application Insights'ın güçlü bir arama aracı kullanarak | Microsoft Docs
description: "Analytics, Application Insights'ın güçlü tanılama arama araç kullanımı "
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
ms.date: 07/02/2018
ms.reviewer: danha
ms.author: mbullwin
ms.openlocfilehash: aa86e2f3b1fb147ab167c948475a5207693143c2
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341571"
---
# <a name="using-analytics-in-application-insights"></a>Application Insights Analytics kullanma
[Analytics](app-insights-analytics.md) güçlü arama özelliğidir [Application Insights](app-insights-overview.md). Bu sayfalar, Log Analytics sorgu dili açıklanmaktadır.

* **[Tanıtım videosunu izlemek](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Test sürüşü Analytics sanal veri çubuğunda](https://analytics.applicationinsights.io/demo)**  uygulamanızın verilerini Application Insights'a henüz değil gönderiliyorsa.

## <a name="open-analytics"></a>Açık analizi
Uygulamanızın giriş kaynaktan Application ınsights, Analytics tıklayın.

![Portal.Azure.com açın, Application Insights kaynağınızı açın ve analiz tıklayın.](./media/app-insights-analytics-using/001.png)

Satır içi öğreticiyi ne yapabileceğiniz hakkında bazı fikirler verir.

Var olan bir [burada daha kapsamlı Turu](app-insights-analytics-tour.md).

## <a name="query-your-telemetry"></a>Telemetrinizi sorgulayın
### <a name="write-a-query"></a>Bir sorgu yazma
![Şema görüntüle](./media/app-insights-analytics-using/150.png)

Sol tarafta listelenen tablolardan adları ile başlayan (veya [aralığı](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/range-operator) veya [birleşim](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/union-operator) işleçleri). Kullanım `|` bir işlem hattı oluşturmak için [işleçleri](https://docs.loganalytics.io/docs/Learn/References/Useful-operators). 

IntelliSense, işleçler ve kullanabileceğiniz ifade öğeleri ile ister. Bilgi simgesine tıklayın (veya CTRL + boşluk tuşlarına basın) daha uzun bir açıklama ve her bir öğesinin kullanımına dair örnekler almak için.

Bkz: [Analytics dil Turu](app-insights-analytics-tour.md) ve [dil başvurusu](app-insights-analytics-reference.md).

### <a name="run-a-query"></a>Bir sorgu çalıştırın
![Bir sorgu çalıştırma](./media/app-insights-analytics-using/130.png)

1. Bir sorgu tek bir satır sonları kullanabilirsiniz.
2. İçinde veya çalıştırmak istediğiniz sorguyu sonunda imleci yerleştirin.
3. Zaman aralığı sorgunuzun denetleyin. (Bunu değiştirebilir veya kendi ekleyerek bunu geçersiz kılmasına [ `where...timestamp...` ](https://docs.loganalytics.io/docs/Learn/Tutorials/Date-and-time-operations) yan tümcesinde sorgunuzu.)
3. Sorguyu çalıştırmak için Git'i tıklatın.
4. Boş satırlar sorgunuzda yerleştirmeyin. Boş satır ile ayırarak birden çok ayrılmış sorguları bir sorgu sekmesinde tutabilirsiniz. İmleç sahip sorgu çalıştırır.

### <a name="save-a-query"></a>Bir sorguyu Kaydet
![Sorgu kaydetme](./media/app-insights-analytics-using/140.png)

1. Geçerli sorgu dosyasını kaydedin.
2. Kaydedilmiş bir sorgu dosyasını açın.
3. Yeni bir sorgu dosyası oluşturun.

## <a name="see-the-details"></a>Ayrıntıları görün
Sonuçları özelliklerinin tam listesini görmek için herhangi bir satırın genişletin. Daha fazla yapılandırılmış değeri - örneğin olan herhangi bir özelliği, özel boyutlar veya listeleyen bir özel durum yığını da genişletebilirsiniz.

![Bir satırı Genişlet](./media/app-insights-analytics-using/070.png)

## <a name="arrange-the-results"></a>Sonuçları düzenleyin
Sıralama, filtreleme, sayfalandırma ve grup, sorgunun döndürdüğü sonuçları.

> [!NOTE]
> Sıralama, gruplandırma ve filtreleme tarayıcıda sorgunuzu yeniden çalıştırmayın. Bunlar yalnızca son sorgunuz tarafından döndürülen sonuçları yeniden düzenleyin. 
> 
> Sonuç döndürmeden önce sunucu bu görevleri gerçekleştirmek için ile sorgunuzu yazma [sıralama](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator), [özetlemek](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) ve [burada](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) işleçleri.
> 
> 

Görmek için bunları yeniden düzenlemek için sütun üst bilgilerini sürükleyin ve kendi kenarlıklarını sürükleyerek sütunları yeniden boyutlandırmak istediğiniz sütunları seçin.

![Sütunları Düzenle](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a>Öğeleri sıralama ve filtreleme
Sonuçlarınızı sütunun baş tıklayarak sıralayın. Başka bir şekilde sıralamak için tekrar tıklayın ve bir üçüncü tıklayın özgün sıralama sorgunuz tarafından döndürülen şekilde geri dönmek için zaman.

Aramanızı daraltmak için filtre simgesini kullanın.

![Sütunları sıralama ve filtreleme](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a>Öğeleri gruplandırma
Birden fazla sütuna göre sıralamak için gruplandırma kullanın. Öncelikle bu özelliği etkinleştirmeniz ve sütun üst bilgilerini yukarıdaki tabloya bir alana sürükleyin.

![Grup](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a>Bazı sonuçlar eksik?

Beklediğiniz sonuçları görmediğiniz düşünüyorsanız, birkaç olası nedeni vardır.

* **Zaman aralığı filtresi**. Varsayılan olarak, yalnızca son 24 saat sonuçlarını göreceksiniz. Kaynak tablolardan alınan sonuçları aralığını sınırlayan bir otomatik filtre yoktur. 

    Ancak, zaman aralığını değiştirebilirsiniz. aşağı açılan menüyü kullanarak filtreleyin.

    Veya kendi ekleyerek otomatik aralığını geçersiz kılabilirsiniz [ `where  ... timestamp ...` yan tümcesi](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) sorgunuzu içine. Örneğin:

    `requests | where timestamp > ago('2d')`

* **Sonuç sınırı**. Şirket portalı'ndan döndürülen sonuçlar yaklaşık 10 k satırlık bir sınır yoktur. Limitini gidin, bir uyarı gösterir. Bu durumda, sonuçlarınızı tabloda sıralama her zaman, tüm gerçek ilk veya son sonuçlar gösterilmez. 

    Sınır ulaşmaktan kaçınmak için iyi bir uygulamadır. Zaman aralığı filtresini kullanın veya gibi işleçler kullanın:

  * [İlk 100 zaman damgası tarafından](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-operator) 
  * [100 Al](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/take-operator)
  * [Özetleme ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) 
  * [Burada zaman damgası > ago(3d)](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator)

(10'dan fazla bin satır istiyorsunuz? Kullanmayı [sürekli dışarı aktarma](app-insights-export-telemetry.md) yerine. Analytics ham verileri yerine analiz için tasarlanmıştır.)

## <a name="diagrams"></a>Diyagramlar
Diyagram istediğiniz türü seçin:

![Bir diyagram türü seçin](./media/app-insights-analytics-using/230.png)

Doğru türlerinde birden çok sütununuz varsa, x ve y eksenleri ve sonuçlarına göre bölmek için Boyutlar sütunu seçebilirsiniz.

Varsayılan olarak, sonuçlar başlangıçta tablo olarak görüntülenir ve diyagram el ile seçin. Ancak, kullanabileceğiniz [işleme yönergesi](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) sonunda bir diyagram seçmek için bir sorgu.

### <a name="analytics-diagnostics"></a>Analytics tanılama


Bir zaman grafiğini üzerinde ani bir değişiklik veya verilerinizi adımda ise bir vurgulanan satırın noktasında görebilirsiniz. Bu analiz tanılama özelliklerinin ani değişiklik filtre belirlediği gösterir. Noktası üzerindeki filtre daha fazla ayrıntı alalım ve filtrelenmiş sürümü görmek için tıklayın. Bu değişikliğin nedenini belirlemenize yardımcı olabilir. 

[Analytics Tanılama hakkında daha fazla bilgi edinin](app-insights-analytics-diagnostics.md)


![Analytics tanılama](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-to-dashboard"></a>Panoya sabitle
Bir diyagram veya tablo birine sabitleyebilirsiniz, [paylaşılan panolar](app-insights-dashboards.md) -PIN tıklamanız yeterlidir. 

![PIN'i tıklatın.](./media/app-insights-analytics-using/pin-01.png)

Bu durum, performans veya web hizmetlerinizi kullanımını izlemenize yardımcı olması için bir Pano birlikte yerleştirdiğinizde, diğer ölçümlerin yanı sıra oldukça karmaşık bir analiz, içerebilir, anlamına gelir. 

Dört veya daha az sütun varsa Tablo panoya sabitleyebilirsiniz. Yalnızca üst yedi satırlar görüntülenir.

### <a name="dashboard-refresh"></a>Pano yenileme
Panoya sabitlenmiş grafik sorgu yaklaşık saatte yeniden çalıştırarak otomatik olarak yenilenir. Yenile düğmesine de tıklayabilirsiniz.

### <a name="automatic-simplifications"></a>Otomatik basitleştirme

Panoya sabitleme belirli basitleştirme grafiğe uygulanır.

**Kısıtlama süresi:** sorgular için son 30 gün içinde otomatik olarak sınırlı. Sorgunuz gibi içeriyorsa etkisi aynıdır `where timestamp > ago(30d)`.

**Depo sayısı kısıtlaması:** çok parçalı depo (genellikle bir çubuk grafik) sahip bir grafiği görüntüler, küçük doldurulmuş depo otomatik olarak gruplandırılır "diğerleri" tek bir depo. Örneğin, bu sorgu:

    requests | summarize count_search = count() by client_CountryOrRegion

Analytics'te şuna benzer:

![Uzun kuyruklu grafik](./media/app-insights-analytics-using/pin-07.png)

Ancak, bir panoya sabitlemek, şöyle görünür:

![Sınırlı depo ile grafik](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-to-excel"></a>Excel'e Aktar
Bir sorgu çalıştırdıktan sonra bir .csv dosyası indirebilirsiniz. Tıklayın **Excel'e Aktar**.

## <a name="export-to-power-bi"></a>Power BI’a aktarma
Bir sorguda imleci yerleştirin ve seçin **dışarı aktarma, Power BI**.

![Analytics'ten Power BI'a aktarma](./media/app-insights-analytics-using/240.png)

Sorguyu Power BI'da çalıştırırsınız. Bir zamanlamaya göre yenilemek için ayarlayabilirsiniz.

Power BI ile çok çeşitli kaynaklardan verileri bir araya getiren panolar oluşturabilirsiniz.

[Power BI için dışarı aktarma hakkında daha fazla bilgi edinin](app-insights-export-power-bi.md)

## <a name="deep-link"></a>Ayrıntılı bağlantı

Altında bir bağlantı alma **dışarı aktarma, bağlantıyı paylaş** , başka bir kullanıcıya gönderebilirsiniz. Kullanıcının sağlanan [, kaynak grubuna erişim](app-insights-resources-roles-access-control.md), sorguyu analiz Arabiriminde açılır.

(Sonra sorgu metni bağlantıdaki görünen "? q =" gzip sıkıştırılmış ve base-64 kodlamalı. Kullanıcılara sağlayan ayrıntılı bağlantılar oluşturmak için kod yazabilirsiniz. Ancak, Kod Analizi çalıştırmak için önerilen yöntem kullanmaktır [REST API](https://dev.applicationinsights.io/).)


## <a name="automation"></a>Otomasyon

Kullanım [veri erişimi REST API'si](https://dev.applicationinsights.io/) analiz sorguları çalıştırmak için. [Örneğin](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (PowerShell kullanarak):

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

Analytics kullanıcı Arabirimi farklı olarak, REST API otomatik olarak herhangi bir zaman damgası sınırlama sorgularınıza eklemez. Kendi where-büyük yanıtları girmeyi önlemek açısından yan tümcesi eklemeyi unutmayın.



## <a name="import-data"></a>Veri içeri aktarma

Verileri bir CSV dosyasından içeri aktarabilirsiniz. Tablolarla, telemetri katılabilir statik verileri içeri tipik bir kullanımdır. 

Örneğin, kimliği doğrulanmış kullanıcılar telemetrinizi bir diğer ad veya karıştırılmış kimliği tanımlanırsa, diğer adlar gerçek adlarını eşleyen tablo içe aktaramadı. İstek telemetrisi üzerinde birleştirme gerçekleştirerek, kullanıcıların gerçek adlarını analiz raporları tarafından tanımlayabilirsiniz.

### <a name="define-your-data-schema"></a>Kendi veri şemasını tanımla

1. Tıklayın **ayarları** (sol üstteki) ve ardından **veri kaynakları**. 
2. Yönergeleri izleyerek bir veri kaynağı ekleyin. En az 10 satırlar içermelidir verileri bir örneğini sağlamanız istenir. Sonra şema de düzeltin.

Bu, daha sonra tek tek tablolar içeri aktarmak için kullanabileceğiniz bir veri kaynağı tanımlar.

### <a name="import-a-table"></a>Tablo alma

1. Veri kaynağı tanımınız açın.
2. "Karşıya Yükle" seçeneğini tıklatın ve tablo karşıya yüklemek için yönergeleri izleyin. Bu REST API çağrısı içerir ve bu nedenle otomatik hale getirmek kolay. 

Tablonuz, artık analiz sorguları kullanmak için kullanılabilir. Analytics'te görünür 

### <a name="use-the-table"></a>Tabloyu kullanın

Veri kaynağı tanımınız çağrılır varsayalım `usermap`, ve iki alan olduğunu `realName` ve `user_AuthenticatedId`. `requests` Tablo adında bir alan da sahip `user_AuthenticatedId`, onlara katılmanız kolaydır:

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
Elde edilen tabloda istekleri başka bir sütuna sahip `realName`.

### <a name="import-from-logstash"></a>LogStash alma

Kullanırsanız [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), Analytics, günlükleri sorgulamak için kullanabilirsiniz. Kullanım [veri Analytics'e geçirir. eklentisini](https://github.com/Microsoft/logstash-output-application-insights). 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

