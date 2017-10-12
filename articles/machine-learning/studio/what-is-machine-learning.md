---
title: Azure'da Machine Learning nedir? | Microsoft Belgeleri
description: "Bulutta makine öğrenimine ilişkin temel kavramları açıklar, bunu ne için kullanabileceğinizi anlatır ve makine öğrenimi terimlerini tanımlar."
keywords: "makine öğrenimi nedir, makine öğrenimi terimleri,tahmine dayalı,tahmine dayalı analiz nedir,makine öğrenimi terimleri"
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: eaee083e-eaa1-4408-838b-93e51423d159
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: cgronlun;tedway;olgali
ms.openlocfilehash: 38c5f722029c80d7e61039ebe05346b345573e34
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-machine-learning-in-the-azure-cloud"></a>Azure bulutta Machine Learning’e giriş

## <a name="what-is-machine-learning"></a>Machine learning nedir?
Makine öğrenimi; bilgisayarların var olan verileri kullanarak gelecekteki davranışları, sonuçları ve eğilimleri öngörmelerini sağlayan bir veri bilimi tekniğidir. Bilgisayarlar, makine öğrenimini kullanarak açıkça programlamaya gerek kalmadan öğrenir.

Makine öğreniminin öngörüleri veya tahminleri, uygulama ve cihazları daha akıllı hale getirir. Çevrimiçi alışveriş yaparken makine öğrenimi önceden satın aldıklarınızı temel alarak beğenebileceğiniz diğer ürünleri önermede yardımcı olur. Kredi kartınız makineden geçirildiğinde, makine öğrenimi işlemi bir işlem veritabanıyla karşılaştırır ve sahtekarlıkların saptanmasına yardımcı olur. Elektrikli süpürge robotunuz bir odayı temizlediğinde, makine öğrenimi robotunuzun işin tamamlanıp tamamlanmadığına karar vermesine yardımcı olur.

Kısa bir genel bakış için [Yeni Başlayanlar için Veri Bilimi](data-science-for-beginners-the-5-questions-data-science-answers.md) video serisini deneyin. Yeni Başlayanlar için Veri Bilimi, jargon veya matematik kullanmadan makine öğrenimini tanır ve basit bir tahmin modelini adım adım gösterir.

## <a name="what-is-machine-learning-in-the-microsoft-azure-cloud"></a>Microsoft Azure bulutta Machine Learning nedir?
Azure Machine Learning, tahmine dayalı modelleri analiz çözümleri olarak hızlı bir şekilde oluşturmayı ve dağıtmayı mümkün kılan bulut tabanlı ve tahmine dayalı analiz hizmetidir.

Kullanıma hazır algoritma kitaplığıyla çalışabilir, bunları kullanarak internete bağlı bir bilgisayarda model oluşturabilir ve tahmine dayalı çözümünüzü hızlıca dağıtabilirsiniz. [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/)’deki kullanıma hazır örnekler ve çözümlerle çalışmaya başlayın.

