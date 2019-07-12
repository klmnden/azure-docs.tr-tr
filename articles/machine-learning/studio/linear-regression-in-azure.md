---
title: Excel'den geçirme analizi
titleSuffix: Azure Machine Learning Studio
description: Excel ve Azure Machine Learning Studio'da doğrusal regresyon modeli karşılaştırması
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 7db66f6f4efa5e48f2af9380115de8bcfb75cb86
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786681"
---
# <a name="migrate-analytics-from-excel-to-azure-machine-learning-studio"></a>Analytics, Azure Machine Learning Studio'da Excel'den geçirme

> *Kate Baroni* ve *Ben Boatman* Kurumsal çözüm mimarları, Microsoft'un veri öngörüleri mükemmel Merkezi olan. Bu makalede, bunlar Azure Machine Learning Studio'yu kullanarak bulut tabanlı bir çözüme varolan bir regresyon analiz paketini geçiş deneyimlerini açıklanmaktadır.

## <a name="goal"></a>Hedef

İki hedefleri düşünerek Projemizin kullanmaya: 

1. Bizim kuruluşun aylık gelir projeksiyonlar doğruluğunu artırmak için Tahmine dayalı analiz kullanın 
2. Azure Machine Learning Studio kullanmak doğrulamak için en iyi duruma getirme, hızı, artırmak ve ettiğimiz sonuçların ölçeğini. 

Gibi çok sayıda işletme, kuruluşumuz işlem tahmini aylık bir gelir gider. Küçük ekibimiz iş analistleri, işlem desteği ve tahmin doğruluğunu artırmak için Azure Machine Learning Studio kullanmaya görevli. Takım, birden fazla kaynaktan veri toplama ve istatistiksel çözümleme hizmetleri satış tahmini ilgili kilit özniteliklerini tanımlayan aracılığıyla veri öznitelikleri çalışan birkaç ay ayırıyor. Sonraki adım, prototip oluşturma istatistiksel gerileme modeli Excel'de veri çubuğunda başlamak için oluştu. Birkaç hafta içinde geçerli bir alan ve işlemleri tahmin Finans geride bir Excel regresyon modeli vardı. Bu, temel tahmin sonuç hale geldi. 

Ardından sonraki adıma Studio Tahmine dayalı performansı nasıl geliştirebileceği kullanıma bulmak için Studio bizim Tahmine dayalı analiz taşınmadan için attık.

## <a name="achieving-predictive-performance-parity"></a>Tahmine dayalı performans eşlik elde edin
İlk bizim önceliğimiz, regresyon modelleri Studio ile Excel arasında denkliğini sağlamamız oluştu. Eğitim ve test verileri için verilen, aynı verileri ve aynı bölme, Excel ile Studio arasındaki öngörülebilir performans denkliğini sağlamamız istedik. Başlangıçta alamadık. Excel modeline Studio model bırakmıştır. Studio taban araç ayarında anlayış eksikliği nedeniyle başarısız oldu. Studio ürün takımı ile bir eşitleme sonra size daha iyi bir ayarı bizim veri kümeleri için gerekli temel anlayış kısalttılar ve iki model arasındaki eşlik elde edebilirsiniz. 

### <a name="create-regression-model-in-excel"></a>Excel'de regresyon modeli oluşturun
Bizim Excel regresyon Excel çözümleme araç içinde bulunan standart doğrusal regresyon modeli kullanılır. 

Biz hesaplanan *ortalama mutlak % Error* ve model için performans ölçümü kullanılır. Excel kullanarak bir çalışma modeli ulaşması için 3 ay sürdüğü. Biz çok learning içine sonuçta gereksinimleri anlamak yararlı Studio deneme yaptı.

### <a name="create-comparable-experiment-in-studio"></a>Studio'da karşılaştırılabilir deneme oluşturma
Bizim deneme Studio'da oluşturmak için aşağıdaki adımları izlenen: 

1. Veri kümesi Studio (çok küçük dosyası) bir csv dosyası olarak karşıya yüklendi
2. Yeni bir deneme oluşturulur ve kullanılır [kümesindeki sütunları seçme][select-columns] modülü Excel'in kullanılan aynı veri özellikleri seçmek için 
3. Kullanılan [verileri bölme][split] Modülü (ile *göreli ifade* modu) verileri Excel'de bitti olarak aynı eğitim kümelerine ayırmak için 
4. İle deneme [doğrusal regresyon][linear-regression] Modülü (yalnızca varsayılan seçenek), belgelenmiş ve Excel regresyon modelimizi sonuçları karşılaştırma

### <a name="review-initial-results"></a>İlk sonuçlarını gözden geçirin
İlk başta, Excel modeline açıkça Studio model ayları için: 

