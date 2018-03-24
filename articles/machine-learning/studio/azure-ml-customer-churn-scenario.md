---
title: Machine Learning kullanarak müşteri karmaşıklığı çözümleme | Microsoft Docs
description: Çözümleme ve müşteri karmaşası Puanlama için tümleşik bir model geliştirerek, örnek olay incelemesi
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 1333ffe2-59b8-4f40-9be7-3bf1173fc38d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.openlocfilehash: 6c64444fc8d42782065d42ed5ee0c193678bb1f1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="analyzing-customer-churn-using-azure-machine-learning"></a>Azure Machine Learning kullanarak müşteri karmaşıklığı analiz etme
## <a name="overview"></a>Genel Bakış
Bu makalede, Azure Machine Learning kullanılarak oluşturulmuş bir müşteri karmaşası analiz proje başvurusu uyarlamasını gösterir. Bu makalede, bütünsel endüstriyel müşteri karmaşası sorunu çözmek için ilişkili genel modelleri tartışın. Biz de Machine Learning kullanılarak oluşturulan modelleri doğruluğunu ölçmek ve daha fazla geliştirme için yol tarifi değerlendirin.  

### <a name="acknowledgements"></a>Bildirimler
Bu deneme geliştirilmiş ve Serge Berger, asıl veri Bilimcisi Microsoft'ta ve Roger Barga, önceden Microsoft Azure Machine Learning için ürün Yöneticisi test. Azure belgelerine takım minnettar uzmanlara kabul ettikten ve bu teknik incelemede paylaşmak için teşekkürler.

