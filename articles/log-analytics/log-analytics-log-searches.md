---
title: "Azure günlük analizi günlük aramaları verilerle Bul | Microsoft Docs"
description: "Günlük aramaları, ortamınızdaki birden fazla kaynaktan makine verilerini birleştirmenize ve ilişkilendirmenize olanak tanır."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 0d7b6712-1722-423b-a60f-05389cde3625
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: cef7fc282edc7396a0f26dab98ea7f1087315b23
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2018
---
# <a name="find-data-using-log-searches-in-log-analytics"></a>Günlük analizi günlük aramaları kullanarak veri bulma

>[!NOTE]
> Bu makalede eski sorgu dili günlük analizi kullanarak günlük aramaları açıklanmaktadır.  Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), için başvurmalıdır sonra [anlama günlük arar (yeni) günlük analizi](log-analytics-log-search-new.md).


Günlük analizi özünde birleştirmek ve ortamınızda birden fazla kaynaktan herhangi bir makine verilerin bağıntısını olanak tanıyan günlük arama özelliğidir. Çözümleri ayrıca günlük tarafından desteklenen ölçümleri getirmek için arama özetlenebilir belirli sorunu etrafına.

Arama sayfasında, bir sorgu oluşturabilirsiniz ve arama yaptığınızda sonra sonuçları modeli denetimlerini kullanarak filtre uygulayabilirsiniz. Dönüştürme, filtre ve raporlamak için Gelişmiş sorgular sonuçlarınıza de oluşturabilirsiniz.

Ortak günlük arama sorguları çoğu çözüm sayfalarında görüntülenir. OMS portalı döşeme tıklatın ya da günlük arama kullanarak öğenin ayrıntılarını görüntülemek için diğer öğeleri ayrıntıya gidin.

Bu öğreticide, biz günlük arama kullandığınızda tüm temel kavramları kapsar örneklerle gösterilecektir.

Biz basit, pratik örnekler başlatın ve böylece pratik kullanım örnekleri sözdizimi verilerden istediğiniz öngörüleri ayıklamak için nasıl kullanılacağı hakkında bir anlayış alabilir üzerlerinde yapı.

