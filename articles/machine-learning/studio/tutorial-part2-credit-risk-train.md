---
title: '2. Öğretici: Kredi riski modellerini eğitin'
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio'da bir kredi riski değerlendirmesi için Tahmine dayalı analiz çözümü oluşturmayı gösteren ayrıntılı bir öğretici. Bu öğreticide iki üç bölümlü öğretici serisinin bir parçasıdır. Bu eğitme ve değerlendirme modelleri nasıl gösterir.
keywords: kredi riski, tahmine dayalı analiz çözümü, risk değerlendirmesi
author: sdgilley
ms.author: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: tutorial
ms.date: 02/11/2019
ms.openlocfilehash: 45407e183c70fe67e6bd59e3fd86a50a31844c47
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60735700"
---
# <a name="tutorial-2-train-credit-risk-models---azure-machine-learning-studio"></a>2. Öğretici: Kredi riski modelleri - Azure Machine Learning Studio'da eğitim

Bu öğreticide, bir Tahmine dayalı analiz çözümü geliştirme işleminin genişletilmiş bir göz atın. Machine Learning Studio'da basit bir model geliştirmektir.  Ardından bir Azure Machine Learning web hizmeti olarak modeli dağıtacağız.  Bu dağıtılmış modelin yeni verileri kullanarak tahmin yapabileceği. Bu öğretici **üç bölümlü öğretici serisinin İkinci bölüm**.

Bir kişinin kredi başvurusunda verdiği bilgilere dayanarak kredi riskini tahmin etmeniz gerektiğini varsayalım.  

Kredi riski değerlendirmesi karmaşık bir sorundur ancak bu öğreticide, bir bit basitleştirir. Microsoft Azure Machine Learning Studio'yu kullanarak Tahmine dayalı analiz çözümü nasıl oluşturabileceğinize dair bir örnek olarak kullanacaksınız. Azure Machine Learning Studio ve Machine Learning web hizmeti bu çözüm için kullanacaksınız.  

Üç bölümü olan Bu öğreticide, genel kullanıma açık kredi risk verileriyle başlayın.  Ardından geliştirin ve Tahmine dayalı bir model eğitip.  Son olarak modeli bir web hizmeti olarak dağıtalım.

İçinde [öğreticinin birinci kısmında](tutorial-part1-credit-risk.md), bir Machine Learning Studio çalışma alanı oluşturulduğunda, veri karşıya yüklendi ve bir deneme oluşturulur.

Öğreticinin bu bölümünde:
 
> [!div class="checklist"]
> * Birden çok modeli eğitme
> * Puanlama modelleri ve değerlendirme


İçinde [öğreticinin üçüncü bölüm](tutorial-part3-credit-risk-deploy.md), bir web hizmeti olarak modeli dağıtacaksınız.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


## <a name="prerequisites"></a>Önkoşullar

Tam [öğreticinin birinci kısmında](tutorial-part1-credit-risk.md).

## <a name="train"></a>Birden çok modeli eğitme

Makine öğrenimi modelleri oluşturmak için Azure Machine Learning Studio kullanmanın avantajlarından biri, bir zaman tek bir denemede birden fazla tür modelinin deneyin ve sonuçları karşılaştırmak için olanağıdır. Bu tür bir deneme sorununuz için en iyi çözüm bulmanıza yardımcı olur.

Biz bu öğreticide, geliştirmekte olduğunuz deneme iki farklı türde modeller oluşturun ve sonra son bizim deneme kullanmak istediğiniz algoritmayı karar vermek için Puanlama sonuçlarını karşılaştırın.  

