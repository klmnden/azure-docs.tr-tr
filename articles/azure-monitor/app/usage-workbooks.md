---
title: Azure İzleyici çalışma kitapları ile etkileşimli raporlar oluşturun | Microsoft docs
description: Önceden oluşturulmuş ve özel parametreli çalışma kitapları ile karmaşık raporlama basitleştirin
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/19/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: bcd4ae5b7092aafc452c5af3d17fd22c263b5d35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60373632"
---
# <a name="create-interactive-reports-with-azure-monitor-workbooks"></a>Azure İzleyici çalışma kitapları ile etkileşimli raporlar oluşturun

Çalışma kitapları, metin, birleştirme [analiz sorguları](https://docs.microsoft.com/azure/application-insights/app-insights-analytics), Azure ölçümleri ve zengin etkileşimli raporları parametreleri. Çalışma kitapları, aynı Azure kaynaklarına erişimi olan tüm diğer ekip üyeleri tarafından düzenlenebilir.

Çalışma kitapları gibi senaryolar için yararlıdır:

* İlgilendiğiniz ölçümleri önceden bilmediğinizde uygulamanızın kullanımını keşfetme: kullanıcılar, elde tutma oranları, dönüştürme oranlarını vb. sayısı. Başka kullanım analizi Araçları çalışma kitapları, birden çok türde görselleştirmeler ve analizleri, bu tür bir serbest biçimli araştırması için harika yönetilmelerini birleştirmek olanak tanır.
* Ekibinizin yeni kullanıma sunulan bir özellik performansıyla açıklanması, anahtar etkileşimleri ve diğer ölçümleri gösteren bir kullanıcı tarafından sayar.
* Bir A sonuçların paylaşılması / B deneme takımınızın diğer üyeleriyle uygulamanızda. Hedefler için metin ile deneme açıklar ve her bir kullanım ölçümü ve her ölçümü yukarıda veya aşağıda-hedefi olup için açık çağrı-çıkarmayı birlikte deneme değerlendirmek için kullanılan bir Analytics sorgusunu göster.
* Kesinti etkisini veri, açıklama metnini ve gelecekte kesintileri önlemek için sonraki adımlar ayrıntılı bir birleştirme uygulamanızın kullanım raporlama.

## <a name="starting-with-a-template-or-saved-workbook"></a>Bir şablonla başlayarak veya kaydedilmiş bir çalışma kitabı

Bir çalışma kitabı oluşan bağımsız olarak düzenlenebilir grafikleri, tablolar, metin, bölümlerini yapılır ve giriş denetimleri. Çalışma kitapları daha iyi anlamak için bir açık en iyisidir. 

Seçin **çalışma kitapları** içinde sol taraftaki menüden uygulamanız için Application Insights karşılaşırsınız.

![Çalışma kitapları için Gezinti bölmesinin ekran görüntüsü](./media/usage-workbooks/001-workbooks.png)

Bu, bir çalışma kitabı Galerisi başlamanıza yardımcı olmak için önceden oluşturulmuş çalışma kitaplarını sayısı ile başlatır.

![Çalışma kitabı galeri görüntüsü](./media/usage-workbooks/002-workbook-gallery.png)

İle başlayacağız **varsayılan şablon**, başlığı altında bulunan **Hızlı Başlangıç**.

![Çalışma kitabı galeri görüntüsü](./media/usage-workbooks/003-default-template.png)

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Düzenleme, yeniden düzenleme, kopyalama ve çalışma kitabı bölümleri silme

Çalışma kitapları iki mod vardır: **düzenleme modunda**, ve **Okuma modunda**. Varsayılan çalışma kitabı ilk kez başlatıldığında açılır **düzenleme modunda**. Bu, tüm adımlar ve aksi takdirde gizli parametreleri de dahil olmak üzere çalışma kitabının içeriğini gösterir. **Okuma Modu** Basitleştirilmiş rapor stil görünümünü sunar. Bu, hemen yine de temel alınan mekanizması yalnızca birkaç tıklamayla ulaşabiliyoruz değiştirilmek üzere gerektiğinde yaparken bir raporu oluşturmaya giden karmaşıklığı soyutlamak sağlar.

![Application Insights çalışma kitapları bölümünde düzenleme denetimleri](./media/usage-workbooks/editing-controls-new.png)

1. Bitirdiğinizde bir bölümünü Düzenleyen, tıklayın **düzenleme Bitti** bölümünün sol alt köşedeki.

2. Bir bölümün bir kopyasını oluşturmak için tıklayın **bu bölümü kopyalayın** simgesi. Yinelenen bölümleri oluşturmak, önceki yinelemelerin kaybetmeden bir sorgu üzerinde gezinmek için yol için harika bir işlemdir.

3. Bir çalışma kitabı bir bölümde yukarı taşımak için tıklatın **Yukarı Taşı** veya **Aşağı Taşı** simgesi.

4. Bir bölümün kalıcı olarak kaldırmak için tıklayın **Kaldır** simgesi.

## <a name="adding-text-and-markdown-sections"></a>Metin ve Markdown bölümleri ekleme

Başlıklarını, açıklamaları ve yorum, çalışma kitaplarına ekleme yardımcı tablolar ve grafikler bir dizi bir anlatım açın. Çalışma kitapları destek metin bölümlerinde [Markdown söz dizimi](https://daringfireball.net/projects/markdown/) başlıklar, kalın, italik ve madde işaretli listeler gibi biçimlendirme metin.

Bir metin bölümüne çalışma kitabınıza eklemek için **metin eklemek** çalışma kitabını sayfanın alt kısmında veya herhangi bir bölümü altındaki düğmeyi.

## <a name="adding-query-sections"></a>Sorgu bölümleri ekleme

![Sorgu bölümünde çalışma kitapları](./media/usage-workbooks/analytics-section-new.png)

Çalışma kitabınıza sorgu bölümü eklemek için **Sorgu Ekle** çalışma kitabını alt kısmındaki ya da herhangi bir bölümü altındaki düğmesi.

Sorgu bölümler son derece esnektir ve aşağıdaki gibi sorulara yanıt için kullanılabilir:

* Kaç özel bir reddetme kullanımı ile aynı süre boyunca sitenizin durumunu oluşturamadı?
* Sayfa yükleme sürelerinin sayfa görüntüleme kullanıcılar için dağıtım neydi?
* Kaç kullanıcının bazı sayfalar kümesi sitenizde görüntülenebilir, ancak olmayan bazı diğer sayfalarında ayarlanmış? Bu kümeleri farklı alt kümelerinde sitenizin işlevselliği kullanan kullanıcı olup olmadığını anlamak yararlı olabilir (kullanın `join` işleciyle `kind=leftanti` değiştiricisi [Kusto sorgu dili](/azure/kusto/query/)).

Siz de yalnızca sınırlı olmayan çalışma kitabından veri başlatılan uygulama bağlamında, sorgulama. Bu kaynaklara erişim iznine sahip olduğu sürece birden fazla Application Insights'ın izlenen uygulamalar olarak Log Analytics çalışma alanlarını sorgulayabilirsiniz.

Ek dış Application Insights kaynakları kullanımdan Sorgulanacak **uygulama** tanımlayıcısı.

```
union app('app01').requests, app('app02').requests, requests
| summarize count() by bin(timestamp, 1h)
```  

Bu sorgu, üç farklı uygulama isteklerinden birleştirmektir. Bir uygulama app01, app02 ve yerel Application Insights kaynağı gelen istekleri adlı bir uygulama adı.

Dış bir Log Analytics çalışma alanı kullanım verilerini çekmesini **çalışma** tanımlayıcısı.

Bilgi edinmek için kaynaklar arası sorguları hakkında daha fazla başvurmak [resmi rehberlik](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search).

### <a name="advanced-analytic-query-settings"></a>Gelişmiş analitik sorgu ayarları

Her bölüm ayarlar simgesi erişilebilir kendi Gelişmiş ayarları olan ![Application Insights çalışma kitapları bölümünde düzenleme denetimleri](./media/usage-workbooks/005-settings.png) sağında bulunan **parametreleri ekleme** düğmesi.

![Application Insights çalışma kitapları bölümünde düzenleme denetimleri](./media/usage-workbooks/0006-settings-expanded.png)

   |         |          |
   | ---------------- |:-----|
   | **Özel bir genişlik**    | Daha iyi grafikler ve tablolar etkileşimli zengin raporlar düzenlemenize olanak sağlayan tek bir satırda öğe sayısını sığacak şekilde bir öğenin bir rastgele boyutu haline getirmek için bunu ayarlayın.  |
   | **Koşullu olarak görünür** | Okuma modunda olduğunda bir parametresini temel alan adımları gizlemek için bunu kullanın. |
   | **Parametreyi Dışarı Aktar**| Bu kılavuzda veya değerleri değiştirmesini veya görünür olmasını için sonraki adımları neden grafik seçili bir satır sağlar.  |
   | **Düzenlenmediği sırada sorguyu Göster** | Bu, yukarıdaki grafiğe veya tabloya bile sorgu Okuma modunda olduğunda görüntüler.
   | **Düzenlenmediği sırada analiz düğmesi açık Göster** | Bu, tek tıklamayla erişim izni vermek için grafiğin sağ köşesinde için mavi Analytics simgesi ekler.|

Bu ayarların birçoğu oldukça kolay anlaşılır ancak anlamak için **parametreyi dışarı aktar** bir çalışma kitabı incelemek iyidir yapar bu işlevi kullanın.

Önceden oluşturulmuş çalışma kitaplarını biri etkin kullanıcılar bilgi sağlar.

Çalışma kitabı ilk bölümünü analitik sorgu veri dayanır:

![Application Insights çalışma kitapları bölümünde düzenleme denetimleri](./media/usage-workbooks/003-active-users.png)

İkinci bölüm de analitik sorgu verilere dayalı, ancak ilk tabloda bir satır seçilmesi grafik içeriğini etkileşimli olarak güncelleştirir:

![Application Insights çalışma kitapları bölümünde düzenleme denetimleri](./media/usage-workbooks/004-active-users-trend.png)

 Bu aracılığıyla mümkündür **bir öğe seçildiğinde, parametreyi dışarı aktar** Gelişmiş tablonun Analytics sorgunuzda etkin ayarlar.

![Application Insights çalışma kitapları bölümünde düzenleme denetimleri](./media/usage-workbooks/007-settings-export.png)

Bir satır seçili olduğunda ikinci analiz sorgusunu sonra verilen değerleri kullanır. Hiçbir satır seçilmişse genel değerleri temsil eden satır için varsayılan olarak. 

```
let start = startofday(ago({TimeRange} + {Metric}));
union customEvents, pageViews
| where timestamp >= start
| where name in ({Activities}) or '*' in ({Activities}) or ('%' in ({Activities}) and itemType == 'pageView') or ('#' in ({Activities}) and itemType == 'customEvent')
{OtherFilters}
| where '{Filter}' == '' or '{Filter}' == '🔸 Overall' or {AnalyzeBy} == replace('🔹 ', '', '{Filter}')
| evaluate activity_engagement(user_Id, timestamp, start, now(), 1d, {Metric})
| where timestamp >= startofday(ago({TimeRange}))
| project timestamp, ["Active User"] = dcount_activities_outer
| render timechart 
```

## <a name="adding-metrics-sections"></a>Ölçümleri bölümleri ekleme

Ölçümleri bölümleri etkileşimli raporlarınızda içeri Azure İzleyici ölçüm verilerini birleştirmek için tam erişim verin. Birçok önceden oluşturulmuş çalışma kitapları hem analitik sorgu verileri hem de tek bir yerde her iki özelliği en iyi şekilde tam anlamıyla yararlanmanıza olanak tanıyan ölçüm verileri içerir. Ayrıca, erişiminiz olan Aboneliklerde kaynaklardan gelen ölçüm verilerini çekin olanağı vardır.

Sanal makine verilerinin kılavuz görselleştirme CPU performans sağlamak için bir çalışma kitabına çekilen bir örnek aşağıda verilmiştir:

![Application Insights çalışma kitapları bölümünde düzenleme denetimleri](./media/usage-workbooks/008-metrics-grid.png)

## <a name="adding-parameter-sections"></a>Parametre bölümleri ekleme

Çalışma kitabı parametreleri sorgu veya metin bölümleri el ile düzenlemek zorunda kalmadan değerleri çalışma kitabında değiştirmenize izin verir.  Bu, temel alınan analytics sorgu dili anlamanıza gerek kalmadan gerekliliğini ortadan kaldırır ve çalışma kitabı tabanlı raporlama olası hedef kitle büyük ölçüde genişletir.

Parametre değerlerini sorgu, metin veya diğer parametre bölümleri gibi köşeli ayraçlar parametrenin adını koyarak değiştirilir ``{parameterName}``.  Parametre adları benzer kurallara JavaScript tanımlayıcıları, temelde alfabetik karakterleri veya alt çizgi, ardından alfasayısal karakterler veya alt çizgi sınırlıdır. Örneğin, **a1** izin verilir, ancak **1a** izin verilmiyor.

Çalışma kitabının En üstten başlayarak ve sonraki adımları aşağı akan doğrusal parametrelerdir.  Bir çalışma kitabında olarak bildirilen parametreleri ayarlama daha fazla bildirilmiş o geçersiz kılabilirsiniz.  Bu kullanım sorguları tanımlanan parametreler değerlere erişmek için daha fazla yukarı parametreleri de sağlar.  Bir parametrenin adım içinde kendisi de burada aynı o adımda daha önce bildirilen bir parametre sağa parametreleri güvenebileceğiniz soldan sağa doğrusal parametrelerdir.
 
Dört farklı şu anda desteklenen parametre türleri vardır:

  |         |          |
   | ---------------- |:-----|
   | **Metin**    | Kullanıcı bir metin kutusu düzenleyeceğiz ve isteğe bağlı olarak, varsayılan değeri doldurmak için bir sorgu sağlayabilirsiniz. |
   | **Açılan menü** | Kullanıcı, bir değerler kümesinden seçersiniz. |
   | **Zaman aralığı Seçici**| Kullanıcıya önceden tanımlı bir zaman aralığı değerleri kümesinden seçin veya bir özel zaman aralığı seçin.|
   | **Kaynak Seçici** | Kullanıcı, çalışma kitabı için seçilen kaynakları arasından seçim yapacağı.|

### <a name="using-a-text-parameter"></a>Bir metin parametresini kullanma

Değer metin kutusuna bir kullanıcı türleri kaçış veya Alıntısı, doğrudan sorgusunda değiştirilir. Gereksinim duyduğunuz değer bir dize ise, sorgu parametresi tırnak olmalıdır (gibi **'{parameter}'**).

Bu, herhangi bir kullanılmak üzere bir metin kutusundaki değeri sağlar. Tablo adı, sütun adı, işlev adı, işleç, vb. olabilir.

Bir ayar metin typ parametru **varsayılan değeri analiz sorgusundan Al**, bir sorgu, metin kutusu için varsayılan değer doldurmak için kullanılacak çalışma kitabı yazarı sağlar.

(Satır 0, 0 sütun) ilk satırın yalnızca ilk değer, bir analytics sorgusunu varsayılan değerden kullanırken, varsayılan değer olarak kullanılır. Bu nedenle, yalnızca bir satır ve bir sütun döndürmek için sorguyu sınırlamak için önerilir. Sorgu tarafından döndürülen diğer veriler yoksayılır. 

Her sorgunun döndürdüğü değer doğrudan hiçbir kaçış veya Alıntısı ile değiştirilecek. Sorgu hiçbir satır döndürürse, parametre sonuçları (parametre gerekli değilse), boş bir dize olan veya (parametre gerekli ise) tanımlı değil.

### <a name="using-a-dropdown"></a>Bir açılır menüyü kullanarak

Açılan parametre türü, bir açılan menü denetimi oluşturun, bir veya daha çok değerlerin seçilmesine olanak verecek sağlar.

Açılan bir analytics sorgusunun tarafından doldurulur. Sorgu bir sütun döndürürse, bu sütun her ikisi de değerler **değer** ve **etiket** aşağı açılır denetimden. Sorgu iki sütun döndürürse, ilk sütundur **değer**, ve ikinci sütun **etiket** açılan listede gösterilen.  Sorgu üç sütun döndürürse, 3 sütun, aşağı açılır menüde varsayılan seçimi göstermek için kullanılır.  Bu sütun her türlü olabilir, ancak basit bool veya sayısal türler, burada 0 false ve 1 true kullanmaktır.

 Sütun bir dize türü ise, null veya boş dize false olarak kabul edilir ve başka bir değer true olarak değerlendirilir. Tek seçim listeler için varsayılan seçim true değerine sahip ilk değer kullanılır.  Birden çok seçimi açılan için varsayılan seçili kümesi olarak true değerine sahip tüm değerler kullanılır. Açılır öğelerde hangi sırada satırları sorgu döndürülen gösterilmektedir. 

Etkin kullanıcılar raporda mevcut parametreler bakalım. Düzenle simgesini tıklayın **TimeRange**.

![Application Insights çalışma kitapları bölümünde düzenleme denetimleri](./media/usage-workbooks/009-time-range.png)

Bu düzen parametresi menü öğesi başlatılır:

![Application Insights çalışma kitapları bölümünde düzenleme denetimleri](./media/usage-workbooks/010-time-range-edit.png)

Sorgu bir özellik adında analytics sorgu dili kullanır. bir **datatable** olanak tanıyan, tam Basit hava dışında içeriğinin rastgele bir tablo oluşturun! Örneğin, aşağıdaki analytics sorgusu:

```
datatable( column1:string, column2:string )
[
 "row 1 column 1", "row 1 column 2",
"row 2 column 1", "row 2 column 2"
]
```

Sonucu oluşturur:

![Application Insights çalışma kitapları bölümünde düzenleme denetimleri](./media/usage-workbooks/011-data-table.png)

Bir açılan ülkeler kümesinden adına göre seçmek için daha uygun bir örnek kullanır:

```
customEvents
| where timestamp >= ago(14d)
| summarize count() by client_CountryOrRegion
| top 100 by count_
| project  client_CountryOrRegion 
| order by client_CountryOrRegion asc
```

Sorgu sonuçları aşağıdaki gibi görüntülenir:

![Ülke açılan kutusu](./media/usage-workbooks/012-country-dropdown.png)

Açılan özelleştirme ve etkileşimli raporlar oluşturmak için son derece güçlü araçlardır.

### <a name="time-range-parameters"></a>Zaman aralığı parametreleri

Kendi özel zaman aralığı parametresi açılan parametre türü aracılığıyla yapabilirsiniz, ancak aynı derecede esnekliğe ihtiyacınız yoksa kullanıma hazır zaman aralığı parametre türü kullanabilirsiniz. 

Beş dakika veya Son 90 gün önce 15 varsayılan aralıklarını zaman aralığı parametre türleri vardır. Açık Başlat'ı seçin ve zaman aralığına ilişkin değerleri durdurmak rapor operatörü izin veren özel saat aralığı seçimine izin vermek için bir seçenek de mevcuttur.

### <a name="resource-picker"></a>Kaynak seçici

Kaynak Seçici parametre türü, bazı kaynak türleri için raporun kapsamını olanağı sağlar. Kaynak Seçici türü yararlanan önceden oluşturulmuş çalışma kitabının bir örneğini **hatası Insights** çalışma kitabı.

![Ülke açılan kutusu](./media/usage-workbooks/013-resource-picker.png)

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Kaydetme ve çalışma kitapları takımınızla paylaşma

Çalışma kitapları, içinde bir Application Insights kaynağı kaydedilir **raporlarım** , hem de özel bölümü **paylaşılmış olan raporlar** erişimi olan herkesin erişebileceği bölümü Application Insights kaynağı. Kaynakta tüm çalışma kitaplarını görüntülemek için tıklayın **açık** eylem çubuğunda düğme.

Şu anda kullanımda bir çalışma kitabını paylaşmak için **raporlarım**:

1. Tıklayın **açık** eylem çubuğunda
2. Paylaşmak istediğiniz çalışma kitabını yanındaki "…" düğmesine tıklayın
3. Tıklayın **paylaşılan Raporlar'a Taşı**.

Bir çalışma kitabı bağlantısını içeren bir ya da e-posta ile paylaşmak için tıklatın **paylaşmak** eylem çubuğunda. Bağlantının alıcıları çalışma kitabını görüntülemek için Azure portalında bu kaynağa erişimi gerektiğini aklınızda bulundurun. Gereken en az alıcılar düzenlemeler yapmak için kaynak için katkıda bulunan izinleri.

Azure panosu için bir çalışma kitabı bağlantısını sabitlemek için:

1. Tıklayın **açık** eylem çubuğunda
2. Sabitlemek istediğiniz çalışma kitabının yanındaki "…" düğmesine tıklayın
3. Tıklayın **panoya Sabitle**.

## <a name="contributing-workbook-templates"></a>Çalışma kitabı şablonlarına katkıda bulunan

Sahip olduğunuz bir mükemmel çalışma kitabı şablonu oluşturduğunuz ve toplulukla paylaşmak istiyorsunuz? Daha fazla bilgi edinmek için sunduğumuz [GitHub deposunu](https://github.com/Microsoft/Application-Insights-Workbooks/blob/master/README.md).

## <a name="next-steps"></a>Sonraki adımlar
- Kullanım deneyimlerini etkinleştirmek için göndermeye başlayın [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olay veya sayfa görüntülemesi zaten gönderirseniz, kullanıcıların hizmetinizin nasıl öğrenmek için kullanım araçları keşfedin.
    - [Kullanıcılar, Oturumlar, Etkinlikler](../../azure-monitor/app/usage-segmentation.md)
    - [Huniler](../../azure-monitor/app/usage-funnels.md)
    - [Bekletme](../../azure-monitor/app/usage-retention.md)
    - [Kullanıcı Akışları](../../azure-monitor/app/usage-flows.md)
    - [Kullanıcı bağlamı Ekle](../../azure-monitor/app/usage-send-user-context.md)