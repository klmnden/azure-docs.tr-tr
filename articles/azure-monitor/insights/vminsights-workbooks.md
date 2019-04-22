---
title: Azure İzleyici çalışma kitapları ile etkileşimli raporlar oluşturun | Microsoft Docs
description: Karmaşık önceden tanımlanmış ve özel parametreli içeren çalışma kitapları için Azure İzleyici VM'ler için raporlama basitleştirin.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/05/2019
ms.author: magoedte
ms.openlocfilehash: 90c236347380bb5d5e51db56d0f431d2659a7258
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59288719"
---
# <a name="create-interactive-reports-with-azure-monitor-workbooks"></a>Azure İzleyici çalışma kitapları ile etkileşimli raporlar oluşturun

Çalışma kitapları, metin, birleştirme [oturum sorguları](../log-query/query-language.md), Ölçümler ve zengin etkileşimli raporları parametreleri. Çalışma kitapları, aynı Azure kaynaklarına erişimi olan tüm diğer ekip üyeleri tarafından düzenlenebilir.

Çalışma kitapları gibi senaryolar için yararlıdır:

* Sanal makinenizin kullanım, ilgilendiğiniz ölçümleri önceden bilmediğinizde keşfetme: CPU kullanımı, disk alanı, bellek, ağ bağımlılıklarını, vs. Başka kullanım analizi Araçları çalışma kitapları, birden çok türde görselleştirmeler ve analizleri, bu tür bir serbest biçimli araştırması için harika yönetilmelerini birleştirmek olanak tanır.
* Takımınız için son sağlanan bir VM, anahtar sayaçları ve diğer günlük olayları için ölçümleri göstererek performansıyla açıklayan.
* Sanal makinenizin yeniden boyutlandırma bir deneme sonuçlarını diğer takım üyeleriyle paylaşma. Denemeyi metinle amaçlarını açıklayan ve ardından her kullanım ölçümü ve analiz sorguları Göster her ölçümü yukarıda veya aşağıda-hedefi olup için açık çağrı-çıkarmayı birlikte deneme değerlendirmek için kullanılır.
* Kesinti etkisini vm'nizin gelecekte kesintileri önlemek için verileri, açıklama metnini ve sonraki adımları ayrıntılı bir birleştirme, kullanım raporlama.

VM'ler için Azure İzleyici başlamanıza yardımcı olmak için birkaç çalışma kitaplarını içerir ve bunlar aşağıdaki tabloda özetlenmiştir.

| Çalışma kitabı | Açıklama | Kapsam |
|----------|-------------|-------|
| Performans | Üst N listesi ve tüm etkin Log Analytics performans sayaçları yararlanan tek bir çalışma kitabı grafikler görünümünde özelleştirilebilir bir sürümü sağlar.| Uygun ölçekte |
| Performans sayaçları | Üst N grafik görünümü arasında çok sayıda performans sayaçları. | Uygun ölçekte |
| Bağlantılar | Bağlantıları, izlenen Vm'lerden gelen ve giden bağlantılara kapsamlı bir görünümünü sağlar. | Uygun ölçekte |
| Etkin Bağlantı Noktaları | İzlenen Vm'leri ve seçilen bir zaman çerçevesinde etkinliklerini bağlantı noktalarına bağlı işlemlerin bir listesini sağlar. | Uygun ölçekte |
| Açık Bağlantı Noktaları | Bağlantı noktalarının sayısı izlenen Vm'lerinizde açın ve Ayrıntılar Bu bağlantı noktalarını açma sağlar. | Uygun ölçekte |
| Başarısız Bağlantılar | Başarısız bağlantı sayısı hatası eğilimi, izlenen Vm'leri görüntülemek ve hatalarının yüzdesini büyümediğini. | Uygun ölçekte |
| Güvenlik ve Denetim | Genel bağlantılarda IP uç noktalarını genel bulunduğu kötü amaçlı bağlantıları, raporlar, TCP/IP'yi trafik analizi.  Tüm özellikleri etkinleştirmek için güvenlik algılanabilmesi gerekir. | Uygun ölçekte |
| TCP Trafiği | İzlenen Vm'lerinizi ve bunların gönderilen, alınan ve toplam ağ kişilerinin sıralı bir rapor, bir kılavuzda trafiği ve bir eğilim çizgisi görüntülenir. | Uygun ölçekte |
| Trafik Karşılaştırması | Bu çalışma kitaplarını tek bir makine ya da bir makine grubu için ağ trafiği eğilimleri karşılaştırmanıza olanak tanır. | Uygun ölçekte |
| Performans | Tüm etkin Log Analytics performans sayaçları yararlanan bizim performans görünümü özelleştirilebilir bir sürümü sağlar. | Tek VM | 
| Bağlantılar | Bağlantıları, sanal makineden gelen ve giden bağlantılara kapsamlı bir görünümünü sağlar. | Tek VM |
 