Tercih edebilirsiniz çeşitli modeli vardır. Kullanılabilir modelleri görmek için genişletin **Machine Learning** modül paletindeki düğümünü ve ardından **modeli Başlat** ve altındaki düğümleri. Bu deneyin amacı doğrultusunda, seçersiniz [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] (SVM) ve [iki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modüller.

> [!TIP]
> Çözmeye çalıştığınız belirli sorun hangi Machine Learning algoritmasını en iyi uyan karar vermeyle ilgili Yardım almak için bkz. [Microsoft Azure Machine Learning Studio için algoritma seçme](algorithm-choice.md).
> 
> 

Her ikisi de ekleyeceksiniz [iki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modülü ve [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] bu denemede Modülü .

### <a name="two-class-boosted-decision-tree"></a>İki sınıflı Artırmalı karar ağacı

İlk olarak, artırmalı karar ağacı modelini ayarlayın.

1. Bulma [iki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modül paletindeki modülünü tuvale sürükleyin.

1. Bulma [modeli eğitme] [ train-model] modülünü tuvale sürükleyin ve ardından çıktısını bağlanın [iki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modülünün sol giriş bağlantı noktasına [modeli eğitme] [ train-model] modülü.
   
   [İki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modülü genel model başlatır ve [modeli eğitme] [ train-model] geliştirmek için eğitim verilerini kullanır. Model. 

1. Sol sol çıkışını [R betiği yürütme] [ execute-r-script] modülünün sağ giriş bağlantı noktasına [modeli eğitme] [ train-model] modülde (Bu öğretici [sol tarafındaki gelen verileri kullanılan](#train) verileri bölme modülünün eğitim için).
   
   > [!TIP]
   > iki giriş ve çıkışları gerekmeyen [R betiği yürütme] [ execute-r-script] bunları eklenmemiş bırakabilirsiniz şekilde bu deneme için modülü. 
   > 
   > 

Bu bölümü deneme şimdi aşağıdakine benzer:  

![Modeli](./media/tutorial-part2-credit-risk-train/experiment-with-train-model.png)

Artık bildirmek gerekiyor [modeli eğitme] [ train-model] model değeri kredi riskini tahmin etmek istediğiniz modülü.

1. Seçin [modeli eğitme] [ train-model] modülü. İçinde **özellikleri** bölmesinde tıklayın **Sütun seçiciyi Başlat**.

1. İçinde **tek bir sütun** iletişim kutusunda, türü altında arama alanına "Kredi riski" **kullanılabilir sütunlar**, aşağıdaki "Kredi riski"'ı seçin ve sağ ok düğmesine tıklayın (**>**) "Kredi riski" taşımak için **seçili sütun**. 

    ![Train Model modülü için kredi riskini sütununu seçin](./media/tutorial-part2-credit-risk-train/train-model-select-column.png)

1. Tıklayın **Tamam** işaretleyin.

### <a name="two-class-support-vector-machine"></a>Çift Sınıflı Destek Vektör Makinesi

Ardından, SVM modelini ayarlayın.  

İlk olarak, SVM hakkında biraz açıklama. Artırmalı karar ağaçları de özelliklerle herhangi bir türde çalışır. Ancak doğrusal bir sınıflandırıcı SVM modülü oluşturur olduğundan, tüm sayısal özellikleri aynı ölçek olduğunda ürettiği model en iyi test hatası vardır. Tüm sayısal özellikleri aynı ölçek dönüştürmek için bir "Tanh" dönüşümü kullanın (ile [Normalleştir veri] [ normalize-data] Modülü). Bu bizim numaraları [0,1] aralığa dönüştürür. Dize özellikleri el ile dönüştürme yapmak zorunda kalmazsınız SVM modülü kategorik özellikleri ve ardından ikili 0/1 özellikleri, dize özellikleri dönüştürür. Ayrıca, kredi riski sütun (sütun 21) dönüştürmek istemediğiniz - sayısal ancak bırakır, böylece, tahmin etmek için modeli eğitmek değerdir.  

SVM modeli'kurmak için aşağıdakileri yapın:

1. Bulma [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] modül paletindeki modülünü tuvale sürükleyin.

1. Sağ [modeli eğitme] [ train-model] modülü seçin **kopyalama**ve ardından seçin ve tuvalin sağ tıklayarak **Yapıştır**. Kopyasını [modeli eğitme] [ train-model] modülü orijinalle aynı sütun seçimi içeriyor.

1. Çıkışını [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] modülünün sol giriş bağlantı noktasına ikinci [modeli eğitme] [ train-model] modülü.

1. Bulma [Normalleştir veri] [ normalize-data] modülünü tuvale sürükleyin.

1. Sol sol çıkışını [R betiği yürütme] [ execute-r-script] modülü, bu modülün (bildirim için birden fazla modülü bir modülün çıkış bağlantı noktasına bağlı olduğunu) giriş.

1. Sol çıkış bağlantı noktasına bağlanmak [Normalleştir veri] [ normalize-data] modülünün sağ giriş bağlantı noktasına ikinci [modeli eğitme] [ train-model] modülü.

Bu bölümü, deneme aşağıdakine benzer görünmelidir:  

![İkinci modeli eğitimi](./media/tutorial-part2-credit-risk-train/svm-model-added.png)

Şimdi Yapılandır [Normalleştir veri] [ normalize-data] Modülü:

1. Seçmek için tıklatın [Normalleştir veri] [ normalize-data] modülü. İçinde **özellikleri** bölmesinde **Tanh** için **dönüştürme yöntemi** parametresi.

1. Tıklayın **Sütun seçiciyi Başlat**, "Hiç sütun" seçin **şununla Başla**seçin **INCLUDE** ilk açılan menüyü seçin **sütun türü** ikinci açılır ve select **sayısal** üçüncü açılır. Bu, tüm sayısal sütunları (ve yalnızca sayısal) dönüştürülür belirtir.

1. Bu satır için artı işaretini (+) tıklayın; bunu açılan bir satır oluşturur. Seçin **hariç** ilk açılan menüyü seçin **sütun adları** ikinci açılır ve "risk metin alanına kredi" girin. Bu kredi riski sütun yoksayılıp yoksayılmaması gerektiğini belirtir (Bu sütunu sayısal olduğundan ve hariç yaramadı tutarsanız dönüştürülmesi böylece bunu gerekir).

1. Tıklayın **Tamam** işaretleyin.  

    ![Normalize veri modülü için sütunları seçin](./media/tutorial-part2-credit-risk-train/normalize-data-select-column.png)


[Normalleştir veri] [ normalize-data] modülü kredi riski sütun dışındaki tüm sayısal sütunlarda Tanh dönüştürme gerçekleştirmek için şimdi ayarlanır.  

## <a name="score-and-evaluate-the-models"></a>Puanlama modelleri ve değerlendirme

tarafından ayrıldı kullanıma test verileri kullanarak [verileri bölme] [ split] bizim eğitilen modellerini puanlamanıza olanak modülü. sonra sonuçlarını daha iyi sonuçlar oluşturulduğu görmek için iki modeli de karşılaştırabilirsiniz.  

### <a name="add-the-score-model-modules"></a>Score Model Modül Ekle

1. Bulma [Score Model] [ score-model] modülünü tuvale sürükleyin.

1. Connect [modeli eğitme] [ train-model] bağlı Modülü [iki sınıflı artırılmış karar ağacı] [ two-class-boosted-decision-tree] modülünün sol giriş bağlantı noktasına [Score Model] [ score-model] modülü.

1. Sağa bağlanma [R betiği yürütme] [ execute-r-script] Modülü (test verilerimizi) sağ giriş bağlantı noktasına [Score Model] [ score-model] modülü.

    ![Score Model modülü bağlı](./media/tutorial-part2-credit-risk-train/score-model-connected.png)

   
   [Score Model] [ score-model] modülü artık test verileri kredi bilgiler alın, modeliyle çalıştırın ve modeli oluşturur gerçek kredi riski sütununda ile tahminleri karşılaştırın verileri test etme.

1. Kopyalama ve yapıştırma [Score Model] [ score-model] ikinci bir kopya oluşturmak için modül.

1. SVM modeli çıkışına bağlayın (diğer bir deyişle, çıkış bağlantı [modeli eğitme] [ train-model] bağlı Modülü [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] Modülü) ikinci giriş bağlantı noktasına [Score Model] [ score-model] modülü.

1. SVM modeli için eğitim verileri için yaptığınız gibi test verilerini aynı dönüşümü yapmamız gerekir. Bu nedenle kopyalama ve yapıştırma [Normalleştir veri] [ normalize-data] ikinci bir kopyasını oluşturun ve sağa bağlamak için modül [R betiği yürütme] [ execute-r-script] modülü.

1. İkinci sol çıkışını [Normalleştir veri] [ normalize-data] modülünün sağ giriş bağlantı noktasına ikinci [Score Model] [ score-model] modülü.

    ![Bağlı her iki Model Puanlama Modülü](./media/tutorial-part2-credit-risk-train/both-score-models-added.png)


### <a name="add-the-evaluate-model-module"></a>Model değerlendirme Modül Ekle

İki Puanlama sonuçları değerlendirin ve bunları karşılaştırmak için kullandığınız bir [Evaluate Model] [ evaluate-model] modülü.  

1. Bulma [Evaluate Model] [ evaluate-model] modülünü tuvale sürükleyin.

1. Çıkış bağlantı noktasına bağlanmak [Score Model] [ score-model] modülünün sol giriş bağlantı noktasına artırmalı karar ağacı modeli ile ilişkili [Evaluate Model] [ evaluate-model] modülü.

1. Diğer connect [Score Model] [ score-model] modülünün sağ giriş bağlantı noktasına.  

    ![Modeli değerlendirme bağlı Modülü](./media/tutorial-part2-credit-risk-train/evaluate-model-added.png)


### <a name="run-the-experiment-and-check-the-results"></a>Denemeyi çalıştırma ve sonuçları denetleyin

Denemeyi çalıştırmak için tıklayın **ÇALIŞTIRMA** tuvalin altındaki düğme. Bu işlem birkaç dakika sürebilir. Çalıştığını ve daha sonra yeşil bir onay işareti modülü tamamladığını gösterir. her modül bir dönen göstergesi gösterir. Tüm modüllerin bir onay işareti varsa, denemeyi çalışması sona erdi.

Deneme şimdi aşağıdakine benzer görünmelidir:  

![Her iki modeli değerlendirme](./media/tutorial-part2-credit-risk-train/final-experiment.png)


Sonuçları denetlemek için çıkış bağlantı noktasına tıklayın [Evaluate Model] [ evaluate-model] modülü ve select **Görselleştir**.  

[Evaluate Model] [ evaluate-model] modül, eğriler ve iki puanlanmış modeli sonucunu karşılaştırmaya izin veren ölçümler bir çift oluşturur. Alıcı işleci özellikleri (ROC) eğrileri, duyarlık/geri çağırma eğriler ve Lift eğrileri olarak sonuçlarını görüntüleyebilirsiniz. Ek veri görüntülendiğini, bir karışıklık matrisi eğrinin (AUC) ve diğer ölçümleri alanında toplu değerlerini içerir. Kaydırıcıyı sola veya sağa hareket ettirerek eşik değerini değiştirebilir ve bu ölçüm kümesini nasıl etkilediğini görebilir.  

Graf sağında tıklayın **veri kümesi Puanlanmış** veya **karşılaştırmak için veri kümesi Puanlanmış** ilişkili eğri vurgulayın ve aşağıdaki ilişkili ölçümleri görüntülemek için. Eğri için göstergede "Puanlanmış veri kümesi" sol giriş bağlantı noktasına karşılık gelen [Evaluate Model] [ evaluate-model] modülü - örneğimizde artırmalı karar ağacı modeli budur. Sağ giriş bağlantı noktasına - SVM modeli bizim durumumuzda, "karşılaştırmak için veri kümesi puanlanmış" karşılık gelir. Bu etiketler birine tıkladığınızda, bu model için eğri vurgulanır ve aşağıdaki grafikte gösterildiği gibi ilgili ölçümleri görüntülenir.  

![Modelleri için ROC Eğriler](./media/tutorial-part2-credit-risk-train/roc-curves.png)

Bu değerleri inceleyerek hangi Aradığınız sonuçları vermiş, en yakın modeldir karar verebilirsiniz. Geri dönün ve farklı modelleri değerlerde parametresi değiştirerek üzerinde denemenizi yineleme. 

Bilim ve son teknoloji ürünü bu sonuçları yorumlayarak destek sağlama ve model performansını ayarlama, bu öğreticinin kapsamı dışında olan. Ek Yardım için bu makaleleri okuyun:
- [Azure Machine Learning Studio'da model performansını değerlendirme](evaluate-model-performance.md)
- [Azure Machine Learning Studio'da algoritmalarınızı iyileştirmek için parametreleri seçin](algorithm-parameters-optimize.md)
- [Azure Machine Learning Studio'da model sonuçlarını yorumlama](interpret-model-results.md)

> [!TIP]
> Denemeyi bu yineleme kaydını her çalıştırdığınızda, çalıştırma geçmişi tutulur. Bu yinelemeler görüntülemek ve herhangi biri, tıklayarak dönüş **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE** tuval aşağıda. Ayrıca **önceki çalıştırma** içinde **özellikleri** , hemen bir önceki yinelemeye döndürülecek bölmesi açık olması.
> 
> Tıklayarak, denemenizi herhangi bir yinelemesini kopyasını yapabilirsiniz **SAVE AS** tuval aşağıda. 
> Deneme kullanın **özeti** ve **açıklama** , deneme yinelemelerini denediyseniz, kaydını tutmak için özellikleri.
> 
> Daha fazla bilgi için [Azure Machine Learning Studio'da deneme yinelemelerini yönetme](manage-experiment-iterations.md).  
> 
> 

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [machine-learning-studio-clean-up](../../../includes/machine-learning-studio-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bu adımlar tamamlandı: 
 
> [!div class="checklist"]
> * Deneme oluşturma
> * Birden çok modeli eğitme
> * Puanlama modelleri ve değerlendirme

Artık bu veri modelleri dağıtmaya hazırsınız.

> [!div class="nextstepaction"]
> [Öğretici 3 - Modelleri dağıtma](tutorial-part3-credit-risk-deploy.md)


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/