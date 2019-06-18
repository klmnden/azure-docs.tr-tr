---
title: Müşteri karmaşıklığını çözümleyin
titleSuffix: Azure Machine Learning Studio
description: Analiz etme ve Azure Machine Learning Studio ile müşteri karmaşıklığını puanlamaya yönelik tümleşik bir model geliştirme incelemesi.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 12/18/2017
ms.openlocfilehash: e6a7eaa94e7196c830a66b2d77023bd562119c92
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64699434"
---
# <a name="analyze-customer-churn-using-azure-machine-learning-studio"></a>Azure Machine Learning Studio'yu kullanarak müşteri değişim sıklığını çözümleme
## <a name="overview"></a>Genel Bakış
Bu makalede, Azure Machine Learning Studio kullanılarak oluşturulmuş bir müşteri karmaşıklığı çözümleme projesinin referans uygulaması gösterir. Bu makalede, ilişkili genel modellerini bütünlüklü olarak endüstriyel müşteri karmaşıklığı sorununu çözmek için ele alır. Biz de Machine Learning kullanılarak oluşturulan model doğruluğunu ölçmek ve daha fazla geliştirme yönergeleri değerlendirin.  

### <a name="acknowledgements"></a>Bildirimler
Bu deneyde geliştirildiği ve Serge Berger, Microsoft'ta asıl veri uzmanı ve eski Microsoft Azure Machine Learning Studio için ürün yöneticisi olan Roger Barga test. Azure belgeleri takımının minnettar uzmanlıklarını bildirir ve bu teknik incelemeyi paylaşmak için teşekkürler.