> [!NOTE]
> Bu deneme için kullanılan verileri genel kullanıma açık değil. Karmaşası çözümleme için makine öğrenimi modeli oluşturmak nasıl bir örnek için bkz: [perakende karmaşıklığı model şablonunun](https://gallery.cortanaintelligence.com/Collection/Retail-Customer-Churn-Prediction-Template-1) içinde [Azure AI Galerisi](http://gallery.cortanaintelligence.com/)
> 
> 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-problem-of-customer-churn"></a>Müşteri karmaşası sorunu
İşletmeler tüketici Pazar ve tüm kurumsal kesimler karmaşası ile gerekir. Bazen karmaşası aşırı ve ilke kararlarını etkiler. Geleneksel yüksek propensity churners tahmin etmek ve gereksinimlerine, pazarlama kampanyalarının bir danışman hizmeti aracılığıyla veya özel dispensations uygulayarak adres için çözümüdür. Bu yaklaşım, endüstri endüstri farklılık gösterebilir. Bunlar bile belirli tüketici kümeden bir endüstri (örneğin, telekomünikasyon) içinde başka bir farklılık gösterebilir.

İşletmeler bu özel müşteri bekletme çalışmalarını en aza indirmek gereken ortak faktördür. Bu nedenle, bir doğal Metodoloji karmaşası olasılığını her müşteriyle Puanlama ve üst N olanları adresini olacaktır. En iyi müşteriler En Karlı olanları olabilir. Örneğin, daha karmaşık senaryolarda, kar işlevi özel dispensation için aday seçim sırasında kullanılmaz. Ancak, bu noktalar karmaşası ilgilenmek için tam stratejisi yalnızca bir parçası değildir. İşletmeler ayrıca hesap risk (ve ilişkili risk toleransınıza), düzeyini ve maliyetini müdahalesi ve yatkýn müşteri kesimleme gitmesi.  

## <a name="industry-outlook-and-approaches"></a>Endüstri outlook ve yaklaşımlar
Karmaşası Gelişmiş şekilde işlenmesi, olgun sektör işaretidir. Klasik telekomünikasyon endüstri burada aboneleri bir sağlayıcıdan sık geçiş bilinen bir örnektir. Bu gönüllü karmaşası prime bir konudur. Ayrıca, sağlayıcıları önemli bilgi hakkında toplanan *sürücüleri karmaşıklığı*, geçiş yapmak için müşteriler sürücü etken bulunmaktadır.

Örneğin, ahize veya aygıt cep telefonu iş karmaşası, iyi bilinen bir sürücü seçimdir. Sonuç olarak, bir popüler ahize fiyat yeni aboneler için subsidize ve yükseltme için var olan müşteriler için tam bir fiyat uyguladığınız ilkesidir. Tarihsel olarak, bu ilkeyi yeni indirim almak için başka bir sağlayıcıdan atlamalı müşterilere açmıştır. Bu, buna karşılık, kendi stratejileri iyileştirmek için sağlayıcıları istemde.

Ahize teklifleri içinde yüksek volatilite karmaşası geçerli ahize modellerinde dayalı modelleri hızla geçersiz kılan bir faktördür. Ayrıca, cep telefonları yalnızca telekomünikasyon cihazlar olmayan, ayrıca şekilde deyimleri (iPhone göz önünde bulundurun) oldukları. Bu sosyal predictors normal telekomünikasyon veri kümesi kapsamı dışında ' dir.

Net modelleme için bilinen nedenlerle karmaşıklığı ortadan kaldırarak bir ses İlkesi insanlara olamaz sonucudur. Aslında, (örneğin, karar ağaçları), kategorik değişkenleri ölçme Klasik modelleri de dahil olmak üzere bir sürekli modelleme, stratejidir **zorunlu**.

Büyük veri kümeleri müşterilerinin kullanarak, kuruluşlar, etkili bir yaklaşım soruna yönelik olarak büyük veri analizi (özellikle, büyük veri dayalı karmaşası algılama içinde) yapıyorsunuz. ETL bölümünde önerileri karmaşıklık sorun büyük veri yaklaşımı hakkında daha fazla bilgi bulabilirsiniz.  

## <a name="methodology-to-model-customer-churn"></a>Model müşteri karmaşası yöntemi
Şekil 1-3'te Müşteri karmaşası çözmek için ortak bir sorunu çözme işlemi belirtilmiştir:  

1. Risk modeli Eylemler olasılık ve risk nasıl etkilediğini dikkate almanız gereken sağlar.
2. Bir araya modeli araya düzeyini karmaşası ve müşteri miktarını olasılığını nasıl etkileyebilecek dikkate almanız gereken yaşam döngüsü değerinden (CLV) sağlar.
3. Bu çözümleme kendisini en iyi teklif sunmak için müşteri kesimine hedefleyen öngörülü bir pazarlama kampanyası için ilerletilen qualitative analiz için uygundur.  

![][1]

İleri görünümlü bu yaklaşım karmaşası işlemek için en iyi yoludur ancak karmaşıklık ile gelir: birden çok model archetype ve modelleri arasındaki bağımlılıkları izlemenizi geliştirme gerekir. Modelleri arasındaki etkileşim aşağıdaki çizimde gösterildiği gibi kapsüllenmiş:  

![][2]

*Şekil 4: çok model archetype birleşik*  

Müşteri bekletme bütünsel bir yaklaşım sunmak için varsa modelleri arasındaki etkileşim anahtardır. Her model mutlaka zamanla düşürür; Bu nedenle, bir örtük döngü mimarisidir (benzer şekilde NET-DM veri araştırma standardına göre ayarlamak archetype [***3***]).  

Genel döngü riski karar pazarlama kesimleme/ayrıştırma hala birçok iş sorunlarını için geçerli olan bir genelleştirilmiş, yapısıdır. Basitleştirilmiş Tahmine dayalı bir çözüm izin vermeyen bir karmaşık iş sorunun tüm nitelikler sergilediğinden karmaşası analiz yalnızca bir güçlü, bu grubun sorunları temsilcisidir. Modern yaklaşım karmaşıklığı sosyal yönlerini özellikle yaklaşım vurgulanmış değil, ancak tüm modelde olduğu gibi sosyal yönlerini modelleme archetype içinde kapsüllenir.  

Bir ilginç burada büyük veri analizi ektir. Günümüzün telekomünikasyon ve perakende işletmeler müşterilerine hakkında ayrıntılı verileri toplamak ve biz kolayca birden çok model bağlantı gereksinimini nesnelerin interneti ve birden çok katman akıllı çözümleri kullanmayı iş izin bulunabilen cihazlar gibi ortaya çıkan eğilimleri verilen ortak bir eğilim olacak öngörüyor.  

 

## <a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>Machine Learning Studio'da modelleme archetype uygulama
Yalnızca sorunun verildiğinde, tümleşik bir model oluşturma ve yaklaşım Puanlama uygulamak için en iyi yolu nedir? Bu bölümde, nasıl Biz bu Azure Machine Learning Studio kullanılarak başarılır gösterecek.  

Birden çok model şart karmaşası için genel bir archetype tasarlarken yaklaşımdır. Yaklaşım bile Puanlama (Tahmine dayalı) parçası çok model olması gerekir.  

Aşağıdaki diyagramda, hangi karmaşası tahmin etmek için Machine Learning Studio'da dört Puanlama algoritmalar kullanır oluşturduğumuz prototip gösterir. Birden çok model bir yaklaşım kullanarak nedeni değil yalnızca doğruluk artırmak için ancak ayrıca atlayarak sığdırma karşı korumak ve düzenleyici özellik seçimi artırmak için bir ensemble sınıflandırıcı oluşturmaktır.  

![][3]

*Şekil 5: Prototipi yaklaşım modelleme karmaşası*  

Aşağıdaki bölümler, Machine Learning Studio kullanılarak uygulanan modeli Puanlama prototip hakkında daha fazla ayrıntı sağlar.  

### <a name="data-selection-and-preparation"></a>Veri seçimi ve hazırlık
Veri modelleri oluşturmak için kullanılan ve puanı müşterilerin müşteri gizliliğini korumak için gizlenmiş verilerle CRM dikey çözümden elde. Verileri ABD 8.000 abonelikleri hakkında bilgi içerir ve üç kaynağı birleştirir: verileri (abonelik meta verileri), etkinlik verileri (kullanım sisteminin) ve müşteri destek verileri sağlama. Herhangi bir şirket verilerini içermez ilgili müşteriler hakkında; bilgi Örneğin, bağlılık meta veriler veya kredi puanları içermez.  

Verileri hazırlama zaten olduğunu varsayıyoruz çünkü kolaylık sağlamak için ETL ve işlemler temizleme veri kapsamının dışında başka bir yerde yapılır.   

Modelleme için özellik seçimi başlangıç anlamlı rastgele orman modülü kullanan işlemine dahil predictors kümesinin Puanlama temel alır. Machine Learning Studio'da uygulama için ortalama, Orta ve temsili özellikleri aralıklarını hesaplanır. Örneğin, toplamalar nitel verisi için kullanıcı etkinliği için minimum ve maksimum değerleri gibi eklendi.    

Biz de en son altı ay boyunca zamana bağlı bilgi yakalandı. Verileri bir yıl için analiz ettik ve olmasa bile istatistiksel olarak önemli eğilimleri, karmaşıklık üzerindeki etkisini önemli ölçüde altı ay sonra düşer kuruldu.  

En önemli bütün işlem ETL, özellik seçimi de dahil olmak üzere ve modelleme, Machine Learning Studio'da, Microsoft Azure veri kaynaklarında kullanarak uygulanmıştır noktasıdır.   

Aşağıdaki diyagramlarda kullanılan veri gösterilmektedir.  

![][4]

*Şekil 6: Alıntı (gizlenmiş) veri kaynağının*  

![][5]

*Şekil 7: veri kaynağından ayıklanan özellikleri*
 

> Bu veriler özeldir ve bu nedenle modeli ve veri paylaşılamaz unutmayın.
> Ancak, genel kullanıma açık verileri kullanarak bir benzer modeli için bu örnek, denemeler bkz [Azure AI galeri](http://gallery.cortanaintelligence.com/): [Telco müşteri karmaşıklığı](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Cortana Intelligence Suite kullanarak karmaşası analiz modeli nasıl uygulayacağınıza dair hakkında daha fazla bilgi için ayrıca öneririz [bu videoyu](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) üst düzey Program Yöneticisi Wee Hyong Tok tarafından. 
> 
> 

### <a name="algorithms-used-in-the-prototype"></a>Prototip kullanılan algoritmaları
Prototip (özelleştirme yok) oluşturmak için aşağıdaki dört machine learning algoritmaları kullanılır:  

1. Lojistik regresyon (LR)
2. Artırılmış karar ağacı (BT)
3. Ortalama perceptron (AP)
4. Vektör makinesi (SVM) desteği  

Aşağıdaki diyagramda bir kısmı modelleri oluşturulduğu dizisini gösterir deneme tasarım yüzeyi gösterilmektedir:  

![][6]  

*Şekil 8: Machine Learning Studio'da modelleri oluşturma*  

### <a name="scoring-methods"></a>Puanlama yöntemleri
Biz, dört modeli etiketli eğitim veri kümesi kullanarak skoru.  

Biz de SAS Kurumsal araştırıcısı 12 Masaüstü sürümü kullanılarak oluşturulmuş bir karşılaştırılabilir modeli Puanlama kümesine gönderildi. Biz SAS modelini ve tüm dört Machine Learning Studio'da modelleri doğruluğunu ölçülür.  

## <a name="results"></a>Sonuçlar
Bu bölümde, Puanlama veri kümesine dayalı modeller, doğruluğunu hakkında bizim bulgularını sunar.  

### <a name="accuracy-and-precision-of-scoring"></a>Doğruluk ve puanlama, duyarlık
Genellikle, Azure Machine Learning uygulamasında SAS doğruluğu yaklaşık 10-%15 (alanı altında eğri veya AUC) tarafından kullanılıyor.  

Ancak, en önemli ölçüm karmaşıklığı içinde misclassification oranıdır: başka bir deyişle, tahmin edilen sınıflandırıcı tarafından olarak ilk N churners hangisinin gerçekte vermedi **değil** karmaşıklığı ve henüz özel işleme alınan? Aşağıdaki diyagramda bu misclassification oranı tüm modelleri için karşılaştırılır:  

![][7]

*Şekil 9: Passau prototip eğri alanında*

### <a name="using-auc-to-compare-results"></a>Sonuçları karşılaştırmak için AUC kullanma
Alanı altında eğri (AUC) olan genel bir ölçü temsil eden bir ölçüm *separability* pozitif ve negatif ortalamaya puanlarını dağıtımlar arasında. Geleneksel alıcı işleci karakteristiğini (ROC) grafiğe benzer, ancak bir önemli AUC ölçüm bir eşik değeri seçmenizi gerektirmediği farktır. Bunun yerine, üzerinden sonuçları özetlenmektedir **tüm** olası seçenekler. Buna karşılık, geleneksel ROC grafik pozitif oranı dikey eksen ve yanlış pozitif oranı yatay eksende gösterir ve sınıflandırma eşik değişir.   

AUC değerlerine yoluyla Karşılaştırılacak modelleri izin verdiğinden AUC genellikle oluşan bir ölçüyü olarak farklı algoritmalar (ya da farklı sistemleri) için kullanılır. Bu, endüstriler meteorology ve biosciences gibi popüler bir yaklaşımdır. Bu nedenle, AUC sınıflandırıcı performansını değerlendirmek için popüler bir araç temsil eder.  

### <a name="comparing-misclassification-rates"></a>Misclassification oranları karşılaştırma
Biz, söz konusu veri kümesi misclassification oranlarına yaklaşık 8000 aboneliklerin CRM verileri kullanarak karşılaştırılan.  

* SAS misclassification oranı % 10-15 oluştu.
* Machine Learning Studio misclassification oranı ilk 200-300'ü churners 15-%20 oluştu.  

Telekomünikasyon endüstrideki en yüksek riskli bir danışman hizmeti veya diğer özel işleme sunarak karmaşıklığı yükleyen müşterileri adres önemlidir. Bu bakımdan, Machine Learning Studio uygulaması SAS modelini ile eşit düzeye sonuçları elde eder.  

Biz çoğunlukla doğru olası churners sınıflandırma ilgilendiğiniz aynı şekilde, doğruluk duyarlık daha önemlidir.  

Aşağıdaki diyagram Wikipedia gelen canlı, kolay anlaşılır bir grafik ilişkisinde gösterir:  

![][8]

*Şekil 10: Kolaylığını doğruluğunu ve duyarlık arasında*

### <a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Artırılmış karar ağacı modeli için doğruluğunu ve duyarlık sonuçları
Aşağıdaki grafikte dört modelleri arasında en doğru olması için artırılmış karar ağacı modeli için Machine Learning prototipi kullanarak Puanlama ham sonuçları görüntüler:  

![][9]

*Şekil 11: Artırılmış karar ağacı model özellikleri*

## <a name="performance-comparison"></a>Performans karşılaştırma
Biz, Machine Learning Studio'da modelleri ve SAS Kurumsal araştırıcısı 12,1 Masaüstü sürümü kullanılarak oluşturulmuş bir karşılaştırılabilir modeli kullanarak veri aktarılma skoru hızı karşılaştırılan.  

Aşağıdaki tabloda algoritmaları performansını özetlenmektedir:  

*Tablo 1. Genel performans (doğruluk) algoritmaları*

| LR | BT | AP | SVM |
| --- | --- | --- | --- |
| Ortalama modeli |En iyi modeli |Underperforming |Ortalama modeli |

Machine Learning Studio'da büyük ölçüde nominal üzerinde yürütme ancak doğruluğu hızı için 15-25 oranında outperformed SAS olduğu barındırılan modeller.  

## <a name="discussion-and-recommendations"></a>Tartışma ve öneriler
Telekomünikasyon sektörün karmaşası, analiz etmek için çeşitli yöntemler çıkmıştır dahil olmak üzere:  

* Dört temel kategorileri için ölçümleri türetir.
  * **Varlık (örneğin, bir abonelik)**. Abonelik ve/veya karmaşası konu müşteri hakkındaki temel bilgileri sağlayın.
  * **Etkinlik**. Örneğin, oturum açma sayısı varlıkla ilgili tüm olası kullanım bilgilerini edinin.
  * **Müşteri desteği**. Aboneliğin sorunlar veya müşteri desteği ile etkileşim var olup olmadığını belirtmek için müşteri destek günlükleri bilgileri elde etme.
  * **Rekabet ve iş verileri**. Müşteri ile ilgili olası tüm bilgiler elde (örneğin, kullanılamıyor veya izlemek sabit olabilir).
* Sürücü özellik seçimi için önem kullanın. Bu, artırılmış karar ağacı modeli taahhüdü bir yaklaşım her zaman olduğu anlamına gelir.  

Bu dört kategorileri kullanımını çalışabilmesine oluşturan basit bir *belirleyici* her kategori, makul etkenlere biçimlendirilmiş dizinleri dayalı yaklaşım, yeterli karmaşası için risk müşterileri belirlemek için. Ne yazık ki, bu kavram yatkýn görünüyor rağmen yanlış anlama açıktır. , Karmaşıklık zamana bağlı bir etkisidir ve karmaşıklığı katkıda bulunan Etkenler genellikle geçici durumda nedenidir. Ne bugün bırakarak dikkate alınması gereken bir müşteri adaylarını yarın farklı olabilir ve bu kesinlikle altı ay şu andan itibaren farklı olacaktır. Bu nedenle, bir *probabilistic* zorunlu modelidir.  

Bu önemli gözlem genellikle genellikle iş zekası yönelimli bir yaklaşım analitik tercih iş atlamış, çoğunlukla olduğu için daha kolay bir satış ve basit Otomasyon admits.  

Bununla birlikte, Self Servis analizi, Machine Learning Studio kullanarak promise dört bilgi kategorileri arasında bölme veya departmanı tarafından türünden, machine learning karmaşası hakkında için değerli bir kaynak olmasıdır.  

Azure Machine Learning ile gelen başka bir heyecan verici özel bir modül zaten kullanılabilen önceden tanımlanmış modülleri depoya ekleme yeteneği bir özelliktir. Bu özelliği, aslında, kitaplığı seçin ve dikey pazarda şablonları oluşturmak için bir fırsat oluşturur. Bir önemli Azure Machine Learning fark yaratan Pazar yerinde olur.  

Bu konu gelecekte devam etmek özellikle büyük veri analizi için ilgili umuyoruz.
  

## <a name="conclusion"></a>Sonuç
Bu raporda bir genel çerçeve kullanarak müşteri karmaşası yaygın sorun tackling duyarlı bir yaklaşım açıklar. Biz modelleri Puanlama için prototip olarak kabul ve Azure Machine Learning kullanarak uygulanmadı. Son olarak, SAS karşılaştırılabilir algoritmalara açısından prototip çözüm performansını ve doğruluğunu uygunluk.  

 

## <a name="references"></a>Başvurular
[1] Tahmine dayalı analiz: tahminleri, Batı McKnight bilgi yönetimi, Temmuz/Ağustos 2011, p.18-20 ötesinde.  

[2] Wikipedia makale: [doğruluğunu ve duyarlılık](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [NET-DM 1.0: adım adım veri araştırma Kılavuzu](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [büyük veri pazarlama: müşterilerinize daha etkili bir şekilde gerçekleştirmesine ve sürücü değeri](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco karmaşıklığı model şablonunun](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) içinde [Azure AI Galerisi](http://gallery.cortanaintelligence.com/) 
 

## <a name="appendix"></a>Ek
![][10]

*Şekil 12: Anlık görüntüsünü karmaşası prototip sunu*

[1]: ./media/azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/azure-ml-customer-churn-scenario/churn-10.png
