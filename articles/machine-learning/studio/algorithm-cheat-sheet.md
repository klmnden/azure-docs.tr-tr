---
title: Makine öğrenimi algoritma bilgi sayfası - Azure | Microsoft Docs
description: Bir yazdırılabilir makine öğrenimi algoritma bilgi sayfası Azure Machine Learning Studio'da Tahmine dayalı model doğru algoritması seçmenize yardımcı olur.
keywords: Makine öğrenimi algoritmasının algoritma bilgi sayfası, kural sayfası
services: machine-learning
author: pakalra
ms.author: pakalra
manager: cgronlun
editor: cgronlun
ms.assetid: e1dc31ec-1acb-463f-ba77-de565d4ddf4d
ms.service: machine-learning
ms.component: studio
ms.topic: article
ms.date: 12/18/2017
ms.openlocfilehash: 4a6fdfec4c4c95ba47f17efeb0dc87521a86c03c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244993"
---
# <a name="machine-learning-algorithm-cheat-sheet-for-azure-machine-learning-studio"></a>Makine öğrenimi algoritma bilgi sayfasına için Azure Machine Learning Studio

**Azure Machine Learning algoritması kural sayfası** öngörülebilir bir analitik model doğru algoritması seçmenize yardımcı olur.

[Azure Machine Learning Studio](https://studio.azureml.net/) algoritmalarından büyük bir kitaplık olan ***regresyon***, ***sınıflandırma***, ***Kümeleme***, ve  ***anomali algılama*** aileleri. Her farklı türde bir makine öğrenme sorunu gidermek için tasarlanmıştır.

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>İndirin: Makine öğrenimi algoritma bilgi sayfası

**Buradaki ipuçlarını indirin: [makine öğrenimi algoritma kural sayfası (11 x 17 inç)](https://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet-v7.pdf)**

![Makine öğrenimi algoritma bilgi sayfası: bir makine öğrenimi algoritma seçme hakkında bilgi edinin.][cheat-sheet]

[cheat-sheet]: ./media/algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet-small_v_0_6-01.png

İndirin ve Machine Learning Studio algoritması hile kullanışlı korumak ve bir algoritma seçme konusunda yardım almak için tabloid boyutunda yazdırma sayfası.

> [!NOTE]
> Doğru algoritması yanı sıra, farklı türlerdeki makine öğrenimi algoritmaları ve bunların nasıl kullanıldığı daha ayrıntılı bir tartışma seçmek için bu kural sayfası kullanarak daha fazla yardım için bkz: [Microsoft Azure Machine Learning içinalgoritmaseçme](algorithm-choice.md).

## <a name="notes-and-terminology-definitions-for-the-machine-learning-studio-algorithm-cheat-sheet"></a>Notlar ve terim tanımları için Machine Learning Studio algoritması kopya sayfası

* Bu algoritma bilgi sayfası içinde sunulan yaklaşık kuralları-ın-thumb önerilerdir. Bazı Eğilmiş ve bazı flagrantly ihlal edildi. Bu bir başlangıç noktası önermek için tasarlanmıştır. Verileriniz üzerinde çeşitli algoritmalar arasındaki head-to-head yarışma çalıştırılacak Korkmayın. Ve her bir algoritmanın prensipleri anlama ve verilerinizi oluşturulan sistemini anlama yalnızca hiçbir yedek yok.

* Her makine öğrenimi algoritmasının kendi stilde veya *Endüktif sapması*. Belirli bir sorun için çeşitli algoritmalar uygun olabilir ve bir algoritma diğerlerinden daha uygun olabilir. Ancak, her zaman en uygun olan önceden bilmeniz mümkün değildir. Bu gibi durumlarda, çeşitli algoritmalar kağıdı içinde birlikte listelenir. Uygun bir strateji, bir algoritma deneyebilirsiniz ve sonuçları henüz tatmin edicidir, değilse, diğer deneyin olacaktır. İşte bir örnek [Azure AI Gallery](http://gallery.azure.ai/) aynı verilere karşı çeşitli algoritmalar çalışır ve sonuçları karşılaştıran bir deneme: [çok sınıflı sınıflandırıcılar karşılaştırın: Harf tanıma](http://gallery.azure.ai/Details/a635502fc98b402a890efe21cec65b92).

* Machine Learning üç ana kategori vardır: **denetimli öğrenme**, **Denetimsiz öğrenme**, ve **pekiştirmeye dayalı öğrenme**.

  * İçinde **denetimli öğrenme**, her veri noktası etiketlenmiş veya bir kategori veya ilgi alanı değeri ile ilişkili.  Kategorik bir etiket örneği 'cat' veya 'dog' olarak görüntü atanmasıdır.  Satış fiyatı bir araba ile ilişkili bir değer etiketi örneğidir. Bunlar gibi pek çok etiketli örnekleri incelemek için denetimli öğrenme amacı olan ve gelecekteki veri noktaları hakkında tahminde bulunmak için. Örneğin, yeni Fotoğraf doğru donatarak veya diğer kullanılan otomobiller için doğru satış fiyatları atama ile tanımlama. Machine Learning, popüler ve kullanışlı bir türü budur. Öğrenimi algoritmaları hariç tüm modülleri Azure Machine Learning denetimli [K-ortalamaları Kümeleme][k-means-clustering].

  * İçinde **Denetimsiz öğrenme**, veri noktanız olması ilişkili etiket yok. Bunun yerine, Denetimsiz öğrenme algoritmasının bir şekilde verileri düzenleme veya yapısını tanımlamak için hedeftir. K-ortalamaları gibi kümeler halinde gruplandırarak ya da daha basit görünür bir karmaşık verilere baktığımızda, farklı yöntemleri sürekli düzende bulmak anlamına gelebilir.

  * İçinde **pekiştirmeye dayalı öğrenme**, her veri noktası için yanıt, bir eylem seçmek üzere algoritma alır. Robotlara ilişkin nerede zaman içinde bir noktadaki sensör okumaları kümesini bir veri noktasıdır ve algoritma robot'ın sonraki eylemi seçmeniz gerekir, yaygın bir yaklaşımdır. Ayrıca bir doğal anahtardır nesnelerin interneti uygulamalar için uygun. Öğrenme algoritmasını ödül sinyal kısa kararı ne kadar iyi olduğunu gösteren bir süre daha da alır. Algoritma bunu temel alarak, yüksek ödül elde etmek için kendi stratejisi değiştirir. Şu anda hiçbir pekiştirmeye öğrenme algoritmasını modülleri Azure ML vardır.

* **Bayes yöntemleri** istatistiksel olarak bağımsız veri noktaları olduğu varsayımını yaparsınız. Bunun anlamı bir veri noktası modellenmemiş sonuçlarındaki başkalarıyla uncorrelated, diğer bir deyişle, tahmin edilemez. Örneğin, kaydedilen veri sonraki subway train ulaşan kadar geçecek dakika sayısını ise, bir gün sonraya alınan iki ölçümlere istatistiksel olarak bağımsızdır. Ancak, bir dakika sonraya alınan iki ölçümlere istatistiksel olarak bağımsız değildir - değer bir diğerinin değeri yüksek oranda Tahmine dayalı.

* **Artırılmış karar ağacı regresyonu** özellik örtüşmesi veya özellikler arasındaki etkileşimi avantajlarından yararlanır. Bir özellik değerini herhangi belirli veri noktasına başka bir değeri biraz Tahmine dayalı, anlamına gelir. Örneğin, günlük yüksek/düşük sıcaklık verileri güne ait düşük sıcaklık bilerek en yüksek makul bir tahmin yapmanızı sağlar. İki özellik içinde yer alan bilgileri biraz gereksizdir.

* Veri sınıflandırma ikiden fazla kategoriye yapılabilir bir çok sınıflı kendiliğinden sınıflandırıcı kullanarak veya iki sınıflı sınıflandırıcılar kümesinin birleştirilmesinin bir **topluluğu**. Topluluğu yaklaşımda her sınıf için ayrı iki sınıflı sınıflandırıcı vardır - her biri veri iki kategoriye ayırır: "Bu class" ve "Bu sınıfı değil." Ardından bu sınıflandırıcılar, veri noktasının doğru atamaya oy verin. Bu arkasında işletimsel ilkesidir [veya bir vs tüm çoklu sınıflar][one-vs-all-multiclass].

* Lojistik regresyon ve Bayes noktası makinesi dahil olmak üzere çeşitli yöntemler varsayar **doğrusal sınıfı sınırları**. Diğer bir deyişle, bunlar sınıfları arasındaki sınırları yaklaşık düz çizgileri (veya daha fazla genel durumda hyperplanes) olduğunu varsayar. Genellikle bu, ayrı girişimi yaptınız sonra kadar tanımadığınız verileri bir özelliğidir, ancak önceden görselleştirerek genellikle öğrenilmesi bir şey olduğunu. Sınıf sınırları çok düzensiz bakarsanız, karar ağaçları ile devam edin, harikası karar, vektör makine ya da sinir ağları destekler.

* Sinir ağları kullanılabilir kategorik değişkenlerle oluşturarak bir **işlevsiz değişkeni** her kategori için 1 durumlarda nerede kategori uygular, burada değil 0 olarak ayarlamak.

## <a name="next-steps"></a>Sonraki adımlar

* Algoritmalar açıklar ve örnekler sağlayan bir indirilebilir bilgi grafiği için bkz: [indirilebilir bilgi grafiği: Makine öğrenimi algoritma örnekleri ile Temelleri](basics-infographic-with-algorithm-examples.md).

* Machine Learning Studio'da kullanılabilen tüm makine öğrenimi algoritma kategoriye göre bir listesi için bkz [modeli Başlat] [ initialize-model] Machine Learning Studio algoritma ve modül Yardımı.

* Bir tam alfabetik listesi algoritmaları ve Machine Learning Studio'daki modüller için bkz: [Machine Learning Studio modüllerinin A-Z listesi] [ a-z-list] Machine Learning Studio algoritma ve modül Yardımı.

* Machine Learning Studio'nun işlevlerine genel bir bakış sağlayan bir diyagram indirmek ve yazdırmak için bkz. [Azure Machine Learning Studio'nun işlevlerine genel bakış diyagramı](studio-overview-diagram.md).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

<!-- Module References -->
[a-z-list]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/a-z-module-list
[initialize-model]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/machine-learning-initialize-model
[k-means-clustering]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/k-means-clustering
[one-vs-all-multiclass]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/one-vs-all-multiclass