## <a name="starting-with-a-template-or-saved-workbook"></a>Bir şablonla başlayarak veya kaydedilmiş bir çalışma kitabı

Bir çalışma kitabı oluşan bağımsız olarak düzenlenebilir grafikleri, tablolar, metin, bölümlerini yapılır ve giriş denetimleri. Çalışma kitapları daha iyi anlamak için şimdi bir şablonu açarak işleme başlayın ve özel bir çalışma kitabı oluşturmada size yol. 

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Seçin **sanal makineler**.

3. Listeden bir VM seçin.

4. VM sayfasında içinde **izleme** bölümünden **Insights (Önizleme)**.

5. VM içgörüler sayfasında **performans** veya **haritalar** sekmesini seçip **görünümü çalışma kitapları** sayfasındaki bağlantıdan. 

    ![Çalışma kitapları için Gezinti bölmesinin ekran görüntüsü](media/vminsights-workbooks/workbook-option-01.png)

6. Aşağı açılan listesinden **Galerisine Git** listenin en üstündeki.

    ![Çalışma kitabı aşağı açılan listesinin ekran görüntüsü](media/vminsights-workbooks/workbook-dropdown-gallery-01.png)

    Çalışma kitabı Galerisi başlamanıza yardımcı olmak için önceden oluşturulmuş çalışma kitaplarını sayısı ile başlatılır.

7. İle başlayacağız **varsayılan şablon**, başlığı altında bulunan **Hızlı Başlangıç**.

    ![Çalışma kitabı galeri görüntüsü](media/vminsights-workbooks/workbook-gallery-01.png)

## <a name="editing-workbook-sections"></a>Çalışma kitabı bölümleri düzenleme

Çalışma kitapları iki mod vardır: **düzenleme modunda**, ve **Okuma modunda**. Varsayılan şablon çalışma kitabı ilk kez başlatıldığında açılır **düzenleme modunda**. Bu, tüm adımlar ve aksi takdirde gizli parametreleri de dahil olmak üzere çalışma kitabının içeriğini gösterir. **Okuma Modu** Basitleştirilmiş rapor stil görünümünü sunar. Okuma Modu, yine de temel alınan mekanizması yalnızca birkaç tıklamayla ulaşabiliyoruz değiştirilmek üzere gerektiğinde yaparken bir raporu oluşturmaya giden karmaşıklığı hemen soyut olanak tanır.

