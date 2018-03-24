---
title: Doğrusal regresyon Machine Learning kullanarak | Microsoft Docs
description: Excel'de ve Azure Machine Learning Studio'da doğrusal regresyon modellerin karşılaştırması
metakeywords: ''
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 417ae6ab-de4f-4bdd-957a-d96133234656
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: 2ea5a2720542217d3bb6a0a2b1309312fb74a953
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="using-linear-regression-in-azure-machine-learning"></a>Azure Machine Learning’de doğrusal regresyonu kullanma
> *Kate Baroni* ve *Ben Boatman* Microsoft'un veri Öngörüler mükemmel merkezi çözüm mimarları Kurumsal şunlardır. Bu makalede, bunlar Azure Machine Learning kullanarak bulut tabanlı bir çözüme varolan bir Regresyon çözümleme paketini geçirme deneyimlerini açıklanmaktadır. 
> 
> 

&nbsp; 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="goal"></a>Hedef
Projemizin iki hedefleri düşünerek kullanmaya: 

1. Bizim kuruluşun aylık gelir tahminleri doğruluğunu artırmak için Tahmine dayalı analiz kullanın 
2. Azure Machine Learning kullanma doğrulamak için en iyi duruma getirme, hız, artırma ve ölçek bizim sonuçları. 

Birçok işletme gibi kuruluşunuzun işlem tahmin aylık gelir gider. Küçük ekibimiz iş analistleri işlemini desteklemek ve tahmin doğruluğunu artırmak için Azure Machine Learning kullanarak görevli. Takım birden fazla kaynaktan veri toplama ve istatistiksel çözümleme hizmetleri satış tahmin ilgili kilit özniteliklerini tanımlayan aracılığıyla veri öznitelikleri çalıştıran birkaç ay harcanan. Excel'de veri prototipi oluşturulurken istatistiksel regresyon modellerinde başlamak için bir sonraki adım oluştu. Birkaç hafta içinde geçerli alan ve işlemleri tahmin Finans outperforming bir Excel regresyon modeli vardı. Bu temel tahmin sonuç hale geldi. 

Biz ardından sonraki adım için nasıl Machine Learning Tahmine dayalı üzerinde performansı çıkışı bulmak için Azure Machine Learning, Tahmine dayalı analiz taşıma geçen.

## <a name="achieving-predictive-performance-parity"></a>Tahmine dayalı performans eşlik elde
Machine Learning ve Excel regresyon modelleri arasında eşlik elde etmek için bizim birinci öncelik oluştu. Eğitim ve test verileri için verilen, aynı veri ve aynı bölünmüş, biz Excel ve Machine Learning arasındaki eşlik Tahmine dayalı performans elde etmek istedik. Başlangıçta alamadık. Excel modeli Machine Learning modelini outperformed. Machine Learning temel aracı ayarında anlayış eksikliği nedeniyle başarısız oldu. Machine Learning ürün ekibi ile bir eşitleme sonra size daha iyi bizim veri kümeleri için gerekli ayarlama taban anlamak kazanılan ve iki model arasındaki eşlik elde. 

### <a name="create-regression-model-in-excel"></a>Regresyon modeli Excel'de oluşturun
Bizim Excel regresyon Excel çözümleme araç bulunan standart doğrusal regresyon modeli kullanılır. 

Biz hesaplanan *ortalama mutlak % hata* ve model için performans ölçü olarak kullanılır. Excel kullanarak bir çalışma modeli gelmesi 3 ay sürdü. Biz sonuçta anlama gereksinimleri açısından yararlı olan Machine Learning Studio'da deneme içine öğrenme çoğunu duruma getirdi.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Azure Machine Learning ile karşılaştırılabilir deneme oluşturma
Bizim denemeyi Machine Learning Studio'da oluşturmak için bu adımları izlenen: 

1. Machine Learning Studio (çok küçük dosyası) bir csv dosyası olarak dataset karşıya
2. Yeni bir deneme oluşturulur ve kullanılan [Select Columns in Dataset sütun] [ select-columns] Excel'de kullanılan aynı veri özellikleri seçmek için Modülü 
3. Kullanılan [bölünmüş veri] [ split] Modülü (ile *göreli ifade* modu) Excel'de yapıldığı şekilde verileri aynı eğitim kümelerine ayırmak için 
4. İle experimented [doğrusal regresyon] [ linear-regression] Modülü (yalnızca varsayılan seçenekleri), belgelenmiş ve bizim Excel regresyon modeli sonuçları karşılaştırma

### <a name="review-initial-results"></a>İlk sonuçlarını gözden geçirin
İlk başta, Excel modeli açıkça Machine Learning Studio modeli outperformed: 

