---
title: '4. adım: Eğitmek ve Tahmine dayalı analitik modelleri değerlendirme | Microsoft Docs'
description: "4. adımını geliştirme Tahmine dayalı bir çözüm izlenecek yol: eğitme, Puanlama ve Azure Machine Learning Studio'da birden fazla modeli değerlendirin."
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: d905f6b3-9201-4117-b769-5f9ed5ee1cac
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: 3f722fa314b42501e0f6144f71b9d74c0a9127e9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Kılavuz Adımı 4: Öngörücü analiz modelleri hakkında eğitim ve değerlendirme
Bu konu, izlenecek dördüncü adımı içerir [Azure Machine learning'de Tahmine dayalı analiz çözümü geliştirme](walkthrough-develop-predictive-solution.md)

1. [Bir Machine Learning çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md)
2. [Mevcut verileri yükleme](walkthrough-2-upload-data.md)
3. [Yeni bir deneme oluşturma](walkthrough-3-create-new-experiment.md)
4. **Modelleri eğitme ve değerlendirme**
5. [Web hizmetini dağıtma](walkthrough-5-publish-web-service.md)
6. [Web hizmetine erişim](walkthrough-6-access-web-service.md)

- - -
Machine learning modellerini oluşturmak için Azure Machine Learning Studio'da kullanmanın avantajlarından biri tek bir deneme zamanında birden fazla türde bir model deneyin ve sonuçları karşılaştırmak için yeteneğidir. Bu tür bir deney sorununuzu için en iyi çözüm bulmanıza yardımcı olur.

Bu kılavuzda geliştirme yapıyorsanız denemesinde biz modelleri iki farklı türde oluşturacak ve bizim son deneme şu kullanmak istediğiniz algoritmayı karar vermek için Puanlama sonuçlarını karşılaştırın.  

Biz arasından seçim çeşitli modellerin vardır. Kullanılabilir modelleri görmek için genişletin **Machine Learning** modül paletindeki düğümünü genişletin ve ardından **modeli Başlat** ve altındaki düğümleri. Bu deneme amaçları doğrultusunda, biz seçersiniz [iki sınıflı destek vektör makinesi] [ two-class-support-vector-machine] (SVM) ve [iki-Class Boosted karar ağacı] [ two-class-boosted-decision-tree] modüller.    

> [!TIP]
> Çözmeye çalıştığınız belirli sorun Machine Learning algoritmayı en iyi uyan karar vermeyle ilgili Yardım almak için bkz: [için Microsoft Azure Machine Learning algoritmaları seçme](algorithm-choice.md).
> 
> 

## <a name="train-the-models"></a>Modelleri eğitme

Her ikisi de ekleyeceğiz [iki-Class Boosted karar ağacı] [ two-class-boosted-decision-tree] modülü ve [iki sınıflı destek vektör makinesi] [ two-class-support-vector-machine] bu deneme modülünde.

### <a name="two-class-boosted-decision-tree"></a>İki sınıflı artırılmış karar ağacı

İlk olarak, şimdi artırılmış karar ağacı modelini ayarlayın.

1. Bul [iki-Class Boosted karar ağacı] [ two-class-boosted-decision-tree] modül paletindeki modülünü tuvale sürükleyin.

2. Bul [Train Model] [ train-model] modülünü tuvale sürükleyin ve çıkışına bağlayın [iki-Class Boosted karar ağacı] [ two-class-boosted-decision-tree] Modül sol giriş bağlantı noktası [Train Model] [ train-model] modülü.
   
   [İki-Class Boosted karar ağacı] [ two-class-boosted-decision-tree] modülü genel model başlatır ve [modeli eğitmek] [ train-model] eğitmek için eğitim verileri kullanır Model. 

3. Sol sol çıkışına bağlayın [R betiği yürütün] [ execute-r-script] modülü sağ giriş bağlantı noktası [Train Model] [ train-model] (karar içinde Modülü [Adım 3](walkthrough-3-create-new-experiment.md) bölünmüş veri modülünün eğitim sol taraftan gelen veriler kullanmak için bu kılavuzun).
   
   > [!TIP]
   > İki girişleri ve çıkışları birini gerekmez [R betiği yürütün] [ execute-r-script] biz bunları eklenmemiş bırakabilirsiniz, bu nedenle bu deneme için modülü. 
   > 
   > 

Bu bölümü denemeyi şimdi şuna benzer:  

![Bir model eğitimi][1]

Bildirmek ihtiyacımız artık [Train Model] [ train-model] modülü modelin kredi riski değer tahmin etmek istiyoruz.

1. Seçin [Train Model] [ train-model] modülü. İçinde **özellikleri** bölmesinde tıklatın **başlatma Sütun seçiciyi**.

2. İçinde **tek bir sütun seçin** iletişim kutusunda, türü "Kredi riski" altındaki arama alanında **kullanılabilir sütunlar**, aşağıda "Kredi riski"'ı seçin ve sağ ok düğmesine tıklayın (**>**) "Kredi riski" taşımak için **seçili sütun**. 

    ![Train Model modülü için kredi riski sütun seçin][0]

3. Tıklatın **Tamam** onay işareti.

### <a name="two-class-support-vector-machine"></a>Çift Sınıflı Destek Vektör Makinesi

Ardından, SVM modelini ayarlayın.  

Öncelikle, SVM hakkında biraz açıklama. Artırılmış karar ağaçları iyi herhangi bir türde özellikleri ile çalışmaz. Doğrusal bir sınıflandırıcı SVM modülü oluşturur olduğundan, tüm sayısal özellikleri aynı ölçeğini varsa ancak ürettiği modeli en iyi test hata var. Bir "Tanh" dönüştürmesi kullanırız tüm sayısal özellikleri aynı ölçek dönüştürmek için (ile [normalleştirin veri] [ normalize-data] Modülü). Bu bizim numaraları [0,1] aralığına dönüştürür. Biz dize özellikleri el ile dönüştürme gerek kalmaması SVM modülü kategorik özellikleri ve ardından ikili 0/1 özellikleri, dize özellikleri dönüştürür. Ayrıca, kredi riski sütun (sütun 21) dönüştürmek istemediğiniz - sayısal ancak tek başına bırakmak ihtiyacımız şekilde tahmin etmek için model eğitim değerdir.  

SVM modeli kurmak için aşağıdakileri yapın:

1. Bul [iki sınıflı destek vektör makinesi] [ two-class-support-vector-machine] modül paletindeki modülünü tuvale sürükleyin.

2. Sağ [Train Model] [ train-model] modülü, select **kopya**, seçin ve tuvale sağ tıklatın **Yapıştır**. Kopyasını [Train Model] [ train-model] modül aynı sütun seçimi özgün olarak sahiptir.

3. Çıkışına bağlayın [iki sınıflı destek vektör makinesi] [ two-class-support-vector-machine] modülü sol giriş bağlantı noktası saniye [Train Model] [ train-model] modülü.

4. Bul [normalleştirin veri] [ normalize-data] modülünü tuvale sürükleyin.

5. Sol sol çıkışına bağlayın [R betiği yürütün] [ execute-r-script] giriş (bildirim için birden fazla modülü modülünün çıkış bağlantı noktasına bağlı olduğunu) Bu modülün modül.

6. Sol çıkış bağlantı noktasına bağlanmak [normalleştirin veri] [ normalize-data] modülü sağ giriş bağlantı noktası saniye [Train Model] [ train-model] modülü.

Bu bizim deneme kısmı bu gibi görünmelidir:  

![Eğitim ikinci modeli][2]  

Şimdi yapılandırmak [normalleştirin veri] [ normalize-data] Modülü:

1. Seçmek için tıklatın [normalleştirin veri] [ normalize-data] modülü. İçinde **özellikleri** bölmesinde, **Tanh** için **dönüştürme yöntemi** parametresi.

2. Tıklatın **başlatma Sütun seçiciyi**, "Hiçbir sütun" seçin **şununla Başla**seçin **INCLUDE** ilk açılır menüde seçin **sütun türü** ikinci açılır ve seçin **sayısal** üçüncü açılır. Bu, tüm sayısal sütunlar (ve yalnızca sayısal) dönüştürüldüğünden belirtir.

3. Bu satır sağındaki artı (+) tıklayın - bu bırakmalar satırının oluşturur. Seçin **hariç** ilk açılır menüde seçin **sütun adları** ikinci açılır ve "metin alanında risk kredi" girin. Bu kredi riski sütun yoksayılmalıdır belirtir (Bu sütunu sayısal olduğundan ve biz bunu siz bıraksanız dönüştürülmesi böylece bunun ihtiyacımız).

4. Tıklatın **Tamam** onay işareti.  

    ![Veri normalleştirin modülü sütunlarını seçin][5]

[Normalleştirin veri] [ normalize-data] modülü kredi riski sütun dışında tüm sayısal sütunlarda Tanh dönüştürme gerçekleştirmek için şimdi ayarlanır.  

## <a name="score-and-evaluate-the-models"></a>Puanlama ve modelleri değerlendir

Tarafından ayrıldı çıkışı test verilerinin kullanırız [bölünmüş veri] [ split] bizim eğitilmiş modeller Puanlama modülü. Biz, ardından, daha iyi sonuçlar oluşturulan görmek için iki model sonuçlarını karşılaştırabilirsiniz.  

### <a name="add-the-score-model-modules"></a>Score Model modülleri ekleme

1. Bul [Score Model] [ score-model] modülünü tuvale sürükleyin.

2. Bağlantı [Train Model] [ train-model] bağlı Modülü [iki-Class Boosted karar ağacı] [ two-class-boosted-decision-tree] modülü sol giriş bağlantı noktası [Score Model] [ score-model] modülü.

3. Sağa bağlanmak [R betiği yürütün] [ execute-r-script] Modülü (test verilerimizi) sağ giriş bağlantı noktası [Score Model] [ score-model] modülü.

    ![Score Model modülünün bağlı][6]
   
   [Score Model] [ score-model] modülü artık test verilerinin kredi bilgileri alın, modeli aracılığıyla çalıştırmak ve modeli oluşturur gerçek kredi riski sütuna tahminleri Karşılaştır verileri test etme.

4. Kopyalama ve yapıştırma [Score Model] [ score-model] modülü ikinci bir kopyasını oluşturun.

5. SVM modeli çıkışına bağlayın (diğer bir deyişle, çıkış bağlantı [Train Model] [ train-model] bağlı Modülü [iki sınıflı destek vektör makinesi] [ two-class-support-vector-machine] Modülü) ikinci giriş noktasına [Score Model] [ score-model] modülü.

6. SVM modeli için eğitim verileri yaptığımız gibi test verileri için aynı dönüştürme yapmak sahibiz. Bu nedenle kopyalama ve yapıştırma [normalleştirin veri] [ normalize-data] ikinci bir kopyasını oluşturmak ve sağa bağlanmak için Modülü [R betiği yürütün] [ execute-r-script] modülü.

7. İkinci sol çıkışına bağlayın [normalleştirin veri] [ normalize-data] modülü sağ giriş bağlantı noktası saniye [Score Model] [ score-model] modülü.

    ![Bağlı her iki Score Model modülleri][7]

### <a name="add-the-evaluate-model-module"></a>Evaluate Model Modül Ekle

İki Puanlama sonuçları değerlendirin ve bunları karşılaştırmak için kullanırız bir [Evaluate Model] [ evaluate-model] modülü.  

1. Bul [Evaluate Model] [ evaluate-model] modülünü tuvale sürükleyin.

2. Çıkış bağlantı noktasına bağlanmak [Score Model] [ score-model] sol giriş bağlantı noktası için artırılmış karar ağacı modeli ile ilişkili Modülü [Evaluate Model] [ evaluate-model] modülü.

3. Diğer bağlanmak [Score Model] [ score-model] modülü sağ giriş bağlantı noktası.  

    ![Modeli değerlendirin bağlı Modülü][8]

### <a name="run-the-experiment-and-check-the-results"></a>Denemeyi çalıştırmak ve sonuçları denetleyin

Denemeyi çalıştırmak için tıklatın **çalıştırmak** tuvale aşağıdaki düğmesine. Birkaç dakika sürebilir. Dönen göstergesi her modül çalışıp çalışmadığını ve ardından modülünün tamamladığında yeşil bir onay işareti gösterir gösterir. Tüm modülleri bir onay işareti varsa, deneme çalışması sona erdi.

Denemeyi bu gibi görünmelidir:  

![Her iki modeli değerlendirme][3]

Sonuçları denetlemek için çıkış bağlantı noktasına tıklayın [Evaluate Model] [ evaluate-model] modülü ve select **Görselleştir**.  

[Evaluate Model] [ evaluate-model] modülü eğriler ve iki puanlanmış modelleri sonuçları karşılaştırmak izin ölçümleri çifti oluşturur. Alıcı işleci karakteristiğini (ROC) Eğriler, duyarlık/geri çağırma Eğriler ya da yükseltme Eğriler olarak sonuçlarını görüntüleyebilirsiniz. Görüntülenen ek verileri karışıklığı matrisi, eğri (AUC) ve diğer ölçümleri alanında toplu değerlerini içerir. Kaydırıcıyı sola veya sağa hareket ettirerek eşik değerini değiştirin ve ölçümleri kümesi nasıl etkilediği bakın.  

Grafik sağında tıklatın **veri kümesi belirtmek** veya **karşılaştırmak için veri kümesi belirtmek** ilişkili eğri vurgulayın ve aşağıdaki ilişkili ölçümleri görüntülemek için. Eğriler göstergede "veri kümesi belirtmek" sol giriş bağlantı noktasına karşılık gelen [Evaluate Model] [ evaluate-model] modül - Örneğimizde, artırılmış karar ağacı modeli budur. "Karşılaştırmak için veri kümesi belirtmek" sağ giriş bağlantı noktasını - bu örnekte SVM modeli karşılık gelir. Bu etiketler birine tıkladığınızda, bu model için eğri vurgulanır ve aşağıdaki grafikte gösterildiği gibi ilgili ölçümleri görüntülenir.  

![Modelleri için ROC Eğriler][4]

Bu değerleri inceleyerek, hangi model Aradığınız sonuçları vermiş, en yakın olan karar verebilirsiniz. Geri dönün ve farklı modellerde parametre değerlerini değiştirerek denemenizi üzerinde yineleme. 

Bilim bu sonuçları yorumlayarak ve model performans ayarlama resim ise bu kılavuzun kapsamı dışındadır. Ek Yardım için aşağıdaki makaleler okuyun:
- [Azure Machine Learning modeli performansını değerlendirmek nasıl](evaluate-model-performance.md)
- [Azure Machine Learning algoritmaları en iyi duruma getirmek için parametreleri seçin](algorithm-parameters-optimize.md)
- [Azure Machine Learning modeli sonuçlarında yorumlama](interpret-model-results.md)

> [!TIP]
> Denemeyi bu yineleme kaydını her çalıştırdığınızda çalıştırma geçmişi korunur. Bu yineleme görüntülemek ve tıklayarak herhangi biri, geri dönüp **ÇALIŞTIRMA GEÇMİŞİNİ GÖRÜNTÜLE** tuvale aşağıda. Tıklatabilirsiniz **önceki Çalıştır** içinde **özellikleri** hemen bir önceki yineleme dönmek için bölmeniz Aç.
> 
> Tıklayarak denemenizin herhangi bir yinelemesini kopyasını yapabilirsiniz **SAVE AS** tuvale aşağıda. 
> Deneme kullanmak **Özet** ve **açıklama** özellikleri, deneme yinelemelerini çalıştınız kaydını tutun.
> 
> Daha fazla ayrıntı için bkz: [Azure Machine Learning Studio'da deneme yinelemelerini yönetme](manage-experiment-iterations.md).  
> 
> 

- - -
**Sonraki: [web hizmetini dağıtma](walkthrough-5-publish-web-service.md)**

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
