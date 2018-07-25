---
title: Makine öğrenimi algoritma bilgi sayfası - Azure | Microsoft Docs
description: Bir yazdırılabilir makine öğrenimi algoritma bilgi sayfası Azure Machine Learning Studio'da Tahmine dayalı model doğru algoritması seçmenize yardımcı olur.
keywords: Makine öğrenimi algoritmasının algoritma bilgi sayfası, kural sayfası
services: machine-learning
documentationcenter: ''
author: pakalra
ms.author: pakalra
manager: cgronlun
editor: cgronlun
ms.assetid: e1dc31ec-1acb-463f-ba77-de565d4ddf4d
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.openlocfilehash: b080a739aa73e3c8ef95c7db9a6358d942e94bba
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238395"
---
# <a name="machine-learning-algorithm-cheat-sheet-for-microsoft-azure-machine-learning-studio"></a>Microsoft Azure Machine Learning Studio için machine learning algoritması kopya sayfası
**Microsoft Azure Machine Learning algoritması sayfası hile** öngörülebilir bir analitik model doğru algoritması seçmenize yardımcı olur.

[Azure Machine Learning Studio](https://studio.azureml.net/) algoritmalarından büyük bir kitaplık olan ***regresyon***, ***sınıflandırma***, ***Kümeleme***, ve  ***anomali algılama*** aileleri. Her farklı türde bir makine öğrenme sorunu gidermek için tasarlanmıştır.

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>İndirin: Makine öğrenimi algoritma bilgi sayfası
**Buradaki ipuçlarını indirin: [makine öğrenimi algoritma kural sayfası (11 x 17 inç)](http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf)**

![Makine öğrenimi algoritma bilgi sayfası: Machine Learning algoritmasını seçim yapmayı öğrenin.][cheat-sheet]

[cheat-sheet]: ./media/algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet-small_v_0_6-01.png

İndirin ve Machine Learning algoritması hile kullanışlı korumak ve bir algoritma seçme konusunda yardım almak için tabloid boyutunda yazdırma sayfası.

> [!NOTE]
> Makaleye göz atın [Microsoft Azure Machine Learning için algoritma seçme](algorithm-choice.md) bu kopya kağıdı kullanarak bir ayrıntılı kılavuz için.
> 
> 

## <a name="more-help-with-algorithms"></a>Algoritmalar ile ilgili daha fazla yardım
* Doğru algoritması yanı sıra, farklı türlerdeki makine öğrenimi algoritmaları ve bunların nasıl kullanıldığı daha ayrıntılı bir tartışma seçmek için bu kural sayfası kullanarak daha fazla yardım için bkz: [Microsoft Azure Machine Learning içinalgoritmaseçme](algorithm-choice.md).
* Algoritmalar açıklar ve örnekler sağlayan bir indirilebilir bilgi grafiği için bkz: [indirilebilir bilgi grafiği: Makine öğrenimi algoritma örnekleri ile Temelleri](basics-infographic-with-algorithm-examples.md).
* Machine Learning Studio'da kullanılabilen tüm makine öğrenimi algoritma kategoriye göre bir listesi için bkz [modeli Başlat] [ initialize-model] Machine Learning Studio algoritma ve modül Yardımı.
* Bir tam alfabetik listesi algoritmaları ve Machine Learning Studio'daki modüller için bkz: [Machine Learning Studio modüllerinin A-Z listesi] [ a-z-list] Machine Learning Studio algoritma ve modül Yardımı.
* Machine Learning Studio'nun işlevlerine genel bir bakış sağlayan bir diyagram indirmek ve yazdırmak için bkz. [Azure Machine Learning Studio'nun işlevlerine genel bakış diyagramı](studio-overview-diagram.md).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="notes-and-terminology-definitions-for-the-machine-learning-algorithm-cheat-sheet"></a>Notlar ve terim tanımları için makine öğrenimi algoritması kopya sayfası

* Bu algoritma bilgi sayfası içinde sunulan yaklaşık kuralları-ın-thumb önerilerdir. Bazı Eğilmiş ve bazı flagrantly ihlal edildi. Bu bir başlangıç noktası önermek için tasarlanmıştır. Verileriniz üzerinde çeşitli algoritmalar arasındaki head-to-head yarışma çalıştırılacak Korkmayın. Ve her bir algoritmanın prensipleri anlama ve verilerinizi oluşturulan sistemini anlama yalnızca hiçbir yedek yok.

