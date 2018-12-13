---
title: Algoritmalar - Azure Machine Learning Studio en iyi duruma getirme | Microsoft Docs
description: Azure Machine Learning Studio'da bir algoritma için en iyi parametresi seçin açıklanmaktadır.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 6717e30e-b8d8-4cc1-ad0b-1d4727928d32
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: b0494a9da422b7c0effc14ff4188d3a5b20b8e9d
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53081940"
---
# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da algoritmalarınızı iyileştirmek için parametreleri seçin

Bu konuda, Azure Machine learning'de algoritma için doğru hiper parametre seçin açıklar. Çoğu makine öğrenimi algoritmaları ayarlanacak parametrelere sahip. Bir model eğitip yaparken bu parametrelerin değerlerini sağlamasını gerekir. Eğitilen modelin çalışıp çalışmadığını seçtiğiniz model parametreleri bağlıdır. Parametreleri en uygun kümesini bulma işlemi olarak bilinir *model seçimi*.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

Seçimi modellemek için çeşitli yollar vardır. Machine Learning, çapraz doğrulama model seçimi için en yaygın olarak kullanılan yöntemlerden biridir ve onu varsayılan model seçimi Azure Machine Learning mekanizmasıdır. Azure Machine Learning, R ve Python hem desteklediğinden, R veya Python kullanarak her zaman kendi model seçim mekanizmalarını uygulayabilirsiniz.

En iyi parametre kümesi bulma işlemi dört adım vardır:

1. **Parametre alanı tanımlama**: algoritma için istediğiniz dikkate alınması gereken tam parametre değerlerini ilk karar verin.
2. **Çapraz doğrulama ayarlarını tanımlayın**: veri kümesi için çapraz doğrulama hatları seçmenizi nasıl karar verin.
3. **Ölçüm tanımlama**: en iyi doğruluğu gibi parametreleri kümesini belirlemek için kullanılacak hangi ölçüm karar, kök ortalama karesi alınmış hata, kesinlik, geri çağırma veya f puanı.
4. **Eğitim, değerlendirmek ve karşılaştırma**: her benzersiz birleşimi parametre değerleri için çapraz doğrulama tarafından gerçekleştirilen ve tanımladığınız hata ölçüme göre. Değerlendirme ve karşılaştırma sonra en yüksek performansa modeli seçebilirsiniz.

Aşağıdaki görüntüde gösterildiği Azure Machine Learning'de bunu nasıl sağlanabilir gösterir.

![En iyi parametre kümesi bulma](./media/algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Parametre alanı tanımlayın
Model başlatma adım kümesi parametresi tanımlayabilirsiniz. Tüm makine öğrenimi algoritma parametresi bölmesinde iki trainer modu vardır: *tek parametre* ve *parametresi aralık*. Parametre aralık modu seçin. Parametre aralık modunda her parametre için birden çok değer girebilirsiniz. Metin kutusuna, virgülle ayrılmış değerler girebilirsiniz.

![İki sınıflı artırmalı karar ağacı, tek bir parametre](./media/algorithm-parameters-optimize/fig2.png)

 Alternatif olarak, maksimum ve minimum noktaları ve kılavuz ile oluşturulması gereken noktaları toplam sayısını tanımlayabilirsiniz **kullanım aralık oluşturucu**. Varsayılan olarak, parametre değerlerini bir doğrusal ölçek üzerinde oluşturulur. Ancak **Logaritmik ölçek** denetlenir değerlerin günlük ölçek oluşturulur (diğer bir deyişle, bitişik noktalar oranını yerine kendi fark sabittir). Tamsayı parametre için bir kısa çizgi kullanarak bir aralığı tanımlayabilirsiniz. 1 ile 10 (her ikisi de dahil) arasında tüm tamsayıların parametre kümesi oluşturur Örneğin, "1-10" anlamına gelir. Karma mod da desteklenir. Örneğin, parametre "1-10, 20, 50" tamsayılar 1-10, 20, içerir ve 50.

![İki sınıflı artırmalı karar ağacı, parametre aralık](./media/algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Çapraz doğrulama hatları tanımlayın
[Bölüm ve örnek] [ partition-and-sample] modülü, rastgele verileri hatları atamak için kullanılabilir. Aşağıdaki örnek yapılandırmada modülü için beş hatları tanımlayın ve Katlama birkaç örnek örneklerine rastgele atayın.

![Bölüm ve örnek](./media/algorithm-parameters-optimize/fig4.png)

## <a name="define-the-metric"></a>Ölçüm tanımlama
[Model ayarlama Hiperparametreleri] [ tune-model-hyperparameters] modül türü belirli algoritma ve veri kümesi için en iyi parametrelerinin seçmek için destek sağlar. Diğer bilgilere ek olarak, modeli eğitmek ilgili **özellikleri** bölmesinde Bu modülün en iyi parametre kümesini belirlemek için ölçüm içerir. Sınıflandırma ve regresyon algoritmalar için iki farklı aşağı açılan liste kutuları sırasıyla sahiptir. Algoritma odaklanılan bir sınıflandırma algoritmasıdır ise, regresyon ölçüm göz ardı edilir ve bunun tersi de geçerlidir. Bu belirli örnekte unsurdur **doğruluğu**.   

![Gözden geçirme parametreleri](./media/algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Eğitim, değerlendirin ve karşılaştırın
Aynı [Model ayarlama Hiperparametreleri] [ tune-model-hyperparameters] ölçüme göre parametresine karşılık gelen tüm modelleri ayarlayın, çeşitli ölçümleri değerlendirir ve ardından en eğitim modeli oluşturur modülü trenler tabanlı ' ı seçin. Bu modül, iki zorunlu giriş vardır:

* Deneyimsiz öğrenici
* Veri kümesi

Modül giriş isteğe bağlı bir veri kümesi de vardır. Zorunlu veri kümesi giriş Katlama bilgilerle veri kümesine bağlanın. Veri kümesi herhangi bir Katlama bilgi uygulanmamışsa 10-fold bir çapraz doğrulama otomatik olarak varsayılan olarak yürütülür. Katlama atama yapılmaz ve isteğe bağlı bir veri kümesi bağlantı noktalarından sağlanan doğrulama veri kümesi, eğitin ve test modu seçilir ve ilk veri kümesi için her parametre bileşimi modeli eğitmek için kullanılır.

![Artırmalı karar ağacı Sınıflandırıcısı](./media/algorithm-parameters-optimize/fig6a.png)

Model doğrulama veri kümesinde ardından değerlendirilir. Modülünün sol çıkış bağlantı noktasına parametre değerlerinin işlevleri farklı ölçümlerini gösterir. Seçilen ölçüm göre en yüksek performansa modeline karşılık gelen eğitilen model doğru çıkış bağlantı noktasına sağlar (**doğruluğu** bu durumda).  

![Doğrulama veri kümesi](./media/algorithm-parameters-optimize/fig6b.png)

Doğru çıkış bağlantı noktasına görselleştirerek seçilen tam parametreleri görebilirsiniz. Bu model, bir sınama kümesi Puanlama veya çalışır hale getirilen web hizmeti olarak eğitilen bir modelin kaydettikten sonra kullanılabilir.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
