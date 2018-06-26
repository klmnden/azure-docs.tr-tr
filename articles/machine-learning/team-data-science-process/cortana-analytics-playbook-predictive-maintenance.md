---
title: Tahmine dayalı bakım çözümleri için Azure AI Kılavuzu | Microsoft Docs
description: Birden fazla dikey sektörlerde Tahmine dayalı bakım çözümü'nın temelini oluşturan veri bilimi, kapsamlı bir açıklama.
services: machine-learning
author: fboylu
manager: cgronlun
editor: ''
ms.assetid: 2e8b66db-91eb-432b-b305-6abccca25620
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: fboylu
ms.openlocfilehash: ff2e1660ffcc1f397697b27084e000371c7c84f3
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938018"
---
# <a name="azure-ai-guide-for-predictive-maintenance-solutions"></a>Tahmine dayalı bakım çözümleri için Azure AI Kılavuzu

## <a name="summary"></a>Özet

Tahmine dayalı Bakım (**PdM**) birkaç sektörlerde işletmelerde yüksek varlık kullanımı ve işletim maliyetlerini tasarruf yardımcı olabilecek Tahmine dayalı analiz yaygın bir uygulamadır. Bu kılavuz iş ve analitik yönergeleri ve başarıyla geliştirmek ve PdM çözümleri kullanarak dağıtmak için en iyi yöntemler araya getirir [Microsoft Azure AI platform](https://azure.microsoft.com/overview/ai-platform) teknolojisi.

Yeni başlayanlar, bu kılavuzda sektöre özgü iş senaryolarını ve bu senaryolar için PdM niteleme işlemi sunar. Veri gereksinimleri ve modelleme teknikleri PdM çözümleri oluşturmak üzere de sağlanır. Ana içerik Kılavuzu'nun veri hazırlığı, özellik Mühendisliği, model oluşturma ve model operationalization adımları dahil olmak üzere veri bilimi üzerinde - işlemidir. Bu anahtar kavramları tamamlamak için bu kılavuzu PdM uygulama geliştirme hızlandırmak için çözüm şablonları kümesini listeler. Kılavuzu de yararlı eğitim kaynakları bakış veri bilimi arkasında AI hakkında daha fazla bilgi için işaret eder. 

### <a name="data-science-guide-overview-and-target-audience"></a>Veri bilimi Kılavuzu genel bakış ve hedef kitle
Bu kılavuzun ilk yarı tipik iş sorunları, bu sorunları gidermek için PdM uygulama yararlarını açıklar ve bazı ortak kullanım durumları listeler. İşletmelerin karar alıcıları (BDMs) bu içeriği yararlı olacaktır. İkinci yarısındaki PdM arkasında veri bilimi açıklanmakta ve bu Kılavuzu'nda gösterilen ilkeleri kullanılarak oluşturulan PdM çözümlerinin listesini sağlar. Öğrenme yolları ve eğitim malzemesi işaretçiler de sağlar. Teknik Karar Alıcılar (TDMs) bu içeriği yararlı bulabilirsiniz.

| İle Başlat... | Kullanıyorsanız... |
|:---------------|:---------------|
| [Tahmine dayalı bakım için İş Gerekçesi](#Business-case-for-predictive-maintenance) |kapalı kalma süresi ve işletim maliyetlerini düşürmeye ve donanım kullanımını geliştirmek için arayan bir iş karar veren (BDM) |
| [Tahmine dayalı bakım için veri bilimi](#Data-Science-for-predictive-maintenance) |benzersiz veri işleme ve Tahmine dayalı bakım AI gereksinimleri anlamak için PdM teknolojileri değerlendiren bir teknik karar veren (TDM) |
| [Tahmine dayalı bakım çözümü şablonları](#Solution-templates-for-predictive-maintenance)|bir yazılım Mimarı veya AI demo veya bir, kavram hızlı bir şekilde bildirimde isteyen Geliştirici |
| [Tahmine dayalı bakım için eğitim kaynakları](#Training-resources-for-predictive-maintenance) | Yukarıdaki, bir bölümünü veya tamamını ve veri bilimi, araçları ve teknikleri temel kavramları öğrenin.

### <a name="prerequisite-knowledge"></a>Önkoşul bilgisi
BDM içeriği tüm önceki veri bilimi bilgisine sahip okuyucu beklemiyor. TDM içerik için istatistikler ve veri bilimi temel bilgiye yardımcı olur. Azure veri ve AI Hizmetleri, Python, R, XML ve JSON önerilir. AI teknikleri, Python ve R paketlerinde uygulanır. Çözüm şablonları Azure Hizmetleri, geliştirme araçları ve SDK kullanılarak uygulanır.

## <a name="business-case-for-predictive-maintenance"></a>Tahmine dayalı bakım için İş Gerekçesi

İşletmelerin en yüksek verimlilik ve kendi dönüş sermaye yatırımlarına hayata geçirmek için kullanımı çalıştırması için kritik donanım gerektirir. Bu varlıklar uçak motorları, turbines, asansörler veya milyonlarca - her gün cihazları fotokopi makineleri, kahve makineler veya Su coolers gibi aşağıya doğru maliyet endüstriyel chillers - çeşitli işlemleri ifade edebilir.
- Varsayılan olarak, çoğu işletme Bel _düzeltme Bakım_, burada bölümleri olarak değiştirilir ve bunlar başarısız olduğunda. Düzeltme bakım sağlar bölümleri tamamen kullanılır (Bu nedenle değil İsraf bileşenin ömür), ancak maliyetleri iş kapalı kalma süresi, İşgücü ve zamanlanmamış bakım gereksinimlerini (kapalı, saat, ya da kullanışsız konumları).
- İleri düzey, işletmelerin uygulama adresindeki _önleyici bakım_, burada bunlar bir bölümü için yararlı kullanım ömrü belirlemek ve korumak veya bir hatadan önce değiştirin. Koruyucu bakım zamanlanmamış ve geri dönülemez hatalarını önler. Ancak zamanlanmış kapalı kalma süresi yüksek maliyetinden, bileşenin tam yaşam süresi kullanın ve işgücü hala önce eksik kullanımı kalır.
- Amacı _Tahmine dayalı Bakım_ etkinleştirerek, düzeltme ve önleyici bakım arasındaki dengeyi en iyi duruma getirmek için _tam zamanında_ bileşenlerinin değiştirme. Yakın bir hata olduğunda bu yaklaşım yalnızca bu bileşenlerin yerini alır. Bileşen lifespans (koruyucu bakım için karşılaştırıldığında) genişletme tarafından ve zamanlanmamış Bakım ve işgücü maliyetleri (düzeltme bakım) azaltma, işletmelerin maliyet tasarrufu ve rekabet avantajları elde edebilirsiniz.

## <a name="business-problems-in-pdm"></a>PdM iş sorunları
İşletmeler yüz beklenmeyen hatalar nedeniyle işletimsel yüksek riskli ve sorunları kök nedenini bir anlayış karmaşık sistemlerinde sınırlı. Anahtar iş sorularını bazıları şunlardır:

- Daha fazla bilgi, donanım ya da sistemin performansını veya işlev açısından algıla.
- Bir varlık yakın gelecekte başlatılamayabilir olup olmadığını tahmin.
- Bir malın kalan kullanım ömrü tahmin edin.
- Bir varlık başarısızlığını ana nedenlerini belirleyin.
- Bakım işlemleri, zaman tarafından bir varlık üzerinde yapılması gerekenleri belirlemenize.

Tipik hedef deyimleri PdM gelen şunlardır:

- Görev kritik ekipmanının işletimsel riskini azaltır.
- Varlıklar üzerinde geri dönüş oranını ortaya çıkmadan önce hataları tahmin ederek çalışmasını artırın.
- Yalnızca zaman bakım işlemleri sağlayarak bakım maliyeti kontrol eder.
- Müşteri attrition alt, marka görüntü ve kaybolan satış geliştirmek.
- Stok düzeylerini sipariş noktayı tahmin ederek çalışmasını azaltarak daha düşük stok maliyetleri.
- Çeşitli bakım sorunlarına bağlı desenleri bulur.
- KPI'ları (ana performans göstergelerini) gibi sistem durumu puanları varlık koşulları belirtin.
- Kalan kullanım ömrü varlıklarını tahmin edin.
- Zamanında bakım etkinlikleri öneririz.
- Yalnızca zaman envanterinde sipariş tarihleri bölümlerinin değiştirmeye tahminlerle etkinleştirin.

Bu hedef deyimleri için başlangıç noktaları şunlardır:

- _Veri bilimcilerine_ analiz ve Tahmine dayalı belirli sorunları çözmek için.
- _Bulut mimarları ve geliştiricileri_ bir uçtan uca çözüm araya için.

## <a name="qualifying-problems-for-predictive-maintenance"></a>Tahmine dayalı bakım sorunları niteleme
Tüm kullanım veya iş sorunlarını etkili bir şekilde PdM tarafından çözülebilir vurgulamak önemlidir. Sorun seçim sırasında ele alınması gereken üç önemli belirleme ölçütlerini vardır:

- Sorun doğası gereği Tahmine dayalı olması gerekir; diğer bir deyişle, olmamalıdır bir hedef veya tahmin etmek için bir sonuç. Sorun de oldukları algılandığında hatalarını önlemek için eylem Temizle yolu olmalıdır.
- Sorun içeren ekipmanının işletimsel geçmişinin bir kayda sahip olmalıdır _iyi ve hatalı sonuçlar_. Hatalı sonuçları azaltmak için yapılan Eylemler kümesi de kullanılabilir olmalıdır kayıtlarının bir parçası olarak. Hata raporlarını, performans düşüşünü bakım günlüklerini onarın ve Değiştir günlükleri de önemlidir. Ayrıca, bunları ve değiştirme kayıtları geliştirmek için üstlendiği onarım de yararlıdır.
- Kaydedilen geçmiş yansıtılması _ilgili_ , verileri _yeterli_ kullanım örneğini desteklemek için yeterli kalite. Veri uygunluğu ve sufficiency hakkında daha fazla bilgi için bkz: [Tahmine dayalı bakım için veri gereksinimlerini](#Data-requirements-for-predictive-maintenance).
- Son olarak, iş sorununun NET bir anlayış sahip etki alanı uzmanlar olması gerekir. Bunlar, iç işlemler ve anlama ve verileri yorumlama analist yardımcı olması için yöntemler farkında olmalıdır. Bunlar ayrıca gerekirse sorunları için doğru verileri topla yardımcı olmak için var olan iş süreçlerini için gerekli değişiklikleri yapın yapabiliyor olmanız gerekir.

## <a name="sample-pdm-use-cases"></a>Örnek PdM kullanım örnekleri
Bu bölümde Havacılık, yardımcı programlar ve taşıma gibi birkaç sektörlerde PdM kullanım örneklerinden koleksiyonu odaklanır. Her bölümde problemini ile başlar ve PdM, iş sorununu ve son olarak bir PdM çözümü avantajlarını çevreleyen ilgili verilerin avantajları açıklanmıştır.

| İş sorununu | PdM gelen avantajları |
|:-----------------|-------------------|
|**Aviation**      |                   |
|_Uçuş gecikmesi ve iptalleri_ mekanik sorunlar nedeniyle. Zamanında onarılamıyor hataları iptal edilmesi uçuşlar neden ve zamanlama ve işlemlerini kesintiye uğratabilir. |PdM çözümleri Gecikmeli veya mekanik hataları nedeniyle iptal Uçağın olasılığını tahmin edebilirsiniz.|
|_Uçak motoru bölümleri hatası_: Uçak motoru bölümü değişikliklerini olan en sık kullanılan bakım görevlerini uçak endüstri içinde arasında. Bakım çözümleri bileşen stok kullanılabilirliği, teslim ve planlama dikkatli yönetim gerektirir|Bileşen güvenilirlik üzerinde Intelligence önemli ölçüde azalma yatırım maliyetlerinde müşteri adayları toplamak bölümlemeye.|
|**Finans** |                         |
|_ATM hatası_ bankacılık endüstri içinde sık karşılaşılan bir sorundur. Buradaki sorun ATM nakit mevzuatı işlem nakit dağıtıcısının kağıt sıkıştı veya bir bölümü hata nedeniyle kesintiye uğrarsa olasılık rapor etmektir. İşlem hataları tahminleri üzerinde bağlı olarak, ATM proaktif olarak hataları oluşmasını önlemek için hizmet verebilir.| Makine yarıda bir işlem üzerinden vermesine izin vermek yerine, istenen hizmetini reddetmek için makine üzerinde tahmin tabanlı program için alternatiftir.|
|**Enerji** |                          |
|_Rüzgar Türbin hataları_: Rüzgar turbines çevre sorumlu ülkede ana enerji kaynağı ve yüksek sermaye maliyetlerini içerir. Bir anahtar Rüzgar turbines Oluşturucu motor bileşenidir. kendi hatası Türbin etkisiz işler. Düzeltmek yüksek oranda pahalı olması.|MTTF (saati başarısızlık) gibi KPI'leri tahmin etmeye Türbin hatalarını önlemek ve en düşük kapalı kalma olun enerji şirketleri yardımcı olabilir. Hata olasılıklar büyük olasılıkla yakında başarısız turbines izlemek için teknisyenleri bildirin ve zamana dayalı bakım regimes zamanlayın. Tahmine dayalı modelleri kök sorunlarının olası nedenleri teknisyenleri yardımcı hatası katkıda bulunan farklı Etkenler Öngörüler daha iyi anlamak sağlar.|
|_Devre kesici hataları_: dağıtımı elektrik ev ve işletmeler için güç satırları enerji teslim güvence altına almak için her zaman çalışır durumda gerektirir. Bağlantı hattı ayırıcıları Yardım sınırlamak veya güç zarar kaçının satırları aşırı yüklemesi sırasında veya olumsuz koşullar hava durumu. İş burada devre kesici hataları tahmin etmek için sorunudur.| PdM çözümleri onarım maliyetlerini azaltma ve bağlantı hattı ayırıcıları gibi donanım Sysprep'in artırmak yardımcı olur. Güç ağ beklenmeyen hataları ve hizmet kesintilerine azaltarak kalitesini yardımcı olurlar.|
|**Taşıma ve lojistik** |    |
|_Fırsatınızdır kapı hataları_: büyük fırsatınızdır şirketler dünyanın işlevsel asansörler milyonlarca için tam yığını hizmeti sağlar. Fırsatınızdır güvenlik, güvenilirlik ve çalışır durumda kalma süresi müşterileri için ana sorunları var. Bu şirketler, düzeltme ve önleyici bakım ve yardımcı olmak için algılayıcılar aracılığıyla bu ve diğer çeşitli öznitelikleri izler. Bir fırsatınızdır, en belirgin müşteri sorun fırsatınızdır kapıları düzgün çalışmıyor. Bu durumda iş sorununu kapı hatalarının olası neden tahmin bir Bilgi Bankası Tahmine dayalı uygulaması sağlamaktır.| Asansörler olası bir 20-30 yıllık kullanım ömrü için büyük yatırımlar ' dir. Bu nedenle her bir potansiyel satış son derece rekabetçi olabilir; Bu nedenle hizmet ve destek için beklentiler yüksek. Tahmine dayalı bakım bu şirketler ürünlerinin içinde kendi rakiplere üzerinde bir avantaj sağlar ve hizmet.|
|_Tekerlek hataları_: tüm yarısı derailments eğitmek ve genel raylar sektör üzerindeki milyarlarca maliyet hataları hesap Tekerlek. Tekerlek hataları de bazen erken bölüneceği raylar neden bozulmasına rayları neden olur. Derailments gibi yıkıcı olaylara raylar sonları sağlama. Bu tür örnekleri önlemek için railways Tekerlek performansını izlemek ve bunları bir önleyici şekilde değiştirin. İş burada tekerleği hataları tahmin sorunudur.| Tekerlek, Tahmine dayalı bakım Tekerlek yalnızca zaman değişikliği ile yardımcı olur |
|_Yeraltı treni tren kapı hataları_: bir ana Yeraltı treni işlemlerinde gecikmeler tren araba kapı hatalarının nedeni. İş burada tren kapı hataları tahmin etmek için sorunudur.|Kapı hatası ya da bir kapı hatası kadar gün sayısını erken tanıma zamanlamaları bakım kapı eğitmek iş İyileştir yardımcı olur.|

Sonraki bölümde, yukarıda açıklanan PdM faydaları hayata geçirmek nasıl ayrıntılarını içine alır.

## <a name="data-science-for-predictive-maintenance"></a>Tahmine dayalı bakım için veri bilimi

Bu bölüm için PdM veri bilimi ilkeleri ve uygulama genel yönergeleri sağlar. TDM, çözümü Mimarı yardımcı olmayı amaçlar veya bir geliştirici anlamak önkoşulları ve PdM için uçtan uca AI uygulamaları oluşturmak için işlem. Bu bölümde gösterileri gözden birlikte okuyabilir ve kavram kanıtı şablonları listelenen [Tahmine dayalı bakım çözümü şablonları](#Solution-templates-for-predictive-maintenance). Ardından, Azure'da PdM çözümünüzü uygulamak için bu ilkeleri ve en iyi yöntemler kullanabilirsiniz.

> [!NOTE]
> Bu kılavuz, veri bilimi okuyucu öğretmeyi tasarlanmamıştır. Çeşitli yararlı kaynaklardan bölümünde daha fazla bilgi için sağlanan [Tahmine dayalı bakım için eğitim kaynakları](#Training-resources-for-predictive-maintenance). [Çözüm şablonları](#Solution-templates-for-predictive-maintenance) kılavuzda listelenen bazı AI tekniğin belirli PdM sorunları gösterir.

## <a name="data-requirements-for-predictive-maintenance"></a>Tahmine dayalı bakım için veri gereksinimleri

Tüm öğrenme başarısını ne öğrettin (a) kalite ve (b) öğrenen özelliği bağlıdır. Tahmine dayalı modelleri geçmiş verileri modellerinden öğrenmek ve bu gözlemlenen düzenlerini esas alarak belirli olasılık ile gelecekteki sonuçları öngörmek. Tahmine dayalı bir modelin doğruluğunu ilgi, sufficiency ve eğitim ve test veri kalitesi bağlıdır. 'Bu modeli kullanarak puanlanır' yeni veri, eğitim ve test verileri olarak aynı özellikleri ve şema olması gerekir. Yeni veri özelliği özelliklere (türü, yoğunluğu, dağıtım vb.), eğitim ve sınama veri kümesi ile eşleşmesi gerekir. Bu bölümde, bu tür veri gereksinimleri sağlamaktır.

### <a name="relevant-data"></a>İlgili veriler

İlk olarak, verileri olmak zorundadır _sorun ilgili_. Göz önünde bulundurun _Tekerlek hatası_ ele alınan kullanım eğitim verileri tekerleği işlemleriyle ilgili özellikleri yukarıdaki - içermelidir. Sorun başarısızlığını tahmin etmek için ise _traction sistem_, eğitim verileri ait farklı bileşenler için traction sistem kapsayacak şekilde sahiptir. İkinci durumda daha büyük bir alt sistemi başarısızlığını hedefler ancak ilk harfi belirli bir bileşeni hedefler. Genel ikinci daha veri dağınık beri büyük alt sistemleri yerine belirli bileşenleri hakkında öngörü sistemleri tasarlamak için önerilir. Etki alanı Uzman (bkz [sorunlar Tahmine dayalı bakım için uygun](#Qualifying-problems-for-predictive-maintenance)) verilerini analiz için en uygun kümelerine seçilmesinde yardımcı olmalıdır. İlgili veri kaynaklarına daha ayrıntılı olarak ele alınmıştır [Tahmine dayalı bakım için veri hazırlığı](#Data-preparation-for-predictive-maintenance).

### <a name="sufficient-data"></a>Yeterli veri
İki sık sorulan sorular hatası geçmiş verileri sabittir: (1) "kaç hatası olaylarının bir modeli eğitmek için gerekli midir?" (2) "kaç kayıt olarak kabul"yeterli"?" Hiçbir kesin yanıtlar, ancak yalnızca kuralları altın vardır. (1) için daha hatası olaylarının sayısı model daha iyi. (2) için ve hata olaylarının tam sayı veri ve problem Çözüldü bağlamında bağlıdır. Ancak, bir makine çok sık başarısız olursa Çevir tarafında sonra iş, hangi hatası örnekleri azaltır yerini alır. Burada yeniden Uzman etki alanından Kılavuzu önemlidir. Ancak sorun başa çıkacak şekilde yöntemleri vardır _nadir olayları_. Bölümünde açıklanan [imbalanced veri işleme](#Handling-imbalanced-data).

### <a name="quality-data"></a>Kalite veri
Veri Kalitesi kritik - her bir göstergesi olduğu öznitelik değeri olmalıdır _doğru_ hedef değişkeninin değeri ile birlikte. Veri Kalitesi istatistikler ve veriler yönetim iyi studied alanındadır ve bu nedenle out bu kılavuzun kapsamının.

> [!NOTE]
> Çeşitli kaynaklar ve kalite verileri sunmak için Kurumsal ürünleri vardır. Başvuruları örneği aşağıda verilmiştir:
> - Dasu, T, Johnson, T., Keşif veri araştırma ve verileri, 2003 Wiley, temizleme.
> - [Keşif veri analizi, Wikipedia](https://en.wikipedia.org/wiki/Exploratory_data_analysis)
> - [Hellerstein, J, nicel veri büyük veritabanları için temizleme](http://db.cs.berkeley.edu/jmh/papers/cleaning-unece.pdf)
> - [de Jonge, E, van der Ara, M, veri temizleme r giriş](https://cran.r-project.org/doc/contrib/de_Jonge+van_der_Loo-Introduction_to_data_cleaning_with_R.pdf)

## <a name="data-preparation-for-predictive-maintenance"></a>Tahmine dayalı bakım için veri hazırlama

### <a name="data-sources"></a>Veri kaynakları

Tahmine dayalı bakım ilgili veri kaynaklarına arasında ancak bunlarla sınırlı değildir:
- Hata geçmişi
- Bakım/onarma geçmişi
- Makine işletim koşulları
- Donanım meta verileri

#### <a name="failure-history"></a>Hata geçmişi
Hata olayları PdM uygulamalarda seyrek kullanılır. Ancak, tahmin modelleri oluştururken, bir bileşenin normal işlem düzeni yanı sıra hakkında kendi hatası desenleri öğrenmek algoritma gerekiyor. Bu nedenle eğitim verileri her iki kategoriden örnekleri yeterli sayıda içermelidir. Bakım kaydeder ve bölümleri değiştirme geçmiş hata olaylarını bulmak için uygun kaynaklardır. Bazı etki alanı bilgi yardımıyla, eğitim verileri içinde anormallikleri hataları olarak tanımlanabilir.

#### <a name="maintenancerepair-history"></a>Bakım/onarma geçmişi
Bir varlık bakım geçmişini bileşenleri yerine, vb. gerçekleştirilen onarım etkinlikleri hakkındaki ayrıntıları içerir. Bu olaylar düşüşü desenleri kaydedin. Eğitim verileri bu önemli bilgileri yokluğu yanıltıcı modeli sonuçlara yol açabilir. Özel hata kodları veya bölümleri için sipariş tarihleri olarak hatası geçmişi ayrıca bakım geçmişi içinde bulunabilir. Hata desenleri etkilemek ek veri kaynaklarını araştırılan ve etki alanı uzmanlar tarafından sağlanan gerekir.

#### <a name="machine-operating-conditions"></a>Makine işletim koşulları
İşlemde ekipmanının algılayıcı tabanlı (veya diğer) akış verilerini bir önemli veri kaynağıdır. Bir anahtar PdM, bir makinenin sistem durumu rutin çalışması sırasında zamanla düşüyor varsayılır. Verileri bu eskime düzeni ve düşmesine yol açar anormallikleri yakalama zaman değişen özellikleri içermesi beklenir. Zamana bağlı en boy veri hata ve hatasız örneklerinin zamanla öğrenmek algoritma için gereklidir. Bu veri noktalarında bağlı olarak, daha fazla birim sayısı zaman bir makine başarısız önce çalışmaya devam edebilirsiniz tahmin etmek algoritma öğrenir.

#### <a name="static-feature-data"></a>Statik özelliği verileri
Donanım hakkındaki meta verileri statik özellikleridir. Örnekler Donatım Markası, modeli, üretilen tarihi, başlangıç tarihi hizmetinin, sistem ve diğer teknik özellikleri konumunu.

İlgili verileri örnekleri [örnek PdM kullanım](#Sample-PdM-use-cases) aşağıdaki tabloda verilmiştir:

| Kullanım örneği | İlgili veri örnekleri |
|:---------|---------------------------|
|_Uçuş gecikmesi ve iptalleri_ | Uçuş rota bilgilerini uçuş ayaktan ve sayfa günlükleri biçiminde. Uçuş leg verileri ayrılma/varış tarih, saat, havaalanı, layovers vb. gibi yönlendirme ayrıntılarını içerir. Sayfa günlük bir dizi başından başlayarak bakım personeli tarafından kaydedilen hata ve bakım kodları içerir.|
|_Uçak motoru bölümleri hatası_ | Çeşitli bölümlerini koşula bilgilerini uçak algılayıcılar gelen veri toplanmadı. Bakım kayıtları bileşen hataları oluştuğunda ve bunlar değiştirilmiştir tanımlamaya yardımcı olur.|
|_ATM hatası_ | Sensör okumaları (nakit/onay ürünü) her bir işlem için ve nakit dağıtmak. Bilgi boşluk ölçüm notlar arasında üzerinde Not kalınlığı, Not varış uzaklığı, öznitelikleri vb. denetleyin. Hata kodları, onarım bilgileri, en son nakit dağıtıcısının zaman sağlamak bakım kayıtları refilled.|
|_Rüzgar Türbin hatası_ | Algılayıcılar sıcaklık, Rüzgar yönü, oluşturulan güç, oluşturucu hızını vb. gibi Türbin koşullar izleyin. Birden çok Rüzgar turbines çeşitli bölgelerde bulunan Rüzgar gruplarından gelen veriler toplanır. Genellikle, her Türbin ölçümler bir sabit bir zaman aralığında geçişini birden çok sensör okumaları sahip olacaktır.|
|_Devre kesici hataları_ | Düzeltme, önleyici ve sistematik eylemlerini içeren bakım günlüğe kaydedilir. Açık ve kapalı eylemler gibi hattı ayırıcıları gönderilen otomatik ve el ile komutları içerir işletimsel veri. Üretim, konum, model vb. tarihi gibi cihaz meta veriler. Devre kesici belirtimleri voltaj düzeyleri, coğrafi konuma, ortam koşulları gibi.|
|_Fırsatınızdır kapı hataları_| Tür fırsatınızdır, üretilen tarih, bakım sıklığı, yapı türü vb. gibi fırsatınızdır meta veriler. Kapı döngü, ortalama kapı Kapat saat sayısı gibi işletimsel bilgiler. Hata geçmişi ile neden olur.|
|_Tekerlek hataları_ | Ölçüler hızlandırma, braking örnekleri, yönlendirmeli uzaklığı, hız vb. Tekerlek algılayıcı verileri. Statik bilgilerini tarih üreticisi gibi Tekerlek üzerinde üretilen. Sipariş tarihleri ve sayıları izleyen parça siparişi veritabanından çıkarımı yapılan hatası verileri.|
|_Yeraltı treni tren kapı hataları_ | Kapı açma ve kapatma süreleri, tren kapıları geçerli durumu gibi diğer işlemsel veri. Statik verileri varlık tanımlayıcısı, saati ve durum değeri sütunlarında dahildir.|

### <a name="data-types"></a>Veri türleri
Yukarıdaki veri kaynakları verildiğinde, PdM etki alanında gözlenen iki ana veri türleri şunlardır:

- _Zamana bağlı veri_: işletimsel telemetri, makine koşullar, iş emri türleri, zaman damgaları kaydını aynı anda sahip öncelik kodları. Ayrıca hata, bakım/onarma ve kullanım geçmişini her olayla ilişkili zaman damgaları sahip olur.
- _Statik veri_: Makine özellikler ve işleci özellikler genel statik bu yana makineler veya işleci özniteliklerin teknik özellikleri açıklanmaktadır. Bu özellikler zaman içinde değişebilir, ayrıca zaman damgaları ilişkili olmalıdır.

Bir göstergesi olduğu ve hedef değişkenleri ön işlemesi yapılan/dönüştürülmüş olmalıdır [sayısal, kategorik ve diğer veri türleri](https://www.statsdirect.com/help/basics/measurement_scales.htm) bağlı olarak kullanılan algoritma.

### <a name="data-preprocessing"></a>Verileri ön işleme
İçin önkoşul olarak _özellik Mühendisliği_, onu olduğu özellikleri oluşturmak kolay bir şema oluşturmak için çeşitli akışları verileri hazırlama. Önce bir kayıt tablosu görselleştirmek. Tablodaki her satır bir eğitim örneği temsil eder ve sütunları temsil _bir göstergesi olduğu_ özellikleri (bağımsız öznitelikler veya değişkenleri olarak da bilinir). Son sütunlara olacak şekilde, verileri düzenlemek _hedef_ (bağımlı değişken). Her bir eğitim örneği için Ata bir _etiket_ bu sütunun değeri olarak.

Zamana bağlı verileri için zaman birimlerine algılayıcı verilerini süresini böler. Her bir kaydı bir varlık için bir zaman birimi ait olması gereken _ve farklı bilgi sunması gereken_. Zaman birimleri'nün katları saniye, dakika, saat, gün, iş gereksinimlerinize göre tanımlanan ay, vb. Zaman birimi _veri toplama sıklığı ile aynı olması gerekmez_. Sıklık yüksekse, veri önemli herhangi bir birimi farkı diğer göstermeyebilir. Örneğin, ortam sıcaklık 10 saniyede toplanan varsayın. Eğitim verileri için aynı aralık kullanarak yalnızca örnek sayısı ek bilgileri sağlamadan Şişir. Bu durumda, daha iyi bir stratejisi üzerinde 10 dakika veya bir saat verileri üzerinde iş gerekçesinin göre ortalama kullanmak olabilir.

Statik verileri için
- _Bakım kayıtları_: ham bakım verileri içeren bir varlık tanıtıcısı ve belirli bir anda zamanında gerçekleştirilmiş bakım etkinlikleri hakkında bilgi zaman damgası. Bakım etkinliklerine dönüştürme _kategorik_ sütunları, burada her kategori tanımlayıcısı benzersiz olarak eşleyen bir özel bakım eylemi. Bakım kayıtları için şemayı varlık tanıtıcısı, saat ve Bakım eylemi içerir.

- _Hata kayıtları_: belirli hata kodları veya hata olayları belirli iş koşullarına göre tanımlanan olarak hataları veya hatası nedeniyle kaydedilebilir. Donanım birden çok hata kodları sahip olduğu durumlarda, etki alanı Uzman hedef değişkeni ilgili olanları tanımlamaya yardımcı olmalıdır. Kalan hata kodları veya hata koşulları oluşturmak için kullandığı _bir göstergesi olduğu_ bu hata ile ilişkilendirmek özellikleri. Hata kayıtları için şemayı varlık tanımlayıcısı, saat, hata veya hata nedeni - varsa içerir.

- _Makine ve işleci meta verilerini_: bir varlık ilgili öznitelikleriyle birlikte kendi işleci ilişkilendirmek için bir şema makine ve işleci verileri birleştirin. Makine koşulları için şema varlık tanımlayıcısı, varlık özellikleri, işleci tanımlayıcısı ve işleci özellikleri içerir.

Adımları ön işleme diğer verileri içerecek _eksik değerleri işleme_ ve _normalleştirme_ öznitelik değerleri. Ayrıntılı bir tartışma bu kılavuzun kapsamı dışındadır - bazı yararlı başvuruları için sonraki bölüme bakın.

Veri kaynakları yerinde özellik Mühendisliği varlık tanımlayıcısı ve zaman damgası göre yukarıdaki tabloların katılmak için önce son dönüştürme yukarıdaki ön işlemesi yapılan. Makine normal işlemde olduğunda ortaya çıkan tabloda hatası sütunu için null değerler gerekir. Bu null değerler, normal işlem için bir gösterge tarafından imputed. Bu hata sütun oluşturulacağı _Tahmine dayalı bir model için etiketleri_. Daha fazla bilgi için bölümüne bakarak [teknikler Tahmine dayalı bakım için modelleme](#Modeling-techniques-for-predictive-maintenance).

## <a name="feature-engineering"></a>Özellik mühendisliği
Özellik Mühendisliği modelleme verileri önce ilk adımdır. Veri bilimi işleminde rolü [burada açıklanan](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/create-features). A _özelliği_ - sıcaklık, baskısı, Titreşim vb. gibi modeli için Tahmine dayalı bir özniteliktir. PdM için özellik Mühendisliği boyutlandırılabilir süresi boyunca toplanan geçmiş veriler üzerinde bir makinenin sistem durumu özetleyen içerir. Bu anlamda eşlerine Uzaktan izleme, anomali algılama ve hata algılama gibi farklı. 

### <a name="time-windows"></a>Zaman pencereleri
Sürümünden itibaren gerçekleşen olayları raporlama kapsar izleme uzaktan _zaman içinde nokta_. Anomali algılama modelleri zamanında veri noktaları itibariyle bayrağı anormallikleri için gelen akışları (puan) değerlendirin. Hata algılama noktaları zamanında göründüklerinde belirli türlerdeki olmasını hataları sınıflandırır. Buna karşılık, hataları öngörmektir PdM içerir bir _gelecekte süre_üzerinden makine davranışını temsil eden özelliklere göre _geçmiş süre_. PdM için özellik zaman tek tek noktalarından Tahmine dayalı olarak gürültülü verilerdir. Her bir özellik için veri olması gerekir böylece _smoothened_ veri noktaları zaman pencereleri toplama tarafından.

### <a name="lag-features"></a>Gecikme özellikleri
İş gereksinimlerini ne kadar geleceğe tahmin etmek model sahip tanımlayın. Buna karşılık, bu süre, 'ne kadar geri model aramak sahip' tanımlamasına yardımcı olur bu Öngörüler yapma. Bu 'geri dönemi arayan' adlı _gecikme_, ve bu gecikme süresi içinde mühendislik özellikler adlandırılır _öteleme özellikleri_. Bu bölümde, zaman damgaları ve özellik oluşturma statik veri kaynaklarından veri kaynaklarından oluşturulan öteleme özellikleri açıklanmaktadır. Gecikme özellikleridir genellikle _sayısal_ yapısı.

> [!IMPORTANT]
> Pencere boyutunu deneme belirlenir ve bir etki alanı Uzman yardımı ile sonlandırılır. Aynı uyarısıyla seçimi ve gecikme özelliklerini, kendi toplamalar ve Windows'un tür tanımı için tutar.

#### <a name="rolling-aggregates"></a>Toplamalar alınıyor
Her bir varlık kayıt için çalışırken bir pencere boyutu "W" toplamları hesaplamak için zaman birimlerini sayı olarak seçilir. Gecikme özellikleri sonra hesaplanan W nokta kullanarak _tarihinden önce_ o kaydın. Şekil 1'de mavi satırlar her zaman birimi için bir varlık için kaydedilen algılayıcı değerlerini gösterir. Bunlar, bir boyut W = 3 pencere üzerinde çalışırken bir özellik değerlerinin ortalamasını gösterir. Ortalama zaman damgalı t aralıktaki tüm kayıtları üzerinden hesaplanır<sub>1</sub> (turuncu içinde) t<sub>2</sub> (yeşil içinde). W genellikle dakika ya da veri yapısına bağlı olarak saat değeridir. Ancak büyük W (12 ay deyin) çekme sorunları, kaydı zamana kadar bir varlık tam geçmişini belirli sağlayabilir.

![Şekil 1. Toplama özellikleri çalışırken](./media/cortana-analytics-playbook-predictive-maintenance/rolling-aggregate-features.png) Şekil 1 '. Toplama özellikleri alınıyor

Bir zaman penceresi toplamalar çalışırken, örnek sayısı, ortalama, CUMESUM (toplu sum) ölçüler, min/max değerleri verilmiştir. Ayrıca, fark, standart sapma ve N Standart sapmanın ötesinde aykırı değerlerini sayısı genellikle kullanılır. Örnekler için uygulanabilir Toplamaların [kullanım](#Sample-PdM-use-cases) bu kılavuzda aşağıda listelenmiştir. 
- _Uçuş gecikme_: hata kodları üzerinden geçen gün/hafta sayısı.
- _Uçak motoru bölümü hata_: anlamına gelir, standart sapma ve toplam çalışırken içinde son gün, hafta vb. Bu ölçüm iş etki alanı ile birlikte Uzman belirlenmesi.
- _ATM hataları_: çalışırken anlamına gelir, Orta, aralığı, Standart sapmanın, üç Standart sapmanın, üst ve alt CUMESUM ötesinde aykırı değerlerini sayısı.
- _Yeraltı treni tren kapı hataları_: gün, hafta, iki hafta vb. üzerinden sayısı.
- _Devre kesici hataları_: hata geçen hafta içinde yıl, üç yıla vb. sayar.

Başka bir yararlı PdM içinde eğilim değişiklikler, ani ve anormallikleri verileri algılama algoritmalarını kullanarak düzeyi değişiklikleri yakalamak için tekniğidir.

#### <a name="tumbling-aggregates"></a>Dönen toplar
Her biri için bir varlık, bir pencere boyutunu kaydının etiketli _W -<sub>k</sub>_  , tanımlandığı _k_ windows boyutunun sayısı _W_. Toplamalar üzerinden oluşturulduktan sonra _k_ _dönen windows_ _W-k, W -<sub>(k-1)</sub>,..., W -<sub>2</sub>, W -<sub>1</sub>_  bir kaydın zaman damgası önce süreyle. _k_ kısa vadeli efektleri yakalamak için küçük bir sayı veya uzun vadeli düşüşü desenleri yakalamak için çok sayıda olabilir. (bkz. Şekil 2).

![Şekil 2. Dönen toplama özellikleri](./media/cortana-analytics-playbook-predictive-maintenance/tumbling-aggregate-features.png) Şekil 2 '. Toplama özellikleri dönen

W = 1 k ile Rüzgar turbines kullanım örneği oluşturulabilir özellikleri örneğin, öteleme = 3. Bunlar her üst ve alt aykırı değerlerini kullanarak son üç ay için gecikme kapsıyor.

### <a name="static-features"></a>Statik özellikler

Üretim, model numarası, konum, tarihi gibi ekipmanının teknik özellikleri statik özelliklerin bazı örnekler verilmiştir. Bunlar kabul _kategorik_ modelleme değişkenleri. Devre kesici kullanım örneği için örnek voltaj, geçerli, güç kapasitesi, transformer türü ve güç kaynağı verilebilir. Tekerlek hataları için örnek lastiği Tekerlek (karışımı vs çelik) türüdür.

Şu ana kadar ele alınan verileri hazırlama çaba aşağıda gösterildiği gibi düzenlenmiş veri neden. Eğitim, test ve doğrulama verileri (Bu örnekte zaman gün birimlerinde gösterir) Bu mantıksal şeması olması gerekir.

| Varlık Kimliği | Zaman | <Feature Columns> | Etiket |
| ---- | ---- | --- | --- |
| A123 |Günde 1 | . . . | . |
| A123 |Gün 2 | . . . | . |
| ...  |...   | . . . | . |
| B234 |Günde 1 | . . . | . |
| B234 |Gün 2 | . . . | . |
| ...  |...   | . . . | . |

Özellik Mühendisliği son adımdır **etiketleme** hedef değişkeni. Bu işlem modelleme tekniği bağlıdır. Buna karşılık, iş sorununu ve kullanılabilir veri yapısını modelleme teknikleri bağlıdır. Etiketleme, sonraki bölümde ele alınmıştır.

> [!IMPORTANT]
> Veri hazırlama ve özellik Mühendisliği başarılı PdM çözümleri gelmesi teknikleri modelleme olarak önemli. Etki alanı uzman ve bakış doğru özellikler ve modeli için veriler geliş önemli süre yatırım. Özellik Mühendisliği üzerinde birçok kitaplarında küçük bir örnek aşağıda listelenmiştir:
> - Pyle, veri 1999 (Morgan Kaufmann dizide veri yönetim sistemleri), araştırma için D. veri hazırlama
> - Zheng, A., Casari, Machine Learning için A. özellik Mühendisliği: ilkeler ve veri Bilimcilerine, O'Reilly, 2018 teknikler.
> - Dong, G. Liu H. (Düzenleyiciler) özelliği Machine Learning ve veri analizi (Chapman & Hall/CRC veri araştırma ve bilgi bulma serisi) için CRC tuşuna 2018 mühendislik.

## <a name="modeling-techniques-for-predictive-maintenance"></a>Tahmine dayalı bakım modelleme teknikleri

Bu bölümde, kendi özel etiket oluşturma yöntemlerini birlikte PdM sorunları için ana modelleme teknikleri açıklanmıştır. Tek modelleme teknik farklı endüstriler arasında kullanılabilir dikkat edin. Modelleme teknikleri veri elinizdeki bağlamında yerine veri bilimi sorunu eşleştirilmiş.

> [!IMPORTANT]
> Etiketleme stratejisi ve hata durumları için etiketleri seçimi  
> Uzman etki alanı ile çalışmaya olarak belirlenmesi.

### <a name="binary-classification"></a>İkili sınıflandırma
İkili sınıflandırma için kullanılan _ekipman parçasını gelecekteki bir süre içinde başarısız olma olasılığını tahmin_ - çağrılan _gelecekteki yatay dönemi X_. X, Uzman etki alanı ile çalışmaya, iş sorununu ve veri elinizdeki tarafından belirlenir. Örnekler şunlardır:
- _Minimum sağlama süresi_ bileşenleri değiştirmek için gerekli, bakım kaynakları dağıtmak, o dönemde gerçekleşmesi olası bir sorunu önlemek için bakım gerçekleştirin.
- _Minimum sayısı_ bir sorun oluşmadan önce oluşabilir.

Bu teknik eğitim örnekleri iki tür tanımlanır. Pozitif bir örnek _bir hata gösterir_, etiketle = 1. Normal işlemleri gösterir, negatif bir örnek, etiketle = 0. Hedef değişkeni ve bu nedenle etiket değerlerini olduğunuz _kategorik_. Model, her yeni örnek başarısız veya sonraki zaman birimleri X üzerinden normal olarak çalışmak büyük olasılıkla olarak tanımlamanız gerekir.

#### <a name="label-construction-for-binary-classification"></a>İkili sınıflandırma için etiket oluşturma
Soru burada: "varlık sonraki başarısız olur olasılık nedir zaman birimleri X?" Bu soru, etiket X kayıtları "için yaklaşık başarısız" olarak bir varlık arıza öncesinde yanıtlamak için (Etiket = 1) ve "normal" olarak tüm kayıtları etiket (etiket = 0). (bkz. Şekil 3).

![Şekil 3. İkili sınıflandırma etiketleme](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-binary-classification.png) Şekil 3 '. İkili sınıflandırma etiketleme

Bazı kullanım durumları için stratejisi etiketleme örnekleri aşağıda listelenmiştir.
- _Uçuş gecikmeler_: X seçilebilir 1 gün gecikmeler sonraki 24 saat içindeki tahmin etmek için. Ardından hataları önce 24 saat içinde olan tüm uçuşları 1 olarak etiketlenir.
- _ATM nakit barkod hataları_: bir işlem sonraki bir saat içinde hata olasılığını belirlemek için bir hedef olabilir. Bu durumda, hata son bir saat içinde gerçekleşen tüm işlemleri 1 olarak etiketlenir. Hata olasılığını sonraki N para birimi tahmin etmek için dispensed, notlar hata son N notlarına dispensed tüm notları 1 olarak etiketlenir.
- _Devre kesici hataları_: sonraki devre kesici komut hatası tahmin etmek için hedef olabilir. Bu durumda, bir sonraki komut olmasını X seçilir.
- _Eğitmek kapı hataları_: X iki gün olarak seçilebilir.
- _Rüzgar Türbin hataları_: X iki ay seçilebilir.

### <a name="regression-for-predictive-maintenance"></a>Tahmine dayalı bakım regresyon
Regresyon modeli için kullanılan _bir malın kalan kullanım ömrü (RUL) işlem_. RUL sonraki hata gerçekleşmeden önce bir varlık çalışır durumda süre miktarını tanımlanır. Bir zaman birimine ait bir kayıt her eğitim örnektir _nY_ bir varlık için burada _n_ katı. Modelin her yeni örnek olarak RUL hesaplamak bir _sürekli numarası_. Bu hatadan önce kalan süreyi gösterir.

#### <a name="label-construction-for-regression"></a>Regresyon etiket oluşturma
Soru burada: "Ekipmanın kalan kullanım ömrü (RUL) nedir?" Arıza öncesinde her kayıt için sonraki hatasından önce kalan süre birimlerinin sayısı etiketi hesaplayın. Bu yöntemde, etiketleri sürekli değişkenlerdir. (Bkz: Şekil 4)

![Şekil 4. Regresyon için etiketleme](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-regression.png) Şekil 4 '. Regresyon için etiketleme

Regresyon için etiketleme başvuru içeren bir hata noktası yapılır. Ne kadar varlık hatasından önce derdi bitti bilmeden özelliği, hesaplama mümkün değildir. Bu nedenle zıt ikili sınıflandırma için veri herhangi bir hata olmadan varlıklar modelleme için kullanılamaz. Bu sorunu en iyi adında başka bir istatistik teknik tarafından ele [hayatta analiz](https://en.wikipedia.org/wiki/Survival_analysis). Ancak olası karışıklıklardan sık aralıklarla zaman değişen verilerle ilgili PdM kullanım durumları için bu tekniği uygularken ortaya çıkabilir. Acil ihtiyaç çözümleme hakkında daha fazla bilgi için bkz: [bu bir çağrı](https://www.cscu.cornell.edu/news/statnews/stnews78.pdf).

### <a name="multi-class-classification-for-predictive-maintenance"></a>Tahmine dayalı bakım için birden çok sınıf sınıflandırma
Birden çok sınıf sınıflandırma teknikleri PdM çözümleri için iki senaryo kullanılabilir:
- Tahmin _iki gelecekteki sonuçları_: ilk sonucu olan _başarısızlık zaman aralığı_ bir varlık için. Varlık birden fazla olası süreler biri olarak atanır. İkinci sonucu gelecekteki bir süre nedeniyle hata olasılığını bulunduğu _birden çok kök biri neden_. Bu tahmin belirtileri ve planı bakım takvimleri izlemek bakım ekibi sağlar.
- Tahmin _en olası kök nedeni_ verilen hata. Bu sonucu bir hata düzeltme için bakım eylemleri doğru yetenek kümesini önerir. Temel nedenler ve önerilen onarım sıralı bir listesini onarım eylemlerini bir hatadan sonra öncelik teknisyenleri yardımcı olabilir.

#### <a name="label-construction-for-multi-class-classification"></a>Birden çok sınıf sınıflandırması için etiket oluşturma
Soru burada: "bir varlık sonraki başarısız olur olasılık nedir _nZ_ zaman birimleri nerede _n_ nokta sayısı?" Bu soruyu yanıtlamak için süre (3Z, 2Z, Z) demet kullanarak bir varlık arıza öncesinde nZ kayıtları etiketleyin. Etiket diğer tüm kayıtları "normal" olarak (etiket = 0). Bu yöntemde, hedef değişkeni tutan _kategorik_ değerleri. (Bkz. Şekil 5).

![Şekil 5. İçin hata zaman tahmini çok sınıflı sınıflandırma etiketleme](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-failure-time-prediction.png) Şekil 5. İçin hata zaman tahmini çok sınıfı sınıflandırma etiketleme

Soru burada: "varlık sonraki başarısız olur olasılık nedir kök nedeni/sorunu nedeniyle zaman birimleri X _P<sub>ı</sub>_?" Burada _ı_ olası nedenlerini sayısıdır. Bu soru, bir varlık arıza öncesinde X etiketi kayıtları yanıtlamak için "kök nedenden dolayı başarısız üzere _P<sub>ı</sub>_" (etiket = _P<sub>ı</sub>_). "Normal" olarak tüm kayıtları etiket (etiket = 0). Bu yöntem aynı zamanda, kategorik (bkz: Şekil 6) etiketleridir.

![Şekil 6. İçin kök nedeni tahmin çok sınıflı sınıflandırma etiketleme](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-root-cause-prediction.png) Şekil 6. İçin kök nedeni tahmin çok sınıfı sınıflandırma etiketleme

Model hatası olasılığını her nedeniyle atar _P<sub>ı</sub>_  hiçbir hata olasılığını yanı sıra. Bu olasılıklar gelecekte gerçekleşmesi en olası sorunları tahmin izin vermek için büyüklük sıralanabilir.

Soru burada: "Bakım eylemleri bir hatadan sonra önerilir?" Bu soruyu yanıtlamak için etiketleme _çekilmesi için gelecekteki bir yatay gerektirmez_, model hatası gelecekte tahmin etmektir değil. Bu yalnızca en olası kök nedeni tahmin etmektir _hatası zaten gerçekleştirilmedi sonra_.

## <a name="training-validation-and-testing-methods-for-predictive-maintenance"></a>Eğitim, doğrulama ve Tahmine dayalı bakım için test yöntemleri
[Takım veri bilimi işlemi](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) modeli eğitme test doğrulama döngüsü tam kapsamını sağlar. Bu bölümde PdM için benzersiz yönleri açıklanmaktadır.

### <a name="cross-validation"></a>Çapraz doğrulama
Amacı [çapraz doğrulama](https://en.wikipedia.org/wiki/Cross-validation_(statistics)) "model eğitim aşamasında test etmek için" bir veri kümesi tanımlamaktır. Bu veri kümesi olarak adlandırılan _doğrulama kümesi_. Bu teknik yardımcı sınırı sorunları gibi _overfitting_ ve model bağımsız bir veri kümesine nasıl generalize hakkında bilgi verir. Diğer bir deyişle, hangi gerçek bir sorun olabilir bir bilinmeyen bir veri kümesi. PdM için eğitim ve sınama yordamını daha iyi üzerinde görünmeyen gelecekteki veri genelleştirmek için zaman çeşitli yönlerini dikkate almanız gerekir.

Çok sayıda makine öğrenimi algoritmalarını modeli performansını önemli ölçüde değiştirebilirsiniz hyperparameters sayısına bağlıdır. Bu hyperparameters en iyi değerlerini otomatik olarak modeli eğitimindeki hesaplanır değil. Veri Bilimcisi tarafından belirtilmelidir. Hyperparameters iyi değerlerini bulma birkaç yolu vardır.

En sık karşılaşılan biridir _k Katlama çapraz doğrulama_ rastgele içine örnekler böler _k_ Katlama. Öğrenme algoritmasını her hyperparameters değer kümesi çalıştırmak _k_ kez. Her bir yinelemede, örnekler geçerli kat doğrulama kümesi ve örneklerin rest bir eğitim kümesi olarak kullanın. Algoritma eğitim örnekler eğitmek ve performans ölçümleri doğrulama örnekler üzerinde işlem. Bu döngü sonunda, ortalama işlem _k_ performans ölçümleri. Her hyperparameter değer kümesi için en iyi ortalama performans sahip olanları seçin. Hyperparameters seçme genellikle doğası gereği Deneysel bir görevdir.

PdM sorunları, verileri çeşitli veri kaynaklarından gelen olayların zaman serisi olarak kaydedilir. Bu kayıtları etiketleme zaman göre sıralı olabilir. Bu nedenle, veri kümesi bölerseniz _rastgele_ eğitim ve doğrulama kümesi içine _eğitim örnekleri bazıları zamanında doğrulama örnekler bazıları sonraki olabilir_. Gelecekte performans hyperparameter değerlerin tahmini gelen bazı verileri alarak _önce_ model eğitildi. Bu tahminler özellikle zaman serisi sabit değildir ve zaman içinde dönüşmesi aşırı iyimser olabilir. Sonuç olarak, seçilen hyperparameter değerler yetersiz olabilir.

Eğitim ve kümesinde doğrulama örnekler Böl için önerilen yoldur bir _zamana bağımlı_ tüm doğrulama örnekler olduğu tüm eğitim örnekleri zamanında sonraki şekilde,. Her hyperparameter değer kümesi algoritma eğitim veri kümesi üzerinde eğitmek. Modelin performans aynı doğrulama küme üzerinde ölçün. En iyi performansı Göster hyperparameter değerleri seçin. Rastgele çapraz doğrulama tarafından seçilen değerlerle daha iyi gelecekteki modeli performans tren/doğrulama bölünmüş sonucu tarafından seçilen Hyperparameter değerleri.

Son modeli en iyi hyperparameter değerleri kullanarak tüm eğitim verilerini bir öğrenme algoritması eğitim tarafından oluşturulabilir.

### <a name="testing-for-model-performance"></a>Model performansını test etme
Bir model oluşturulduktan sonra yeni verilerin gelecekteki kendi performansını tahmini gereklidir. İyi bir performans ölçüm hyperparameter değerlerin doğrulama küme üzerinde hesaplanan veya bir ortalama performans ölçümü çapraz doğrulama hesaplanan tahmin edin. Bu tahminler genellikle aşırı iyimser. İş çoğunlukla bunlar modeli sınama yönteminizi bazı ek yönergeler olabilir.

Önerilen PdM doğrulama, eğitim içine örnekler bölüneceği yoludur ve test verilerini ayarlar bir _zamana bağımlı_ şekilde. Tüm test örnekleri tüm eğitim ve doğrulama örnekler zaman içinde daha sonra olmalıdır. Bölünmüş sonra model oluşturmak ve daha önce açıklandığı gibi kendi performansını ölçmek.

Zaman serisi tahmin etmek kolay ve sabit olduğunda rastgele hem zamana bağımlı yaklaşımlar gelecekteki performansını benzer tahminler oluşturur. Ancak zaman serisi ileti örneği olmayan ve/veya tahmin etmek sabit olduğunda, zamana bağlı yaklaşım gelecekteki performansını daha gerçekçi tahminleri oluşturur.

### <a name="time-dependent-split"></a>Zamana bağlı Böl
Bu bölümde zamana bağımlı bölünmüş uygulamak için en iyi uygulamaları açıklar. Eğitim ve sınama gruplarını arasında zamana bağımlı iki yönlü bölme aşağıda açıklanmıştır.

Bir akış gibi çeşitli algılayıcılar ölçümler zaman damgalı olayların varsayalım. Özellikler ve etiketleri eğitim ve test örnekleri birden çok olay içeren aralıklarına tanımlayın. Örneğin, ikili sınıflandırma özellikleri son olaylara dayanarak oluşturun ve "X" gelecekte zaman birimleri içinde gelecekteki olayları temel alan etiketleri oluşturun (bölümlere bakın [özellik Mühendisliği](#Feature-engineering) ve [modelleme teknikleri](#Modeling-techniques-applied-to-PdM-use-cases)). Bu nedenle, bir örneğin etiketleme zaman çerçevesi zaman çerçevesi özellikleri daha sonra gelir.

Zamana bağlı bölme için çekme bir _kesme zamanı T eğitim<sub>c</sub>_  veren hyperparameters T kadar geçmiş verileri kullanarak olarak ayarlanmış olan bir modeli eğitmek<sub>c</sub>. T gelecekteki etiketler sızıntısını önlemek için<sub>c</sub> eğitim verilerini, etiket eğitim örnekleri X olması için en son saati seçin T önce birimleri<sub>c</sub>. Şekil 7'de gösterilen örnekte, her kare burada özellikleri ve etiketleri yukarıda açıklandığı gibi hesaplanır veri kümesindeki bir kaydı temsil eder. Şekilde eğitim ve ayarlar X = 2 ve W = 3 için test gitmesi kayıtları gösterilmektedir:

![Şekil 7. Zamana bağlı için ikili sınıflandırma bölme](./media/cortana-analytics-playbook-predictive-maintenance/time-dependent-split-for-binary-classification.png) Şekil 7. Zamana bağlı için ikili sınıflandırma bölme

Yeşil kareler için eğitim kullanılabilmesi için zaman birimlerini ait kayıtları temsil eder. Geçmiş dikkate alarak her eğitim örnek oluşturulan özellik oluşturma için üç nokta ve T önce etiketleme iki gelecekteki nokta<sub>c</sub>. İki gelecekteki nokta herhangi bir kısmını T dışında olduğunda<sub>c</sub>, hiçbir görünürlük T varsayıldığından, örnek eğitim veri kümesinden hariç<sub>c</sub>.

Siyah kareler yukarıdaki kısıtlaması verilen eğitim veri kümesinde kullanılmamalıdır son etiketli veri kümesinin kayıtları temsil eder. Bu kayıtları aynı zamanda T önce olduğundan veri testinde kullanılmayacak<sub>c</sub>. Ayrıca, kendi etiketleme aralıklarına kısmen eğitim zaman çerçevesi ideal olmayan bağlıdır. Eğitim ve test verileri ayrı etiketleme etiket bilgi sızıntısını önlemek için aralıklarına sahip olması gerekir.

Eğitim ve test T yakın zaman damgaları sahip örnekler arasında örtüşme kadarki açıklanan teknikleri sağlar<sub>c</sub>. W zaman T içinde birimleridir örnekler dışlamak için büyük ayırma elde etmek için bir çözüm olan<sub>c</sub> testten ayarlayın. Ancak agresif bir bölme geniş veri kullanılabilirliğine bağlıdır.

RUL etmede kullanılan regresyon modeli daha ciddi bir şekilde sızıntısını sorundan etkilenen. Rastgele split yöntemini kullanarak aşırı atlayarak sığdırma yol açar. Regresyon sorunları için bölme olmalıdır şekilde T önce hataları olan varlıklar ait kayıtları<sub>c</sub> Eğitim kümesi uygulamasına gidin. Sonra kesme hataları olan varlıklar kayıtlarının test kümesi olarak gidin.

Eğitim ve test için veri bölmek için başka bir en iyi uygulama varlığı kimliğe göre bölme kullanmaktır. Eğitim kümesinde kullanılan varlıklar hiçbiri modeli performans testinde kullanılan şekilde bölme olmalıdır. Bu yaklaşımı kullanarak, bir model ile yeni varlıklar daha gerçekçi sonuçları sağlama şansı vardır.

### <a name="handling-imbalanced-data"></a>İmbalanced verileri işleme
' Den bir sınıfın diğer, daha fazla örnek varsa sınıflandırma sorunları, veri kümesi olarak kabul edilir _imbalanced_. İdeal olarak, her sınıfın eğitim verileri içinde yeterli temsilcileri farklı sınıflar arasında ayrım etkinleştirmek için tercih ettiği. Bir sınıf verilerin % 10'dan az ise, veri imbalanced olarak kabul edilir. Underrepresented sınıfı olarak adlandırılan bir _azınlık sınıfı_.

Diğer sınıf veya sınıfları için karşılaştırıldığında burada bir sınıf ciddi bir şekilde underrepresented böyle imbalanced veri kümeleri, birçok PdM sorunuyla karşılaşacak. Bazı durumlarda, yalnızca %0,001 toplam veri noktalarının azınlık sınıfı oluşturan. Sınıf dengesizliği PdM için benzersiz değil. Hataları ve anormallikleri nadir oluşum olduğu diğer etki alanlarındaki örnekler, sahtekarlığı önlemek ve ağ yetkisiz erişim için benzer bir sorun karşılaşıyor. Bu hatalar azınlık sınıfı örnekleri olun.

Genel hata oranı en aza hedefleyin beri verilerde sınıfı dengesizliği ile çoğu standart learning algoritmaları performansını, tehlikeye. % 99 negatif ve %1 pozitif örnekleri ile bir veri kümesi için tüm örnekleri negatif olarak etiketleme tarafından % 99 doğruluğunu sağlamak için bir model gösterilebilir. Ancak model yanlış pozitif örnekleri sınıflandırır; doğruluğunu yüksek olsa bile, bu nedenle algoritması yararlı bir değil. Sonuç olarak, geleneksel değerlendirme ölçümleri gibi _hata oranı üzerinde genel doğruluğu_ imbalanced learning için yeterli değil. İmbalanced veri kümeleriyle karşılaştığı olduğunda, diğer ölçümleri modeli değerlendirmesi için kullanılır:
- Duyarlılık
- Geri çekme
- F1 puanları
- Ayarlanmış maliyeti ROC (alıcı işletim özellikleri)

Bu ölçümler hakkında daha fazla bilgi için bkz: [model değerlendirme](#Model-evaluation).

Ancak, sınıf düzeltmek Yardım bazı yöntemler vardır dengesizliği sorun. İki önemli olanlardır _teknikleri örnekleme_ ve _hassas öğrenme maliyet_.

#### <a name="sampling-methods"></a>Örnekleme yöntemleri
İmbalanced öğrenme dengeli bir veri kümesi için eğitim veri kümesi değiştirmek için yöntemleri örnekleme kullanımını içerir. Örnekleme test kümesine uygulanan değil yöntemleridir. Çeşitli örnekleme teknikler olsa da, çoğu düz iletme olanlardır _rastgele oversampling_ ve _örnekleme altında_.

_Rastgele oversampling_ azınlık sınıfından rasgele bir örnek seçerek, bu örnekler çoğaltılması ve eğitim veri kümesine ekleme içerir. Sonuç olarak, azınlık sınıfı örneklerde sayısı artar ve sonunda farklı sınıfların örnekleri sayısını dengelemek. Oversampling dezavantajı, belirli örnekleri birden çok örneğini sınıflandırıcı atlayarak sığdırma önde gelen çok belirli hale gelmesine neden olabilir ' dir. Model yüksek eğitim doğruluğu gösterebilir, ancak kendi performansını görünmeyen test verileri yetersiz olabilir.

Buna karşılık, _örnekleme altında rastgele_ rasgele bir örnek çoğunluğu sınıfından seçme ve bu örnekleri eğitim veri kümesinden kaldırma. Ancak, çoğu sınıfından örnekler kaldırma çoğunluğu sınıfına ilgili önemli kavramları kurtulması sınıflandırıcı neden olabilir. _Karma örnekleme_ burada azınlık sınıfı aşırı örneklenen ve çoğu sınıf altında-aynı anda örneklenen süre başka bir uygun bir yaklaşımdır.

Birçok gelişmiş örnekleme tekniği vardır. Veri özellikleri ve veri Bilimcisi tarafından yinelemeli denemeler sonuçlarını seçilen teknik bağlıdır.

#### <a name="cost-sensitive-learning"></a>Hassas öğrenme maliyet
PdM içinde azınlık sınıfı oluşturduğunu normal örnekler'den daha fazla ilgi hatalarıdır. Bu nedenle hatalarda algoritması'nın performansına çoğunlukla odak noktasıdır. Hatalı pozitif bir sınıf negatif bir sınıf olarak tahmin etmeye birden çok tam tersini maliyeti olabilir. Bu durum genellikle farklı sınıflara eşit olmayan kaybı veya yanlış sınıflandırma öğelerini asimetrik maliyetini olarak adlandırılır. İdeal sınıflandırıcı yüksek tahmin doğruluğunu çoğunluğu sınıfı doğruluğu ödün vermeden azınlık sınıfı teslim.

Bu Bakiye elde etmek için birden çok yolu vardır. Eşit olmayan kaybı sorunu azaltmak için yüksek maliyet azınlık sınıfı hatalı sınıflandırılması atayın ve genel maliyeti en aza indirmek deneyin. Algoritmalar ister _SVMs (Destek vektör makineler)_ eğitim sırasında belirtilmesini pozitif ve negatif örnekler maliyetini izin vererek bu yöntem doğası gereği, benimsemeyi. Benzer şekilde, yöntemleri gibi artırmanın _karar ağaçları boosted_ genellikle iyi bir performans imbalanced verilerle göster.

## <a name="model-evaluation"></a>Model değerlendirme
Hatalı sınıflandırma iş yanlış alarmlar maliyeti yüksek olduğu PdM senaryolar için önemli bir sorundur. Örneğin, hatalı bir altyapı hatası tahmin üzerinde temel Uçağın yerde kararını zamanlamaları kesintiye ve seyahat planları. Bir makine çevrimdışı bir derleme satırından alma gelir kaybına yol açabilir. Bu nedenle yeni test verileri karşı doğru performans ölçümlerle modeli değerlendirme kritik öneme sahiptir.

PdM modelleri değerlendirmek için kullanılan normal performans ölçümleri, aşağıda ele alınmıştır:

- [Doğruluk](https://en.wikipedia.org/wiki/Accuracy_and_precision) olan sınıflandırıcı 's performans tanımlamak için kullanılan en popüler ölçüm. Ancak doğruluğu veri dağıtımları için duyarlıdır ve imbalanced veri kümeleriyle senaryoları için etkisiz ölçüsüdür. Diğer ölçümleri onun yerine kullanılır. Araçlar [karışıklığı matris](https://en.wikipedia.org/wiki/Confusion_matrix) işlem ve modelin doğruluğunu hakkında neden için kullanılır.
- [Duyarlık](https://en.wikipedia.org/wiki/Precision_and_recall) PdM modelleri yanlış alarmlar oranını ilişkilidir. Modelin düşük duyarlık genellikle false alarmlar daha yüksek bir hızı karşılık gelir.
- [Geri çağırma](https://en.wikipedia.org/wiki/Precision_and_recall) oranı gösterir hataları için test kümesi içinde kaç doğru model tarafından belirlendi. Yüksek geri çağırma hızları model doğru hataları tanımlamak için başarılı olduğu anlamına gelir.
- [F1 puan](https://en.wikipedia.org/wiki/F1_score) harmonic duyarlık ve 0 (kötü) 1 (iyi) arasında değişen değeriyle geri çağırma ortalamasıdır.

İkili sınıflandırma için
- [Eğriler (ROC) işletim alıcı](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) de popüler bir ölçümüdür. ROC Eğriler modeli performans dayalı bir sabit ROC üzerinde işletim noktası olarak yorumlanır.
- Ancak PdM sorunları _decile tabloları_ ve _grafikleri Yükselt_ daha bilgilendirici şunlardır. Bunlar yalnızca pozitif sınıfında (hata) odaklanmanıza ve daha karmaşık bir algoritma performans resim ROC eğri den sağlayın.
  - _Decile tabloları_ test örnekleri bir hata olasılıklar azalan sırada kullanılarak oluşturulur. Sıralı örnekleri sonra deciles gruplandırılır (yüksek olasılık ile örnekleri % 10 sonra % %20 30 ve benzeri). Her decile oranı (doğru pozitif hızı) /(random baseline) her decile algoritması performansı tahmin etmenize yardımcı olur. Rastgele temel 0.1, 0.2 ve benzeri değerlerini alır.
  - [Grafikler Yükselt](http://www2.cs.uregina.ca/~dbd/cs831/notes/lift_chart/lift_chart.html) decile true pozitif oranı tüm deciles rastgele true pozitif oranı karşı çizmek. En büyük kazancı Göster bu yana ilk deciles genellikle sonuçları, odak ' dir. İlk deciles ayrıca "riskli", temsili olarak PdM için kullanıldığında görülebilir.

## <a name="model-operationalization-for-predictive-maintenance"></a>Tahmine dayalı bakım için model operationalization

Yalnızca zaman eğitilen model işletimsel yapılan veri bilimi uygulamadır avantajı gerçekleşmiş. Diğer bir deyişle, yeni, daha önce görünmeyen verilerine dayalı Öngörüler yapmak için iş sistemleri içine model dağıtılması gerekir.  Yeni veriler için tam olarak uymalıdır _modeli imza_ iki yolla eğitilen modeli:
- tüm özellikler yeni veriler her mantıksal örneğinde (deyin tablodaki satır) mevcut olması gerekir.
- Yeni verilerin önceden işlenmesi gereken ve özelliklerin her biri, eğitim verileri tam olarak aynı şekilde tasarlanmıştır.

Yukarıdaki işlem akademik ve endüstri belgeleri Birçok bakımdan belirtilir. Ancak tüm aşağıdaki deyimleri aynı anlama gelir:
- _Yeni veri puan_ modelini kullanma
- _Model geçerli_ yeni verileri
- _Faaliyete_ modeli
- _Dağıtma_ modeli
- _Modeli Çalıştır_ yeni veri karşı

Daha önce belirtildiği gibi model operationalization PdM için eşlerden farklıdır. Anomali algılama ve hata algılama genellikle içeren senaryoları uygulayan _çevrimiçi Puanlama_ (olarak da bilinir _gerçek zamanlı Puanlama_). Burada, model _puanları_ her gelen kayıt ve tahmin döndürür. Anomali algılama için tahmini bir anomali oluştuğunu göstergesidir (örnek: bir sınıf SVM). Hata algılaması için tür veya hatanın sınıfı olacaktır.

Buna karşılık, PdM içerir _toplu Puanlama_. Model imza uyacak şekilde yeni veriler özelliklerinde eğitim verileri aynı şekilde mühendislik gerekir. Yeni veriler için tipik olan büyük veri kümeleri için özellikleri zaman pencereleri toplanır ve toplu işlemde skoru. Toplu Puanlama genellikle gerçekleştirilir gibi dağıtılmış sistemlerdeki [Spark](http://spark.apache.org/) veya [Azure Batch](https://docs.microsoft.com/azure/batch/batch-api-basics). Birkaç alternatifleri - yetersiz hem de vardır:
- Akış veri altyapılar toplama windows bellekte üzerinden desteklemez. Bu nedenle çevrimiçi Puanlama destekledikleri tartışılabilir. Ancak bu sistemler üzerinde daha geniş windows saat veya seyrek öğelerin dar Windows yoğun veri için uygundur. Bunlar iyi yoğun veri için daha geniş zaman pencereleri PdM senaryolarda görülen ölçeklendirme değil.
- Toplu Puanlama kullanılabilir değilse, aynı anda küçük gruplar halinde yeni verileri işlemek için çevrimiçi Puanlama uyum çözümüdür.

## <a name="solution-templates-for-predictive-maintenance"></a>Tahmine dayalı bakım çözümü şablonları

Bu kılavuzun son bölümü PdM çözüm şablonları, eğitim ve Azure'da uygulanan denemeler listesini sağlar. Bu PdM uygulamalar, bazı durumlarda dakika içinde bir Azure aboneliği içine dağıtılabilir. Kavram kanıtı tanıtımlar alternatifleri ya da gerçek üretim uygulamaları için Hızlandırıcıları denemeniz için korumalı alanlar olarak kullanılabilir. Bu şablonlar bulunan [Azure AI galeri](http://gallery.azure.ai) veya [Azure GitHub](https://github.com/Azure). Bu farklı örnekleri bu çözüm şablonuna zamanla alınacak.

| # | Unvan | Açıklama |
|--:|:------|-------------|
| 1 | [Tahmine dayalı bakım Azure Machine Learning örnek](https://github.com/Azure/MachineLearningSamples-PredictiveMaintenance) |Sonraki N zaman birimi üzerinde hata tahmin için PdM örnek. Bu örnek bir Azure ML çalışma ekranı projesi olarak yazılır ve PdM için yeni başlayanlar için idealdir. [Ek belgeler](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/scenario-predictive-maintenance) Bu örnek için ilgili.|
| 2 | [Azure Tahmine dayalı bakım çözüm şablonu](https://github.com/Azure/AI-PredictiveMaintenance) | Birden çok PdM senaryolar göstermek için bir uçtan uca çerçevesi. Bu şablon iki senaryo sunar: ilk gerçek zamanlı hata koşulu sınıflandırma yeni bir kullanım örneği değil. Yalnızca bir tümleştirmeye çözüm [1], bu çözüm şablonu ikinci senaryodur. Yeni veya var olan diğer senaryolar eklemek için aynı dağıtılan altyapısını yeniden kullanmak nasıl gösterir.|
| 3 | [Tahmine dayalı bakım için öğrenme derin](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) | Azure not defteri ile Tahmine dayalı bakım için LSTM (uzun kısa vadeli bellek) ağları (yinelenen sinir ağları sınıfı) kullanarak gösteri çözümü olan bir [Bu örneği blog gönderisi](https://azure.microsoft.com/blog/deep-learning-for-predictive-maintenance).|
| 4 | [Tahmine dayalı bakım modelleme Kılavuzu'nda R](https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) | R kodlarıyla PdM modelleme Kılavuzu|
| 5 | [Havacılık için Azure Tahmine dayalı bakım](https://gallery.azure.ai/Solution/Predictive-Maintenance-for-Aerospace-1) | Azure ML v1.0 uçak bakım göre ilk PdM çözüm şablonlarından birini. Bu projeyi kaynaklanan bu kılavuzu. |
| 6 | [IOT kenar için Azure AI Araç Seti](https://github.com/Azure/ai-toolkit-iot-edge) | AI TensorFlow kullanarak IOT edge'de; Araç Seti paketleri derin Azure IOT kenar uyumlu Docker kapsayıcıları modellerinde öğrenme ve bu modelleri REST API'leri olarak kullanıma sunar.
| 7 | [Azure IOT Tahmine dayalı bakım](https://github.com/Azure/azure-iot-predictive-maintenance) | Azure IOT paketi PC'leri - önceden yapılandırılmış çözümü. IOT paketi ile uçak bakım PdM şablonu. [Başka bir belge](https://docs.microsoft.com/azure/iot-suite/iot-suite-predictive-overview) ve [izlenecek](https://docs.microsoft.com/azure/iot-suite/iot-suite-predictive-walkthrough) aynı projeyle ilgili. |
| 8 | [SQL Server R Services kullanarak Tahmine dayalı bakım şablonu](https://gallery.azure.ai/Tutorial/Predictive-Maintenance-Template-with-SQL-Server-R-Services-1) | R hizmetlerini temel alarak kalan kullanım ömrü senaryo Demo. |
| 9 | [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.azure.ai/Collection/Predictive-Maintenance-Modelling-Guide-1) | R ile kullanarak mühendislik uçak bakım dataset özelliğini [denemeler](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Modelling-Guide-Experiment-1) ve [veri kümeleri](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Modelling-Guide-Data-Sets-1) ve [Azure not defteri](https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) ve [denemeler](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2)AzureML v1.0 içinde|

## <a name="training-resources-for-predictive-maintenance"></a>Tahmine dayalı bakım için eğitim kaynakları

[Tahmine dayalı bakım için Azure AI öğrenme yolu](https://github.com/Azure/AI-PredictiveMaintenance/blob/master/docs/azure-ai-learning-path-for-predictive-maintenance.md) , kavramlar ve matematik algoritmalar ve teknikler PdM sorunlarını kullanılan arkasında daha derin bir anlayış için eğitim malzemesi sağlar. 

Microsoft Azure ücretsiz içerik ve eğitim genel AI kavramlar ve yöntem sunar.

| Eğitim kaynak  | Kullanılabilirlik |
|:-------------------|--------------|
| [Azure üzerinde AI Geliştirici](http://azure.microsoft.com/training/learning-paths/azure-ai-developer) | Genel |
| [Microsoft AI Okul](http://aischool.microsoft.com/learning-paths) | Genel |
| [Github'dan Azure AI öğrenme](https://github.com/Azure/connectthedots/blob/master/readme.md) | Genel |
| [LinkedIn öğrenme](http://www.linkedin.com/learning) | Genel |
| [Microsoft AI Youtube Web Seminerlerini](https://www.youtube.com/watch?v=NvrH7_KKzoM&t=4s) | Genel |
| [Microsoft AI Göster](http://channel9.msdn.com/Shows/AI-Show) | Genel |
| [LearnAI@MS](http://learnanalytics.microsoft.com) | Microsoft iş ortakları için |
| [Microsoft iş ortağı ağı](http://learningportal.microsoft.com) | Microsoft iş ortakları için |

Ayrıca, AI MOOCS (açık yoğun çevrimiçi kurslar) boş Stanford ve MIT gibi akademik kurumlarının çevrimiçi sunulan ve diğer eğitim şirketler ' dir.