|  | Excel | Studio |
| --- |:---:|:---:|
| Performans | | |
| <ul style="list-style-type: none;"><li>R kare ayarlanmış</li></ul> |0.96 |Yok |
| <ul style="list-style-type: none;"><li>Katsayısı <br />Belirleme</li></ul> |Yok |0.78<br />(düşük doğruluk) |
| Mean Absolute Error |$9.5 DK |$ 19.4 M |
| Mean Absolute Error (%) |6.03% |12.2% |

Bizim işlemi ve sonuçları veri uzmanları ve geliştiriciler Machine Learning ekibi karşılaştık, bunlar bazı yararlı ipuçları hızla sağlanan. 

* Kullanırken [doğrusal regresyon][linear-regression] Studio modülünde iki yöntem sağlanır:
  * Çevrimiçi gradyan düşüşü: Büyük ölçekli sorunları için daha uygun olabilir
  * Sıradan kareler: Çoğu kişi, doğrusal regresyon duyduğunuzda düşünün yöntem budur. Küçük veri kümeleri için sıradan kareler daha iyi bir seçim olabilir.
* L2 Kurallaştırma ağırlığı parametresi, performansı artırmak için ince ayar yapma göz önünde bulundurun. 0,001 için varsayılan olarak ayarlanmış, ancak bizim küçük veri kümesi için performansı artırmak için 0.005 için ayarladık. 

### <a name="mystery-solved"></a>Çözülen sırrı!
Öneriler uyguladığımız, Studio'yu olarak Excel ile aynı temel performans alanımız: 

|  | Excel | Studio (Başlangıç) | En küçük kareler ile Studio |
| --- |:---:|:---:|:---:|
| Etiketli değeri |Fiili (sayısal) |Aynı |Aynı |
| Öğrenici |Excel -> veri analizi, regresyon -> |Doğrusal regresyon. |Çizgisel Regresyon |
| Learner seçenekleri |Yok |Varsayılanları |sıradan kareler<br />L2 0.005 = |
| Veri kümesi |26 satırı, 3 özellikleri, 1 etiketi. Tüm sayısal. |Aynı |Aynı |
| Bölünmüş: Eğitim |Son 8 satırlarda test ilk 18 satırlarda Excel eğitim. |Aynı |Aynı |
| Bölünmüş: Test etme |Son 8 satırlara uygulanan Excel regresyon formülü |Aynı |Aynı |
| **Performans** | | | |
| R kare ayarlanmış |0.96 |Yok | |
| Katsayısı |Yok |0.78 |0.952049 |
| Mean Absolute Error |$9.5 DK |$ 19.4 M |$9.5 DK |
| Mean Absolute Error (%) |<span style="background-color: 00FF00;"> 6.03%</span> |12.2% |<span style="background-color: 00FF00;"> 6.03%</span> |

Ayrıca, Excel katsayıları iyi özellik ağırlıkları Azure eğitilen modeli ile karşılaştırıldığında:

|  | Excel katsayıları | Azure özelliği ağırlıkları |
| --- |:---:|:---:|
| Intercept/sapması |19470209.88 |19328500 |
| Özellik A |0.832653063 |0.834156 |
| Özellik B |11071967.08 |11007300 |
| Özellik C |25383318.09 |25140800 |

## <a name="next-steps"></a>Sonraki Adımlar
Machine Learning web hizmetini Excel'den kullanma istedik. Bizim iş analistleri Excel kullanan ve bir şekilde bir Excel veri satırı ile Machine Learning web hizmetini çağırın ve sahip tahmin edilen değer Excel'e ihtiyacımız. 

Ayrıca modelimiz, en iyi duruma getirme seçenekleri ve Studio'da kullanılabilen algoritmaları kullanarak istedik.

### <a name="integration-with-excel"></a>Excel ile tümleştirme
Machine Learning regresyon modelimizi eğitilmiş modelden bir web hizmeti oluşturarak çalışır hale getirme Çözümümüzü oluştu. Birkaç dakika içinde web hizmeti oluşturuldu ve tahmin edilen gelire değerini döndürmek için doğrudan Excel'den diyoruz. 

*Web Hizmetleri Pano* indirilebilir bir Excel çalışma kitabı bölüm içerir. Çalışma kitabı katıştırılmış web hizmeti API ve şema bilgilerini ile önceden biçimlendirilmiş gelir. Tıkladığınızda *Excel çalışma kitabını indirin*, çalışma kitabı açılır ve yerel bilgisayarınıza kaydedin. 

![Web Hizmetleri panodan Excel çalışma kitabını indirin](./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png)

