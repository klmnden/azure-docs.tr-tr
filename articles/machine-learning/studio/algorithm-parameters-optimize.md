---
title: "Azure Machine Learning algoritmaları en iyi duruma getirme | Microsoft Docs"
description: "Azure Machine learning'de algoritma için en iyi parametresinde seçin açıklanmaktadır."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 6717e30e-b8d8-4cc1-ad0b-1d4727928d32
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev;garye
ms.openlocfilehash: 664ab97cdfb663d9c8a4cc6c7b748eebfbdf580c
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning"></a>Azure Machine Learning algoritmaları en iyi duruma getirmek için parametreleri seçin
Bu konuda, Azure Machine learning'de algoritma için ayarlanmış sağ hyperparameter seçmek açıklar. Çoğu machine learning algoritmaları ayarlamak için parametrelere sahip. Bir modeli eğitmek zaman o parametreler için değerler sağlamanız gerekir. Seçtiğiniz model parametreleri eğitilen model sürecinin bağlıdır. En iyi parametrelerinin bulma işleminin olarak bilinen *model seçimi*.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

Seçim modellemek için çeşitli yolları vardır. Machine learning, çapraz doğrulama model seçimi için en yaygın olarak kullanılan yöntemleri biridir ve varsayılan model seçimi mekanizma Azure Machine Learning değil. Azure Machine Learning R ve Python desteklediğinden, R veya Python kullanarak her zaman kendi model seçimi mekanizmaları uygulayabilirsiniz.

En iyi parametre kümesi bulma sürecinde dört adım vardır:

1. **Parametre alanı tanımlamak**: algoritması, ilk olarak istediğiniz dikkate alınması gereken tam parametre değerlerini karar verin.
2. **Çapraz doğrulama ayarlarını tanımla**: veri kümesi için çapraz doğrulama Katlama seçmek nasıl karar verin.
3. **Ölçümü tanımlayın**: en iyi doğruluğu gibi parametreleri kümesini belirlemek için kullanılacak hangi ölçüm karar, kök ortalama karesi alınmış hata, kesinlik, geri çağırma veya f-score.
4. **Eğitim, değerlendirmek ve karşılaştırmak**: parametre değerlerini benzersiz her birleşimi için çapraz doğrulama tarafından yürütülen ve tanımladığınız hata ölçüm tabanlı. Değerlendirme ve karşılaştırma sonra en çok gerçekleştirme modeli seçebilirsiniz.

Aşağıdaki resim Azure Machine Learning ile bu nasıl sağlanabilir gösterir gösterilmektedir.

![En iyi parametre kümesi Bul](./media/algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Parametre alanını tanımlayın
Parametre modeli başlatma adımda kümesi tanımlayabilirsiniz. Tüm makine öğrenimi algoritma parametresi bölmesini iki Eğitmen modu vardır: *tek bir parametre* ve *parametre aralık*. Parametre aralık modu seçin. Parametre aralık modunda her parametre için birden çok değer girebilirsiniz. Metin kutusunda, virgülle ayrılmış değerler girebilirsiniz.

![İki sınıflı artırılmış karar ağacı, tek bir parametre](./media/algorithm-parameters-optimize/fig2.png)

 Alternatif olarak, kılavuz ve nokta ile oluşturulacak toplam sayısı maksimum ve minimum noktalarının tanımlayabilirsiniz **kullanım aralık oluşturucu**. Varsayılan olarak, parametre değerlerini doğrusal bir ölçekte üretilir. Ancak **günlük ölçek** denetlenir değerlerin, günlük ölçeğin oluşturulur (diğer bir deyişle, bitişik noktalarının yerine kendi fark sabit orandır). Tamsayı Parametreler için kısa çizgi kullanarak bir aralığı tanımlayabilirsiniz. 1 ile 10 (her ikisi de dahil) arasındaki tüm tamsayılara parametre kümesi form Örneğin, "1-10" anlamına gelir. Karma mod da desteklenir. Örneğin, parametre kümesini "1-10, 20, 50" tamsayılar 1-10, 20, içerir ve 50.

![İki sınıflı artırılmış karar ağacı, parametre aralığı](./media/algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Çapraz doğrulama Katlama tanımlayın
[Bölüm ve örnek] [ partition-and-sample] modülü, rastgele verileri Katlama atamak için kullanılabilir. Aşağıdaki örnek yapılandırma modülü için beş Katlama tanımlayın ve rastgele Katlama birkaç örnek örneklerine atayın.

![Bölüm ve örnek](./media/algorithm-parameters-optimize/fig4.png)

## <a name="define-the-metric"></a>Ölçümü tanımlayın
[Model ayarlama Hiperparametreleri] [ tune-model-hyperparameters] modülü empirically verilen algoritması ve veri kümesi için en iyi parametrelerinin seçme için destek sağlar. Diğer bilgilerine ek olarak eğitim modeli ilgili **özellikleri** Bu modülün bölmesinde en iyi parametre kümesi belirlemek için ölçüm içerir. Sınıflandırma ve regresyon algoritmalar için iki farklı aşağı açılan liste kutusu sırasıyla sahiptir. Algoritma odaklanılan bir sınıflandırma algoritmasıdır ise, regresyon ölçüm göz ardı edilir ve tersi. Bu belirli örnekte ölçümüdür **doğruluğu**.   

![Tarama parametreleri](./media/algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Eğitim, değerlendirmek ve karşılaştırma
Aynı [Model ayarlama Hiperparametreleri] [ tune-model-hyperparameters] üzerinde ölçüm parametresine karşılık gelen tüm modelleri ayarlayın, çeşitli ölçümleri değerlendirir ve en iyi eğitim modeli oluşturur modülü trenler dayalı ' ı seçin. Bu modül iki zorunlu giriş vardır:

* Eğitimsiz öğrenen
* Veri kümesi

Modül de giriş isteğe bağlı bir veri kümesine sahiptir. Zorunlu dataset giriş için Katlama bilgilerle dataset bağlayın. Veri kümesi herhangi bir Katlama bilgi atanmamışsa bir 10-fold çapraz doğrulama otomatik olarak varsayılan olarak yürütülür. Katlama atama yapılmaz ve isteğe bağlı veri kümesi bağlantı noktasına bir doğrulama dataset sağlanır, tren test modu seçilir ve ilk veri kümesini her parametre birleşimi için modeli eğitmek için kullanılır.

![Artırılmış karar ağacı sınıflandırıcı](./media/algorithm-parameters-optimize/fig6a.png)

Model doğrulama veri kümesine sonra değerlendirilir. Modülünün sol çıkış bağlantı noktasına parametre değerleri olarak işlevler farklı ölçütleri gösterir. Doğru çıkış bağlantı noktasına en gerçekleştirme modele seçilen ölçüm göre karşılık gelen eğitilen model sağlar (**doğruluğu** bu durumda).  

![Doğrulama veri kümesi](./media/algorithm-parameters-optimize/fig6b.png)

Doğru çıkış bağlantı noktasına görselleştirme tarafından seçilen tam parametreleri görebilirsiniz. Bu model, bir sınama kümesi Puanlama veya bir kullanıma hazır hale getirilmiş web hizmeti modeli olarak kaydettikten sonra kullanılabilir.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