|  | Excel | Studio |
| --- |:---:|:---:|
| Performans | | |
| <ul style="list-style-type: none;"><li>R kare ayarlandı</li></ul> |0.96 |Yok |
| <ul style="list-style-type: none;"><li>Katsayısı <br />Belirleme</li></ul> |Yok |0.78<br />(düşük doğruluğu) |
| Mutlak hata anlama |$9. 5M |$ 19.4 M |
| Ortalama mutlak hata (%) |6.03% |12.2% |

Bizim işlemi ve sonuçları veri bilimcilerine ve geliştiricilere Machine Learning ekibi karşılaştık, hızlı bir şekilde bazı yararlı ipuçları sağladıkları. 

* Kullandığınızda [doğrusal regresyon] [ linear-regression] modülü Machine Learning Studio'da iki yöntem sağlanır:
  * Çevrimiçi gradyan düşüşü: büyük ölçekli sorunları için daha uygun olabilir
  * Sıradan kareler: Çoğu kişi doğrusal regresyon duyduğunuzda düşünmek yöntem budur. Küçük veri kümeleri için normal kareler daha uygun bir seçenek olabilir.
* Performansı artırmak için L2 Regularization ağırlığı parametresi uyguladıkça göz önünde bulundurun. 0,001 için varsayılan olarak ayarlanır, ancak bizim küçük veri kümesi için performansı artırmak için 0.005 için ayarlarız. 

### <a name="mystery-solved"></a>Çözülen sırrı!
Biz önerileri uygulandığında, Machine Learning Studio'daki Excel ile aynı temel performans elde: 

|  | Excel | Studio (Başlangıç) | Studio w/ Least Squares |
| --- |:---:|:---:|:---:|
| Etiketli değer |Fiili (sayı) |Aynı |Aynı |
| Öğrenen |Excel -> veri analizi regresyon -> |Doğrusal regresyon. |Doğrusal regresyon |
| Öğrenen seçenekleri |Yok |Varsayılan olarak |sıradan kareler<br />L2 0.005 = |
| Veri kümesi |26 satır, 3 özellikleri, 1 etiketi. Tüm sayısal. |Aynı |Aynı |
| Böl: eğitimi |İlk 18 satırda son 8 satırlarda test Excel eğitildi. |Aynı |Aynı |
| Split: Test |Son 8 satırlara uygulanan Excel regresyon formülü |Aynı |Aynı |
| **Performans** | | | |
| R kare ayarlandı |0.96 |Yok | |
| Katsayısı |Yok |0.78 |0.952049 |
| Mutlak hata anlama |$9. 5M |$ 19.4 M |$9. 5M |
| Ortalama mutlak hata (%) |<span style="background-color: 00FF00;"> 6.03%</span> |12.2% |<span style="background-color: 00FF00;"> 6.03%</span> |

Ayrıca, Excel katsayısını de Azure eğitilen modeli özellik ağırlıkları Karşılaştırılan:

|  | Excel katsayısını | Azure özellik ağırlıkları |
| --- |:---:|:---:|
| Intercept/sapması |19470209.88 |19328500 |
| Özelliği A |0.832653063 |0.834156 |
| Özellik B |11071967.08 |11007300 |
| Özellik C |25383318.09 |25140800 |

## <a name="next-steps"></a>Sonraki Adımlar
Excel'den Machine Learning web hizmeti kullanmak istedik. Bizim iş analistleri Excel kullanır ve bir şekilde bir Excel veri satırı ile Machine Learning web hizmetini çağırmak ve sahip tahmin edilen değer Excel'e gerektiğini. 

Biz de modelimizi, en iyi duruma getirme seçenekleri ve Machine Learning Studio'da kullanılabilen algoritmaları kullanarak istedik.

### <a name="integration-with-excel"></a>Excel ile tümleştirme
Bir web hizmeti eğitilen modeli oluşturarak bizim Machine Learning regresyon modeli faaliyete için bizim çözüm bulunuyordu. Birkaç dakika içinde web hizmeti oluşturuldu ve tahmin edilen gelir değerini döndürmek için doğrudan Excel'den diyoruz. 

*Web Hizmetleri Pano* bölüm indirilebilir bir Excel çalışma kitabı içerir. Çalışma kitabı katıştırılmış web hizmeti API ve şema bilgileri ile önceden biçimlendirilmiş gelir. Tıkladığınızda *karşıdan Excel çalışma kitabı*, çalışma kitabı açılır ve yerel bilgisayarınıza kaydedin. 

![][1]