Arama teknikleri alışık olduğunuz sonra inceleyebilirsiniz [günlük analizi oturum Arama başvurusu](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Temel filtreleri kullanın
Bir arama ilk bölümü, önce sorgu bilmeniz ilk şey. "|" dikey çizgi karakteri olan her zaman bir *filtre*. Bunu bir WHERE yan tümcesinde TSQL--olarak bunu belirler düşünün *ne* günlük analizi çalışma alanı dışına çıkarmak için veri alt kümesini. Veri deposunda arama büyük ölçüde sorguda WHERE yan tümcesi ile başladığını doğal şekilde ayıklamak istediğiniz verilerin özelliklerini belirtme hakkında olur.

Kullanabileceğiniz en temel filtreler *anahtar sözcükleri*'error' veya 'zaman aşımı' veya bir bilgisayar adı gibi. Bu basit sorgu türleri genellikle aynı sonuç kümesi içindeki verilerin farklı şekiller döndür. Bu günlük analizi farklı sahip olmasından *türleri* verilerin System.

### <a name="to-conduct-a-simple-search"></a>Basit arama yapmak için
1. OMS Portalı'nda tıklatın **günlük arama**.  
    ![Ara kutucuğu](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. Sorgu alanına yazın `error` ve ardından **arama**.  
    ![Arama hatası](./media/log-analytics-log-searches/oms-search-error.png)  
    Örneğin, sorgu için `error` aşağıdaki görüntüde 100.000 döndürülen **olay** (günlük yönetimi tarafından toplanan), kayıt 18 **ConfigurationAlert** (yapılandırma değerlendirme tarafından oluşturulan) kaydeder ve 12 **ConfigurationChange** (değişiklik izleme tarafından yakalanan) kaydeder.   
    ![Arama sonuçları](./media/log-analytics-log-searches/oms-search-results01.png)  

Bu filtreler gerçekten nesne türleri/sınıfları değildir. *Tür* yalnızca bir etiket veya bir özellik ya da bir dize/ad/veri parçası için bağlı kategorisi. Sistem bazı belgelerde olarak etiketlenir **türü: ConfigurationAlert** ve bazı olarak etiketlenir **türü: Perf**, veya **türü: olay**ve benzeri. Her bir arama sonucu, belge, kaydı veya giriş tüm Ham Özellikler ve değerlerinin her veri bu parçalarını görüntüler ve alan değeri belirtilen bulunduğu filtresinde yalnızca kayıtları almak istediğinizde belirtmek için bu alan adlarını kullanabilirsiniz.

*Tür* aslında tüm kayıtlar varsa, bir alan olup, diğer herhangi bir alandan farklı değildir. Bu tür alanı değerini temel kuruldu. Bu kayıt, farklı bir şekil veya form sahip olur. Arada **türü Perf =**, veya **türü olay =** de performans verileri veya olay için sorgulamak için ihtiyacınız olanları söz dizimi aşağıdaki gibidir.

Alan adı sonra ve değeri önce iki nokta üst üste (:) veya bir eşittir işareti (=) kullanabilirsiniz. **Olay türü:** ve **türü olay =** olan anlamı eşdeğer, tercih ettiğiniz stili seçebilirsiniz.

Böylece, eğer türü Perf = kayıtları 'CounterName' adlı bir alana sahip sonra benzeyen bir sorgu yazabilirsiniz `Type=Perf CounterName="% Processor Time"`.

Performans sayacı adı "% işlemci zamanı" olduğu Bu, yalnızca performans verileri verir.

### <a name="to-search-for-processor-time-performance-data"></a>İşlemci zamanı performans verilerini aramak için
* Arama sorgusu alanına yazın `Type=Perf CounterName="% Processor Time"`

Ayrıca daha belirli ve kullanabilirsiniz **InstanceName = _ 'Toplam'** sorguda olduğu bir Windows performans sayacı. Bir model ve başka de seçebilirsiniz **alan: değer**. Filtre sorgu çubuğundaki filtre otomatik olarak eklenir. Bu aşağıdaki resimde görebilirsiniz. Eklenecek yeri gösterir **InstanceName: '_Total'** herhangi bir şey yazarak olmadan sorgulanamıyor.

![Arama modeli](./media/log-analytics-log-searches/oms-search-facet.png)

Sorgunuz artık hale gelir `Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

Bu örnekte, belirtmeniz gerekmez **türü Perf =** bu sonucu elde etmek için. CounterName ve InstanceName alanları türündeki kayıtlar için yalnızca olduğundan Perf, = uzun, önceki bir aynı sonuçları döndürmek için belirli bir sorgudur:

```
CounterName="% Processor Time" InstanceName="_Total"
```

Sorgudaki tüm filtreleri olarak değerlendirilir olmasıdır *ve* birbirleriyle. Etkili bir şekilde, daha fazla alan ölçüt eklemek daha az almak daha belirli ve Gelişmiş sonuçları.

Örneğin, sorgu `Type=Event EventLog="Windows PowerShell"` aynıdır `Type=Event AND EventLog="Windows PowerShell"`. Oturum açmış ve Windows PowerShell olay günlüğünden toplanan tüm olayları döndürür. Sorunu tamamen yüzeysel--ise, bir filtre birden çok kez art arda aynı modelin seçerek eklerseniz, arama çubuğunda dağıtmayı, ancak örtük AND işleci her zaman olduğu için bunu hala aynı sonuçları döndürür.

Örtük ve işlecini açıkça değil işlecini kullanarak kolayca ters çevirebilirsiniz. Örneğin:

`Type:Event NOT(EventLog:"Windows PowerShell")` ya da eşdeğer `Type=Event EventLog!="Windows PowerShell"` Windows PowerShell günlük olmayan tüm diğer günlüklerinden tüm olayları döndürür.

Ya da diğer Boole işleci gibi kullanabilir 'Veya'. Aşağıdaki sorgu ya da uygulama veya sistem olay günlüğüne olduğu kayıtları döndürür.

```
EventLog=Application OR EventLog=System
```

Yukarıdaki sorgu kullanarak, girişleri için aynı sonuç kümesi içinde her iki günlükleri için elde edersiniz.

Örtük bırakarak ve yerinde veya kaldırırsanız, her iki günlüklerine ait bir olay günlüğü girişi olmadığından Bununla birlikte, ardından aşağıdaki sorguyu herhangi bir sonuç oluşturmaz. Her olay günlüğü girişi iki günlükleri yalnızca biri için yazılmıştır.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Ek filtreleri kullanın
Aşağıdaki sorguda veri gönderdiğiniz tüm bilgisayarlar için 2 olay günlüğü girişlerini döndürür.

```
EventLog=Application OR EventLog=System
```

![arama sonuçları](./media/log-analytics-log-searches/oms-search-results03.png)

Alanları veya filtreleri birini seçerek, diğer tüm olanlar hariç olmak üzere belirli bir bilgisayar için sorgu daraltın. Sonuçta elde edilen sorgu aşağıdakine benzeyecektir.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Örtük and nedeniyle aşağıdaki eşdeğer olduğu

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Her sorgu aşağıdaki açık sırayla değerlendirilir. Parantez unutmayın.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Olay günlüğü alanı yalnızca gibi ekleyerek yalnızca belirli bir bilgisayarlar kümesi için verileri alabilir veya. Örneğin:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Benzer şekilde, bu aşağıdaki sorgu döndürme **% CPU süresi** yalnızca seçilen iki bilgisayar için.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```

### <a name="field-types"></a>Alan türleri
Filtre oluştururken, farklı türde günlük aramaları tarafından döndürülen alanları ile çalışma farklar anlamanız gerekir.

**Aranabilir alanlara** mavi arama sonuçlarında göster.  Arama koşulu belirli aranabilir alanlara alanına aşağıdaki gibi kullanabilirsiniz:

```
Type: Event EventLevelName: "Error"
Type: SecurityEvent Computer:Contains("contoso.com")
Type: Event EventLevelName IN {"Error","Warning"}
```

**Serbest metin aranabilir alanları** arama sonuçlarında gri gösterilir.  Arama koşulu alanına aranabilir alanları gibi belirli ile kullanılamaz.  Yalnızca bir sorgu aşağıdaki gibi tüm alanlar boyunca gerçekleştirirken aranır.

```
"Error"
Type: Event "Exception"
```


### <a name="boolean-operators"></a>Boole işleçleri
DateTime ve sayısal alanlar ile kullanarak değerleri için arama yapabilirsiniz *büyük*, *küçük daha*, ve *küçük veya eşit*. Basit işleçleri gibi kullanabilir >, <>, =, < =,! sorgu arama çubuğunda =.

Belirli bir süre için belirli bir olay günlüğü sorgulayabilirsiniz. Örneğin, son 24 saat aşağıdaki anımsatıcı ifade ile ifade edilir.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Boole işleci kullanarak aramak için
* Arama sorgusu alanına yazın `EventLog=System TimeGenerated>NOW-24HOURS`  
    ![Boole değeri ile arama](./media/log-analytics-log-searches/oms-search-boolean.png)

Zaman aralığı grafik denetleyebilir ve çoğu kez, yapmak isteyebilirsiniz, ancak sorguyu doğrudan zaman filtresi dahil olmak üzere avantajları vardır. Örneğin, bu harika burada kılabilirsiniz her bölme için zaman bakılmaksızın panolarla çalışır *genel* Pano sayfasındaki Saat Seçici. Daha fazla bilgi için bkz: [zaman önemlidir panosunda](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Zamanına göre filtreleme, için sonuçlar elde göz önünde bulundurmanız *kesişim* iki dönemler: OMS Portalı'nda (S1) belirtilen ve (S2) sorguda belirtilen bir.

![kesişim](./media/log-analytics-log-searches/oms-search-intersection.png)

Süre, örneğin burada seçtiğiniz OMS portalında kesiştiği yok ise, yani **bu hafta** ve burada tanımladığınız sorgusunda **geçen hafta**, ardından hiçbir kesişim yoktur ve herhangi bir sonuç almazsınız.

Karşılaştırma işleçleri TimeGenerated alanı için kullanılan aynı zamanda diğer durumlarda faydalıdır. Örneğin, sayısal alanlar ile.

Örneğin, yapılandırma değerlendirmesi'nın uyarıları aşağıdaki önem değerlere sahip verilen:

* 0 bilgi =
* 1 uyarı =
* 2 kritik =

Uyarı ve kritik uyarılar için sorgu ve de aşağıdaki sorguyu bilgilendirme raporlarla hariç:

```
Type=ConfigurationAlert  Severity>=1
```


Aralık belirtilen sorguların de kullanabilirsiniz. Bu değerleri bir sırada başlangıcını ve bitişini aralığı sağlayabilir anlamına gelir. EventID daha büyük veya eşit 2100 ancak 2199 büyük olduğu Operations Manager olay günlüğündeki olaylar isterseniz, örneğin, ardından aşağıdaki sorguyu bunları döndürecektir.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> İki nokta üst üste (:) alan: değer ayırıcı kullanmalısınız aralığı sözdizimi şöyledir ve *değil* eşittir (=). Aralığın alt ve üst sınırı köşeli parantez içine almanız ve bunları iki nokta (.) ile ayırın.
>
>

## <a name="manipulate-search-results"></a>Arama sonuçları işlemek
Verileri için arama yapıyorsanız, arama sorgunuzu daraltın ve sonuçları üzerinde denetim iyi düzeyine sahip istersiniz. Sonuçları alınırken, bunları dönüştürmek için komutları uygulayabilirsiniz.

Günlük analizi aramaları komutlarda *gerekir* dikey çizgi karakteri (|) sonra izleyin. Bir filtre her zaman bir sorgu dizesi ilk bölümü olması gerekir. Üzerinde çalıştığınız veri kümesini tanımlar ve ardından "Bu sonuçların bir komuta geçirir". Ardından kanal ek komutlar eklemek için de kullanabilirsiniz. Bu Windows PowerShell ardışık düzene geniş benzer.

Genel olarak, günlük analizi arama dili PowerShell stili ve BT uzmanları için benzer yapmak ve öğrenme eğrisi kolaylaştırmak için yönergeleri izleyin dener.

Ne yaptıklarını kolayca anlayabilirsiniz şekilde komutları fiiller adlara sahip.  

### <a name="sort"></a>Sırala
Sort komutu, bir veya birden çok alanlara göre sıralamayı tanımlamanıza olanak sağlar. Varsayılan olarak kullanmayın olsa bile, azalan bir zorunlu kılınır. En son sonuçları her zaman, arama sonuçları en üstünde olacak. Bunun anlamı ile bir aramayı çalıştırırken `Type=Event EventID=1234` ne gerçekten sizin için yürütülen olan:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Günlüklerde bilginiz deneyimi türü olmasıdır. Örneğin, Windows Olay Görüntüleyicisi'nde.

Sıralama sonuçları döndüren şeklini değiştirmek için kullanabilirsiniz. Aşağıdaki örnekler, bunun nasıl çalıştığını gösterir.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


Yukarıdaki basit örnekler komutları nasıl--Göster filtre döndürülen sonuçları şeklini değiştirin.

### <a name="limit-and-top"></a>Ve üst sınırı
Daha az bilinen başka bir komuta sınırıdır. PowerShell benzeri fiil sınırıdır. Sınır üst komutuna işlevsel olarak aynıdır. Aşağıdaki sorgularda aynı sonuçları döndürür.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Üst kullanarak aramak için
* Arama sorgusu alanına yazın `Type=Event EventID=600 | Top 1`   
    ![Arama üst](./media/log-analytics-log-searches/oms-search-top.png)

Yukarıdaki resimde EventID 358 bin kayıtlarıyla vardır = 600. Alanlar, modelleri ve soldaki filtreleri döndürülen sonuçlar hakkında bilgiler her zaman göster *filtre bölümü tarafından* sorgusunun herhangi dikey çizgi karakterinden önce bir parçasıdır. **Sonuçları** bölmesinde çünkü örnek komut şeklinde ve sonuçları dönüştürülmüş en son 1 sonuç yalnızca döndürür.

### <a name="select"></a>Şunu seçin:
SELECT komutu Select-Object PowerShell'de gibi davranır. Tüm özgün özelliklerine sahip olmayan filtrelenen sonuçları döndürür. Bunun yerine, yalnızca belirttiğiniz özellikleri seçer.

#### <a name="to-run-a-search-using-the-select-command"></a>Select komutu kullanarak bir arama çalıştırmak için
1. Aramada, yazın `Type=Event` ve ardından **arama**.
2. Tıklatın **+ daha fazla Göster** sonuçlara sahip tüm özelliklerini görüntülemek için sonuçları birinde.
3. Bazı bu açıkça seçin ve sorguyu değişikliklerini `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Ara'yı seçin](./media/log-analytics-log-searches/oms-search-select.png)

Bu komut, arama çıkış denetlemek ve yalnızca gerçekten genellikle tam kayıt değil, araştırması önemli veri bölümlerini seçin istediğinizde özellikle yararlıdır. Farklı türlerde kayıtlar varsa, bu da yararlıdır *bazı* ortak özellikler, ama *tüm* kendi ortak özellikleridir. , Daha doğal olarak bir tablo gibi görünen bir çıktı oluşturmak veya bir CSV dosyasına dışarı ve Excel'de massaged iyi çalışır.

## <a name="use-the-measure-command"></a>Ölçü komutunu kullanın
ÖLÇÜ günlük analizi aramaları en çok yönlü komutlar biridir. Uygulamaya istatistiksel tanır *işlevleri* veri ve belirli bir alana göre gruplandırılmış toplama sonuçları. Ölçü destekleyen birden çok istatistik işlevleri vardır.

### <a name="measure-count"></a>Ölçü Count() işlevi
Çalışmak için ilk istatistiksel işlev ve basit anlamak biri *count()* işlevi.

Bir arama sorgusundan gibi sonuçları `Type=Event`, arama sonuçları sol tarafta modelleri olarak da bilinir filtrelerini göster. Filtreleri yürütülen arama sonuçları için belirli bir alan değerleriyle dağıtımını gösterir.

![Ara ölçü birimi sayısı](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Örneğin, yukarıdaki resimde göreceğiniz **bilgisayar** alan ve gösterir sonuçlarında neredeyse 739 bin olayları içinde olduğunu 68 benzersiz ve ayrı değerlerini **bilgisayar** bu kayıtlardaki alan. Döşeme yalnızca üst yazıldığı en yaygın 5 değerler 5 gösterir **bilgisayar** alanları), bu alan belirli değeri içeren belgeleri sayısına göre sıralanmış. Görüntüde neredeyse 369 bin olayları arasında– 90 bin OpsInsights04.contoso.com bir bilgisayardan 83 bin DB03.contoso.com bilgisayardan gelen vb. – görebilirsiniz.

Ne kutucuğu yalnızca yalnızca ilk 5 gösterdiğinden tüm değerleri görmek mi istiyorsunuz?

Ölçü komutu count() işlevi ile neler yapabileceğinizi olmasıdır. Bu işlev, herhangi bir parametre kullanmaz. Gruplandırma ölçütü – istediğiniz alan yalnızca belirttiğiniz **bilgisayar** bu durumda alan:

`Type=Event | Measure count() by Computer`

![Ara ölçü birimi sayısı](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

Ancak, **bilgisayar** kullanılan bir alan *içinde* veri – her parçası ilişkisel veritabanı dahil olan ve ayrı olmadığından **bilgisayar** herhangi bir yere nesne. Yalnızca değerleri *içinde* verileri hangi varlık bunları ve diğer özellikleri sayısı ve veri – bu nedenle yönlerini oluşturulan açıklayabilirsiniz terimi *modeli*. Ancak, yalnızca de diğer alanlara göre gruplandırabilirsiniz. Ölçü komuta yöneltilen neredeyse 739 bin olayları özgün sonuçlarını da adlı bir alana sahip olduğundan **EventID**, bu alana göre gruplandırma ve EventID olayların sayısını almak için aynı yöntemleri uygulayabilirsiniz:

```
Type=Event | Measure count() by EventID
```

Belirli bir değer içeren gerçek kayıt sayısı ilgilendiğiniz değil ancak bunun yerine, yalnızca değerler listesini istiyorsanız, ekleyebileceğiniz bir *seçin* sonunda, komut ve yalnızca ilk sütununu seçin:

```
Type=Event | Measure count() by EventID | Select EventID
```

Daha karmaşık alın ve sorgu sonuçlarında önceden sıralama veya kılavuz sütunları çok yalnızca tıklatın ardından.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Ölçü sayısı kullanarak aramak için
* Arama sorgusu alanına yazın `Type=Event | Measure count() by EventID`
* Append `| Select EventID` sorgu sonuna.
* Son olarak, append `| Sort EventID asc` sorgu sonuna.

Dikkat edin ve vurgulamak için birkaç önemli nokta vardır:

İlk olarak, gördüğünüz sonuçları özgün ham sonuçları artık değildir. Bunun yerine, toplanan sonuçları – oldukları temelde sonuçları grupları. Bu bir sorun değildir, ancak farklı veri toplama/istatistiksel işlevi sonucunda kolay bir şekilde oluşturulduğunu özgün ham şeklin bir çok farklı şekli ile etkileşim anlamanız gerekir.

İkinci, **ölçmek sayısı** şu anda yalnızca ilk 100 farklı sonuçlar döndürür. Bu sınır diğer istatistik işlevleri için geçerli değildir. Bu nedenle, genellikle daha kesin bir filtre ilk ölçü count() uygulamadan önce belirli öğeleri aramak için kullanmanız gerekecektir.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Maksimum ve minimum işlevleri ile ölçü komutunu kullanın
Çeşitli senaryolarda nerede **ölçü Max()** ve **ölçü MIN()** yararlıdır. Bununla birlikte, her işlev ters olduğundan birbirinden biz Max() göstermeye ve, MIN() ile kendiniz deneyebilirsiniz.

Güvenlik olayları için sorgu, sahip oldukları bir **düzeyi** değişebilir özelliği. Örneğin:

```
Type=SecurityEvent
```

![Ara ölçü sayısı Başlat](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Tüm güvenlik için en yüksek değer görüntülemek istiyorsanız, ortak bir bilgisayar, Grup alana göre verilen olayları kullanabilirsiniz

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![search measure max computer](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Sahip bilgisayarlar için görüntüleyeceği **düzeyi** kayıtları, bunların çoğu sahip en az düzey 8, birçok 16 düzeyini vardı.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![bilgisayar arama ölçü max süresi oluşturulan](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Bu işlev numaralarıyla iyi çalışır, ancak DateTime alanları ile de çalışır. Herhangi bir her bilgisayar için dizine veri parçası için son veya en son zaman damgasını denetlemek kullanışlıdır. Örneğin: ne zaman en son güvenlik olayı bildirildi her makine için?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Ölçü komutu ile ortalama işlevini kullanın
Ölçü ile kullanılan Avg() istatistiksel işlevi bazı alanı ve aynı ya da diğer alana göre grup sonuçları için ortalama değer hesaplamak sağlar. Bu durumda, performans verileri gibi çeşitli yararlı olur.

Performans verileri ile başlayacağız. Günlük analizi şu anda hem Windows hem de Linux makineler performans sayaçlarını toplar unutmayın.

Aranacak *tüm* performans verileri, en temel sorgudur:

```
Type=Perf
```

![Arama avg Başlat](./media/log-analytics-log-searches/oms-search-avg01.png)

Günlük analizi, üç Perspektifler gösterir fark ilk şey olduğunu: hangi grafikleri; gerçek kayıtları gösteren gösterir listesi Tablo, performans sayacı verileri tablo görünümünü gösterir; ve performans sayaçları için grafikleri gösteren ölçümleri.

Yukarıdaki resimde aşağıdaki belirtmek işaretlenmiş alanlar iki kümesi vardır:

* İlk küme sorguyu filtreye Windows performans sayacı adı, nesne adı ve örnek adını tanımlar. Bu büyük olasılıkla en yaygın olarak modelleri/filtre olarak kullanacağınız alanları
* **CounterValue** sayaç gerçek değeri. Bu örnekte, değerdir *75*.
* **TimeGenerated** 12:51, 24 saatlik zaman biçiminde değil.

Bir grafik ölçümlerini görünümünü aşağıdadır.

![Arama avg Başlat](./media/log-analytics-log-searches/oms-search-avg02.png)

Perf kayıt şekli okumayı ve diğer arama teknikleri hakkında okuyun sonra bu tür sayısal veri toplamak için ölçü Avg() kullanabilirsiniz.

Basit bir örnek aşağıda verilmiştir:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![Ortalama görüntülendiğinden arama](./media/log-analytics-log-searches/oms-search-avg03.png)

Bu örnekte, bilgisayar tarafından toplam CPU süresi performans sayacı ve ortalama seçin. Yalnızca son 6 saat sonuçlarınızı daraltmak isterseniz, zamanı filtre denetimi kullanabilir veya sorgunuzda aşağıdaki gibi belirtin:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Ölçü komutuyla avg işlevi kullanarak aramak için
* Arama sorgusu kutusuna `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.

Toplama ve ilişkilendirmek veri *arasında* bilgisayarlar. Örneğin, burada her düğüm herhangi başka birine eşit ve bunlar yalnızca aynı çalışmanın yazın ve yük kabaca dengelenmelidir grubu çeşit konakların kümesine sahip düşünün. Tek aşağıdaki sorgu tüm grup için ortalamalar alma gidin ve bunların sayaçlarını alabilirsiniz. Aşağıdaki örnek bilgisayarlarla seçerek başlatabilirsiniz:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Bilgisayarlar sahip olduğunuza göre aynı zamanda yalnızca iki ana performans göstergelerini (KPI'lar) seçmek istediğiniz: CPU kullanımı % ve % boş Disk alanı. Bu nedenle, bu sorgu parçası haline gelir:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Artık bilgisayarlar ve aşağıdaki örnekte sayaçlarıyla ekleyebilirsiniz:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Belirli bir seçim olduğundan **Avg() ölçmek** komutu dönebilirsiniz ortalama bilgisayar tarafından değil, ancak Grup CounterName gruplandırarak tarafından yeterlidir. Örneğin:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Bu, birkaç ortamınızı ait KPI'ları yararlı compact bir görünümünü sağlar.

![Arama avg gruplandırma](./media/log-analytics-log-searches/oms-search-avg04.png)

Arama sorgusu bir Panoda kolayca kullanabilirsiniz. Örneğin, arama sorgusunu kaydetmek ve Pano adlı ondan oluşturma *Web grubu KPI'ları*. Panoları kullanma hakkında daha fazla bilgi için bkz: [günlük analizi özel bir pano oluşturun](log-analytics-dashboards.md).

![Arama avg Panosu](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>Sum işlevi ile ölçü komutunu kullanın
Sum işlevi ölçü komut diğer işlevlere benzerdir. Sum işlevini kullanma hakkında bir örnek görebilirsiniz [W3C IIS günlüklerini Microsoft Azure operasyonel Öngörüler aramada](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).

Sayı, tarih saatler ve metin dizelerini Max() ve MIN() kullanabilirsiniz. Metin dizelerini ile alfabetik olarak sıralanan ve ilk alın ve en son.

Ancak, sayısal alanlar dışında herhangi bir şeyle Sum() kullanamazsınız. Bu durum, Avg() için de geçerlidir.

### <a name="use-the-percentile-function-with-the-measure-command"></a>Kullanım ölçüm komutuyla yüzdebirlik işlevi
Bunu yalnızca sayısal alanlarında kullanabilirsiniz yüzdebirlik işlevi Avg() ve Sum() benzer. 1 ile 99 arasında bir sayısal alana arasında herhangi bir yüzdebirlik kullanabilirsiniz. Her ikisi de kullanabilirsiniz **yüzdelik** ve **pct** komutları. Aşağıda birkaç örnek verilmiştir:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Where kullanmak komutu
Komutu bir filtre gibi çalışır, ancak daha fazla ölçü komutu tarafından – üretilen toplanmış sonuçlara filtre uygulamak için ardışık düzendeki uygulanabilir olduğunda ham sonuçları aksine, filtrelenir. bir sorgu başında.

Örneğin:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Başka bir kanal Ekle "|" karakter ve yalnızca bilgisayar, ortalama CPU olduğu aşağıdaki örnekle % 80 yukarıda almak için komut nereye:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Microsoft System Center - Operations Manager aşinaysanız where komutunun Yönetim Paketi bağlamında düşünebilirsiniz. Örnek bir kural olsaydı, sorgunun ilk bölümü veri kaynağı olabilir ve nerede komutu koşul algılama olacaktır.

Bir kutucuk olarak sorgu kullanabilirsiniz **My Pano**, farklı türlerdeki bilgisayar CPU aşırı kullanılan olduğunda görmek için bir izleyici. Panolar hakkında daha fazla bilgi için bkz: [günlük analizi özel bir pano oluşturun](log-analytics-dashboards.md). Ayrıca, oluşturabilir ve mobil uygulamayı kullanarak panoları kullanın. Daha fazla bilgi için bkz: [OMS mobil uygulama ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). Aşağıdaki görüntü iki alt döşemeleri İzleyicisi görüntülenen listesini görebilirsiniz ve bir sayı olarak. Esas olarak, her zaman sıfır ve listesi boş olamaz sayının istiyor. Aksi halde, bir uyarı koşulu belirtir. Gerekli olursa, bunu göz atmak için kullanabileceğiniz hangi makinelerin baskısı altında olduğunu adresindeki.

![Mobil Pano](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>İçinde işlecini kullanın
*IN* işleci ile birlikte *NOT ın* bağımsız değişken olarak başka bir arama dahil aramalar subsearches kullanmanıza olanak sağlar. Bunların içerdiği içinde başka bir küme ayraçları {} içinde *birincil* veya *dış* arama. Subsearch, genellikle farklı sonuçlar listesi sonucunu daha sonra kendi birincil arama bir bağımsız değişken olarak kullanılır.

Arama ifadesi, ancak bir aramadan oluşturulabilir, doğrudan açıklamak olamaz, veri alt kümesi eşleştirmek için subsearches kullanabilirsiniz. Örneğin, tüm olayları bulmak için bir arama kullanmayı düşünüyorsanız *güvenlik güncelleştirmeleri eksik olan bilgisayarlar*, ilk tanımlayan bir subsearch tasarlamanız gerekir sonra *güvenlik güncelleştirmeleri eksik olan bilgisayarlar* önce bu ana bilgisayarları ait olayları bulur.

Bu nedenle, express *şu anda eksik olan bilgisayarlar gerekli güvenlik güncelleştirmeleri* aşağıdaki sorguyla:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![Arama örnekte](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Listesine sahip olduktan sonra bu bilgisayarlar için olayları arar bir dış (birincil) arama içine bilgisayarların listesini akışına iç arama olarak aramayı kullanabilirsiniz. İç arama ayraçlar içinde kapsayan ve ın işlecini kullanarak dış arama filtresi/alan için olası değerler olarak sonuçlarını besleme yapın. Sorgu benzeyecektir:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![Arama örnekte](./media/log-analytics-log-searches/oms-search-in02-revised.png)

Sistem Güncelleştirme değerlendirmesi 24 saatte bir anlık görüntü tüm bilgisayarların aldığından zaman filtresi iç aramada kullanılan de dikkat edin. İç sorgu, yalnızca bir gün için arama yaparak daha basit ve hassas yapabilirsiniz. Dış arama bunun yerine zaman seçimi kullanıcı arabiriminde, son 7 gün olayları alınırken kullanır. Bkz: [Boole işleçleri](#boolean-operators) zamanı işleçleri hakkında daha fazla bilgi.

Çünkü, gerçekten yalnızca bir filtre olarak iç arama sonuçlarını dış biri için değer, dış arama komutları yine uygulayabilirsiniz. Örneğin, başka bir ölçü komutuyla yukarıdaki olayları yine de gruplandırabilirsiniz:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![Arama örnekte](./media/log-analytics-log-searches/oms-search-in03-revised.png)

Genellikle, iç sorgunuzu günlük analizi için hizmet tarafı zaman aşımları olduğundan hızlı bir şekilde çalıştırmak ve sonuçları az miktarda dönmek için istersiniz. İç sorgu daha fazla sonuç döndürürse, sonuç listesi, hangi büyük olasılıkla yanlış sonuçları döndürmek dış arama neden olabilecek kesilmiş.

İç arama şu anda sağlamak için ihtiyaç duyduğu başka bir kuraldır *toplanmış* sonuçları. Diğer bir deyişle, içermesi gereken bir *ölçü* komut; şu anda bir dış arama ham sonuçları akışı olamaz.

Ayrıca, yalnızca bir ın işleci olabilir ve son Filtre sorgusundaki olmalıdır. Bu temelde engeller birden çok subsearches çalıştıran veya birden çok giriş işleç olamaz: önemli noktadır her dış arama için yalnızca bir alt/iç arama mümkündür.

Bu sınırları olsa da pek çok bağlantılı aramalar IN sağlar ve bilgisayarlar gibi gruplarına benzer bir şey tanımlamanızı sağlar kullanıcılar veya dosyaları – ne olursa olsun, veri alanları içermelidir. Daha fazla örnekler şunlardır:

**Tüm güncelleştirmeler, otomatik güncelleştirme ayarlarını devre dışı bilgisayarlardan eksik**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**SQL Server (SQL değerlendirmesi çalıştırdığı =) çalıştıran bilgisayarlardan tüm hata olayları**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Etki alanı denetleyicileri (AD değerlendirmesi çalıştırdığı =) bilgisayarlardan tüm güvenlik olayları**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Hangi diğer hesapları burada hesabı BACONLAND\jochan günlüğe bilgisayarlara oturum?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Ayrı komutunu kullanın
Adı da anlaşılacağı gibi bu komutu bir alan için farklı değerler listesini sağlar. Bu, şaşırtıcı basit ancak çok yararlı olur. Aynı ölçü count() komutuyla yanı sıra, aşağıda gösterilen elde edilebilir.

```
Type=Event | Measure count() by Computer
```

![FARKLI arama komut örneği](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Tüm ilgilendiğiniz ancak, farklı değerleri olmayan değerleri sonra DISTINCT sağlayabilir sahip belge sayısını ve yalnızca bir listesi temizleyici ve çıkış ve aşağıda gösterildiği gibi daha kısa sözdizimi okuması daha kolay olur.

```
Type=Event | Distinct Computer
```
![FARKLI arama komut örneği](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Kullanım ölçümü komutuyla countdistinct işlevi
Countdistinct işlevi her grup içindeki farklı değerleri sayar. Örneğin, her tür için raporlama benzersiz bilgisayar sayısı için kullanılabilir:

```
* | measure countdistinct(Computer) by Type
```

![OMS-countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Ölçü aralığı komutunu kullanın
İle gerçek zamanlı performans verileri toplama toplayabilir ve herhangi bir performans sayacı günlük analizi, görselleştirme. Yalnızca sorgu girme **türü: Perf** sayaçları ve günlük analizi ortamınızdaki sunucuların sayısına dayalı ölçüm grafikleri binlerce döndürür. İsteğe bağlı ölçüm toplama, bir üst düzey ve gerektiği gibi daha ayrıntılı veri derin Dalış ortamınızdaki genel ölçümlere da bakabilirsiniz.

Tüm bilgisayarlar arasında ortalama CPU ne olduğunu bilmeniz istediğiniz varsayalım. Sonuçları düzeltilmesini çünkü her bilgisayar için ortalama CPU bakarak faydalı olmayabilir. Daha fazla bilgi aramak için Sonuç kümenizi daha küçük birer penceresi öbekleri ve görünüm bir zaman serisinin arasında farklı boyutları toplayabilirsiniz. Örneğin, saatlik ortalama CPU kullanımı tüm bilgisayarlar arasında şu şekilde gerçekleştirebilirsiniz:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![Ölçü ortalama aralığı](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Varsayılan olarak bu sonuçları çok serisi etkileşimli satırı grafik görüntülenir.  Bu grafik serisi (y ekseni rescaling ile), yakınlaştırma ve vurgulama geçiş destekler.  Tablo görüntüleme seçeneği gerekirse ham verileri görüntülemek için kullanılabilir durumda kalır.

Diğer alanlara göre de gruplandırabilirsiniz. Bu örnekte, belirli bir bilgisayar için tüm % sayaçları adresindeki arayan ve saatlik 70 yüzdebirlik değeri her sayacının ne olduğunu bilmek istiyorum:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Unutmayın, bu sorguları performans sayaçları için sınırlı değildir ' dir. Bunları herhangi bir ölçümü uygulayabilirsiniz. Bu örnekte, ı W3C IIS günlüklerine arayan. Her isteği işlemek için bir 5 dakikalık aralık boyunca geçen en fazla süreyi biliyor isteyebilirsiniz:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Tek bir sorguda birden çok toplamalar kullanın
Birden çok toplama yan tümcesi bir ölçü komutta belirtebilirsiniz.  Her biri bağımsız olarak diğer adı olabilir.  Bir diğer ad verilmemişse elde edilen alan adı olan toplama işlevi (yani, avg(CounterValue)) için "avg(CounterValue)". kullanılan olacaktır

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Bir örnek daha:

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Sonraki adımlar
Günlük aramaları hakkında ek bilgi için bkz:

* Kullanım [günlük analizi özel alanları](log-analytics-custom-fields.md) günlük aramaları genişletmek için.
* Gözden geçirme [günlük analizi oturum Arama başvurusu](log-analytics-search-reference.md) tüm arama alanlarını ve günlük analizi kullanılabilir modelleri görüntülemek için.