Aşağıda gösterildiği gibi çalışma açık mavi parametresi bölüme önceden tanımlanmış parametrelerinizi kopyalayın. Parametreler girildikten sonra Excel için Machine Learning web hizmetini çağıran ve tahmin edilen puanlanmış etiketler yeşil tahmin edilen değerler bölümünde görüntülenir. Çalışma kitabı, eğitilen model parametreleri altında girilen tüm satır öğeleri için temel parametreleri için Öngörüler oluşturmak devam eder. Bu özelliğin nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [bir Azure Machine Learning Web hizmetini Excel'den kullanma](consuming-from-excel.md). 

![Şablon Excel çalışma kitabı dağıtılan web hizmetine bağlanma](./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png)

### <a name="optimization-and-further-experiments"></a>En iyi duruma getirme ve denemeler daha fazla
Excel modelimizi temel vardı, önceden sunduğumuz Machine Learning doğrusal regresyon modelinin en iyi duruma getirme geçtiğimizi. Modül kullandık [özellik seçimi süzgeç tabanlı][filter-based-feature-selection] bizim ilk veri seçimine göre iyileştirmek için öğeleri ve bunu bize bir performans geliştirmesinden %4.6 iyi Yardım Mean Absolute Error. İleride gerçekleştirilecek projeler için bize hafta doğru ortaklık model için kullanılacak özellikler kümesi bulmak için veri öznitelikleri üzerinden yineleme tasarruf bu özelliğini kullanacağız. 

Sonraki gibi ek algoritmaları içer planlıyoruz [Bayes][bayesian-linear-regression] or [Boosted Decision Trees][boosted-decision-tree-regression] bizim denemede performansını karşılaştırmak için. 

Regresyonla denemek istiyorsanız, denemek için iyi bir veri kümesi sayısal öznitelikler çok sayıda olan enerji verimliliğini regresyon örnek veri kümesi var. Veri kümesi Studio'da örnek veri kümelerini bir parçası olarak sağlanır. Öğrenme modülleri çeşitli ısıtma yük veya yük soğutma tahmin etmek için kullanabilirsiniz. Aşağıdaki grafik, enerji verimliliğini veri kümesi hedef değişkeni soğutma yük tahmin karşı farklı regresyon performans karşılaştırması öğrenir şöyledir: 

| Model | Mean Absolute Error | Kök ortalama karesi alınmış hata | Göreli mutlak hata | Göreli karesi alınmış hata | Katsayısı |
| --- | --- | --- | --- | --- | --- |
| Artırmalı karar ağacı |0.930113 |1.4239 |0.106647 |0.021662 |0.978338 |
| Doğrusal regresyon (gradyan düşüşü) |2.035693 |2.98006 |0.233414 |0.094881 |0.905119 |
| Sinir Ağı Regresyonu |1.548195 |2.114617 |0.177517 |0.047774 |0.952226 |
| Doğrusal regresyon (sıradan kareler) |1.428273 |1.984461 |0.163767 |0.042074 |0.957926 |

## <a name="key-takeaways"></a>Önemli dersler
Çok tarafından çalışan Excel regresyon ve Studio denemeleri paralel öğrendik. Temel modeli Excel'de oluşturma ve makine Öğrenimini kullanarak modelleriyle karşılaştırma [doğrusal regresyon][linear-regression] Yardım bize Studio öğrenin ve veri seçimi ve model performansı arttırmaya yönelik fırsatlar bulduk. 

Ayrıca kullanmak için önerilir bulduk [özellik seçimi süzgeç tabanlı][filter-based-feature-selection] gelecekteki tahmini projeleri hızlandırmak için. Özellik Seçimi verilerinize uygulayarak, daha iyi bir genel performansı ile geliştirilmiş bir model Studio'da oluşturabilirsiniz. 

Tahmine dayalı analiz Studio'dan Excel'e systemically tahmin taşıma imkanı önemli bir artış sonuçları geniş iş kullanıcının hedef kitlesine için başarılı bir şekilde sağlama olanağı sağlar. 

## <a name="resources"></a>Kaynaklar
Regresyon ile çalışmanıza yardımcı olacak bazı kaynaklar aşağıda verilmiştir: 

* Excel'de regresyon. Excel'de regresyon hiçbir zaman denediyseniz, Bu öğretici, kolaylaştırır: [https://www.excel-easy.com/examples/regression.html](https://www.excel-easy.com/examples/regression.html)
* Tahmin regresyon vs. Tyler Chessman serisi iyi bir başlangıç doğrusal regresyon açıklamasını içeren Excel'de tahmini süreyi açıklayan bir blog makalesi yazıldı. [https://www.itprotoday.com/sql-server/understanding-time-series-forecasting-concepts](https://www.itprotoday.com/sql-server/understanding-time-series-forecasting-concepts) 
* Sıradan az doğrusal regresyon Squares: Açıkları, sorunları ve zorlukları belirlemenizin. Bir giriş ve regresyon hakkında ayrıntılı bilgi için: [https://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ ](https://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/)

<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

