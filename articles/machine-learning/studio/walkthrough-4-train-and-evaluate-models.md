---
title: '4. Adım: Tahmine dayalı analiz modellerini eğitin'
titleSuffix: Azure Machine Learning Studio
description: "Adım 4 / geliştirme Tahmine dayalı çözüm Kılavuzu: Eğitme, Puanlama ve Azure Machine Learning Studio'da birden fazla modeli değerlendirin."
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: garyericson
ms.author: garye
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/23/2017
ms.openlocfilehash: 50b0a6276d34567651da2cb3e76f0686e8b10fea
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55511573"
---
# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models-in-machine-learning-studio"></a>Kılavuz adımı 4: Eğitme ve Machine Learning Studio'da Tahmine dayalı analiz modellerini değerlendirme
Bu konu örnek dördüncü adımı içeren [bir Azure Machine learning'de Tahmine dayalı analiz çözümü geliştirin](walkthrough-develop-predictive-solution.md)

1. [Machine Learning Studio çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md)
2. [Mevcut verileri yükleme](walkthrough-2-upload-data.md)
3. [Yeni bir deneme oluşturma](walkthrough-3-create-new-experiment.md)
4. **Modelleri eğitme ve değerlendirme**
5. [Web hizmetini dağıtma](walkthrough-5-publish-web-service.md)
6. [Web hizmetine erişme](walkthrough-6-access-web-service.md)

- - -
Makine öğrenimi modelleri oluşturmak için Azure Machine Learning Studio kullanmanın avantajlarından biri, bir zaman tek bir denemede birden fazla tür modelinin deneyin ve sonuçları karşılaştırmak için olanağıdır. Bu tür bir deneme sorununuz için en iyi çözüm bulmanıza yardımcı olur.

Biz bu örnekte geliştirmekte olduğunuz deneme biz modelleri iki farklı türde oluşturacak ve sonra kullanmak için bizim son deneme istiyoruz algoritmayı karar vermek için Puanlama sonuçlarını karşılaştırın.  

Biz seçebilirsiniz çeşitli modeli vardır. Kullanılabilir modelleri görmek için genişletin **Machine Learning** modül paletindeki düğümünü ve ardından **modeli Başlat** ve altındaki düğümleri. Bu deneyin amacı doğrultusunda, seçeneğini belirleyeceğiz [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] (SVM) ve [iki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modüller.    

> [!TIP]
> Çözmeye çalıştığınız belirli sorun hangi Machine Learning algoritmasını en iyi uyan karar vermeyle ilgili Yardım almak için bkz. [Microsoft Azure Machine Learning Studio için algoritma seçme](algorithm-choice.md).
> 
> 

## <a name="train-the-models"></a>Modelleri eğitme

Her ikisi de ekleyeceğiz [iki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modülü ve [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] bu denemede modülü.

### <a name="two-class-boosted-decision-tree"></a>İki sınıflı Artırmalı karar ağacı

İlk olarak, artırmalı karar ağacı modelini ayarlayalım.

1. Bulma [iki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modül paletindeki modülünü tuvale sürükleyin.

2. Bulma [modeli eğitme] [ train-model] modülünü tuvale sürükleyin ve ardından çıktısını bağlanın [iki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modülünün sol giriş bağlantı noktasına [modeli eğitme] [ train-model] modülü.
   
   [İki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modülü genel model başlatır ve [modeli eğitme] [ train-model] geliştirmek için eğitim verilerini kullanır. Model. 

3. Sol sol çıkışını [R betiği yürütme] [ execute-r-script] modülünün sağ giriş bağlantı noktasına [modeli eğitme] [ train-model] (verdik, modülü [3. adım](walkthrough-3-create-new-experiment.md) eğitim verileri bölme modülünün sol tarafındaki gelen verileri kullanmak için bu kılavuzun).
   
   > [!TIP]
   > İki giriş ve çıkışları ihtiyaç duymayacağımız [R betiği yürütme] [ execute-r-script] modülü için bu deneyde, böylece biz bunları eklenmemiş bırakabilirsiniz. 
   > 
   > 

Bu bölümü deneme şimdi aşağıdakine benzer:  

![Modeli][1]

Söyleyin ihtiyacımız artık [modeli eğitme] [ train-model] modülü model değeri kredi riskini tahmin etmek istiyoruz.

1. Seçin [modeli eğitme] [ train-model] modülü. İçinde **özellikleri** bölmesinde tıklayın **Sütun seçiciyi Başlat**.

2. İçinde **tek bir sütun** iletişim kutusunda, türü altında arama alanına "Kredi riski" **kullanılabilir sütunlar**, aşağıdaki "Kredi riski"'ı seçin ve sağ ok düğmesine tıklayın (**>**) "Kredi riski" taşımak için **seçili sütun**. 

    ![Train Model modülü için kredi riskini sütununu seçin][0]

3. Tıklayın **Tamam** işaretleyin.

### <a name="two-class-support-vector-machine"></a>Çift Sınıflı Destek Vektör Makinesi

Ardından, SVM modelini ayarlayın.  

İlk olarak, SVM hakkında biraz açıklama. Artırmalı karar ağaçları de özelliklerle herhangi bir türde çalışır. Ancak doğrusal bir sınıflandırıcı SVM modülü oluşturur olduğundan, tüm sayısal özellikleri aynı ölçek olduğunda ürettiği model en iyi test hatası vardır. Tüm sayısal özellikleri aynı ölçek dönüştürmek için biz "Tanh" dönüştürme kullanın (ile [Normalleştir veri] [ normalize-data] Modülü). Bu bizim numaraları [0,1] aralığa dönüştürür. Biz dize özellikleri el ile dönüştürme yapmak zorunda kalmazsınız SVM modülü kategorik özellikleri ve ardından ikili 0/1 özellikleri, dize özellikleri dönüştürür. Ayrıca, kredi riski sütun (sütun 21) dönüştürmek istemediğiniz - sayısal, ancak bu nedenle işlem yapmayın seçmeliyiz tahmin etmek için modeli eğitmek değerdir.  

SVM modeli'kurmak için aşağıdakileri yapın:

1. Bulma [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] modül paletindeki modülünü tuvale sürükleyin.

2. Sağ [modeli eğitme] [ train-model] modülü seçin **kopyalama**ve ardından seçin ve tuvalin sağ tıklayarak **Yapıştır**. Kopyasını [modeli eğitme] [ train-model] modülü orijinalle aynı sütun seçimi içeriyor.

3. Çıkışını [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] modülünün sol giriş bağlantı noktasına ikinci [modeli eğitme] [ train-model] modülü.

4. Bulma [Normalleştir veri] [ normalize-data] modülünü tuvale sürükleyin.

5. Sol sol çıkışını [R betiği yürütme] [ execute-r-script] modülü, bu modülün (bildirim için birden fazla modülü bir modülün çıkış bağlantı noktasına bağlı olduğunu) giriş.

6. Sol çıkış bağlantı noktasına bağlanmak [Normalleştir veri] [ normalize-data] modülünün sağ giriş bağlantı noktasına ikinci [modeli eğitme] [ train-model] modülü.

Bu bölümü, deneme aşağıdakine benzer görünmelidir:  

![İkinci modeli eğitimi][2]  

Şimdi Yapılandır [Normalleştir veri] [ normalize-data] Modülü:

1. Seçmek için tıklatın [Normalleştir veri] [ normalize-data] modülü. İçinde **özellikleri** bölmesinde **Tanh** için **dönüştürme yöntemi** parametresi.

2. Tıklayın **Sütun seçiciyi Başlat**, "Hiç sütun" seçin **şununla Başla**seçin **INCLUDE** ilk açılan menüyü seçin **sütun türü** ikinci açılır ve select **sayısal** üçüncü açılır. Bu, tüm sayısal sütunları (ve yalnızca sayısal) dönüştürülür belirtir.

3. Bu satır için artı işaretini (+) tıklayın; bunu açılan bir satır oluşturur. Seçin **hariç** ilk açılan menüyü seçin **sütun adları** ikinci açılır ve "risk metin alanına kredi" girin. Bu kredi riski sütun yoksayılıp yoksayılmaması gerektiğini belirtir (Bu sütunu sayısal olduğundan ve bu kapsam dışında tutarız alamadık, dönüştürülmesi böylece bunun ihtiyacımız).

4. Tıklayın **Tamam** işaretleyin.  

    ![Normalize veri modülü için sütunları seçin][5]

[Normalleştir veri] [ normalize-data] modülü kredi riski sütun dışındaki tüm sayısal sütunlarda Tanh dönüştürme gerçekleştirmek için şimdi ayarlanır.  

## <a name="score-and-evaluate-the-models"></a>Puanlama modelleri ve değerlendirme

Tarafından ayrıldı kullanıma test verilerini kullanacağız [verileri bölme] [ split] bizim eğitilen modellerini puanlamanıza olanak modülü. Biz, daha sonra daha iyi sonuçlar oluşturulduğu görmek için iki model sonuçlarını karşılaştırabilirsiniz.  

### <a name="add-the-score-model-modules"></a>Score Model Modül Ekle

1. Bulma [Score Model] [ score-model] modülünü tuvale sürükleyin.

2. Connect [modeli eğitme] [ train-model] bağlı Modülü [iki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modülünün sol giriş bağlantı noktasına [Score Model] [ score-model] modülü.

3. Sağa bağlanma [R betiği yürütme] [ execute-r-script] Modülü (test verilerimizi) sağ giriş bağlantı noktasına [Score Model] [ score-model] modülü.

    ![Score Model modülü bağlı][6]
   
   [Score Model] [ score-model] modülü artık test verileri kredi bilgiler alın, modeliyle çalıştırın ve modeli oluşturur gerçek kredi riski sütununda ile tahminleri karşılaştırın verileri test etme.

4. Kopyalama ve yapıştırma [Score Model] [ score-model] ikinci bir kopya oluşturmak için modül.

5. SVM modeli çıkışına bağlayın (diğer bir deyişle, çıkış bağlantı [modeli eğitme] [ train-model] bağlı Modülü [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] Modülü) ikinci giriş bağlantı noktasına [Score Model] [ score-model] modülü.

6. SVM modeli için eğitim verileri yaptığımız gibi test verisi için aynı dönüştürme yapmak sahibiz. Bu nedenle kopyalama ve yapıştırma [Normalleştir veri] [ normalize-data] ikinci bir kopyasını oluşturun ve sağa bağlamak için modül [R betiği yürütme] [ execute-r-script] modülü.

7. İkinci sol çıkışını [Normalleştir veri] [ normalize-data] modülünün sağ giriş bağlantı noktasına ikinci [Score Model] [ score-model] modülü.

    ![Bağlı her iki Model Puanlama Modülü][7]

### <a name="add-the-evaluate-model-module"></a>Model değerlendirme Modül Ekle

İki Puanlama sonuçları değerlendirin ve bunları karşılaştırmak için kullandığımız bir [Evaluate Model] [ evaluate-model] modülü.  

1. Bulma [Evaluate Model] [ evaluate-model] modülünü tuvale sürükleyin.

2. Çıkış bağlantı noktasına bağlanmak [Score Model] [ score-model] modülünün sol giriş bağlantı noktasına artırmalı karar ağacı modeli ile ilişkili [Evaluate Model] [ evaluate-model] modülü.

3. Diğer connect [Score Model] [ score-model] modülünün sağ giriş bağlantı noktasına.  

    ![Modeli değerlendirme bağlı Modülü][8]

### <a name="run-the-experiment-and-check-the-results"></a>Denemeyi çalıştırma ve sonuçları denetleyin

Denemeyi çalıştırmak için tıklayın **ÇALIŞTIRMA** tuvalin altındaki düğme. Bu işlem birkaç dakika sürebilir. Çalıştığını ve daha sonra yeşil bir onay işareti modülü tamamladığını gösterir. her modül bir dönen göstergesi gösterir. Tüm modüllerin bir onay işareti varsa, denemeyi çalışması sona erdi.

Deneme şimdi aşağıdakine benzer görünmelidir:  

![Her iki modeli değerlendirme][3]

Sonuçları denetlemek için çıkış bağlantı noktasına tıklayın [Evaluate Model] [ evaluate-model] modülü ve select **Görselleştir**.  

[Evaluate Model] [ evaluate-model] modül, eğriler ve iki puanlanmış modeli sonucunu karşılaştırmaya izin veren ölçümler bir çift oluşturur. Alıcı işleci özellikleri (ROC) eğrileri, duyarlık/geri çağırma eğriler ve Lift eğrileri olarak sonuçlarını görüntüleyebilirsiniz. Ek veri görüntülendiğini, bir karışıklık matrisi eğrinin (AUC) ve diğer ölçümleri alanında toplu değerlerini içerir. Kaydırıcıyı sola veya sağa hareket ettirerek eşik değerini değiştirebilir ve bu ölçüm kümesini nasıl etkilediğini görebilir.  

Graf sağında tıklayın **veri kümesi Puanlanmış** veya **karşılaştırmak için veri kümesi Puanlanmış** ilişkili eğri vurgulayın ve aşağıdaki ilişkili ölçümleri görüntülemek için. Eğri için göstergede "Puanlanmış veri kümesi" sol giriş bağlantı noktasına karşılık gelen [Evaluate Model] [ evaluate-model] modülü - örneğimizde artırmalı karar ağacı modeli budur. Sağ giriş bağlantı noktasına - SVM modeli bizim durumumuzda, "karşılaştırmak için veri kümesi puanlanmış" karşılık gelir. Bu etiketler birine tıkladığınızda, bu model için eğri vurgulanır ve aşağıdaki grafikte gösterildiği gibi ilgili ölçümleri görüntülenir.  

![Modelleri için ROC Eğriler][4]

Bu değerleri inceleyerek hangi Aradığınız sonuçları vermiş, en yakın modeldir karar verebilirsiniz. Geri dönün ve farklı modelleri değerlerde parametresi değiştirerek üzerinde denemenizi yineleme. 

Bilimi bu sonuçları yorumlayarak destek sağlama ve model performansını ayarlama resim olduğundan, bu kılavuzun kapsamı dışındadır. Ek Yardım için bu makaleleri okuyun:
- [Azure Machine learning'de model performansını değerlendirme](evaluate-model-performance.md)
- [Azure Machine Learning algoritmalarınızı iyileştirmek için parametreleri seçin](algorithm-parameters-optimize.md)
- [Azure Machine learning'de model sonuçlarını yorumlama](interpret-model-results.md)

> [!TIP]
> Denemeyi bu yineleme kaydını her çalıştırdığınızda, çalıştırma geçmişi tutulur. Bu yinelemeler görüntülemek ve herhangi biri, tıklayarak dönüş **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE** tuval aşağıda. Ayrıca **önceki çalıştırma** içinde **özellikleri** , hemen bir önceki yinelemeye döndürülecek bölmesi açık olması.
> 
> Tıklayarak, denemenizi herhangi bir yinelemesini kopyasını yapabilirsiniz **SAVE AS** tuval aşağıda. 
> Deneme kullanın **özeti** ve **açıklama** , deneme yinelemelerini denediyseniz, kaydını tutmak için özellikleri.
> 
> Daha fazla ayrıntı için [Azure Machine Learning Studio'da deneme yinelemelerini yönetme](manage-experiment-iterations.md).  
> 
> 

- - -
**Sonraki: [Web hizmetini dağıtma](walkthrough-5-publish-web-service.md)**

[0]: ./media/walkthrough-4-train-and-evaluate-models/train-model-select-column.png
[1]: ./media/walkthrough-4-train-and-evaluate-models/experiment-with-train-model.png
[2]: ./media/walkthrough-4-train-and-evaluate-models/svm-model-added.png
[3]: ./media/walkthrough-4-train-and-evaluate-models/final-experiment.png
[4]: ./media/walkthrough-4-train-and-evaluate-models/roc-curves.png
[5]: ./media/walkthrough-4-train-and-evaluate-models/normalize-data-select-column.png
[6]: ./media/walkthrough-4-train-and-evaluate-models/score-model-connected.png
[7]: ./media/walkthrough-4-train-and-evaluate-models/both-score-models-added.png
[8]: ./media/walkthrough-4-train-and-evaluate-models/evaluate-model-added.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