![Machine learning nedir? Azure Machine Learning'de tahmine dayalı analizleri faaliyete geçirmenin temel iş akışı.](./media/what-is-machine-learning/machine-learning-service-parts-and-workflow.png)

Azure Machine Learning tahmine dayalı analiz modellemeye yönelik araçlar sağlamanın yanı sıra, tahmine dayalı modellerinizi kullanıma hazır web hizmetleri olarak dağıtmada kullanabileceğiniz tam olarak yönetilen bir hizmet de sunar.

## <a name="what-is-predictive-analytics"></a>Tahmine dayalı analiz nedir?
Tahmine dayalı analiz, gelecekte gerçekleşecek olayları öngörmek için geçmişe dönük veya güncel verilerdeki desenleri ya da eğilimleri çözümleyen ve algoritma adı verilen matematik formülleri kullanır.

## <a name="tools-to-build-complete-machine-learning-solutions-in-the-cloud"></a>Bulutta eksiksiz makine öğrenimi çözümleri oluşturma araçları
Azure Machine Learning, büyük bir algoritma kitaplığından model oluşturmaya yönelik bir stüdyoya ve modelinizi bir web hizmeti olarak dağıtmanın kolay bir yoluna kadar, bulutta kapsamlı tahmine dayalı analiz çözümleri oluşturmada ihtiyacınız olan her şeyi içerir. Tahmine dayalı modelleri hızlıca oluşturun, test edin, faaliyete geçirin ve yönetin.

### <a name="machine-learning-studio-create-predictive-models"></a>Machine Learning Studio: Tahmine dayalı modeller oluşturma
[Machine Learning Studio](what-is-ml-studio.md)'da modülleri sürükleyerek, bırakarak ve bağlayarak hızla tahmine dayalı modeller oluşturabilirsiniz. Farklı birleşimlerle denemeler yapabilir ve [ücretsiz deneyebilirsiniz](https://studio.azureml.net/?selectAccess=true&o=2).

* [Cortana Intelligence Gallery](gallery-how-to-use-contribute-publish.md)’de başkaları tarafından yazılan analiz çözümlerini deneyebilir veya kendi çözümlerinizle katkı sağlayabilirsiniz. Denemeler hakkında topluluğa soru veya yorum gönderin ya da LinkedIn ve Twitter gibi sosyal ağlar aracılığıyla deneme bağlantılarını paylaşın.

  ![Azure Cortana Intelligence Galerisi'nde tahmine dayalı denemeleri deneyin veya kendinizinkini paylaşın.](./media/what-is-machine-learning/machine-learning-cortana-intelligence-gallery.png)
* Tahmine dayalı modellerinizi hızla başlatmak için Machine Learning Studio'daki büyük [Machine Learning algoritmaları ve modülleri](https://msdn.microsoft.com/library/azure/f5c746fd-dcea-4929-ba50-2a79c4c067d7) kitaplığını kullanın. Örnek denemeler, R ve Python paketleri, Xbox ve Bing gibi Microsoft işletmelerinden en iyi sınıf algoritmalar arasından seçim yapın. Özel [R](extend-your-experiment-with-r.md) ve [Python](execute-python-scripts.md) betiklerinizle Studio modüllerini genişletin.

  ![Tahmine dayalı analiz nedir? Azure Machine Learning Studio'da tahmine dayalı bir analiz denemesi örneği](./media/what-is-machine-learning/azure-machine-learning-studio-predictive-score-experiment.png)

### <a name="operationalize-predictive-analytics-solutions-by-publishing-your-own"></a>Tahmine dayalı analiz çözümlerini faaliyete geçirmek için kendi çözümünüzü yayımlayın
Aşağıdaki öğreticiler, tahmine dayalı analiz modellerinizi nasıl faaliyete geçireceğinizi göstermektedir:

 * [Web hizmetlerini dağıtma](publish-a-machine-learning-web-service.md)
 * [API'ler aracılığıyla modelleri yeniden eğitme](retrain-models-programmatically.md)
 * [Web hizmeti uç noktalarını yönetme](create-endpoint.md)
 * [Bir web hizmetini ölçeklendirme](scaling-webservice.md)
 * [Web hizmetlerini kullanma](consume-web-services.md)

## <a name="key-machine-learning-terms-and-concepts"></a>Önemli makine öğrenimi terimleri ve kavramları
Makine öğrenimi terimleri kafanızı karıştırabilir. Size yardımcı olmak açısından önemli terimlerin tanımları aşağıda verilmiştir. Tanımlanmasını istediğiniz başka terim varsa aşağıdaki yorumları kullanarak bize bildirebilirsiniz.

### <a name="data-exploration-descriptive-analytics-and-predictive-analytics"></a>Veri keşfi, açıklayıcı analiz ve tahmine dayalı analiz

**Veri keşfi**, odaklı analiz için özellikler bulmak amacıyla büyük ve genellikle yapılandırılmamış veri kümeleri hakkında bilgi toplama işlemidir.

**Veri madenciliği**, otomatik veri keşfine başvurur.

**Açıklayıcı analiz**, ne olduğunu özetlemek için bir veri kümesini analiz etme işlemidir. Satış raporları, web ölçümleri ve sosyal ağ analizleri gibi iş analizlerinin büyük bir çoğunluğu açıklayıcıdır.

**Tahmine dayalı analiz**, gelecekteki sonuçları öngörmek için geçmiş veya geçerli verilerden model oluşturma işlemidir.

### <a name="supervised-and-unsupervised-learning"></a>Denetimli ve denetimsiz öğrenme
 **Denetimli öğrenme** algoritmaları etiketli verilerle, başka bir deyişle istenen yanıtların örneklerinden oluşan verilerle eğitilir. Örneğin, sahte kredi kartı kullanımını tanımlayan bir model, bilinen sahte ve geçerli ödemelerin veri noktalarıyla etiketlenmiş bir veri kümesinden eğitilir. Machine learning denetimlidir.

 **Denetimsiz öğrenme** hiçbir etiketi olmayan verilerde kullanılır ve amacı verilerdeki ilişkileri bulmaktır. Örneğin, benzer satın alma alışkanlıklarına sahip müşteri demografisi gruplandırmalarını bulmak isteyebilirsiniz.

### <a name="model-training-and-evaluation"></a>Model eğitimi ve değerlendirmesi
Makine öğrenimi modeli, yanıtlamaya çalıştığınız sorunun veya tahmin etmek istediğiniz sonucun bir soyutlamasıdır. Modeller, var olan verilerle eğitilir ve değerlendirilir.

#### <a name="training-data"></a>Eğitim verileri
Bir modeli verilerle eğittiğinizde, bilinen bir veri kümesini kullanırsınız ve en doğru yanıta ulaşmak için gerekli veri özelliklerine bağlı olarak modelde değişiklikler yaparsınız. Azure Machine Learning'de bir model, eğitim verilerini ve puanlama modülü gibi işlev modüllerini işleyen bir algoritma modülünden oluşturulur.

Denetimli öğrenmede bir sahtekarlık algılama modelini eğitiyorsanız sahte veya geçerli olarak etiketlenmiş bir işlem kümesini kullanırsınız. Verilerinizi rastgele ayırıp bir kısmını modeli eğitmek, bir kısmını da modeli test etmek veya değerlendirmek için kullanırsınız.

#### <a name="evaluation-data"></a>Değerlendirme verileri
Bir modeli eğittikten sonra, kalan test verilerini kullanarak modeli değerlendirin. Modelinizin doğru tahmin yapıp yapmadığını anlamanız için sonuçlarını zaten bildiğiniz verileri kullanın.

## <a name="other-common-machine-learning-terms"></a>Diğer ortak makine öğrenimi terimleri
* **algoritma**: Veri işleme, matematik veya otomatik mantık aracılığıyla sorunları çözmek için kullanılan bağımsız bir kural kümesi.
* **anormallik algılama**: Sıra dışı olayları veya değerleri işaretleyen ve sorunları keşfetmenize yardımcı olan bir model. Örneğin, kredi kartı sahtekarlığı algılama modeli sıra dışı satın alama işlemlerini algılar.
* **kategorik veri**: Kategorilere göre düzenlenmiş ve gruplara bölünebilen veriler. Örneğin, otomobiller için kategorik bir veri kümesi yıl, marka, model ve fiyatı belirtebilir.
* **sınıflandırma**: Veri noktalarını, kategori gruplandırmalarının önceden bilindiği bir veri kümesini temel alarak kategorilere göre düzenlemeye yönelik bir model.
* **özellik mühendisliği**: Veri kümesini iyileştirmek ve sonuçları geliştirmek amacıyla bir veri kümesiyle ilgili özellikleri ayıklama veya seçme işlemi. Örneğin, uçak bileti ücreti verileri, haftanın günleri ve tatil günlerine göre iyileştirilebilir. Bkz. [Azure Machine Learning'de özellik seçimi ve özellik mühendisliği](../team-data-science-process/create-features.md).
* **modül**: Machine Learning Studio'da, küçük veri kümelerini girmeyi ve düzenlemeyi sağlayan Veri Girme modülü gibi işlevsel bir parçadır. Bir algoritma da Machine Learning Studio'da bir modül türüdür.
* **model**: Denetimli öğrenme modeli; bir eğitim veri kümesi, bir algoritma modülü ve Score Model (Puan Modeli) modülü gibi işlevsel modüllerden oluşan bir makine öğrenimi denemesinin ürünüdür.
* **sayısal veri**: Ölçümler (sürekli veriler) veya sayımlar (ayrık veriler) olarak anlam ifade eden veriler. *Nicel veri* olarak da adlandırılır.
* **partition**: Verileri örneklere bölme yöntemi. Daha fazla bilgi için bkz. [Bölüm ve Örnek](https://msdn.microsoft.com/library/azure/dn905960.aspx) 
* **tahmin**: Tahmin, makine öğrenimi modeline ilişkin bir değerin veya değerlerin öngörüsüdür. "Tahmin edilen puan" terimini de görebilirsiniz. Ancak, tahmin edilen puanlar bir modelin son çıktısı değildir. Puanın ardından modelin değerlendirmesi gelir.
* **regresyon**: Bir araba fiyatını yılını ve markasını baz alarak tahmin etme gibi bağımsız değişkenlere bağlı bir değeri tahmin etmeye yönelik bir model.
* **puan**: Machine Learning Studio'daki [Score Model (Model Puanlama) modülü](https://msdn.microsoft.com/library/azure/dn905995.aspx) kullanılarak eğitilmiş bir sınıflandırma veya regresyon modelinden oluşturulan tahmin edilen bir değer. Sınıflandırma modelleri, tahmin edilen değerin olasılığı için de bir puan döndürür. Bir modelden puan oluşturduktan sonra, [Evaluate Model (Model Değerlendirme) modülünü](https://msdn.microsoft.com/library/azure/dn905915.aspx) kullanarak modelin doğruluğunu değerlendirebilirsiniz.
* **örnek**: Bütünü temsil etmesi hedeflenen bir veri kümesinin bir parçası. Örnekler, rastgele veya veri kümesinin belirli özellikleri temel alınarak seçilebilir.

## <a name="next-steps"></a>Sonraki adımlar
Tahmine dayalı analizin ve makine öğreniminin temellerini, [adım adım öğretici](create-experiment.md) kullanarak ve [örnekler üzerinden giderek](sample-experiments.md) öğrenebilirsiniz.  

<!-- Module References -->
[learning-with-counts]: https://msdn.microsoft.com/library/azure/81c457af-f5c0-4b2d-922c-fdef2274413c/