Çalışma açık önceden tanımlanmış parametrelerinizi aşağıda gösterildiği gibi mavi parametre bölüme kopyalayın. Parametreleri girildikten sonra Excel için Machine Learning web hizmeti çağrıları ve tahmin edilen puanlanmış etiketler yeşil tahmin edilen değerler bölümünde görüntülenir. Çalışma kitabı, eğitilen model parametreleri altında girilen tüm satır öğeleri için temel parametreleri için tahminde oluşturmak devam eder. Bu özelliği kullanmak hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning Web hizmeti Excel'den tüketen](consuming-from-excel.md). 

![][2]

### <a name="optimization-and-further-experiments"></a>En iyi duruma getirme ve daha fazla denemeleri
Biz temel Excel modelimizi vardı, biz şimdi bizim Machine Learning doğrusal regresyon modeli en iyi duruma getirme taşındı. Modül kullandık [filtre tabanlı özellik seçimi] [ filter-based-feature-selection] ilk veri bizim seçimine artırmak için öğeleri ve bize performans geliştirmesi %4.6 elde Yardım ortalama mutlak hata. Model oluşturma için kullanılacak özellikleri doğru yetenek kümesini bulmak için veri öznitelikleri üzerinden yineleme bize hafta kaydedebilir bu özellik gelecekteki projeleri için kullanacağız. 

Ek algoritmalar gibi planlıyoruz sonraki [Bayesian] [ bayesian-linear-regression] veya [Boosted karar ağaçları] [ boosted-decision-tree-regression] performans karşılaştırmak için bizim deneme içinde. 

Regresyon ile denemek istiyorsanız, denemek için iyi bir veri kümesi çok sayıda sayısal özniteliklere sahip enerji verimliliği regresyon örnek veri kümesi ' dir. Veri kümesi, Machine Learning Studio'daki örnek veri kümesi bir parçası olarak sağlanır. Modülleri öğrenme çeşitli ısıtma yük ya da yük soğutma tahmin etmek için kullanabilirsiniz. Grafik, enerji verimliliğine dataset hedef değişkeni soğutma yükü için tahmin etmeye karşı farklı regresyon performans karşılaştırması öğrenir şöyledir: 

| Model | Mutlak hata anlama | Kök ortalama karesi alınmış hata | Göreli mutlak hata | Göreli karesi alınmış hata | Katsayısı |
| --- | --- | --- | --- | --- | --- |
| Artırılmış karar ağacı |0.930113 |1.4239 |0.106647 |0.021662 |0.978338 |
| Doğrusal regresyon (gradyan düşüşü) |2.035693 |2.98006 |0.233414 |0.094881 |0.905119 |
| Sinir ağı regresyon |1.548195 |2.114617 |0.177517 |0.047774 |0.952226 |
| Doğrusal regresyon (sıradan kareler) |1.428273 |1.984461 |0.163767 |0.042074 |0.957926 |

## <a name="key-takeaways"></a>Anahtar paketler
Biz çok tarafından çalışan Excel regresyon öğrenilen ve paralel olarak Azure Machine Learning denemelerini. Excel'de temel model oluşturma ve Machine Learning kullanarak modelleriyle karşılaştırma [doğrusal regresyon] [ linear-regression] Yardım biz veri seçimi ve model performansını artırmak için çeşitli fırsatlar bulunan ve bize Azure Machine Learning öğrenin. 

Ayrıca kullanmak için önerilir bulduk [filtre tabanlı özellik seçimi] [ filter-based-feature-selection] gelecekteki tahmin projeleri hızlandırmak için. Verileriniz için özellik seçimi uygulayarak, geliştirilmiş bir model Machine Learning ile daha iyi genel performansı oluşturabilirsiniz. 

Tahmine dayalı analitik Machine Learning Excel'e systemically tahmin aktarma yeteneği önemli bir artış başarıyla sonuçları bir geniş iş kullanıcı kitleye sağlama yeteneği verir. 

## <a name="resources"></a>Kaynaklar
Regresyon ile çalışmanıza yardımcı olacak bazı kaynaklar aşağıda verilmiştir: 

* Excel'de regresyon. Excel'de regresyon hiçbir zaman denediyseniz, Bu öğretici kolaylaştırır: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Tahmin regresyon vs. Tyler Chessman doğrusal regresyon iyi bir başlangıç açıklamasını içerir Excel'de tahmin serisi süreyi açıklayan bir blog makale yazıldı. [http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts) 
* Sıradan kareler doğrusal regresyon: Açıkları, sorunları ve Tuzaklar. Bir giriş ve regresyon tartışma için: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ ](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/)

[1]: ./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