> [!NOTE]
> Bu deneme için kullanılan verileri genel olarak kullanılabilir değil. Değişim sıklığı analiz için makine öğrenme modeli oluşturma örneği için bkz: [Perakende karmaşıklığı model şablonunun](https://gallery.azure.ai/Collection/Retail-Customer-Churn-Prediction-Template-1) içinde [Azure AI Gallery](https://gallery.azure.ai/)
> 
> 



## <a name="the-problem-of-customer-churn"></a>Müşteri karmaşıklığı sorununu
İşletmeler tüketici pazarında ve tüm kurumsal kesimde, değişim sıklığı ile uğraşmak zorunda. Bazen değişim sıklığı aşırı ve ilke kararlarını etkiler. Geleneksel bir çözümün yüksek eğilimini churners tahmin edin ve kendi ihtiyaçlarına özel dispensations uygulayarak veya pazarlama kampanyaları, bir concierge hizmetini aracılığıyla adres sağlamaktır. Bu yaklaşım, sektör sektör farklılık gösterebilir. Bunlar bile belirli tüketicinin kümeden bir endüstri (örneğin, telekomünikasyon) içinde başka bir farklılık gösterebilir.

İşletmeler, bu özel müşteri elde tutma çalışmaları en aza indirmek gereken ortak faktördür. Bu nedenle, her müşteri dalgalanması olasılığını ile puan ve üst N olanları adres için doğal bir Metodoloji olacaktır. En iyi müşteriler En Karlı olanları olabilir. Örneğin, daha karmaşık senaryolarda, kar işlevi özel dispensation için adayları seçim sırasında kullanılır. Ancak, bu tam stratejisi karmaşası başa çıkmak için yalnızca bir kısmını faktörlerdir. İşletmelerin de hesap risk (ve ilişkili risk toleransınıza) düzeyini ve müdahale ve yatkýn müşteri Segmentasyonu maliyetini uygulamanız gerekir.  

## <a name="industry-outlook-and-approaches"></a>Sektör outlook ve yaklaşımları
Gelişmiş işleme değişim olgun bir sektör bir işarettir. Klasik örnek, sıklıkla bir sağlayıcısından diğerine geçmek için aboneleri burada bilinen telekomünikasyon endüstri standardıdır. Gönüllü Bu karmaşıklığı prime bir konudur. Sağlayıcılar hakkında önemli bilgileri ayrıca, birikti *sürücüleri karmaşıklığı*, müşterilerin geçiş sürücü etkenler şunlardır.

Cep telefonu iş değişim ahize veya cihaz seçim örneği için bilinen bir sürücüdür. Sonuç olarak, bir popüler ahize fiyatı subsidize yeni abonelere yönelik ve mevcut müşteriler bir yükseltme için tam bir fiyat uyguladığınız ilkesidir. Tarihsel olarak, bu ilke, yeni indirim almak için bir sağlayıcısından diğerine atlamalı müşterilere açmıştır. Bu, istenir, kendi stratejileri iyileştirmek için sağlayıcıları.

Yüksek dalgalanma ahize tekliflere değişim geçerli ahize modellerde dayalı modelleri hızla çıkarır bir faktördür. Ayrıca, cep telefonları yalnızca telekomünikasyon cihazları değil; bunlar da biçimde deyimleri (iPhone göz önünde bulundurun). Bu sosyal adaylarının normal telekomünikasyon veri kümesi kapsamı dışında olan.

Net modelleme için bilinen nedenlerle karmaşıklığı ortadan kaldırarak bir ses ilke insanlara olamaz sonucudur. Aslında, ölçme kategorik değişkenleri (örneğin, karar ağaçları), Klasik modeli de dahil olmak üzere bir sürekli modelleme, stratejidir **zorunlu**.

Müşterilerinin büyük veri kümelerini kullanarak, kuruluşların büyük veri analizi (özellikle, büyük verilere dayalı değişim saptama içinde) sorun yönelik etkili bir yaklaşım olarak çalışıp çalışmadığını denetleyin. ETL bölümüne önerileri karmaşıklığı sorununu büyük veri yaklaşımı hakkında daha fazla bilgi bulabilirsiniz.  

## <a name="methodology-to-model-customer-churn"></a>Model müşteri kaybı için yöntemi
Şekil 1-3'te Müşteri dalgalanması çözmek için yaygın bir sorun çözme işlemi gösterilmiştir:  

1. Bir risk modeli olasılığını ve risk eylemleri nasıl etkileyeceğini göz önünde bulundurun sağlar.
2. Bir araya modeli, değişim sıklığı ve müşteri miktarını olasılığını müdahale düzeyini nasıl etkileyebilecek göz önünde bulundurun (CLV) ömrü değeri sağlar.
3. Bu analiz, kendisini en iyi öneri sunmak için müşteri segmentlerini hedefleyen proaktif bir pazarlama kampanyası için ilerletilmiş bir quantitative analiz için uygundur.  

![Risk toleransı artı karar modelleri verir eyleme dönüştürülebilir Öngörüler nasıl gösteren diyagram](./media/azure-ml-customer-churn-scenario/churn-1.png)

İleri görünümlü bu yaklaşım karmaşası değerlendirmek için en iyi yoludur, ancak karmaşıklığı ile gelir: çok modelli archetype ve modelleri arasındaki bağımlılıkları izleme geliştirmek sunuyoruz. Aşağıdaki diyagramda gösterildiği gibi modelleri arasındaki etkileşimi kapsüllenmiş:  

![Model etkileşim diyagramı değişim sıklığı](./media/azure-ml-customer-churn-scenario/churn-2.png)

*Şekil 4: Birleşik çok modelli archetype*  

Müşteri bekletme için bütünsel bir yaklaşım sunmak için ise modelleri arasındaki etkileşimi anahtardır. Her model, zaman içinde mutlaka düşürür; Bu nedenle, örtük bir döngü mimaridir (benzer şekilde NET-DM veri araştırma standardına göre ayarlama archetype [***3***]).  

Risk karar pazarlama kesimleme/ayrıştırma genel döngüsü hala birçok iş sorunlarını için geçerli olan bir genelleştirilmiş, yapısıdır. Basitleştirilmiş Tahmine dayalı bir çözüm izin vermeyen bir karmaşık iş sorunun tüm nitelikler sergilediğinden karmaşıklığı analizi yalnızca bir güçlü sorunları bu grubun temsilcisidir. Değişim sıklığı için modern yaklaşımı sosyal yönlerini özellikle yaklaşımda vurgulanmış değil, ancak bunlar herhangi bir modelde olduğu gibi sosyal özelliklerini modelleme archetype içinde kapsüllenir.  

Burada ilginç bir ayrıca büyük veri analizi ' dir. Günümüzün telekomünikasyon ve perakende işletmeler, müşterilerine hakkında ayrıntılı verileri toplamak ve biz kolayca çok modelli bağlantısı için gereken belirli eğilimler nesnelerin interneti gibi Gelişmekte olan ve her yerde bulunan bir genel eğilim olacak öngörüyor birden çok katman akıllı çözümler kullanmak istemiyorsunuz iş izin cihazlar.  

 

## <a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>Machine Learning Studio'da model oluşturma archetype uygulama
Açıklanan sorunu göz önünde bulundurulduğunda, tümleşik bir model ve puanlama yaklaşımı uygulamak için en iyi yolu nedir? Bu bölümde, nasıl size bu Azure Machine Learning Studio kullanılarak gerçekleştirilen gösterilecektir.  

Çok modelli bir yaklaşım genel bir archetype değişim sıklığı için tasarlarken zorunluluktur. Çok modelli bir yaklaşım bile Puanlama (Tahmine dayalı) parçası olması gerekir.  

Aşağıdaki çizimde, dört Puanlama algoritmaları dalgalanmasını tahmin Machine Learning Studio'da hangi kullanan oluşturduğumuz prototip gösterir. Çok modelli bir yaklaşım kullanarak nedenini değil yalnızca bir topluluğu Sınıflandırıcısı, doğruluğunu artırmak için ancak de aşırı sığdırma karşı korumak ve öngörücü özellik seçimi artırmak için oluşturmaktır.  

![Çok sayıda birbirine modüllerle karmaşık bir Studio çalışma alanına gösteren ekran görüntüsü](./media/azure-ml-customer-churn-scenario/churn-3.png)

*Şekil 5: Prototip yaklaşım modelleme bir değişim*  

Aşağıdaki bölümler, Machine Learning Studio kullanılarak uygulanan model Puanlama prototip hakkında daha fazla ayrıntı sağlar.  

### <a name="data-selection-and-preparation"></a>Veri seçimi ve hazırlama
Veri modelleri oluşturmak için kullanılan ve puanı müşterilerin müşteri gizliliğini korumak için farklı verilerle CRM dikey çözümden elde. Veri ABD'deki 8000 abonelikler hakkında bilgi içerir ve üç kaynağı birleştirir: veri (abonelik meta veriler), etkinlik verileri (kullanım sistemin) ve müşteri destek verileri sağlama. Veriler müşterilerle ilgili herhangi bir iş ile ilgili bilgi içermez; Örneğin, bağlılık programı meta veriler ya da kredi puanları içermez.  

Veri hazırlama zaten sahip olduğunu varsaydığından kolaylık olması için ETL ve verileri temizleme işlemleri kapsam dışına başka bir yerde yapılır.

Modelleme için özellik seçimi adaylarının, rastgele orman modülü kullanan işlemine dahil kümesinin başlangıç anlam Puanlama temel alır. Machine Learning Studio'da bir uygulama için ortalama, ORTANCA ve aralıkları temsilcisi özellikleri için hesaplanır. Örneğin, kullanıcı etkinliği için minimum ve maksimum değerleri gibi nitel veri toplamaları ekledik.

Ayrıca en son altı ay boyunca zamana bağlı bilgileri yakaladığımız. Verileri bir yıl boyunca analiz ettik ve olmasa bile istatistiksel olarak önemli eğilimleri, değişim sıklığı üzerindeki etkisini önemli ölçüde altı ay sonra düşer kuruldu.  

Microsoft azure'da veri kaynakları kullanarak Machine Learning Studio'da model oluşturma ve ETL, özellik seçimi dahil olmak üzere sürecin tamamı uygulanmıştır en önemli noktasıdır.   

Aşağıdaki diyagramlarda kullanılan verileri gösterilmektedir.  

![Ham değerler ile kullanılan verileri bir örneğini gösteren ekran görüntüsü](./media/azure-ml-customer-churn-scenario/churn-4.png)

*Şekil 6: (Farklı) veri kaynağının Alıntısı*  

![Veri kaynağından ayıklanan istatistiksel özelliklerini gösteren ekran görüntüsü](./media/azure-ml-customer-churn-scenario/churn-5.png)

*Şekil 7: Veri kaynağından ayıklanan özellikleri*
 

> Bu veriler özeldir ve bu nedenle modeli ve veri paylaşılamaz unutmayın.
> Ancak bu örnek deneme herkese verileri kullanarak benzer bir model için bkz [Azure AI Gallery](https://gallery.azure.ai/): [Telekomünikasyon müşteri dalgalanması](https://gallery.azure.ai/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Cortana Intelligence Suite'i kullanarak bir değişim analiz modeli nasıl uygulayacağınıza dair hakkında daha fazla bilgi için ayrıca öneririz [bu videoyu](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) Kıdemli Program Yöneticisi Wee Hyong Tok tarafından. 
> 
> 

### <a name="algorithms-used-in-the-prototype"></a>Prototip kullanılan algoritmalar
Prototip (özelleştirme yok) oluşturmak için aşağıdaki dört makine öğrenimi algoritmaları kullanılır:  

1. Lojistik regresyon (LR)
2. Artırmalı karar ağacı (BT)
3. Ortalama perceptron (AP)
4. Destekli vektör makinesi (SVM)  

Aşağıdaki diyagram, modelleri oluşturulduğu dizisini gösterir deneme tasarım yüzeyine bir bölümünü gösterir:  

![Studio deneme daha küçük bir bölümünün ekran görüntüsü tuval](./media/azure-ml-customer-churn-scenario/churn-6.png)  

*Şekil 8: Machine Learning Studio'da model oluşturma*  

### <a name="scoring-methods"></a>Puanlama yöntemleri
Biz olan dört model oluşturduğunuz bir etiketli bir eğitim veri kümesi kullanarak puanlanmış.  

Biz de SAS Kurumsal Miner 12 Masaüstü sürümü kullanılarak oluşturulan bir karşılaştırılabilir modeli Puanlama veri kümesine gönderilen. Biz, SAS modelini ve tüm dört Machine Learning Studio'da model doğruluğunu ölçülür.  

## <a name="results"></a>Sonuçlar
Bu bölümde, bizim bulguları Puanlama veri kümesini temel alan bir model doğruluğunu hakkında size sunar.  

### <a name="accuracy-and-precision-of-scoring"></a>Doğruluk ve puanlamasını duyarlık
Genellikle, Azure Machine Learning Studio'da SAS doğruluğu yaklaşık 10-%15 (alanı altında eğri veya AUC) içinde uygulamasıdır.  

Ancak, en önemli değişim sıklığı ölçümü misclassification oranıdır: diğer bir deyişle, tahmin edilen sınıflandırıcı tarafından olarak üst N churners hangisinin gerçekten yaptığınız **değil** karmaşıklığı ve özel olarak değerlendirilmesi henüz alınan? Aşağıdaki diyagram bu modelleri misclassification ücretine karşılaştırılır:  

![4 algoritmaların performansını karşılaştırma eğri grafik alanında](./media/azure-ml-customer-churn-scenario/churn-7.png)

*Şekil 9: Eğri Passau prototip alanında*

### <a name="using-auc-to-compare-results"></a>Sonuçları karşılaştırmak için AUC kullanma
Alanı altında eğri (AUC) genel bir ölçü temsil eden bir ölçüm olan *separability* puanlar pozitif ve negatif yerleştirme için dağıtımlar arasında. Geleneksel alıcı işleci özellikleri (ROC) grafiğe benzer, ancak AUC ölçüm eşiği değeri seçmenizi gerektirmeyeceğini bir önemli fark vardır. Bunun yerine, üzerinden sonuçları özetler **tüm** olası seçenekler. Buna karşılık, dikey eksen ve hatalı pozitif sonuç oranı yatay eksende pozitif sonuç oranı geleneksel ROC grafik gösterir ve sınıflandırma eşiği değişir.   

AUC değerlerine yoluyla Karşılaştırılacak modelleri izin verdiğinden AUC bir ölçü olarak farklı algoritmalar (veya farklı sistemler için) kullanılır. Bu, sektörde meteorology ve biosciences gibi popüler bir yaklaşımdır. Bu nedenle, AUC sınıflandırıcı performansını değerlendirmek için popüler bir aracı temsil eder.  

### <a name="comparing-misclassification-rates"></a>Karşılaştırma misclassification oranları
Biz misclassification ücretler söz konusu veri kümesinde yaklaşık 8000 aboneliklerinin CRM verilerinizi kullanarak karşılaştırılır.  

* 10-%15 SAS misclassification gönderebilme hızıydı.
* Machine Learning Studio misclassification oranı ilk 200-300 churners 15-%20 oluştu.  

Telekomünikasyon sektörün concierge hizmetini veya diğer özel olarak değerlendirilmesi sunarak olasılığı en yüksek riskli sahip müşteriler ele almak önemlidir. Bu bakımdan, Machine Learning Studio uygulamasını sonuçları SAS modelini aynı düzeye ulaşır.  

Biz genellikle olası churners doğru sınıflandırma ilgilendiğiniz aynı şekilde, doğruluk duyarlığından daha fazla önemlidir.  

Aşağıdaki Wikipedia diyagramdan canlı, anlaşılması kolay bir grafik ilişkiyi göstermektedir:  

![İki hedefi. Bir hedef gösterir gevşek gruplandırılmış işaretleri isabet ancak yakın olarak işaretlenmiş hedefe tam isabet etmiş göz "düşük doğruluk: iyi trueness, zayıf duyarlılık. Başka bir hedef sıkı bir şekilde gruplandırılmış ancak gölgeden uzak hedefe tam isabet etmiş göz işaretlenmiş "düşük doğruluk: zayıf trueness, iyi duyarlılık"](./media/azure-ml-customer-churn-scenario/churn-8.png)

*Şekil 10: Doğruluk ve duyarlık etmekten*

### <a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Artırmalı karar ağacı modeli doğruluğu ve duyarlık sonuçları
Aşağıdaki grafikte en doğru olan dört model arasında özelleştirmede artırmalı karar ağacı modeli için Machine Learning'i prototype kullanarak Puanlama ham sonuçları görüntüler:  

![Doğruluk, duyarlık geri çekme, gösteren tablo parçacığı F puanı, AUC, ortalama günlük kaybı ve dört algoritmalar için eğitim günlük kaybı](./media/azure-ml-customer-churn-scenario/churn-9.png)

*Şekil 11: Artırmalı karar ağacı model özellikleri*

## <a name="performance-comparison"></a>Performans karşılaştırma
Biz, Machine Learning Studio modelleri ve SAS Kurumsal Miner 12,1 Masaüstü sürümü kullanılarak oluşturulmuş bir karşılaştırılabilir modeli kullanarak verileri puanlanmış hız karşılaştırılır.  

Algoritmaların performansını aşağıdaki tabloda özetlenmiştir:  

*Tablo 1. Algoritmalar genel performansını (doğruluk)*

| LR | BT | AP | SVM |
| --- | --- | --- | --- |
| Ortalama modeli |En iyi modeli |Yeterli performansa sahip olmayan |Ortalama modeli |

Machine Learning Studio'da büyük ölçüde par üzerinde yürütme, ancak doğruluğunu hızı için % 15-25'ü geride bırakmıştır SAS olan barındırılan modeller.  

## <a name="discussion-and-recommendations"></a>Tartışma ve öneriler
Telekomünikasyon sektörün değişim sıklığı, analiz etmek için çeşitli yöntemler çıkmıştır dahil olmak üzere:  

* Dört temel kategorileri için ölçümleri türetilir:
  * **Varlık (örneğin, bir abonelik)** . Abonelik ve/veya değişim konusu müşteri hakkındaki temel bilgileri sağlayın.
  * **Etkinlik**. Örneğin, oturum açma sayısı varlıkla ilgili tüm olası kullanım bilgilerini edinin.
  * **Müşteri desteği**. Aboneliğin sorunları veya müşteri desteği ile etkileşim var olup olmadığını belirtmek için müşteri destek günlüklerinden bilgi toplar.
  * **Rekabetçi ve iş verilerini**. Tüm olası müşteri bilgilerini elde (örneğin, kullanılamıyor veya izlemek zor olabilir).
* Önem derecesi için sürücü özellik seçimi kullanın. Bu, artırmalı karar ağacı modeli her zaman taahhüdü bir yaklaşım olduğunu gösterir.  

Bu dört kategoriden kullanımı, izlenimini yaratır basit *belirleyici* Kategori başına makul etkenlere biçimlendirilmiş dizinleri dayalı bir yaklaşım değişim sıklığı için riskli müşterileri belirlemek için yeterli. Ne yazık ki atayabiliyoruz yatkýn görünüyor olsa da, bu yanlış bir anlayış olur. , Değişim sıklığı geçici etkisidir ve karmaşıklığı süresini etkileyen faktörleri genellikle geçici bir durumda olmadığından nedenidir. Bugün bırakarak dikkate alınması gereken bir müşterinin ne müşteri adayları yarın farklı olabilir ve bu kesinlikle altı ay bundan farklı olacaktır. Bu nedenle, bir *olasılıklara* seçeneği modelidir.  

Bu önemli gözlem genellikle iş zekası odaklı bir yaklaşım analizi için genellikle tercih ettiği iş kaçan, çoğunlukla, olduğundan daha kolay bir satış ve basit bir Otomasyon admits.  

Bununla birlikte, Self Servis analizi, Machine Learning Studio'yu kullanarak bilgi, bölüm veya departmanı tarafından türünden dört kategorileri için machine learning değişim sıklığı hakkında değerli bir kaynağı haline vaattir.  

Azure Machine Learning Studio'da yakında başka bir heyecan verici özellik, özel bir modül zaten kullanılabilen önceden tanımlanmış modüllerinin depoya ekleme olanağı yöneliktir. Bu özellik, temelde, kitaplığı seçin ve dikey pazarları için şablonlar oluşturma fırsatı oluşturur. Bu bir önemli Azure Machine Learning Studio'nun Pazar yerde avantajıdır.  

Bu konuda daha sonra devam etmek özellikle büyük veri analizi ile ilgili umuyoruz.
  

## <a name="conclusion"></a>Sonuç
Bu yazıda, genel framework kullanarak genel müşteri karmaşıklığı sorununu giderme mantıklı bir yaklaşım açıklanmaktadır. Biz Puanlama modelleri için prototip olarak kabul ve Azure Machine Learning Studio kullanılarak uygulanır. Son olarak, doğruluk ve performans SAS karşılaştırılabilir algoritmalar onaylamaz prototip çözümün değerlendirdik.  

 

## <a name="references"></a>Başvurular
[1] Tahmine dayalı analiz: Öngörüler, Batı McKnight bilgi yönetimi, Temmuz/Ağustos 2011 p.18 20.  

[2] Wikipedia makalesi: [Doğruluk ve duyarlık](https://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [NET-DM 1.0: Adım adım veri araştırma Kılavuzu](https://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [büyük veri pazarlama: Müşterileriniz daha etkili bir şekilde etkileşim kurun ve değer sürücü](https://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco karmaşıklığı model şablonunun](https://gallery.azure.ai/Experiment/Telco-Customer-Churn-5) içinde [Azure AI Gallery](https://gallery.azure.ai/) 
 

## <a name="appendix"></a>Ek
![Değişim sıklığı prototipinde sunu anlık görüntü](./media/azure-ml-customer-churn-scenario/churn-10.png)

*Şekil 12: Değişim sıklığı prototipinde sunu anlık görüntü*