![Azure İzleyici Vm'leri çalışma kitapları bölümün denetimleri düzenleme](media/vminsights-workbooks/workbook-new-workbook-editor-01.png)

1. Bitirdiğinizde bir bölümünü Düzenleyen, tıklayın **düzenleme Bitti** bölümünün sol alt köşedeki.

2. Bir bölümün bir kopyasını oluşturmak için tıklayın **bu bölümü kopyalayın** simgesi. Yinelenen bölümleri oluşturmak, önceki yinelemelerin kaybetmeden bir sorgu üzerinde gezinmek için yol için harika bir işlemdir.

3. Bir çalışma kitabı bir bölümde yukarı taşımak için tıklatın **Yukarı Taşı** veya **Aşağı Taşı** simgesi.

4. Bir bölümün kalıcı olarak kaldırmak için tıklayın **Kaldır** simgesi.

## <a name="adding-text-and-markdown-sections"></a>Metin ve Markdown bölümleri ekleme

Başlıklarını, açıklamaları ve yorum, çalışma kitaplarına ekleme yardımcı tablolar ve grafikler bir dizi bir anlatım açın. Çalışma kitapları destek metin bölümlerinde [Markdown söz dizimi](https://daringfireball.net/projects/markdown/) başlıklar, kalın, italik ve madde işaretli listeler gibi biçimlendirme metin.

Bir metin bölümüne çalışma kitabınıza eklemek için **metin eklemek** çalışma kitabını sayfanın alt kısmında veya herhangi bir bölümü altındaki düğmeyi.

## <a name="adding-query-sections"></a>Sorgu bölümleri ekleme

![Sorgu bölümünde çalışma kitapları](media/vminsights-workbooks/005-workbook-query-section.png)

Çalışma kitabınıza sorgu bölümü eklemek için **Sorgu Ekle** çalışma kitabını alt kısmındaki ya da herhangi bir bölümü altındaki düğmesi.

Sorgu bölümler son derece esnektir ve aşağıdaki gibi sorulara yanıt için kullanılabilir:

* Ağ trafiği bir artış ile aynı süre boyunca my CPU kullanımı nasıldı?
* Geçen ay kullanılabilir disk alanı eğilimi neydi?
* Ağ bağlantısı hataları kaç tane sanal Makinem son iki hafta boyunca denediniz mi? 

Siz de yalnızca sınırlı değildir, sanal makinenin bağlamdan sorgulama kitabından başlattı. Bu kaynaklara erişim iznine sahip olduğu sürece birden çok sanal makinelerin yanı sıra Log Analytics çalışma alanları, sorgulayabilirsiniz.

Diğer Log Analytics çalışma alanları veya belirli bir Application Insights uygulamasını kullanarak verileri içerecek şekilde **çalışma** tanımlayıcısı. Kaynaklar arası sorguları hakkında daha fazla bilgi için bkz [resmi rehberlik](../log-query/cross-workspace-query.md).

### <a name="advanced-analytic-query-settings"></a>Gelişmiş analitik sorgu ayarları

Her bölüm ayarları erişilebilir kendi Gelişmiş ayarları olan ![çalışma kitapları bölümünde düzenleme denetimleri](media/vminsights-workbooks/006-settings.png) simgesi sağında bulunan **parametreleri ekleme** düğmesi.

![Azure İzleyici Vm'leri çalışma kitapları bölümün denetimleri düzenleme](media/vminsights-workbooks/007-settings-expanded.png)

|         |          |
| ---------------- |:-----|
| **Özel bir genişlik**    | Daha iyi grafikler ve tablolar etkileşimli zengin raporlar düzenlemenize olanak sağlayan tek bir satırda öğe sayısını sığacak şekilde rastgele boyutu, bir öğe sağlar.  |
| **Koşullu olarak görünür** | Okuma modunda olduğunda bir parametresini temel alan adımları gizlemek için bu seçeneği belirtin. |
| **Parametreyi Dışarı Aktar**| Kılavuz veya değerleri değiştirmesini veya görünür olmasını için sonraki adımları neden grafik seçili bir satır izin verir.  |
| **Düzenlenmediği sırada sorguyu Göster** | Grafiğe veya tabloya bile yukarıdaki sorgunun Okuma modunda olduğunda görüntüler.
| **Düzenlenmediği sırada analiz düğmesi açık Göster** | Tek tıklamayla erişim izni vermek için grafiğin sağ köşesinde için mavi Analytics simgesi ekler.|

Bu ayarların birçoğu oldukça kolay anlaşılır ancak anlamak için **parametreyi dışarı aktar** bir çalışma kitabı incelemek iyidir yapar bu işlevi kullanın.

Önceden oluşturulmuş çalışma kitaplarını - birini **TCP trafiği**, bir VM'den bağlantı ölçümü hakkında bilgi sağlar.

Çalışma kitabı ilk bölümü, günlük veri temel alır. İkinci bölüm de günlük sorgu verilere dayalı, ancak ilk tabloda bir satır seçilmesi grafikleri içeriğini etkileşimli olarak güncelleştirir:

![Azure İzleyici Vm'leri çalışma kitapları bölümün denetimleri düzenleme](media/vminsights-workbooks/008-workbook-tcp-traffic.png)

Davranıştır aracılığıyla olası **bir öğe seçildiğinde, parametreyi dışarı aktar** Gelişmiş tablonun günlük sorgusu içinde etkin olan ayarlar.

![Azure İzleyici Vm'leri çalışma kitapları bölümün denetimleri düzenleme](media/vminsights-workbooks/009-settings-export.png)

Ardından bölüm başlığı ve grafikleri tarafından kullanılan değerleri kümesi oluşturmak için bir satır seçili olduğunda ikinci günlük sorgusu sonra verilen değerleri kullanır. Hiçbir satır seçilirse, grafikler ve bölüm başlığı gizler. 

Örneğin, İkinci bölüm içindeki gizli parametre Kılavuz Seçilen satırdan şu başvuruyu kullanır:

```
VMConnection
| where TimeGenerated {TimeRange}
| where Computer in ("{ComputerName}") or '*' in ("{ComputerName}") 
| summarize Sent = sum(BytesSent), Received = sum(BytesReceived) by bin(TimeGenerated, {TimeRange:grain})
```

## <a name="adding-metrics-sections"></a>Ölçümleri bölümleri ekleme

Ölçümleri bölümleri etkileşimli raporlarınızda içeri Azure İzleyici ölçüm verilerini birleştirmek için tam erişim verin. VM'ler için Azure İzleyici'de önceden oluşturulmuş çalışma kitaplarını genellikle ölçüm verileri yerine analitik sorgu verilerini içerir.  Ölçüm verileri ile çalışma kitaplarını oluşturmak, her iki özelliği en iyi şekilde tek bir yerde yararlanmaya olanak tanıyan seçebilirsiniz. Ayrıca, erişiminiz olan Aboneliklerde kaynaklardan gelen ölçüm verilerini çekin olanağı vardır.

Sanal makine verilerinin kılavuz görselleştirme CPU performans sağlamak için bir çalışma kitabına çekilen bir örnek aşağıda verilmiştir:

![Azure İzleyici Vm'leri çalışma kitapları bölümün denetimleri düzenleme](media/vminsights-workbooks/010-metrics-grid.png)

## <a name="adding-parameter-sections"></a>Parametre bölümleri ekleme

Çalışma kitabı parametreleri sorgu veya metin bölümleri el ile düzenlemek zorunda kalmadan değerleri çalışma kitabında değiştirmenize izin verir. Bu, temel alınan analytics sorgu dili anlamanıza gerek kalmadan gerekliliğini ortadan kaldırır ve çalışma kitabı tabanlı raporlama olası hedef kitle büyük ölçüde genişletir.

Parametre değerlerini sorgu, metin veya diğer parametre bölümleri gibi köşeli ayraçlar parametrenin adını koyarak değiştirilir ``{parameterName}``. Parametre adları benzer kurallara JavaScript tanımlayıcıları, alfabetik bir karakter veya alt çizgi, ardından alfasayısal karakterler veya alt çizgi sınırlıdır. Örneğin, **a1** izin verilir, ancak **1a** izin verilmiyor.

Çalışma kitabının En üstten başlayarak ve sonraki adımları aşağı akan doğrusal parametrelerdir.  Bir çalışma kitabında olarak bildirilen parametreleri, daha önce bildirilen parametreleri geçersiz kılabilirsiniz. Bu sorgular daha önce tanımlanan parametreler değerlere erişmek için kullandığınız parametre de sağlar. Bir parametrenin adım içinde kendisi de burada aynı o adımda daha önce bildirilen bir parametre sağa parametreleri güvenebileceğiniz soldan sağa doğrusal parametrelerdir.
 
Şu anda desteklenen parametreler, dört farklı tür vardır:

|                  |      |
| ---------------- |:-----|
| **Metin**    | Bir metin kutusu düzenlemesine olanak tanır ve isteğe bağlı olarak, varsayılan değeri doldurmak için bir sorgu sağlayabilirsiniz. |
| **Açılan menü** | Kullanıcının bir değerler kümesinden seçmesine izin verir. |
| **Zaman aralığı Seçici**| Önceden tanımlı bir zaman aralığı değerleri kümesinden seçin veya bir özel zaman aralığı seçin izin verir.|
| **Kaynak Seçici** | Çalışma kitabı için seçilen kaynakları arasından seçim izin verir.|

### <a name="using-a-text-parameter"></a>Bir metin parametresini kullanma

Değeri metin kutusuna bir kullanıcı türleri kaçış veya Alıntısı, doğrudan sorgusunda değiştirilir. Gereksinim duyduğunuz değer bir dize ise, sorgu parametresi tırnak olmalıdır (gibi **'{parameter}'**).

Metin parametresi herhangi bir kullanılmak üzere bir metin kutusundaki değeri verir. Tablo adı, sütun adı, işlev adı, işleç, vb. olabilir.  Bir ayar metin typ parametru **varsayılan değeri analiz sorgusundan Al**, metin kutusuna için varsayılan değer doldurmak için bir sorgu kullanılacak çalışma kitabı yazarı sağlar.

(Satır 0, 0 sütun) ilk satırın yalnızca ilk değer, bir günlük sorgusu varsayılan değerden kullanırken, varsayılan değer olarak kullanılır. Bu nedenle, yalnızca bir satır ve bir sütun döndürmek için sorguyu sınırlamak için önerilir. Sorgu tarafından döndürülen diğer veriler yoksayılır. 

Her sorgunun döndürdüğü değer doğrudan hiçbir kaçış veya Alıntısı ile değiştirilecek. Sorgu hiçbir satır döndürürse, parametre sonuçları (parametre gerekli değilse), boş bir dize olan veya (parametre gerekli ise) tanımlı değil.

### <a name="using-a-drop-down"></a>Bir açılan kullanma

Açılan parametre türü, bir açılır denetimi oluşturmak, bir veya daha çok değerlerin seçilmesine olanak verecek sağlar.

Açılan günlük sorgusu veya JSON doldurulur. Sorgu bir sütun döndürürse, bu sütundaki değerleri hem değer hem de açılır denetimin etiketinde ' dir. Sorgu iki sütun döndürürse, ilk sütun değeridir ve ikinci sütun, açılan menü gösterilen etiketidir. Sorgu üç sütun döndürürse, üçüncü sütunda, açılan varsayılan seçimi göstermek için kullanılır. Bu sütun her türlü olabilir, ancak basit bool veya sayısal türler, burada 0 false ve 1 true kullanmaktır.

Sütun bir dize türü ise, null veya boş dize false olarak kabul edilir ve başka bir değer true olarak değerlendirilir. Tek seçim açılır listeleri için varsayılan seçim true değerine sahip ilk değer kullanılır.  Birden çok seçimi açılan listeler için varsayılan seçili kümesi olarak true değerine sahip tüm değerleri kullanılır. Açılır öğelerde hangi sırada satırları sorgu döndürülen gösterilmektedir. 

Bağlantılara genel bakış raporu mevcut parametreler bakalım. Düzenle simgesini tıklayın **yönü**.

![Azure İzleyici Vm'leri çalışma kitapları bölümün denetimleri düzenleme](media/vminsights-workbooks/011-workbook-using-dropdown.png)

Bu başlatacak **Düzenle parametresi** menü öğesi.

![Azure İzleyici Vm'leri çalışma kitapları bölümün denetimleri düzenleme](media/vminsights-workbooks/012-workbook-edit-parameter.png)

JSON içerikle doldurulur rastgele bir tablo oluşturmanıza olanak sağlar. Örneğin, aşağıdaki JSON iki değer açılan menü oluşturur:

```
[
    { "value": "inbound", "label": "Inbound"},
    { "value": "outbound", "label": "Outbound"}
]
```

Daha uygun bir örnek, bir dizi performans sayacı adına göre seçmek için bir açılan kullanıyor:

```
Perf
| summarize by CounterName, ObjectName
| order by ObjectName asc, CounterName asc
| project Counter = pack('counter', CounterName, 'object', ObjectName), CounterName, group = ObjectName
```

Sorgu sonuçları aşağıdaki gibi görüntülenir:

![Performans sayacı açılır](media/vminsights-workbooks/013-workbook-edit-parameter-perf-counters.png)

Açılan listeler özelleştirme ve etkileşimli raporlar oluşturmak için son derece güçlü araçlardır.

### <a name="time-range-parameters"></a>Zaman aralığı parametreleri

Kendi özel zaman aralığı parametresi açılan parametre türü aracılığıyla yapabilirsiniz, ancak aynı derecede esnekliğe ihtiyacınız yoksa kullanıma hazır zaman aralığı parametre türü kullanabilirsiniz. 

Beş dakika veya Son 90 gün önce 15 varsayılan aralıklarını zaman aralığı parametre türleri vardır. Açık Başlat'ı seçin ve zaman aralığına ilişkin değerleri durdurmak rapor operatörü izin veren özel saat aralığı seçimine izin vermek için bir seçenek de mevcuttur.

### <a name="resource-picker"></a>Kaynak seçici

Kaynak Seçici parametre türü, bazı kaynak türleri için raporun kapsamını olanağı sağlar. Kaynak Seçici türü yararlanan önceden oluşturulmuş bir çalışma kitabının bir örneğini **performans** çalışma kitabı.

![Çalışma alanları açılır](media/vminsights-workbooks/014-workbook-edit-parameter-workspaces.png)

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Kaydetme ve çalışma kitapları takımınızla paylaşma

Çalışma kitapları, Log Analytics çalışma alanı veya çalışma kitapları galeri nasıl eriştiğinize bağlı olarak bir sanal makine kaynağı kaydedilir. Çalışma kitabı için kaydedilebilir **raporlarım** , hem de özel bölümü **paylaşılmış olan raporlar** herkesin erişebileceği kaynağına erişimi olan bir bölüm. Kaynakta tüm çalışma kitaplarını görüntülemek için tıklayın **açık** eylem çubuğunda düğme.

Şu anda kullanımda bir çalışma kitabını paylaşmak için **raporlarım**:

1. Tıklayın **açık** eylem çubuğunda
2. Paylaşmak istediğiniz çalışma kitabını yanındaki "…" düğmesine tıklayın
3. Tıklayın **paylaşılan Raporlar'a Taşı**.

Bir çalışma kitabı bağlantısını içeren bir ya da e-posta ile paylaşmak için tıklatın **paylaşmak** eylem çubuğunda. Bağlantının alıcıları çalışma kitabını görüntülemek için Azure portalında bu kaynağa erişimi gerektiğini aklınızda bulundurun. Gereken en az alıcılar düzenlemeler yapmak için kaynak için katkıda bulunan izinleri.

Azure panosu için bir çalışma kitabı bağlantısını sabitlemek için:

1. Tıklayın **açık** eylem çubuğunda
2. Sabitlemek istediğiniz çalışma kitabının yanındaki "…" düğmesine tıklayın
3. Tıklayın **panoya Sabitle**.

## <a name="next-steps"></a>Sonraki adımlar
Sistem durumu özelliği kullanmayı öğrenmek için bkz: [Azure VM durumunu görüntüle](vminsights-health.md), veya bulunan Uygulama bağımlılıklarını görüntülemek için bkz. [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md). 