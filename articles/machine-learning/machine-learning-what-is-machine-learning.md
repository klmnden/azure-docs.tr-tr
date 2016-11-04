---
title: Microsoft Azure'da Machine Learning nedir?| Microsoft Docs
description: Bulutta makine öğrenimine ilişkin temel kavramları açıklar, bunu ne için kullanabileceğinizi anlatır ve makine öğrenimi terimlerini tanımlar.
keywords: makine öğrenimi nedir, makine öğrenimi terimleri,tahmine dayalı,tahmine dayalı analiz nedir,operasyonelleştirme
services: machine-learning
documentationcenter: ''
author: cjgronlund
manager: jhubbard
editor: cgronlun

ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: cgronlun;tedway;olgali

---
# Bulutta makine öğrenimine giriş
## Machine learning nedir?
Makine öğrenimi; bilgisayarların gelecekteki davranışları, sonuçları ve eğilimleri öngörmek amacıyla var olan verilerden ders çıkarmasına yardımcı olan bir veri bilimi tekniğidir.  

Machine learning'in bu öngörüleri veya tahminleri, uygulama ve cihazları daha akıllı hale getirir. Çevrimiçi alışveriş yaparken makine öğrenimi önceden satın aldıklarınızı temel alarak beğenebileceğiniz diğer ürünleri önermede yardımcı olur. Kredi kartınız makineden geçirildiğinde, makine öğrenimi işlemi bir işlem veritabanıyla karşılaştırır ve sahtekarlıkların saptanmasına yardımcı olur. Elektrikli süpürge robotunuz bir odayı temizlediğinde, makine öğrenimi robotunuzun işin tamamlanıp tamamlanmadığına karar vermesine yardımcı olur.

Kısa bir genel bakış için [Yeni Başlayanlar için Veri Bilimi](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) video serisini deneyin. Yeni Başlayanlar için Veri Bilimi, jargon veya matematik kullanmadan makine öğrenimini tanır ve basit bir tahmin modelini adım adım gösterir.

## Microsoft Azure bulutta Machine Learning nedir?
Azure Machine Learning, tahmine dayalı modelleri analiz çözümleri olarak hızlı bir şekilde oluşturmayı ve dağıtmayı mümkün kılan bulut tabanlı ve güçlü bir tahmine dayalı analiz hizmetidir. Makine öğrenimi denemeleri yapmak veya çözüm oluşturmak için Azure bulutu kullanırsanız pahalı donanımlar veya altyapı satın almanız gerekmez.

