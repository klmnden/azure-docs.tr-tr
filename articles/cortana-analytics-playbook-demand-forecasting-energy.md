---
title: "İsteğe bağlı enerji tahmin için Cortana Intelligence çözüm şablonu Playbook | Microsoft Docs"
description: "Bir çözüm şablonu ile Microsoft Cortana Intelligence'de, isteğe bağlı bir enerji yardımcı şirket için tahmini yardımcı olur."
services: cortana-analytics
documentationcenter: 
author: ilanr9
manager: ilanr9
editor: yijichen
ms.assetid: 8855dbb9-8543-45b9-b4c6-aa743a04d547
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2016
ms.author: ilanr9;yijichen;garye
ms.openlocfilehash: 275e387878900154660d044b26ff5ac03a17a65a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>İsteğe bağlı enerji tahmin için Cortana Intelligence çözüm şablonu Playbook
## <a name="executive-summary"></a>Yönetici Özeti
Son birkaç yılda, büyük fırsatı yardımcı programı ve enerji etki alanı oluşturmak için nesnelerin interneti (IOT), alternatif enerji kaynakları ve büyük veri birleştirilmiş sahip. Aynı anda yardımcı programı ve tüm enerji kesim enerji kullanımını denetlemek için daha iyi yollar yoğun tüketicileri düzleştirme tüketim gördünüz. Bu nedenle, akıllı kılavuz şirketler ve yardımcı programı yenilik ve kendilerini yenilemek için harika ihtiyaç var. Ayrıca, birçok gücü ve yardımcı kılavuzlar, süresi dolmuş ve bakımını yapmak ve yönetmek çok yüksek maliyetli hale gelmektedir. Geçen yıl içinde takım katılımlar enerji etki alanı içindeki bir dizi çalışmaktadır. Çoğu durumda, yardımcı programları veya ISV (bağımsız yazılım satıcıları) gelecekteki enerji talebi tahmin içine arayan bu katılımlar sırasında karşılaştık. Bu tahminleri kendi güncel ve gelecekteki iş önemli bir rol oynar ve çeşitli kullanım durumları için temel oldunuz. Bunlar kısa ve uzun vadeli güç yük tahmin, ticaret, Yük Dengeleme, kılavuz iyileştirme vb. içerir. Büyük veri ve Gelişmiş Analytics (AA) yöntemleri Machine Learning (ML) gibi doğru ve güvenilir tahminleri üretmek için anahtar etkinleştiricilerden durumdadır.  

Bu playbook biz iş ve analitik yönergeleri başarılı bir geliştirme için gereken bir araya getirin ve çözüm enerji talep dağıtımını tahmin. Bu önerilen yönergeleri, yardımcı programlar, veri bilimcilerine ve tam olarak kullanıma hazır hale getirilmiş, bulut tabanlı, isteğe bağlı tahmin çözümleri oluşturma, veri mühendisleri yardımcı olabilir. Yalnızca kendi büyük veri ve gelişmiş analizler gezisine başlangıç şirketler için bu tür bir çözüm, uzun vadeli akıllı kılavuz strateji ilk projeksiyondaki temsil edebilir.

> [!TIP]
> Bu şablon mimari bir genel bakış sağlayan bir diyagram indirmek için bkz: [isteğe bağlı enerji tahmin Cortana Intelligence çözüm şablonu mimarisi](cortana-analytics-architecture-demand-forecasting-energy.md).  
> 
> 

## <a name="overview"></a>Genel Bakış
Bu belge, iş, veri ve teknik unsurlarınızın Cortana Intelligence kullanma ve içinde belirli Azure Machine Learning (AML) uygulama ve dağıtım enerji tahmin çözümleri için kapsar. Belge üç ana bölümden oluşur:  

1. Kurumsal yaklaşım  
2. Veri anlama  
3. Teknik uygulama

**İş anlama** bölümü bir anlamak ve yatırım kararı vermeden önce göz önünde bulundurun için gereksinim duyduğu iş en boy özetlenmektedir. Elinizdeki Tahmine dayalı analiz ve makine öğrenme gerçekten etkili ve geçerli olduğundan emin olmak için iş sorununu nitelemek nasıl açıklanmaktadır. Daha fazla belge machine learning ve enerji tahmin sorunları gidermek için nasıl kullanıldığı temelleri açıklanır. Önkoşullar ve kullanım niteliği ölçütünü özetlenmektedir. Bazı örnek senaryolar da sağlanır durumları ve iş durumu kullanın.

Çözüm öğrenme herhangi bir makine için ana tarifi verilerdir. **Veri anlama** bu belgenin bölümü veri önemli bazı yönlerini kapsar. Enerji tahmin, veri kalitesi gereksinimleri ve hangi veri kaynaklarına genellikle mevcut için gerekli veri türünü özetlenmektedir. Ayrıca ham verileri gerçekten modelleme bölümü sürücü veri özellikleri hazırlamak için nasıl kullanıldığını açıklamaktadır.

Belgenin üçüncü bölümü kapsayan **teknik uygulamanın** bir çözüm yönü. Özellik Mühendisliği ve modelleme veri bilimi işlemi özünde ve biraz ayrıntılı olarak bu nedenle açıklanır. Bulut dağıtım Tahmine dayalı analiz çözümleri için önemli bir platformdur web hizmetleri kavramı kapsar. Biz de tipik bir uçtan uca kullanıma hazır hale getirilmiş çözüm mimarisi verilmiştir.

Ayrıca, belgeyi daha fazla teknoloji ve etki alanını anlamak için kullanabileceğiniz başvuru bilgileri içerir.

