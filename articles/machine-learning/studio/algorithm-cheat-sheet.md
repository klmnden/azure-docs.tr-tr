---
title: Makine öğrenimi algoritması bilgi sayfası | Microsoft Docs
description: Bir yazdırılabilir makine öğrenimi algoritması bilgi sayfası Azure Machine Learning Studio'da Tahmine dayalı modelinizin doğru algoritması seçmenize yardımcı olur.
keywords: Makine öğrenme algoritmasını algoritması bilgi sayfası, kopya sayfası
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
ms.openlocfilehash: a448d6931330f7b2f0730add65473097bb2b5a57
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34833572"
---
# <a name="machine-learning-algorithm-cheat-sheet-for-microsoft-azure-machine-learning-studio"></a>Microsoft Azure Machine Learning Studio için machine learning algoritması kopya sayfası
**Microsoft Azure Machine Learning algoritmasını sayfası kopya** Tahmine dayalı analiz modelinin doğru algoritması seçmenize yardımcı olur.

[Azure Machine Learning Studio](https://studio.azureml.net/) algoritmalarından büyük kitaplığı sahip ***regresyon***, ***sınıflandırma***, ***Kümeleme***, ve  ***anomali algılama*** aileleri. Her farklı türde bir machine learning sorun gidermek için tasarlanmıştır.

## <a name="download-machine-learning-algorithm-cheat-sheet"></a>İndirin: Makine öğrenimi algoritması bilgi sayfası
**Kopya sayfası buradan indirin: [makine öğrenme algoritmasını kopya sayfası (11 x 17 inç)](http://download.microsoft.com/download/A/6/1/A613E11E-8F9C-424A-B99D-65344785C288/microsoft-machine-learning-algorithm-cheat-sheet-v6.pdf)**

![Makine öğrenimi algoritması bilgi sayfası: Machine Learning algoritmasını seçin öğrenin.][cheat-sheet]

[cheat-sheet]: ./media/algorithm-cheat-sheet/machine-learning-algorithm-cheat-sheet-small_v_0_6-01.png

Karşıdan yükle ve makine öğrenme algoritmasını kopya elinizin altında tutun ve bir algoritma seçme Yardım almak için sayfanın tabloid boyutunda yazdırın.

> [!NOTE]
> Makalesine bakın [için Microsoft Azure Machine Learning algoritmaları seçme](algorithm-choice.md) bu kopya sayfası kullanımı için ayrıntılı bir kılavuz için.
> 
> 

## <a name="more-help-with-algorithms"></a>Daha fazla yardıma algoritmaları
* Sağ algoritması yanı sıra, machine learning algoritmaları ve bunların nasıl kullanıldığı farklı türdeki daha ayrıntılı bir tartışma seçmek için bu kopya sayfası kullanarak daha fazla yardım için bkz: [Microsoft Azure Machine Learning içinalgoritmalarseçme](algorithm-choice.md).
* Algoritmaları açıklar ve örnekler sağlayan bir indirilebilir bilgi grafiği için bkz: [indirilebilir bilgi grafiği: Makine öğrenme algoritmasını örneklerle temel](basics-infographic-with-algorithm-examples.md).
* Machine Learning Studio'da kullanılabilen tüm makine öğrenimi algoritma kategoriye göre bir listesi için bkz: [modeli Başlat] [ initialize-model] Machine Learning Studio algoritması ve modülü Yardımı'nda.
* Bir tam alfabetik listesi algoritmaları ve Machine Learning Studio'daki modüller için bkz: [Machine Learning Studio modülleri A-Z listesi] [ a-z-list] Machine Learning Studio algoritması ve modülü Yardımı'nda.
* Machine Learning Studio'nun işlevlerine genel bir bakış sağlayan bir diyagram indirmek ve yazdırmak için bkz. [Azure Machine Learning Studio'nun işlevlerine genel bakış diyagramı](studio-overview-diagram.md).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="notes-and-terminology-definitions-for-the-machine-learning-algorithm-cheat-sheet"></a>Notlar ve makine öğrenimi algoritması terim tanımları kopya sayfası

* Bu algoritması bilgi sayfası sunulan yaklaşık kuralları-in-Flash önerilerdir. Bazı Bükülü ve bazı flagrantly ihlal edildi. Bu, bir başlangıç noktası önermek üzere tasarlanmıştır. Verileriniz üzerinde birkaç algoritmaları arasında head-to-head rekabet çalıştırmak korkutmasın. Her algoritma ilkelerini anlama ve verilerinizi oluşturulan sistem anlamak için sadece hiçbir yedek yoktur.

* Her makine öğrenme algoritmasını kendi stilde veya *Endüktif sapması*. Belirli bir sorun için çeşitli algoritmalar uygun olabilir ve bir algoritma diğerlerinden daha iyi bir uyum olabilir. Ancak her zaman en uygun olduğu önceden bilmesi mümkün değildir. Bu gibi durumlarda birkaç algoritmaları kopya sayfada birlikte listelenir. Uygun bir strateji bir algoritma ve sonuçları henüz tatmin edici, değilseniz, diğer deneyin olur. Bir örnek şudur [Azure AI galeri](http://gallery.cortanaintelligence.com/) aynı verilere göre çeşitli algoritmalar çalışır ve sonuçları karşılaştırır deneme: [çok sınıfı sınıflandırıcı karşılaştırın: Harf tanıma](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

* Machine learning üç ana kategoriye ayrılır: **denetimli öğrenme**, **Denetimsiz öğrenme**, ve **öğrenmeyi öğrenme**.

  * İçinde **denetimli öğrenme**, her veri noktası etiketli ya da bir kategori veya ilgi alanı değeri ile ilişkilendirilmiş.  Kategorik bir etiket örneği bir görüntü olarak 'Kat' veya 'köpek' atama.  Bir değer etiketi ile kullanılan bir araba ilişkili satış fiyatı örnektir. Bu gibi birçok etiketli örnekler incelemek için denetimli öğrenme belirtilir ve gelecekteki veri noktaları hakkında Öngörüler olması. Örneğin, yeni fotoğraflar doğru hayvan veya diğer kullanılan araba doğru satış fiyatları atama ile tanımlama. Bu makine öğrenme popüler ve kullanışlı bir türde değil. Learning algoritmaları dışında tüm Azure Machine Learning modülleri denetimli [K-ortalamaları Kümeleme][k-means-clustering].

  * İçinde **Denetimsiz öğrenme**, veri noktalarına sahip kendileriyle ilişkilendirilmiş etiket yok. Bunun yerine, bir Denetimsiz öğrenme algoritmasını verileri bazı şekilde düzenlemek için veya yapısını açıklamak için hedefidir. Bu K-ortalamaları yaptığı gibi kümeler halinde gruplandırmak veya basit görünmesi karmaşık veri arayan farklı yöntemler bulma anlamına gelebilir.

  * İçinde **öğrenmeyi öğrenme**, her veri noktası yanıtta bir eylem seçmenizi algoritma alır. Burada zaman içinde bir noktada sensör okumaları veri noktası kümesidir ve algoritma robot kullanıcının bir sonraki eylem seçmeniz gerekir robotics içinde ortak bir yaklaşımdır. Aynı zamanda bir doğal olan nesnelerin interneti uygulamalar için uygun. Öğrenme algoritmasını de ödül sinyal kısa ne kadar iyi kararı olduğunu belirten bir süre sonra alır. Algoritma bunu temel alarak, yüksek ödül elde etmek için kendi stratejisi değiştirir. Şu anda Azure ML algoritması modülleri öğrenme hiçbir öğrenmeyi vardır.

* **Bayesian yöntemleri** istatistiksel olarak bağımsız veri noktaları olduğu varsayımını yaparsınız. Bunun anlamı bir veri noktası Modellenmemiþ sonuçlarındaki başkalarıyla uncorrelated, diğer bir deyişle, bu tahmin edilemez. Örneğin, kaydedilen veri sonraki Yeraltı treni tren gelene kadar dakika sayısını ise, bir gün parçalayın alınan iki ölçümlere istatistiksel olarak bağımsızdır. Ancak, bir dakika parçalayın alınan iki ölçümlere istatistiksel olarak bağımsız olmayan - birinin değeri, diğer yüksek oranda Tahmine dayalı bir değerdir.

* **Boosted karar ağacı regresyon** özelliği çakışma veya özellikler arasındaki etkileşimi avantajlarından yararlanır. Verilen veri noktasında herhangi bir özellik değerini başka bir değeri biraz Tahmine dayalı olduğunu, anlamına gelir. Örneğin, günlük yüksek/düşük sıcaklık verileri güne ait düşük sıcaklık bilerek en yüksek makul bir tahmin yapmanızı sağlar. İki özellik yer alan bilgileri biraz gereksizdir.

* İkiden fazla kategoriye veri sınıflandırma yapılabilir kendiliğinden çok sınıfı sınıflandırıcı kullanarak veya iki sınıflı sınıflandırıcı bir dizi birleştiren bir **ensemble**. Ensemble yaklaşım her sınıf için ayrı bir iki sınıflı sınıflandırıcı yoktur - her bir veri iki kategoriye ayırır: "" ve "değil Bu sınıf." Ardından bu sınıflandırıcı veri noktasının doğru atamada oy verin. Bu arkasında işletimsel ilkesidir [veya bir vs tüm çoklu sınıflar][one-vs-all-multiclass].

* Lojistik regresyon ve Bayes noktası makinesi dahil olmak üzere çeşitli yöntemleri varsayın **doğrusal sınıfı sınırları**. Diğer bir deyişle, bunlar sınıflar arasındaki sınırların yaklaşık düz satırları (veya daha fazla genel durumda hyperplanes) olduğunu varsayar. Bu özelliği, ayırmak çalıştınız sonra kadar tanımadığınız verilerin görülür, ancak genellikle önceden görselleştirme tarafından öğrenilebilecek bir şey olduğunu. Sınıf sınırları çok düzensiz bakarsanız, karar ağaçları ile takılıyor, ormanları karar, vektör makineleri veya sinir ağları destekler.

* Sinir ağları kullanılabilir kategorik değişkenlerle oluşturarak bir **kukla değişkeni** 1 durumlarda kategori geçerli olduğu, burada değil 0 ayarı, her kategori için.


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