![Machine learning nedir? Azure Machine Learning'de tahmine dayalı analizleri faaliyete geçirmenin temel iş akışı.](./media/machine-learning-what-is-machine-learning/machine-learning-service-parts-and-workflow.png)

Azure Machine Learning tahmine dayalı analiz modellemeye yönelik araçlar sağlamanın yanı sıra, tahmine dayalı modellerinizi kullanıma hazır web hizmetleri olarak dağıtmada kullanabileceğiniz tam olarak yönetilen bir hizmet de sunar. Azure Machine Learning, tamamlanmış tahmine dayalı analiz çözümlerini bulutta oluşturmak için araçlar sağlar: Tahmine dayalı modelleri hızla oluşturun, test edin, faaliyete geçirin ve yönetin.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Tahmine dayalı analiz nedir?
Tahmine dayalı analiz, gelecekte gerçekleşecek olayları öngörmek için geçmişe dönük veya güncel verilerdeki desenleri ya da eğilimleri çözümleyen ve algoritma adı verilen çeşitli matematik formülleri kullanır.

Azure Machine Learning, tahmine dayalı analiz gerçekleştirmede özellikle güçlü bir yöntemdir: Kullanıma hazır algoritma kitaplığıyla çalışabilir, İnternet'e bağlı bir bilgisayarda algoritmalar kullanarak modeller oluşturabilir ve tahmine dayalı çözümünüzü hızla dağıtabilirsiniz. Hızla sonuç almak için [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/)’deki kullanıma hazır örnekler ve çözümlerle çalışmaya başlayın.

## Bulutta eksiksiz makine öğrenimi çözümleri oluşturma
Azure Machine Learning, büyük bir algoritma kitaplığından model oluşturmaya yönelik bir stüdyoya ve modelinizi bir web hizmeti olarak dağıtmanın kolay bir yoluna kadar, bulutta tahmine dayalı analiz çözümleri oluşturmada ihtiyacınız olan her şeyi içerir.

### Machine Learning Studio: Tahmine dayalı modeller oluşturma
[Machine Learning Studio](machine-learning-what-is-ml-studio.md)'da modülleri sürükleyerek, bırakarak ve bağlayarak hızla tahmine dayalı modeller oluşturabilirsiniz. Farklı birleşimlerle denemeler yapmak kolaydır ve [ücretsiz denersiniz](https://studio.azureml.net/?selectAccess=true&o=2).

![Tahmine dayalı analiz nedir? Azure Machine Learning Studio'da tahmine dayalı bir analiz denemesi örneği](./media/machine-learning-what-is-machine-learning/azure-machine-learning-studio-predictive-score-experiment.png)

* [Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md)’de başkaları tarafından yazılan analiz çözümlerini deneyebilir veya kendi çözümlerinizle katkı sağlayabilirsiniz. Denemeler hakkında topluluğa soru veya yorum gönderin ya da LinkedIn ve Twitter gibi sosyal ağlar aracılığıyla deneme bağlantılarını paylaşın.
* Tahmine dayalı modellerinizi hızla başlatmak için Machine Learning Studio'daki büyük [Machine Learning algoritmaları ve modülleri](https://msdn.microsoft.com/library/azure/f5c746fd-dcea-4929-ba50-2a79c4c067d7) kitaplığını kullanın. Örnek denemeler, R ve Python paketleri, Xbox ve Bing gibi Microsoft işletmelerinden en iyi sınıf algoritmalar arasından seçim yapın. Özel [R](machine-learning-r-quickstart.md) ve [Python](machine-learning-execute-python-scripts.md) betiklerinizle Studio modüllerini genişletin.

    ![Azure Cortana Intelligence Galerisi'nde tahmine dayalı denemeleri deneyin veya kendinizinkini paylaşın.](./media/machine-learning-what-is-machine-learning/machine-learning-cortana-intelligence-gallery.png)

### Tahmine dayalı analiz çözümlerini faaliyete geçirme: Web hizmetleri satın alma veya kendinizinkini yayımlama
* [Microsoft Azure Market](https://datamarket.azure.com/browse?query=machine+learning)'ten Öneriler, Metin Analizleri ve Anomali Algılama gibi kullanıma hazır web hizmetlerini satın alın.
* Tahmine dayalı analiz modellerinizi faaliyete geçirme:
  
  * [Web hizmetlerini dağıtma](machine-learning-publish-a-machine-learning-web-service.md)
  * [API'ler aracılığıyla modelleri eğitme ve yeniden eğitme](machine-learning-retrain-models-programmatically.md)
  * [Web hizmeti uç noktalarını yönetme](machine-learning-create-endpoint.md)
  * [Web hizmetini ölçeklendirme](machine-learning-scaling-webservice.md)
  * [Web hizmetlerini kullanma](machine-learning-consume-web-services.md)

## Önemli makine öğrenimi terimleri ve kavramları
Makine öğrenimi terimleri kafanızı karıştırabilir. Size yardımcı olmak açısından önemli terimlerin tanımları aşağıda verilmiştir. Tanımlanmasını istediğiniz başka terim varsa aşağıdaki yorumları kullanarak bize bildirebilirsiniz.

### Veri keşfi, açıklayıcı analiz ve tahmine dayalı analiz
**Veri keşfi**, odaklı analiz için özellikler bulmak amacıyla büyük ve genellikle yapılandırılmamış veri kümeleri hakkında bilgi toplama işlemidir. **Veri madenciliği**, otomatik veri keşfine başvurur.

**Açıklayıcı analiz**, ne olduğunu özetlemek için bir veri kümesini analiz etme işlemidir. Satış raporları, web ölçümleri ve sosyal ağ analizleri gibi iş analizlerinin büyük bir çoğunluğu açıklayıcıdır.

**Tahmine dayalı analiz**, gelecekteki sonuçları öngörmek için geçmiş veya geçerli verilerden model oluşturma işlemidir.

### Denetimli ve denetimsiz öğrenme
 **Denetimli öğrenme** algoritmaları etiketli verilerle, başka bir deyişle istenen yanıtların örneklerinden oluşan verilerle eğitilir. Örneğin, sahte kredi kartı kullanımını tanımlayan bir model, bilinen sahte ve geçerli ödemelerin veri noktalarıyla etiketlenmiş bir veri kümesinden eğitilir. Machine learning denetimlidir.

 **Denetimsiz öğrenme** hiçbir etiketi olmayan verilerde kullanılır ve amacı verilerdeki ilişkileri bulmaktır. Örneğin, benzer satın alma alışkanlıklarına sahip müşteri demografisi gruplandırmalarını bulmak isteyebilirsiniz.

### Model eğitimi ve değerlendirmesi
Makine öğrenimi modeli, yanıtlamaya çalıştığınız sorunun veya tahmin etmek istediğiniz sonucun bir soyutlamasıdır. Modeller, var olan verilerle eğitilir ve değerlendirilir.

#### Eğitim verileri
Bir modeli verilerle eğittiğinizde, bilinen bir veri kümesini kullanırsınız ve en doğru yanıta ulaşmak için gerekli veri özelliklerine bağlı olarak modelde değişiklikler yaparsınız. Azure Machine Learning'de bir model, eğitim verilerini ve puanlama modülü gibi işlev modüllerini işleyen bir algoritma modülünden oluşturulur.

Denetimli öğrenmede bir sahtekarlık algılama modelini eğitiyorsanız sahte veya geçerli olarak etiketlenmiş bir işlem kümesini kullanırsınız. Verilerinizi rastgele ayırıp bir kısmını modeli eğitmek, bir kısmını da modeli test etmek veya değerlendirmek için kullanırsınız.

#### Değerlendirme verileri
Bir modeli eğittikten sonra, kalan test verilerini kullanarak modeli değerlendirin. Modelinizin doğru tahmin yapıp yapmadığını anlamanız için sonuçlarını zaten bildiğiniz verileri kullanın.

## Diğer ortak makine öğrenimi terimleri
* **algoritma**: Veri işleme, matematik veya otomatik mantık aracılığıyla sorunları çözmek için kullanılan bağımsız bir kural kümesi.
* **anormallik algılama**: Sıra dışı olayları veya değerleri işaretleyen ve sorunları keşfetmenize yardımcı olan bir model. Örneğin, kredi kartı sahtekarlığı algılama modeli sıra dışı satın alama işlemlerini algılar.
* **kategorik veri**: Kategorilere göre düzenlenmiş ve gruplara bölünebilen veriler. Örneğin, otomobiller için kategorik bir veri kümesi yıl, marka, model ve fiyatı belirtebilir.
* **sınıflandırma**: Veri noktalarını, kategori gruplandırmalarının önceden bilindiği bir veri kümesini temel alarak kategorilere göre düzenlemeye yönelik bir model.
* **özellik mühendisliği**: Veri kümesini iyileştirmek ve sonuçları geliştirmek amacıyla bir veri kümesiyle ilgili özellikleri ayıklama veya seçme işlemi. Örneğin, uçak bileti ücreti verileri, haftanın günleri ve tatil günlerine göre iyileştirilebilir. Bkz. [Azure Machine Learning'de özellik seçimi ve özellik mühendisliği](machine-learning-feature-selection-and-engineering.md).
* **modül**: Machine Learning Studio'da, küçük veri kümelerini girmeyi ve düzenlemeyi sağlayan Veri Girme modülü gibi işlevsel bir parçadır. Bir algoritma da Machine Learning Studio'da bir modül türüdür.
* **model**: Denetimli öğrenme modeli; bir eğitim veri kümesi, bir algoritma modülü ve Score Model (Puan Modeli) modülü gibi işlevsel modüllerden oluşan bir makine öğrenimi denemesinin ürünüdür.
* **sayısal veri**: Ölçümler (sürekli veriler) veya sayımlar (ayrık veriler) olarak anlam ifade eden veriler. *Nicel veri* olarak da adlandırılır.
* **partition**: Verileri örneklere bölme yöntemi. Daha fazla bilgi için bkz. [Bölüm ve Örnek](https://msdn.microsoft.com/library/azure/dn905960.aspx) 
* **tahmin**: Tahmin, makine öğrenimi modeline ilişkin bir değerin veya değerlerin öngörüsüdür. "Tahmin edilen puan" terimini de görebilirsiniz. Ancak, tahmin edilen puanlar bir modelin son çıktısı değildir. Puanın ardından modelin değerlendirmesi gelir.
* **regresyon**: Bir araba fiyatını yılını ve markasını baz alarak tahmin etme gibi bağımsız değişkenlere bağlı bir değeri tahmin etmeye yönelik bir model.
* **puan**: Machine Learning Studio'daki [Score Model (Model Puanlama) modülü](https://msdn.microsoft.com/library/azure/dn905995.aspx) kullanılarak eğitilmiş bir sınıflandırma veya regresyon modelinden oluşturulan tahmin edilen bir değer. Sınıflandırma modelleri, tahmin edilen değerin olasılığı için de bir puan döndürür. Bir modelden puan oluşturduktan sonra, [Evaluate Model (Model Değerlendirme) modülünü](https://msdn.microsoft.com/library/azure/dn905915.aspx) kullanarak modelin doğruluğunu değerlendirebilirsiniz.
* **örnek**: Bütünü temsil etmesi hedeflenen bir veri kümesinin bir parçası. Örnekler, rastgele veya veri kümesinin belirli özellikleri temel alınarak seçilebilir.

## Sonraki adımlar
Tahmine dayalı analizin ve makine öğreniminin temellerini, [adım adım öğretici](machine-learning-create-experiment.md) kullanarak ve [örnekler üzerinden giderek](machine-learning-sample-experiments.md) öğrenebilirsiniz.  

<!-- Module References -->
[learning-with-counts]: https://msdn.microsoft.com/library/azure/81c457af-f5c0-4b2d-922c-fdef2274413c/



<!--HONumber=Oct16_HO2-->