Biz bu belgede daha derin veri bilimi işlemi kapak düşünmüyorsanız dikkate almak önemlidir, matematiksel ve teknik yönlerini. Bu ayrıntılar bulunabilir [Azure ML belgelerine](http://azure.microsoft.com/services/machine-learning/) ve [bloglar](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Hedef kitle
Bu belgenin hedef kitlesi iş ve bilgi elde etmek ister misiniz teknik personeli olduğundan ve Machine Learning anlayış çözümleri ve özellikle enerji tahmin etki alanı içinde bu nasıl kullanıldığını temel.

Veri bilimcilerine çözüm tahmin enerji dağıtımını sürücüleri yüksek düzeyli işlem daha iyi anlamak için bu belgeyi okuma da yararlı olabilir. Bu bağlamda bu da iyi bir taban çizgisi oluşturmak için kullanılabilir ve daha fazla bilgi için başlangıç noktası ayrıntılı ve malzeme Gelişmiş.

### <a name="industry-trends"></a>Endüstri eğilimleri
Son birkaç yılda, büyük fırsatı yardımcı programı ve enerji alanı oluşturmak için IOT, alternatif enerji kaynakları ve büyük veri birleştirilmiş sahip. Aynı anda yardımcı programı ve tüm enerji kesimler enerji kullanımını denetlemek için daha iyi yollar yoğun tüketicileri düzleştirme tüketim gördünüz.

Çok sayıda yardımcı programı ve akıllı enerji şirketleri pioneering [akıllı kılavuz](https://en.wikipedia.org/wiki/Smart_grid) kullanım sayısı dağıtarak hale kılavuz tarafından oluşturulan verilerin kullanım. Birçok kullanım örnekleri elektrik üretim devralınmış özellikleri Uzayda Döndür: Bu olamaz toplanan ya da kenara envanter olarak depolanan. Bu nedenle, ne üretilen kullanılması gerekir. Daha verimli olmasını istediğiniz yardımcı programlar gereksinim güç tüketimini yalnızca tahmin etmek, büyük yeteneği verir çünkü **dengelemek tedarik ve talep**, böylece enerji atık önleme **greenhouse gaz azaltın çıkması**ve denetim maliyeti.

Maliyetleri konuşurken fiyatıdır başka bir önemli boy yoktur. Yardımcı programlar arasında güç ticari yönelik yeni yetenekler için harika bir gereksinim getirildi **gelecekteki isteğe bağlı ve elektrik gelecekteki fiyatını tahmin**. Bu, üretim birimleri belirlemek şirketler yardımcı olabilir.

Biz 'Akıllı' word kullandığınızda, biz gerçekte öğrenin ve ardından tahminlerde bir kılavuz bakın. Tüketim Mevsimlik değişiklikler tahmin yanı **geçici aşırı durumlarda öngörüyor ve bunun için otomatik olarak ayarla**. Uzaktan tüketimiyle (Akıllı Bu ölçümler Yardım) düzenleyerek yerelleştirilmiş aşırı durumlarda işlenebilir. **İlk tahmin etmeye ve ardından hareket**, kılavuz kendisini akıllı zamanla yapar.

Biz belirli geleceği, tahmin kapak kullanım örnekleri ailesinde odaklanmak bu belgenin geri kalanı için kısa vadeli ve uzun vadeli enerji isteğe bağlı. Biz bu alandaki birkaç ay için çalışmakta ve bazı bilgi ve endüstri düzeyde sonuçlar etmemizi sağlayan yetenek elde etmiştir. Diğer kullanım örnekleri de belgede yakın gelecekte ele alınacaktır.

## <a name="business-understanding"></a>İşin Gereksinimlerini Anlama
### <a name="business-goals"></a>İş hedefleri
**Enerji Demo** hedeftir tipik Tahmine dayalı analiz ve makine öğrenimi çok kısa bir zaman dilimi içinde dağıtılabilir çözüm göstermek için. Özellikle, böylece iş değeri kullanılabilir hızla gerçekleşen ve bağlı işlevden enerji talep tahmin çözümleri etkinleştirmek için geçerli bizim odak noktasıdır. Bu playbook bilgileri aşağıdaki hedefleri gerçekleştirmeye müşteri yardımcı olabilir:

* Kısa süre için makine öğrenme değerini tabanlı çözümün
* Bir pilot genişletme olanağı kullanım örneği diğer kullanım örnekleri veya kendi iş gereksinimleri temelinde daha geniş bir kapsama
* Hızlı bir şekilde Cortana Intelligence Suite ürün bilgisi elde

Bu hedefleri göz önünde bulundurarak, iş ve bu hedeflere elde etmeye yardımcı olacak teknik bilgi teslim etmek bu playbook amaçlar.

### <a name="power-load-and-demand-forecasting"></a>Güç yük ve isteğe bağlı tahmin
Enerji kesim içinde hangi isteğe bağlı olarak tahmin kritik iş sorunlarını çözmenize yardımcı olabilecek birçok yolu olabilir. Aslında, isteğe bağlı tahmin foundation sektörde pek çok çekirdek kullanım örnekleri için kabul edilebilir. Genel olarak, biz enerji isteğe bağlı tahminleri iki tür düşünün: kısa vadeli ve uzun süreli. Her biri farklı bir amaca ve farklı bir yaklaşım kullanmak olabilir. İkisi arasındaki temel fark, biz tahmin geleceğe zaman aralığını anlamı tahmin yatay ' dir.

#### <a name="short-term-load-forecasting"></a>Kısa vadeli yük tahmin
Enerji talep bağlamında kısa süreli yük tahmin (STLF) çeşitli bölümlerini kılavuz (veya bir bütün olarak kılavuz) üzerinde yakın gelecekte tahmini toplanmış yük olarak tanımlanır. Bu bağlamda, kısa vadeli süresini 24 saat için 1 saat aralığında olarak tanımlanır. Bazı durumlarda, 48 saat kapsamını da mümkündür. Bu nedenle, STLF kılavuz işlemsel kullanım durumda çok yaygın bir durumdur. Kullanım örnekleri güdümlü STLF bazı örnekleri şunlardır:

* Tedarik ve talep Dengeleme
* Güç ticaret desteği
* Pazar yapma (ayar güç Fiyat)
* Kılavuz işletimsel en iyi duruma getirme
* [İsteğe yanıt](https://en.wikipedia.org/wiki/Demand_response)
* İsteğe bağlı tahmin en yüksek
* İsteğe bağlı tarafı Yönetimi
* Yük Dengeleme ve önleme aşırı yükleme
* Uzun süreli yük tahmin
* Hataya ve anomali algılama
* Yoğun curtailment/Dengeleme 

STLF model üzerinde yakın geçmişte çoğunlukla dayalı (son gün veya hafta) Tüketim verileri ve kullanım tahmini sıcaklık önemli bir göstergesi olduğu olarak. Sonraki saat için tahmini doğru sıcaklık alma ve ayarlama 24 saate kadar küçük bir sınama şimdi gün durumundadır. Bu modeller Mevsimlik desenleri veya uzun vadeli tüketimi eğilimlerini daha az duyarlıdır.

SLTF çözümleri tahmin çağrıları (hizmet istekleri), yüksek hacimli oluşturmak büyük olasılıkla da bunlar bir saatlik aralıklarla ve bazı durumlarda bile yüksek sıklığı ile çağrılmakta bu yana. Ayrıca, her tek tek alt istasyon veya transformer tek başına model olarak temsil edilir ve bu nedenle tahmin istek hacmini daha da büyük implantation görmek için çok yaygındır.

#### <a name="long-term-load-forecasting"></a>Uzun süreli yük tahmin
Uzun vadeli yük tahmin (LTLF) birden fazla ay için (ve bazı durumlarda yıl sayısı için) 1 haftada arasında değişen bir süresini ile güç talep tahmin etmek için belirtilir. Bu aralığı yatay çoğunlukla planlama için geçerlidir ve yatırım durumlarda kullanın.

Uzun vadeli senaryoları için birden çok yıl (en az 3 yıl) aralığını kapsayan yüksek kaliteli veri olması önemlidir. Bu modeller genellikle mevsimselliğin desenleri geçmiş verileri ayıklamak ve, dış predicators gibi kullanan hava durumu ve iklim desenleri olarak.

Tahmin yatay uzun olduğundan, tahmin az doğru olmayabilir açıklamak önemlidir. Bu nedenle, bazı güvenirlik aralıkları kendi planlama işlemine olası değişim faktörü insanlar belirleyebilmesini gerçek tahmin birlikte üretmek önemlidir.

Tüketim senaryo LTLF için çoğunlukla planlama olduğundan, biz kadar düşük tahmin birimler (karşılaştırıldığında STLF) bekleyebilirsiniz. Biz Excel veya Power BI gibi görselleştirme araçları içine katıştırılmış bu Öngörüler genellikle görürsünüz ve el ile kullanıcı tarafından çağrılan.

### <a name="short-term-vs-long-term-prediction"></a>Kısa vadeli vs. Uzun vadeli tahmin
Aşağıdaki tabloda, en önemli öznitelikleri açısından STLF ve LTLF karşılaştırılır:

| Öznitelik | Kısa vadeli yük tahmin | Yük tahmin'uzun süreli |
| --- | --- | --- |
| Yatay tahmin |1 saat ile 48 saat |1 ile 6 ay veya daha fazla |
| Veri ayrıntı düzeyi |Saatlik |Saatlik veya günlük |
| Tipik kullanım durumları |<ul><li>Talep/tedarik dengeleme</li><li>Saat tahmin seçin</li><li>İsteğe yanıt</li></ul> |<ul><li>Uzun süreli planlama</li><li>Planlama Kılavuzu varlıklar</li><li>Kaynak planlama</li></ul> |
| Tipik predictors |<ul><li>Gün veya hafta</li><li>Günün saati</li><li>Saatlik sıcaklık</li></ul> |<ul><li>Yılın ayı</li><li>Ayın günü</li><li>Uzun vadeli sıcaklık ve iklim</li></ul> |
| Geçmiş verileri aralığı |İki ila üç yıllık veri |10'a beş yıllık veri |
| Tipik doğruluğu |MAPE * %5 veya daha düşük |MAPE * % 25 veya daha düşük |
| Tahmin sıklığı |Her saat veya 24 saatte oluşturulan |Aylık, üç aylık veya yıllık sonra üretilen |

\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – ortalama yüzde hata anlama

Bu tablodan görüldüğü gibi kısa ve uzun süreli bunlar farklı iş gereksinimlerini temsil eden ve farklı dağıtım ve kullanım düzenlerini olabilir senaryoları tahmin ayırt oldukça önemlidir.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Örnek Kullanım örneği 1: eSmart sistemleri – aşırı en iyi duruma getirme
Önemli bir rol, bir [akıllı kılavuz](https://en.wikipedia.org/wiki/Smart_grid) dinamik olarak ve sürekli olarak en iyi duruma getirme ve değişen tüketim desenler için ayarlayın. Güç tüketimini sıcaklık dalgalanmaları tarafından çoğunlukla neden kısa vadeli değişikliklerden etkilenebilir (*ör*, daha fazla güç hava durumu veya ısıtma kullanılır). Aynı anda güç tüketimini de uzun vadeli eğilimleri tarafından etkilenir. Bunlar mevsimselliğin efektler, Ulusal tatilleri, uzun vadeli tüketim büyüme ve tüketici dizin, Petrol fiyat ve GDP gibi bile ekonomik etkenler olabilir.

Bu kullanım örneğindeki [eSmart](http://www.esmartsystems.com/) verilen tüm alt istasyon ızgaranın bir aşırı yüklemesini durumunuza propensity tahmin etmeye sağlayan bulut tabanlı bir çözümü dağıtmak için istedik. Acil eylem önlemenize veya bu sorunu çözmek için gerçekleştirilecek şekilde saat içinde aşırı büyük olasılıkla substations tanımlamak özel olarak, eSmart istedik.

Doğru bir ve hızlı tahmin gerçekleştirme üç Tahmine dayalı modelleri uyarlamasını gerektirir:

* Uzun vadeli modeli, her alt istasyon üzerinde güç tüketimi sonraki birkaç hafta veya ay sırasında tahmin sağlar.
* Belirli bir alt istasyon aşırı durumunuza sonraki saatte tahminini sağlar kısa vadeli modeli
* Birden çok senaryolar gelecekteki sıcaklığını tahmin sağlar sıcaklık modeli

Uzun vadeli modeli amacı (Güç iletim kapasitelerine) sonraki hafta veya ay içinde aşırı yüklemeyi kendi propensity tarafından substations rank. Bu, kısa vadeli tahmin için bir giriş olarak hizmet sunar substations kısa bir listesi oluşturulmasına izin verir. Sıcaklık uzun vadeli modeli için önemli bir göstergesi olduğu gibi sürekli çok senaryo sıcaklık tahminleri oluşturabilir ve bunları uzun vadeli modeli giriş olarak akış gerek yoktur. Kısa vadeli tahmin sonra hangi alt istasyon sonraki saatten fazla aşırı büyük olasılıkla tahmin etmek için çağrılır.

Kısa vadeli ve uzun vadeli modelleri tek tek her alt istasyon dağıtılır. Bu nedenle, bu modeller pratik yürütülmesi kapsamlı orchestration gerektirir. Kısa vadede daha yüksek tahmin doğruluğunu kazanmak için daha ayrıntılı bir model her gün saat için ayrılır. Bu modeller saatte yürütülür ve yanıt ve önleyici gerekirse eylemleri için yeterli zaman izin vermek için birkaç dakika içinde yürütülmesi biter. Bu koleksiyon modellerin dönemsel yeniden eğitme en son verileri kullanarak güncel tutulur.

Bu kullanım durumu hakkında daha fazla bilgi bulunabilir [burada](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Servis talebi niteliği ölçütünü – Önkoşullar kullanın
Cortana Intelligence ana gücünü dağıtmak ve makine öğrenme merkezli çözümlerini ölçeklendirme güçlü içinde yeteneğidir. Eşzamanlı olarak yürütülen tahminleri, binlerce desteği için tasarlanmıştır. Otomatik olarak değişen tüketim desen karşılayacak şekilde ölçeklendirebilirsiniz. Çözümün bu nedenle, doğruluk ve hesaplama performansı sağlamaktır. Örneğin, bir yardımcı programı şirket tahmin sonraki bir saat ve her saat için doğru enerji talep üretmek ilgileniyor. Diğer taraftan, biz isteğe bağlı olarak olması neden tahmin soruyu yanıtlarken daha az ilgilendiğiniz (modeli, ilgilenebilmek).

Bu nedenle, tüm kullanım örnekleri ve iş sorunlarını etkili bir şekilde machine learning kullanarak çözülebilir hayata geçirmek önemlidir.

Cortana Intelligence ve makine öğrenme aşağıdaki ölçütler sağlandığında belirli iş sorununu çözme konusunda son derece verimli olabilir:

* Elle iş sorunudur **Tahmine dayalı** yapısı. Tahmine dayalı kullanım örneği örnek belirli bir alt istasyon güç yükünü sonraki saatte tahmin etmek istediğiniz yardımcı programı bir şirkettir. Diğer taraftan, çözümleme ve geçmiş isteğe bağlı sürücülerin sıralaması olacaktır **açıklayıcı** yapısı ve bu nedenle daha az uygulanabilir.
* Açık olduğundan **eylemin yolunu** tahmini kullanılabilir olduğunda gerçekleştirilecek. Örneğin, bir sonraki saatte bir alt istasyon üzerinde bir aşırı tahmin etmeye o alt istasyon ile ilişkili yük azaltır ve bu nedenle bir aşırı büyük olasılıkla önleme öngörülü bir eylem tetikleyebilir.
* Kullanım örneğini temsil eder bir **tipik tür bir sorun** şekilde Çözüldü olduğunda bu şekilde pave diğer benzer çözme için kullanım.
* Müşteri ayarlayabilirsiniz **nicel ve nitel hedeflerini** başarılı çözüm uygulama göstermek için. Örneğin, enerji talep tahmin için iyi bir nicel hedefi gerekli doğruluğu eşik olur (*ör*, %5 hata varan izin verilir) veya ne zaman alt istasyon tahmin etmeye aşırı ardından kesinlik (doğru pozitif sonuç oranı) ve geri çağırma belirli bir eşiğin (doğru pozitif sonuç kapsamını) olmalıdır. Bu hedefleri müşterinin iş hedeflerden türetilmesi.
* Açık olduğundan **tümleştirme senaryosunun** şirketin iş akışıyla. Örneğin, alt istasyon yük tahmin aşırı önleme etkinlikleri izin vermek için kılavuz denetim merkezine tümleştirilebilir.
* Kullanmak için hazır müşterinin **yeterli kalite verilerle** kullanım örneğini desteklemek için (daha sonraki bölümde **veri kalitesini**, bu playbook olarak).
* Müşteri kapsayan bulut merkezli veri mimarisi veya **bulut tabanlı makine öğrenme**Azure ML ve diğer Cortana Intelligence Suite bileşenleri dahil olmak üzere.
* Müşteri oluşturma konusunda istekli mi **bir uçtan uca veri akışı** bu tesisler veri teslim düzenli olarak, buluta ve istemediği **faaliyete** çözümü.
* Müşteri için hazır **kaynak ayırması** kimin etkin olarak bağlı sırasında ilk pilot uygulaması böylece bilgi ve çözüm sahipliğini başarıyla tamamlandıktan sonra müşteriye aktarılabilir.
* Müşteri kaynak olmalıdır bir **becerikli veri professional**, tercihen veri Bilimcisi.

Yukarıdaki ölçütlere göre kullanım örneğinin niteliğe büyük ölçüde kullanım başarı oranları artırmak ve gelecekteki kullanım örnekleri uygulama için iyi bir beachhead oluşturun.

### <a name="cloud-based-solutions"></a>Bulut tabanlı çözümler
Cortana Intelligence Azure üzerinde bulutta bulunan tümleşik bir ortam paketidir. Önemli avantajlar işletmeler için ve aynı zamanda şirketler için büyük değişiklik hala kullanım BT çözümleri şirket içi olduğunu olduğu anlamına gelebilir bulut ortamında bulunan bir Gelişmiş analiz çözümü dağıtımını tutar. Enerji kesim içinde aşamalı geçiş buluta işlemlerinin Temizle eğilimini yoktur. Bu eğilimin el ele, yukarıda açıklandığı gibi akıllı kılavuz geliştirme birlikte gider **endüstri eğilimleri**. Bu playbook enerji etki alanı bulut tabanlı bir çözümde odaklanmıştır gibi avantajları ve bulut tabanlı bir çözüm dağıtma diğer noktalar açıklamak önemlidir.

Belki de bulut tabanlı bir çözüme en büyük avantajlarından maliyetidir. Bir çözümü olarak kullanır buluta dağıtılan bileşenlerinin ön maliyetleri veya ilişkili SMM (maliyet mal satılan) bileşeni giderleri yoktur. Donanım, yazılım ve BT bakım yatırım gerek yoktur ve bu nedenle iş riskinin önemli ölçüde azalma yoktur anlamına gelir.

Başka bir önemli avantajı bulut tabanlı çözümlerle Kullandıkça Öde maliyet yapısıdır. Bulut tabanlı sunucular bilgi işlem ve depolama için dağıtılan ve gerekli yeni bir temelinde ölçeklendirilmiş. Bu, bulut tabanlı bir çözüme maliyet verimliliği avantajlarından temsil eder.

Son olarak, bu tüm bulut tabanlı teklif parçası olarak BT Bakım veya gelecekteki altyapı geliştirme yatırım yapmak için gerek yoktur. Bu uzantı için yol haritası gelişen tutar ve Cortana Intelligence Suite en iyi sınıf Hizmetleri'nde içerir. Yeni özellik, bileşenleri ve yetenekler sürekli eklenen ve geliştirin.

Yalnızca kendi geçiş buluta başlayarak bir şirket için yüksek oranda bulut geçiş yol haritası uygulayarak aşamalı bir yaklaşım yapılacak öneriyoruz. Yardımcı programlar ve enerji etki alanındaki şirketler için bu playbook ele alınan kullanım örneklerini Tahmine dayalı analiz çözümlerini bulutta Pilot uygulaması için mükemmel bir fırsat temsil eden inanıyoruz.

#### <a name="business-case-justification-considerations"></a>İş örneği doğrulama konuları
Çoğu durumda, müşteri bir bulut tabanlı çözümü ve Machine Learning önemli bileşenleri olduğu belirli kullanım örneği için bir iş gerekçesinin yaparken ilgilenebilirsiniz. Bulut tabanlı bir çözüme söz konusu olduğunda bir şirket içi çözüm aksine ön maliyet bileşenidir en az ve maliyet öğelerin çoğu gerçek kullanımı ile ilişkilendirilmiş. Cortana Intelligence Suite çözümünü tahmin enerji dağıtımına geldiğinde, birden fazla hizmet, tek bir ortak maliyet yapısı ile tümleştirilebilir. Örneğin, veritabanları (*ör*, SQL Azure) ham verileri depolamak için kullanılan ve gerçek Azure ML tahminleri için ardından tahmin hizmetlerini barındırmak için kullanılır. Bu örnekte, depolama ve işlemsel bileşenlerini maliyet yapısı dahil olabilir.

Öte yandan, bir (kısa veya uzun süreli) tahmin enerji isteğe bağlı işletme iş değerinin iyi anlamış olmanız gerekir. Aslında, her tahmin işlemi iş değerini hayata geçirmek önemlidir. Örneğin, doğru şekilde güç yük için sonraki 24 saat tahmin overproduction engelleyebilir veya aşırı kılavuzundaki önlenmesine yardımcı olabilir ve bu günlük olarak finansal tasarrufları bakımından quantified.

İsteğe bağlı finansal yararı hesaplamak için temel bir formül çözüm olacaktır tahmin: ![isteğe bağlı finansal yararı hesaplamak için temel formülü tahmin çözümü](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Cortana Intelligence Suite Kullandıkça Öde bir fiyatlandırma modelini sağlar olduğundan, bu formülü bir sabit maliyet bileşenine yansıtılmasını için gerek yoktur. Bu formülü bir günlük, aylık veya yıllık temelinde hesaplanabilir.

Geçerli Cortana Intelligence Suite ve Azure fiyatlandırma planı ML bulunabilir [burada](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Çözüm geliştirme işlemi
Bulut tabanlı teknolojiler ve hizmetler Cortana Intelligence Suite içinde çözüm genellikle her biri vermiyoruz 4 aşamaları içerir tahmin enerji talebin geliştirme döngüsü kullanır.

Bu, aşağıdaki çizimde gösterilmiştir:

![Akıllı kılavuz döngüsü](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

Aşağıdaki paragraf bu 4 adım işlemi açıklanmaktadır:

1. **Veri toplama** – her Gelişmiş dayalı analiz çözümü verileri kullanır (bkz **veri anlama**). Özellikle, Tahmine dayalı analiz ve tahmin söz konusu olduğunda, biz devam eden, dinamik veri akışını kullanır. Enerji talep tahmin, söz konusu olduğunda bu verileri doğrudan akıllı ölçümler kaynaklanan veya bir şirket içi veritabanında zaten toplanması. Biz de verilerin hava durumu ve sıcaklık gibi diğer dış kaynaklarını kullanır. Devam eden bu veri akışını düzenlenmiş, zamanlanmış depolanan ve gerekir. [Azure Data Factory](http://azure.microsoft.com/services/data-factory/) (ADF), bu görevi gerçekleştirmeye için ana bizim workhorse olduğu.
2. **Modelleme** – doğru ve güvenilir enerji tahminleri için bir gerekir (tren) geliştirmek ve bakımını yapar geçmiş verileri kullanmak ve verileri anlamlı ve Tahmine dayalı modelleri ayıklar harika bir modeli. Machine Learning (ML) alanı ile daha gelişmiş algoritmalar düzenli olarak geliştirilen hızla büyüyor. Azure ML Studio en gelişmiş ML algoritmaları tam iş akışı içinde kullanmasına yardımcı olan bir iyi kullanıcı deneyimi sağlar. İş akışının sezgisel bir akış şemada gösterilmiştir ve veri hazırlığı, özelliği ayıklama, model ve model değerlendirme içerir. Kullanıcı bu ortamda dahil çeşitli modellerin yüzlerce çeker. Bu aşamada son tarafından tam olarak değerlendirilen ve dağıtım için hazır bir çalışma modeli veri Bilimcisi sahip olur.
   
   Aşağıdaki diyagram tipik bir iş akışı çizim şöyledir:
   
   ![İş akışı modelleme](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)
3. **Dağıtım** – çalışma modeli yandan, sonraki adıma dağıtımıdır. Burada model çeşitli tüketim istemcilerinden gelen Internet üzerinden eşzamanlı olarak çağrılabilen bir RESTful API'si gösteren bir web hizmeti dönüştürülür. Azure ML bir modelden doğrudan Azure ML Studio'da bir düğmeye tek bir tıklatmayla dağıtma basit bir yol sağlar. Tüm dağıtım işlemi başlık altında olur. Bu çözüm otomatik olarak gerekli tüketim karşılayacak şekilde ölçeklendirebilirsiniz.
4. **Tüketim** – bu aşamasında, aslında vermiyoruz tahmin modelinin tahminleri üretmek için kullanın. Tüketim bir kullanıcı uygulamadan güdümlü (*ör*, Pano) veya doğrudan gibi bir işletim sisteminden talep/tedarik sistem ya da bir kılavuz iyileştirme çözümün Dengeleme. Birden çok kullanım durumu tek bir modelden güdümlü.

## <a name="data-understanding"></a>Veri anlama
İş konuları kapsayan sonra (bkz **iş anlama**) çözümü tahmin bir enerji isteğe bağlı, biz artık veri bölümü tartışmak hazırsınız. Herhangi bir Tahmine dayalı analiz çözümü güvenilir verileri kullanır. İsteğe bağlı enerji tahmin için size çeşitli ayrıntı düzeyleri ile geçmiş verilere kullanır. Bu geçmiş verileri ham madde kullanılır. Veri Bilimcisi, gerekli tahminleri sonunda oluşturacak bir modeli koyabilirsiniz (özellikleri da bilinir) predictors tanımlayacak dikkatli bir çözümlemesini yapılacaktır.

Bu bölümde kalan biz çeşitli adımları ve veri ve kullanılabilir bir biçime getirmek nasıl anlama konuları anlatmaktadır.

### <a name="the-model-development-cycle"></a>Model geliştirme döngüsü
İyi tahmin modelleri bazı dikkatli hazırlık gerekir ve planlama notlarının oluşturulması. Birden çok adımı modelleme işlemine bölmek ve aynı anda tek bir adımda odaklanan tüm işleminin sonucunu önemli ölçüde arttırabilir.

Aşağıdaki diyagram, modelleme işlemini içine birden çok adımı nasıl bölünebilir gösterir:

![Model geliştirme döngüsü](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Döngü altı adımlardan oluşur görüldüğü gibi:

* Sorun formülasyonu
* Veri alımı ve veri araştırması
* Veri hazırlama ve özellik Mühendisliği
* Modelleme
* Model değerlendirme
* Geliştirme

Bu bölümün geri kalanında biz her adımda göz önünde bulundurmanız gerekenler ve tek tek adımları anlatmaktadır.

### <a name="problem-formulation"></a>Sorun Formülasyonu
Biz sorun formülasyonu aşağıdakilerden herhangi bir Tahmine dayalı analiz çözümü uygulamadan önce gerçekleştirmeniz gereken en önemli adım olarak düşünebilirsiniz. Burada size problemini dönüştürme ve verileri kullanarak ve teknikleri modelleme çözülebilir belirli öğelere parçalayın. Sorun soruları yanıtlamak için isteriz kümesi olarak formüle iyi bir uygulamadır. İsteğe bağlı enerji tahmin kapsamı içinde geçerli olabilecek bazı olası sorular şunlardır:

* Tek bir alt istasyon beklenen yükü sonraki saat veya gün nedir?
* Günün hangi saatinde my kılavuz en yüksek talebi yaşar?
* Nasıl olasılıkla beklenen en yüksek yükü sürdürebilmek için my Kılavuzu mu?
* Ne kadar güç güç istasyon her günün sırasında oluşturmalı mı?

Bu soruları formulating sağ verileri almak ve elinizdeki problemini ile tam olarak hizalanır bir çözüm uygulama odaklanmak olanak tanır. Ayrıca, biz model performansını değerlendirmek etmemizi sağlayan bazı önemli metrikleri sonra ayarlayabilirsiniz. Örneğin, tahmin ne kadar doğru olmalıdır ve hala işi tarafından kabul edilebilir olacak hata aralığını nedir?

### <a name="data-sources"></a>Veri Kaynakları
Modern akıllı kılavuz çeşitli bölümleri ve kılavuz bileşenlerinin veri toplar. Bu veriler işlemi çeşitli yönlerini ve güç kılavuz kullanımını temsil eder. Tahmin enerji isteğe bağlı kapsamında biz gerçek talep tüketim yansıtacak veri kaynaklarında tartışma kısıtlayan. Akıllı ölçümler enerji tüketimi önemli bir kaynağı var. Yardımcı programlar dünyanın kendi Tüketiciler için hızlı bir şekilde akıllı ölçümler dağıtıyorsanız. Akıllı ölçümler gerçek güç tüketimi kaydedebilir ve bu verileri geri yardımcı şirket sürekli geçiş. Veriler toplanır ve 5 dakikada 1 saat arasında değişen geri sabit bir aralık, gönderilir. Daha gelişmiş akıllı metre uzaktan denetim ve bir evin içindeki gerçek tüketim dengelemek için de programlanabilir. Akıllı ölçüm verileri görece güvenilirdir ve zaman damgası içerir. Bu tahmin talep için önemli bir malzeme kolaylaştırır. Ölçer veri bir araya getirilir (yukarı toplanan) kılavuz topoloji içindeki çeşitli düzeylerde: transformer, alt istasyon, bölge, *vb.*. Biz, ardından bir tahmin modeli için oluşturmak için gerekli toplama düzeyi seçebilirsiniz. Örneğin, yardımcı şirket gelecekteki her birinin yüküne göre kendi kılavuz substations tahmin etmek istiyorsanız sonra tüm ölçümler veriler için tek tek her alt istasyon bir araya getirilir ve tahmin modeli için bir giriş olarak kullanılır. Biz akıllı ölçümler için bir iç veri kaynağı olarak bakın.

Bir güvenilir enerji talep tahmin ayrıca diğer dış veri kaynaklarına bağlıdır. Güç tüketimini etkileyen bir önemli hava ya da daha kesin olarak sıcaklık faktördür. Geçmiş verileri dış sıcaklık ve güç tüketimini arasında güçlü bağıntı gösterir. Tüketiciler sıcak Yaz gün boyunca olun kendi hava bağıntıyı ve sistemler ısıtma üzerinde Kış güç sırasında kullanın. Güvenilir bir kaynak kılavuz konumda geçmiş etme, bu nedenle anahtardır. Ayrıca, biz de sıcaklık doğru tahmin üzerinde bir göstergesi olduğu güç tüketimini kullanır.

Diğer dış veri kaynaklarına enerji talep tahmin modelleri oluşturmaya da yardımcı olur. Bu uzun vadeli iklim değişiklikleri, ekonomik dizinleri içerebilir (*ör*, GDP) ve diğerleri. Bu belgede Biz bu diğer veri kaynakları dahil edilmez.

### <a name="data-structure"></a>Veri yapısı
Gerekli veri kaynakları belirledikten sonra toplanan ham verileri doğru veri özelliklerini içerdiğinden emin olmak isteriz. Güvenilir isteğe bağlı bir tahmin modeli oluşturmak için biz toplanan verileri gelecekteki talep tahmin etmeye yardımcı olması veri öğeleri içerdiğinden emin olmak gerekir. Ham verileri veri yapısını (şema) ile ilgili bazı temel gereksinimleri aşağıda verilmiştir.

Ham verileri satırları ve sütunları oluşur. Her ölçüm verileri tek bir satır temsil edilir. Her veri satırının birden çok sütun (Özellikler veya alanlar da bilinir) içerir.

1. **Zaman damgası** – ne zaman ölçümü kaydedilen gerçek zaman zaman damgası alanı temsil eder. Ortak tarih/saat biçimlerinden biri ile uyumlu. Tarih ve saat bölümleri eklenmelidir. Çoğu durumda, ikinci ayrıntı düzeyini kaydedilmesi süre için gerek yoktur. Veri kaydedilen saat dilimini belirtin önemlidir.
2. **Ölçüm kimliği** -Bu alan ölçer veya ölçüm aygıt tanımlar. Kategorik bir değişkendir ve basamak ve karakter bir bileşimi olabilir.
3. **Tüketim değeri** – belirli bir tarih/zaman gerçek tüketim budur. Tüketim kWh (kilowatt-hour) ölçülebilir veya diğer tercih edilen birimleri. Ölçü birimi verilerdeki tüm ölçümler arasında tutarlı kalması gerekir dikkate almak önemlidir. Bazı durumlarda, tüketim üzerinde 3 güç aşamaları sağlanabilir. Bu durumda biz tüm bağımsız tüketim aşamaları toplamak gerekir.
4. **Sıcaklık** – sıcaklık genellikle bağımsız bir kaynaktan toplanır. Ancak, verilere ile uyumlu olmalıdır. Gerçek tüketim verilerle eşitlenmek üzere izin veren yukarıda açıklandığı gibi bir zaman damgası içermelidir. Sıcaklık değer derece derece veya Fahrenhayt belirtilebilir ancak tüm ölçümler arasında tutarlı kalmasını.
5. **Konum –** konum alanı genellikle burada sıcaklık toplanan veriler yer ile ilişkilidir. Bir posta kodu sayı olarak veya enlem/boylam (lat/uzun) biçiminde gösterilebilir.

Aşağıdaki tablolarda iyi kullanım ve sıcaklık veri biçimi örnekler gösterilmektedir:

| **Tarih** | **Saat** | **Ölçer kimliği** | **Aşama 1** | **Aşama 2** | **3. Aşama** |
| --- | --- | --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |ABC1234 |7.0 |2.1 |5.3 |
| 7/1/2015 |10:00:01 |ABC1234 |7.1 |2.2 |4.3 |
| 7/1/2015 |10:00:02 |ABC1234 |6.0 |2.1 |4.0 |

| **Tarih** | **Saat** | **Konum** | **Sıcaklık** |
| --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |11242 |24.4 |
| 7/1/2015 |10:00:01 |11242 |24.4 |
| 7/1/2015 |10:00:02 |11242 |24.5 |

Yukarıdaki görüldüğü gibi bu örnek 3 güç aşaması olan ilişkili tüketimi için 3 farklı değerler içerir. Ayrıca, tarih ve saat alanları ayrılmış olduğunu, ancak bunlar da tek bir sütun halinde birleştirilebilir unutmayın. Bu durumda konum sütunu 5 rakamlı posta kodu biçimi ve sıcaklık derece derece biçiminde gösterilir.

### <a name="data-format"></a>Veri biçimi
Cortana Intelligence Suite, CSV, TSV, JSON, gibi en yaygın veri biçimleri destekleyebilmesi *vb.*. Veri biçimi için tüm yaşam döngüsünü projenin tutarlı kalmasını önemlidir.

### <a name="data-ingestion"></a>Veri Alımı
Enerji talep tahmin sürekli ve sık tahmin olduğundan, biz ham verileri düz ve güvenilir veri alım işlemi yoluyla akan emin olmanız gerekir. Alım işlem ham verileri gereken zaman tahmin işlemi için kullanılabilir olduğunu güvence altına almalıdır. Veri alım sıklığı tahmin sıklığından büyük olması gerektiğini gösterir.

Örneğin: Çözüm tahmin bizim isteğe bağlı 8: 00'da her gün yeni bir tahmini oluşturur sonra tüm son 24 saat sırasında toplanan verileri o noktadan bir tam olarak alınan ve hatta gerekir, son bir saat eklediğinizden emin olun gerekir  veriler.

Bunu gerçekleştirmek için Cortana Intelligence Suite güvenilir veri alım işlemini desteklemek için çeşitli yollar sunar. Bu, daha fazla incelenecektir **dağıtım** bu belgenin bölüm.

### <a name="data-quality"></a>Veri Kalitesi
Güvenilir ve doğru bir talebi tahmin gerçekleştirmek için gerekli olan ham veri kaynağı bazı temel veri kalite ölçütlerini karşılamalıdır. Gelişmiş istatistiksel yöntemler için bazı olası veri kalitesi sorunu dengelemek için kullanılır ancak biz yine biz bazı temel veri kalitesi eşik yeni veri alma zaman geçmesinden emin olmak gerekir. Ham veri kalitesini ilgili birkaç dikkat edilecek noktalar şunlardır:

* **Eksik değeri** – belirli bir ölçü değil toplanan olduğunda bu duruma başvuruyor. Temel burada eksik değeri oranı belirli bir süre için % 10 ' büyük olmamalıdır, gereksinimdir. Durumda, tek bir değer eksik önceden tanımlanmış bir değer kullanarak belirtilmesi (örneğin: '9999') ve 'geçerli bir ölçü olabilen 0'.
* **Ölçüm doğruluğu** – tüketimi veya sıcaklık gerçek değeri doğru olarak kayıtlı olmalıdır. Yanlış ölçümleri yanlış tahminleri oluşturur. Genellikle, ölçüm hata %1 true değeri göre daha düşük olması gerekir.
* **Ölçüm zaman** – gerekli verilerin gerçek zaman damgası toplanan gerçek ölçü doğru zamanı göre 10 saniyeden fazla tarafından sapma değil.
* **Eşitleme** – birden çok veri kaynağı kullanıldığında (*ör*, kullanım ve sıcaklık) biz hiçbir zaman eşitleme olduğundan emin olun aralarında sorunları. Bu, herhangi iki bağımsız veri kaynaklarından toplanan zaman damgası arasındaki zaman farkı 10 saniyeden fazla aşmamalıdır anlamına gelir.
* **Gecikme süresi** - de, yukarıda açıklanan şekilde **veri alımı**, bir güvenilir veri akışı ve alım işleme bağımlı duyuyoruz. Denetimi biz biz veri gecikmesi kontrol emin olmalısınız. Bu, gerçek ölçüm alındığı saat ve veren Cortana Intelligence Suite depoya yüklendi ve kullanıma hazırdır saat arasındaki zaman farkı olarak belirtilir. Kısa vadeli yük için toplam gecikmeyi düşük tahmin 30 dakikadan fazla olmamalıdır. Uzun vadeli yük için toplam gecikmeyi düşük tahmin 1 günden daha büyük olmamalıdır.

### <a name="data-preparation-and-feature-engineering"></a>Veri hazırlama ve özellik Mühendisliği
Ham verileri alınan sonra (bkz **veri alımı**) ve güvenli bir şekilde depolanır; bu işlenmek üzere hazırdır. Veri hazırlama aşamasında temelde ham verileri alan ve (, yeniden şekillendirmek dönüştürme) dönüştürme modelleme aşaması için bir form içine. Ham verileri sütunu olduğundan, gerçek ölçülen değeri, standartlaştırılmış değerleri, gibi daha karmaşık işlemleri kullanarak gibi basit işlemleri içerebilir [zaman İzolasyonu](https://en.wikipedia.org/wiki/Lag_operator)ve diğerleri. Yeni oluşturulan veri sütunlarını veri özellikleri adlandırılır ve bunlar oluşturma işlemine özellik Mühendisliği olarak adlandırılır. Bu işlem sonu biz ham verileri türetilmiş ve model için kullanılacak yeni bir veri kümesi gerekir. Buna ek olarak, veri hazırlık aşamasının eksik değerleri ilgilenebilmek gerekir (bkz **veri kalitesini**) ve bunlar için dengelemek. Bazı durumlarda, biz de tüm değerleri aynı ölçeğini sunulur emin olmak için verileri normalleştirmek gerekir.

Bu bölümde biz enerji dahil edilen ortak veri özelliklerin bir listesi tahmin modelleri isteğe bağlı.

**Özellikler güdümlü saat:** bu özellikler tarih/zaman damgası verilerden türetilir. Bunlar ayıklanan ve gibi kategorik özellikleri dönüştürülür:

* Saat saat değerleri 0 ile 23 alan budur – günü
* Gün haftanın – Bu haftanın gününü temsil eder ve 1 arasında değerleri alır (Pazar) 7 (Cumartesi)
* Gün ayın – Bu gerçek tarihini temsil eder ve değerleri 1 ile 31 alabilir
* Ay yıl – bu ay temsil eder ve 1 arasında değerleri alır (Ocak) ile 12 (aralık)
* Hafta sonu – bu hafta sonu için haftanın günlerini veya 1'için 0 değerini alan bir ikili değer özelliğidir
* Tatil - bu tatil için normal bir gün veya 1 için 0 değerini alan bir ikili değer özelliğidir
* Fourier koşulları – Fourier koşulları zaman damgası ' türetilen ve mevsimselliğin (döngü) yakalamak için kullanılan ağırlıkları verinin içindedir. Birden çok dönemleri bizim verilerde olabilir beri birden çok Fourier koşulları ihtiyacımız. Örneğin, talep değerleri yıllık, haftalık ve günlük dönemleri/3 Fourier koşullarını neden olacak döngüleri olabilir.

**Bağımsız ölçüm özellikleri:** biz predictors bizim modelinde olarak kullanmak istediğiniz tüm veri öğelerini bağımsız özellikleri içerir. Burada tahmin etmek için ihtiyacımız bağımlı özelliğini dışlayın.

* Gecikme özelliği – bunlar gölgeye zaman gerçek talep değerleri. Örneğin, öteleme 1 özellikleri, isteğe bağlı değeri (saatlik veri varsayılarak) göre geçerli zaman damgası önceki saat içinde tutar. Benzer şekilde, biz öteleme 2 eklemek için 3, öteleme *vb.*. Kullanılan öteleme özellik gerçek bileşimi, model oluşturma aşamasında modeli sonuçları değerlendirme tarafından belirlenir.
* Uzun süreli oluşturan eğilim – bu özellik isteğe bağlı yıl arasında doğrusal artışa temsil eder.

**Bağımlı özellik:** tahmin etmek için modelimizi isteriz veri sütun bağımlı özelliğidir. İle [denetimli makine öğrenme](https://en.wikipedia.org/wiki/Supervised_learning), ilk (, aynı zamanda etiketleri olarak da adlandırılır), bağımlı özelliklerini kullanarak modeli eğitmek ihtiyacımız. Bu bağımlı özelliği ile ilişkilendirilen veri desenleri bilgi edinmek model sağlar. Tahmin enerji isteğe bağlı olarak genellikle gerçek talep tahmin etmek istiyoruz ve bu nedenle biz bunu bağımlı bir özellik olarak kullanırsınız.

**Eksik değerleri işleme:** verileri hazırlama aşamasında, biz eksik değerleri işlemek için en iyi stratejisini belirlemek gerekir. Bu çoğunlukla çeşitli istatistik kullanarak yapılır [veri imputation yöntemleri](https://en.wikipedia.org/wiki/Imputation_\(statistics\)). İsteğe bağlı enerji tahmin söz konusu olduğunda, biz genellikle eksik değerleri önceki kullanılabilir veri noktalarından hareketli ortalama kullanarak impute.

**Veri normalleştirme:** veri normalleştirme benzer bir ölçek tahmin talep gibi tüm sayısal verileri getirmek için kullanılan dönüşümünün başka bir türüdür. Bu, genellikle model doğruluğundan ve duyarlık artırılmasına yardımcı olur. Biz genellikle veri aralığına göre gerçek değer bölerek yaparsınız.
Bu ilk değeri -1 ile 1 arasında genellikle daha küçük bir aralık içinde ölçeklenir.

## <a name="modeling"></a>Modelleme
Veri dönüştürme bir modeline gerçekleştiği modelleme aşamasıdır. Bu işlemi var. çekirdek geçmiş verileri (eğitim) tarayın, desenleri ayıklamak ve bir model oluşturma algoritmaları Gelişmiş. Bu model, daha sonra model oluşturmak için kullanılmamış olan yeni veriler üzerinde tahmin etmek için kullanılabilir.

Biz çalışmaya güvenilir bir modeli eğittikten sonra biz ardından, gerekli özellikleri (X) içerecek şekilde yapılandırılmış yeni verilerinizi puanlamada için kullanabilirsiniz. Puanlama işlem kalıcı modelinin (eğitim aşaması nesnesinden) kullanın ve Ŷ tarafından belirtilen hedef değişkeni tahmin hale getirir.

### <a name="demand-forecasting-modeling-techniques"></a>Tahmin modelleme teknikleri talep
İsteğe bağlı vermiyoruz tahmin durumunda zamanına göre sıralanmış geçmiş verilerin kullanın. Biz genellikle zaman boyutu olarak içeren verilere başvuran [zaman serisi](https://en.wikipedia.org/wiki/Time_series). Zaman serisi modelleme hedefte bulmaktır olanlar bir modeline formüle ve zaman ilgili eğilimleri, mevsimselliğin, otomatik bağıntı (bağıntı zaman içinde).

Son yıllarda gelişmiş algoritmalar zaman serisi tahmin uyum sağlayacak şekilde ve tahmin doğruluğunu artırmak için geliştirilmiştir. Biz kısaca birkaç tanesi aşağıda ele alınmıştır.

> [!NOTE]
> Bu bölümde, bir makine öğrenme ve genel bakış tahmin olarak ancak bunun yerine isteğe bağlı tahmin için yaygın olarak kullanılan teknikleri modelleme kısa anket olarak kullanılmak üzere tasarlanmamıştır. Daha fazla bilgi ve eğitim malzemesi zaman serisi tahmin hakkında çevrimiçi kitap öneririz [tahmin: ilkeleri ve uygulama](https://www.otexts.org/book/fpp).
> 
> 

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (hareketli ortalama)**](https://www.otexts.org/fpp/6/2)
Hareketli ortalama zaman serisi tahmin için kullanılan ilk analitik teknikleri birini ve hala bir en sık kullanılan teknikleri bugünden itibaren olur. Bu ayrıca teknikleri tahmin daha gelişmiş temelidir. Hareketli Ortalama biz burada K hareketli ortalama sırasını gösterir K en son noktaları ortalayarak sonraki veri noktası tahmin.

Hareketli Ortalama teknik tahmin düzgünleştirme etkisi vardır ve bu nedenle de büyük volatilite veri işleyebilir değil.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**Madde işaretleri (Üstel düzeltme)**](https://www.otexts.org/fpp/7/5)
Üstel yumuşatma (madde işaretleri) son veri noktalarını ağırlıklı ortalaması sonraki veri noktası tahmin etmek için kullanabileceğiniz çeşitli yöntemler ailesi ' dir. Daha yeni değerleri daha yüksek ağırlıkları atayın ve bu ağırlık eski ölçülen değerler için giderek azaltmak için kullanılan uygulamadır. Birkaç farklı yöntemle bu aile, bunlardan bazıları, veri mevsimselliğin gibi işleme dahil [Holt Winters Mevsimlik yöntemi](https://www.otexts.org/fpp/7/5).

Bu yöntemlerin bazıları aynı zamanda veri mevsimselliğin içinde öğeli.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (otomatik gerileme tümleşik hareketli ortalama)**](https://www.otexts.org/fpp/8)
Otomatik gerileme tümleşik hareketli ortalama (ARIMA) zaman serisi tahmin için yaygın olarak kullanılan başka bir yöntem ailesi ' dir. Pratikte, otomatik gerileme yöntemleri'nin hareketli ortalama ile birleştirir. Otomatik gerileme yöntemler sonraki tarih noktası hesaplamak üzere önceki zaman serisi değerleri gerçekleştirerek regresyon modeli kullanır. ARIMA yöntemleri aynı zamanda veri noktaları arasındaki farkı hesaplama ve bunları özgün ölçülen değeri yerine kullanarak içeren fark kayıt yöntemleri için geçerlidir. Son olarak, ARIMA da yukarıda açıklanan hareketli ortalama teknikler kullanır. Bu yöntemlerin çeşitli şekillerde tümü, ne ARIMA yöntemleri ailesi oluşturur birleşimidir.

Madde işaretleri ve ARIMA yaygın olarak bugün enerji talep tahmin ve diğer birçok tahmin sorunları için kullanılır. Çoğu durumda bunlar birleştirilir birlikte çok doğru sonuçlar teslim etmek için.

**Genel çoklu regresyon** regresyon modelleri için en önemli modelleme yaklaşım makine öğrenme ve İstatistikler etki alanı içinde olabilir. Zaman serisi bağlamında regresyon gelecekteki değerleri tahmin etmek için kullanıyoruz (*ör*, isteğe bağlı olarak). Regresyon içinde predictors doğrusal bileşimini alın ve eğitim işlemi sırasında bu predictors (katsayısını da bilinir) ağırlıkları öğrenin. Bizim tahmin edilen bir değer tahmin regresyon satır üretmek için belirtilir. Hedef değişkeni sayısal olduğunda ve bu nedenle de zaman serisi tahmin uygun regresyon yöntemleri uygundur. Regresyon yöntemleri çok basit regresyon modeller gibi çeşitli yoktur [doğrusal regresyon](https://en.wikipedia.org/wiki/Linear_regression) ve olanları karar ağaçları gibi daha gelişmiş [rastgele ormanlar](https://en.wikipedia.org/wiki/Random_forest), [Neural Ağları](https://en.wikipedia.org/wiki/Artificial_neural_network)ve karar ağaçları Boosted.

Enerji isteğe bağlı bir regresyon sorun tahmin oluşturma bize büyük bir fiili talep zaman serisi veri ve sıcaklık gibi dış etkenler birleştirilebilir bizim veri özellikleri seçerek esneklik sağlar. Seçilen özellikler hakkında daha fazla bilgi özellik Mühendisliği içinde açıklanan (bkz **veri hazırlığı ve özellik Mühendisliği**) bu playbook bölümü.

Uygulama ve enerji isteğe bağlı tahminleri pilot dağıtımı ile bizim deneyimlerden Azure ML bulunan Gelişmiş regresyon modeller en iyi sonuçlar eğilimindedir ve vermiyoruz bulduk bunları kullanın.

## <a name="model-evaluation"></a>Model değerlendirme
Model değerlendirme sahip kritik bir rolde **modeli geliştirme döngüsü**. Bu adımda modeli ve performansını gerçek verilerle doğrulama içine bakın. Modelleme adımı sırasında kullanılabilir verilerin bir kısmını modeli eğitmek için kullanırız. Değerlendirme aşaması sırasında veri modeli test etmek için geri kalanı alın. Pratikte biz yeniden oluşturulamaz ve eğitim veri kümesi ile aynı özellikleri içeren model yeni verileri besleme anlamına gelir. Ancak, doğrulama işlemi sırasında model hedef değişkeni tahmin etmek yerine kullanılabilir hedef değişkeni sağlamak için kullanırız. Biz genellikle modeli Puanlama olarak bu işlem bakın. Biz sonra doğru hedef değerleri kullanın ve bunları tahmin edilen raporlarla karşılaştırın. Burada ölçmek ve tahminleri true değeri arasındaki fark anlamına tahmin hata en aza indirmek için belirtilir. Model ince ayar ve hata gerçekte azalan olup olmadığını doğrulamak isteriz beri hata ölçüm niceleme anahtardır. Model ince ayar yapılabilir öğrenme işlemini denetleyen modeli parametreleri değiştirerek veya ekleyerek veya kaldırarak veri özellikleri (olarak adlandırılan [parametreleri tarama](https://channel9.msdn.com/Blogs/Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Pratikte, biz modelleme, özellik Mühendisliği arasında yinelemek ve biz hata gerekli düzeyine azaltabilirsiniz kadar Değerlendirme Aşaması birden çok kez model gerekebilir anlamına gelir.

Tahmin hatası hiçbir zaman sıfır olarak olacağını Vurgu önemlidir hiçbir mükemmel her sonucu tahmin etmek bir model değildir. Ancak, belirli bir iş tarafından kabul edilebilir hata büyüklüğünü yoktur. Doğrulama işlemi sırasında bizim modeli tahmin hata düzeyinde olduğundan emin olmak isteriz veya iş tolerans düzeyi daha iyi. Bu nedenle döngüsü sırasında başındaki tolerable hata düzeyini ayarlamak önemli olan **sorun Formülasyonu** aşaması.

### <a name="typical-evaluation-techniques"></a>Tipik değerlendirme teknikleri
Hangi tahmin hata ölçülen quantified ve çeşitli yolları vardır. Bu bölümde biz zaman serisi ve özel enerji talebi tahmin ilgili değerlendirme teknikleri hakkında tartışma odaklanır.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE ortalama mutlak yüzdesi hata anlamına gelir. İle MAPE biz arasındaki farkı bilgisayar her nokta ve o noktadan gerçek değeri tahmini. Biz sonra hata noktası başına gerçek değer göreli fark oranını hesaplayarak ölçme. Son adımda bu değerleri ortalama. MAPE için kullanılan matematiksel formül aşağıda verilmiştir:

![MAPE formülü](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*burada A<sub>t</sub> gerçek değer, F<sub>t</sub> tahmin edilen bir değer ve n tahmin yatay.*

## <a name="deployment"></a>Dağıtım
Biz modelleme aşaması nailed ve model performans doğrulanmış sonra biz dağıtım aşaması gitmek hazırsınız. Bu bağlamda dağıtım modeli gerçek tahminleri üzerinde büyük ölçekte çalıştırarak kullanmak üzere müşteri anlamına gelir. Ana Amacımız yalnızca verilerinden öngörü alma aksine tahminleri sürekli çağrılacak olduğundan dağıtım Azure ML anahtarında kavramdır. Dağıtım aşaması, büyük ölçekli olarak kullanılması için modelini etkinleştirme burada parçasıdır.

Enerji talep tahmin bağlamında bizim AIM sürekli ve düzenli tahminleri yeni veri modeli için kullanılabilir olduğundan ve tahmin edilen verilerin kaybı istemcisine geri gönderilir sağlarken çağırmaktır.

### <a name="web-services-deployment"></a>Web Hizmetleri dağıtımı
Azure ML ana dağıtılabilir yapı taşıdır web hizmetidir. Bu, bulutta Tahmine dayalı bir model kullanımını etkinleştirmek için en etkili yoludur. Web hizmeti model yalıtır ve ile sarmalar bir [RESTful](http://www.restapitutorial.com/) API (uygulama programlama arabirimi). API, aşağıdaki çizimde gösterildiği gibi herhangi bir istemci kod parçası olarak kullanılabilir.

![Hizmet dağıtımı ve kullanımı](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Görüldüğü gibi web hizmeti Cortana Intelligence Suite buluta dağıtılan ve kendi gösterilen REST API uç noktası üzerinden çağrılabilir. Çeşitli etki alanlarında istemciler farklı türde Hizmet Web API aracılığıyla aynı anda çalıştırabilirsiniz. Web hizmeti ayrıca eş zamanlı çağrıları binlerce destekleyecek şekilde ölçeklendirebilirsiniz.

### <a name="a-typical-solution-architecture"></a>Tipik çözüm mimarisi
Çözüm tahmin enerji isteğe bağlı dağıtırken, tahmin web hizmeti geçer ve tüm veri akışı kolaylaştıran bir uçtan uca Çözüm dağıtımı konusuna ilgi duyan duyuyoruz. Size yeni bir tahmini çağırma zamanında biz model güncel verileri özelliklerle gönderilir emin olmak gerekir. Bu yeni toplanan ham verileri sürekli alınan, işlenen ve model derlendiği ayarlanmış gerekli özellik içine dönüştürülmüş anlamına gelir. Tahmin edilen verilerin istemcileri tüketen son kullanabilmesi aynı anda isteriz. Bir örnek veri akışı döngüsü (veya veri ardışık) aşağıdaki çizimde gösterilmiştir:

![Uçtan uca veri akışı enerji talep tahmin](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Enerji talep tahmin döngüsü bir parçası olarak gerçekleşmesi adımları şunlardır:

1. Dağıtılan veri ölçümler milyonlarca sürekli güç tüketimi verilerinin gerçek zamanlı olarak veren.
2. Bu veri okunduğunu toplanır ve bir bulut depoya karşıya (*ör*, Azure Blob).
3. İşlenmeden önce ham verileri bir alt istasyon veya bölgesel düzeyinde iş tarafından tanımlanan toplanır.
4. Özellik işleme (bkz **veri hazırlığı ve özellik işleme**) sonra gerçekleşmesi üretir için gerekli olan verileri model ve eğitim veya Puanlama – özellik kümesi verilerinin bir veritabanında depolanır (*örneğin*, SQL Azure).
5. Yeniden eğitim hizmet çağrılan böylece Puanlama web hizmeti tarafından kullanılan tahmin modeli – yeniden eğitmek için model güncelleştirilmiş sürümünün kalıcıdır.
6. Puanlama web hizmeti gerekli tahmin sıklığı uygun bir zamanlamaya göre çağrılır.
7. Tahmin edilen verilerin son tüketim istemci tarafından erişilebilir bir veritabanında depolanır.
8. Tüketim istemci tahminleri alır, geri kılavuza uygular ve uygun şekilde gerekli kullanım örneğini kullanır.

Tüm bu döngüsünü tamamen otomatik hale getirilmiştir ve bir zamanlamaya göre çalışan dikkate almak önemlidir. Bu veri döngüsü tüm orchestration gibi araçları kullanarak yapılabilir [Azure Data Factory](http://azure.microsoft.com/services/data-factory/).

### <a name="end-to-end-deployment-architecture"></a>Uçtan uca dağıtım mimarisi
Pratikte enerji bir isteğe bağlı tahmin çözüm Cortana Intelligence üzerinde dağıtmak için gerekli bileşenleri kurulmuş ve doğru yapılandırıldığından emin olmak ihtiyacımız.

Aşağıdaki diyagramda uygulayan ve yukarıda açıklanan veri akışı döngüsü düzenler tipik bir Cortana Intelligence tabanlı mimari gösterilir:

![Uçtan uca dağıtım mimarisi](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Her bir bileşenleri ve tüm mimarisi hakkında daha fazla bilgi için lütfen enerji çözüm şablona bakın.

