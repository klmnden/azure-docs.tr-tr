---
title: Algoritmalar en iyi duruma getirme
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio'da bir algoritma için en iyi parametresi seçin açıklanmaktadır.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 11/29/2017
ms.openlocfilehash: 6dc9476f603d5664b7ea23489042b69f86647cf5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60752237"
---
# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da algoritmalarınızı iyileştirmek için parametreleri seçin

Bu konuda, Azure Machine Learning Studio'da bir algoritma kümesi doğru hiper parametre seçin açıklar. Çoğu makine öğrenimi algoritmaları ayarlanacak parametrelere sahip. Bir model eğitip yaparken bu parametrelerin değerlerini sağlamasını gerekir. Eğitilen modelin çalışıp çalışmadığını seçtiğiniz model parametreleri bağlıdır. Parametreleri en uygun kümesini bulma işlemi olarak bilinir *model seçimi*.



Seçimi modellemek için çeşitli yollar vardır. Machine learning'de çapraz doğrulama model seçimi için en yaygın olarak kullanılan yöntemlerden biridir ve Azure Machine Learning Studio'da model varsayılan seçim mekanizmasını olduğu. Azure Machine Learning Studio, R ve Python hem desteklediğinden, R veya Python kullanarak her zaman kendi model seçim mekanizmalarını uygulayabilirsiniz.

En iyi parametre kümesi bulma işlemi dört adım vardır:

1. **Parametre alanı tanımlama**: Algoritma için ilk kullanmayı tam parametre değerlerini karar verin.
2. **Çapraz doğrulama ayarlarını tanımlayın**: Veri kümesi için çapraz doğrulama hatları seçmenizi nasıl karar verin.
3. **Ölçüm tanımlama**: En iyi doğruluğu gibi parametreleri kümesini belirlemek için kullanılacak hangi ölçüm karar, hata, kesinlik, geri çağırma veya f puanı kök ortalama karesi alınmış.
4. **Eğitim, değerlendirmek ve karşılaştırma**: Her benzersiz birleşimi parametre değerleri için çapraz doğrulama tarafından gerçekleştirilen ve tanımladığınız hata ölçüme göre. Değerlendirme ve karşılaştırma sonra en yüksek performansa modeli seçebilirsiniz.

Aşağıdaki görüntüde gösterildiği Azure Machine Learning Studio'da bunu nasıl sağlanabilir gösterir.

![En iyi parametre kümesi bulma](./media/algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Parametre alanı tanımlayın
Model başlatma adım kümesi parametresi tanımlayabilirsiniz. Parametre bölmesini tüm machine learning algoritmalarının iki trainer modu vardır: *Tek bir parametre* ve *parametresi aralık*. Parametre aralık modu seçin. Parametre aralık modunda her parametre için birden çok değer girebilirsiniz. Metin kutusuna, virgülle ayrılmış değerler girebilirsiniz.

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