* Her makine öğrenimi algoritmasının kendi stilde veya *Endüktif sapması*. Belirli bir sorun için çeşitli algoritmalar uygun olabilir ve bir algoritma diğerlerinden daha uygun olabilir. Ancak, her zaman en uygun olan önceden bilmeniz mümkün değildir. Bu gibi durumlarda, çeşitli algoritmalar kağıdı içinde birlikte listelenir. Uygun bir strateji, bir algoritma deneyebilirsiniz ve sonuçları henüz tatmin edicidir, değilse, diğer deneyin olacaktır. İşte bir örnek [Azure AI Gallery](http://gallery.cortanaintelligence.com/) aynı verilere karşı çeşitli algoritmalar çalışır ve sonuçları karşılaştıran bir deneme: [çok sınıflı sınıflandırıcılar karşılaştırın: Harf tanıma](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

* Machine Learning üç ana kategori vardır: **denetimli öğrenme**, **Denetimsiz öğrenme**, ve **pekiştirmeye dayalı öğrenme**.

  * İçinde **denetimli öğrenme**, her veri noktası etiketlenmiş veya bir kategori veya ilgi alanı değeri ile ilişkili.  Kategorik bir etiket örneği 'cat' veya 'dog' olarak görüntü atanmasıdır.  Satış fiyatı bir araba ile ilişkili bir değer etiketi örneğidir. Bunlar gibi pek çok etiketli örnekleri incelemek için denetimli öğrenme amacı olan ve gelecekteki veri noktaları hakkında tahminde bulunmak için. Örneğin, yeni Fotoğraf doğru donatarak veya diğer kullanılan otomobiller için doğru satış fiyatları atama ile tanımlama. Machine Learning, popüler ve kullanışlı bir türü budur. Öğrenimi algoritmaları hariç tüm modülleri Azure Machine Learning denetimli [K-ortalamaları Kümeleme][k-means-clustering].

  * İçinde **Denetimsiz öğrenme**, veri noktanız olması ilişkili etiket yok. Bunun yerine, Denetimsiz öğrenme algoritmasının bir şekilde verileri düzenleme veya yapısını tanımlamak için hedeftir. K-ortalamaları gibi kümeler halinde gruplandırarak ya da daha basit görünür bir karmaşık verilere baktığımızda, farklı yöntemleri sürekli düzende bulmak anlamına gelebilir.

  * İçinde **pekiştirmeye dayalı öğrenme**, her veri noktası için yanıt, bir eylem seçmek üzere algoritma alır. Robotlara ilişkin nerede zaman içinde bir noktadaki sensör okumaları kümesini bir veri noktasıdır ve algoritma robot'ın sonraki eylemi seçmeniz gerekir, yaygın bir yaklaşımdır. Ayrıca bir doğal anahtardır nesnelerin interneti uygulamalar için uygun. Öğrenme algoritmasını ödül sinyal kısa kararı ne kadar iyi olduğunu gösteren bir süre daha da alır. Algoritma bunu temel alarak, yüksek ödül elde etmek için kendi stratejisi değiştirir. Şu anda hiçbir pekiştirmeye öğrenme algoritmasını modülleri Azure ML vardır.

* **Bayes yöntemleri** istatistiksel olarak bağımsız veri noktaları olduğu varsayımını yaparsınız. Bunun anlamı bir veri noktası modellenmemiş sonuçlarındaki başkalarıyla uncorrelated, diğer bir deyişle, tahmin edilemez. Örneğin, kaydedilen veri sonraki subway train ulaşan kadar geçecek dakika sayısını ise, bir gün sonraya alınan iki ölçümlere istatistiksel olarak bağımsızdır. Ancak, bir dakika sonraya alınan iki ölçümlere istatistiksel olarak bağımsız değildir - değer bir diğerinin değeri yüksek oranda Tahmine dayalı.

* **Artırılmış karar ağacı regresyonu** özellik örtüşmesi veya özellikler arasındaki etkileşimi avantajlarından yararlanır. Bir özellik değerini herhangi belirli veri noktasına başka bir değeri biraz Tahmine dayalı, anlamına gelir. Örneğin, günlük yüksek/düşük sıcaklık verileri güne ait düşük sıcaklık bilerek en yüksek makul bir tahmin yapmanızı sağlar. İki özellik içinde yer alan bilgileri biraz gereksizdir.

* Veri sınıflandırma ikiden fazla kategoriye yapılabilir bir çok sınıflı kendiliğinden sınıflandırıcı kullanarak veya iki sınıflı sınıflandırıcılar kümesinin birleştirilmesinin bir **topluluğu**. Topluluğu yaklaşımda her sınıf için ayrı iki sınıflı sınıflandırıcı vardır - her biri veri iki kategoriye ayırır: "Bu class" ve "Bu sınıfı değil." Ardından bu sınıflandırıcılar, veri noktasının doğru atamaya oy verin. Bu arkasında işletimsel ilkesidir [veya bir vs tüm çoklu sınıflar][one-vs-all-multiclass].

* Lojistik regresyon ve Bayes noktası makinesi dahil olmak üzere çeşitli yöntemler varsayar **doğrusal sınıfı sınırları**. Diğer bir deyişle, bunlar sınıfları arasındaki sınırları yaklaşık düz çizgileri (veya daha fazla genel durumda hyperplanes) olduğunu varsayar. Genellikle bu, ayrı girişimi yaptınız sonra kadar tanımadığınız verileri bir özelliğidir, ancak önceden görselleştirerek genellikle öğrenilmesi bir şey olduğunu. Sınıf sınırları çok düzensiz bakarsanız, karar ağaçları ile devam edin, harikası karar, vektör makine ya da sinir ağları destekler.

* Sinir ağları kullanılabilir kategorik değişkenlerle oluşturarak bir **işlevsiz değişkeni** her kategori için 1 durumlarda nerede kategori uygular, burada değil 0 olarak ayarlamak.


<!-- This is how you can embed a link in an image in HTML. Don't know how to do this in markdown.
<a href="http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet.pdf">
<img src="C:\Users\garye\azure-docs-pr\articles\media\machine-learning-algorithm-cheat-sheet\cheat-sheet-small.png">
</a>
-->

<!-- Module References -->
[a-z-list]: https://msdn.microsoft.com/library/azure/dn906033.aspx
[initialize-model]: https://msdn.microsoft.com/library/azure/0c67013c-bfbc-428b-87f3-f552d8dd41f6/
[k-means-clustering]: https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/
[one-vs-all-multiclass]: https://msdn.microsoft.com/library/azure/7191efae-b4b1-4d03-a6f8-7205f87be664/
