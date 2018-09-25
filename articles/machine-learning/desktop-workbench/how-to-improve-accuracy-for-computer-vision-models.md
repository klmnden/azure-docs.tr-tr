---
title: Azure Machine learning'de bilgisayar vizyonu ve sınıflandırma modellerini daha iyi doğruluğunu
description: Bilgisayar işleme görüntü sınıflandırması, nesne algılama ve görüntü işleme için Azure Machine Learning paketini kullanarak görüntü benzerlik modellerini doğruluğunu artırmak öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ms.openlocfilehash: 6d6c540cd8711ae89784cdc797ca56d312522a83
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989320"
---
# <a name="improve-the-accuracy-of-computer-vision-models"></a>Görüntü işleme modellerini doğruluğunu artırmak

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

İle **görüntü işleme için Azure Machine Learning paketi**, derleme ve dağıtma görüntü sınıflandırması, nesne algılama ve görüntü benzerlik modelleri. Bu paket ve nasıl yükleneceği hakkında daha fazla bilgi edinin.

Bu makalede, kendi doğruluğunu artırmak için bu modeller ince ayar öğrenin. 

## <a name="accuracy-of-image-classification-models"></a>Görüntü sınıflandırma modellerini doğruluğunu

Bilgisayar işleme paket çok çeşitli veri kümeleri için iyi sonuçlar verir için gösterilir. Ancak, çoğu makine öğrenme projeleri, en iyi olası alma için true olarak yeni bir veri kümesi için sonuçları gerektirir farklı tasarım kararlarına değerlendirme yanı sıra dikkatli parametresi ayarlama. Aşağıdaki bölümler belirli bir veri kümesini doğruluğunu artırmak konusunda rehberlik sağlar, diğer bir deyişle, ilk olarak, hangi değerleri bu parametrelerin birini deneyin ve hangi sorunları önlemek için en iyi duruma taahhüdü parametreleri.

Genel olarak bakıldığında, eğitimi derin öğrenme modelleri ile birlikte gelen eğitim süresini doğruluğu karşı arasında bir denge. Bilgisayar işleme paketin genellikle yüksek doğruluk modelleri üretilirken hızlı eğitim hızına odaklanan önceden ayarlanmış varsayılan parametreleri (ilk satırı aşağıdaki tabloya bakın) gerekir. Bu doğruluğu genellikle daha fazla, örneğin, daha yüksek görüntü çözünürlüğünü veya derin modelleri, ancak kullanılarak geliştirilebilir, 10 kat veya daha fazla faktörüyle eğitim süresini artırır.

Varsayılan parametreleri'ilk iş, bir model eğitip, sonuçları inceleyin, gerektiğinde çığır truth ek açıklamaları düzeltin ve ancak bundan sonra eğitim süresini yavaş parametreleri deneyin önerilir (önerilen parametre değerleri aşağıdaki tabloya bakın). Teknik olarak gerekli sırada bu parametreleri bir anlayış ancak önerilir.


### <a name="best-practices-and-tips"></a>En iyi uygulamalar ve ipuçları

* Veri Kalitesi: yüksek kaliteli eğitim ve test kümelerini olmalıdır. Diğer bir deyişle, görüntüleri açıklamalı olan doğru belirsiz görüntüleri kaldırıldı (tenis topu veya bir Limonlu resmi gösterir, örneğin bir İnsan gözüyle belirsiz olduğu) ve öznitelikleri karşılıklı olarak birbirini dışlar (diğer bir deyişle, her bir görüntü için tam olarak bir öznitelik ait).

* DNN iyileştirme önce SVM sınıflandırıcı önceden eğitilmiş ve sabit bir DNN kullanarak özelliği Oluşturucu eğitim verilmelidir. Bu bilgisayar işleme paketinde desteklenir ve DNN değiştirilmez beri eğitmek için uzun gerektirmez. Basit bu yaklaşım genellikle iyi bir doğruluk elde eder ve bu nedenle sağlam bir temel temsil eder. Daha büyük doğruluk vermelisiniz DNN iyileştirmek için sonraki adım ise.

* Görüntüde faiz, nesne küçükse, görüntü sınıflandırma yaklaşımları düzgün çalışmıyor bilinmektedir. Böyle durumlarda, bilgisayar işleme paketin daha hızlı R-CNN Tensorflow üzerinde temel gibi bir nesne algılama yaklaşım kullanmayı düşünün.

* Daha fazla eğitim verilerini daha iyi. Bir kural-ın-yapacağı işlemleri aşağıdakilerden en az 100 örnekler, her sınıf için diğer bir deyişle, 100 görüntüleri "köpek", "cat", vb. 100 görüntüler olması gerekir. Daha az görüntülerle modeli mümkündür, ancak iyi sonuçlar oluşturamayabilir.

* Eğitim resmi GPU makinede yerel olarak bulunur ve bir SSD sürücüsü (HDD değil) olması gerekir. Aksi takdirde, gelen görüntü okuma gecikme süresi, eğitim hızı (faktörüyle bile 100 x) önemli ölçüde azaltabilir.


### <a name="parameters-to-optimize"></a>Parametreleri en iyi duruma getirme

En iyi değerleri bu parametrelerin bulma önemlidir ve genellikle doğruluğu önemli ölçüde artırabilirsiniz:
* Öğrenme oranı (`lr_per_mb`): doğru hale getirmek için en önemli tartışmaya parametre. DNN iyileştirme ~ %5 sonra büyük olasılıkla öğrenme oranı ya da çok yüksek ise eğitim doğruluğu ayarlarsanız veya eğitim dönemlerinde çok düşük sayısı. Eğitim verileri aşırı uygun, ancak uygulamada bu test için daha iyi modelleri önünü açacak ayarlar ile özellikle küçük veri kümeleri, DNN eğilimi gösterir. Genellikle 15 dönemlerinde öğrenme oranı iki kez azalır burada kullanıyoruz; Bazı durumlarda, daha fazla dönemler kullanarak eğitim performansını iyileştirebilir.

* Giriş çözünürlüğü (`image_dims`): varsayılan görüntü çözünürlüğünü 224 x 224 pikseldir. Daha yüksek görüntü çözünürlüğü birini, örneğin, 500 x 500 piksel veya 1000 x 1000 piksel doğruluğu önemli ölçüde artırabilir ancak DNN iyileştirme yavaşlatır. Bilgisayar işleme paketi, renk kanal sayısı 3'e (kırmızı yeşil mavi bantları) ayarlamak sahip olduğu, (3, 224, 224), örneğin bir tanımlama grubu (renk kanallarını, görüntü genişliğini, resim yüksekliği) için giriş çözünürlüğünün bekliyor.

* Model mimarisi (`base_model_name`): yerine varsayılan ResNet-18 modelini ResNet-34 veya ResNet-50 gibi daha ayrıntılı Dnn'leri kullanmayı deneyin. Resnet-50 model yalnızca daha kapsamlı değildir, ancak çıktısını sondan katmanın olduğundan boyutu 2048 float (vs. 512 float ResNet-18 ve ResNet-34 modellerin). Bu artan işlenemez sabit ve bunun yerine bir SVM sınıflandırıcı eğitim DNN tutma, özellikle yararlı olabilir.

* Minibatch boyutu (`mb_size`): yüksek minibatch boyutları neden olacak için daha hızlı eğitim süresini ancak artan bir DNN bellek tüketimi çoğaltamaz. Bu nedenle, daha derin modelleri (örneğin, ResNet-50 ResNet-18 karşı) ve/veya daha yüksek görüntü çözünürlüğünü seçerken (500\*224 karşı 500 piksel\*224 piksel), genellikle varsa bellek yetersiz hatalarını önlemek için minibatch boyutunu küçültmek. Minibatch boyutu değiştirirken, genellikle de öğrenme oranı aşağıdaki tabloda görüldüğü gibi ayarlanması gerekir.
* Dışı bırakma oranı (`dropout_rate`) ve L2 regularizer (`l2_reg_weight`): (varsayılan değer 0,5 bilgisayar işleme paketindeki) 0,5 ya da daha fazla düşme oranı kullanarak ve regularizer ağırlık artırarak DNN atlayarak Sığdırma'nin indirgenebilir (varsayılan değer 0.0005, görüntü işleme Paketi). Ancak özellikle küçük veri kümeleriyle DNN sabit ve önlemek imkansız genellikle fazla sığdırma olduğunu unutmayın.


### <a name="parameter-definitions"></a>Parametre tanımları

- **Öğrenme oranı**: adım gradyan düşüşü öğrenimi sırasında kullanılan boyutu. Çok yüksek kümesi sonra modeli iyi bir çözüme yakınsama değil, çok düşük kümesi sonra modeli eğitmek için birçok dönemlerinde alacaksa. Genellikle bir zamanlama öğrenme oranı belirli sayıda dönemlerinde sonra azalır burada kullanılır. Zamanlama E.For örnek öğrenme oranı `[0.05]*7 + [0.005]*7 + [0.0005]` bir öğrenme oranı 0.05 için ilk yedi dönemlerinde kullanarak karşılık gelen, için başka bir yedi dönemlerinde 0.005 azaltılmış öğrenme hızı x bir 10 arkasından ve son olarak, tek bir dönem için model hassas ayar yapma bir 100 x ile 0.0005 öğrenme oranı azalır.

- **Minibatch boyutu**: GPU'ları Paralel hesaplama hızlandırmak için birden çok görüntü işleyebilir. Paralel işlenen görüntülerin bir minibatch da adlandırılır. Yüksek hızlı eğitim minibatch boyutu, ancak olur artan bir DNN bellek tüketimi çoğaltamaz.

### <a name="recommended-parameter-values"></a>Önerilen parametre değerleri

Aşağıdaki tabloda, yüksek doğruluk modelleri çok çeşitli görüntü sınıflandırma görevleri üretmek için gösterilen farklı parametre kümeleri sağlar. Belirli bir veri kümesini ve kullanılan tam GPU en uygun parametreleri bağlıdır, bu nedenle tablo yalnızca bir kural olarak başlatmasında gösterilmelidir. Bu parametreleri denedikten sonra görüntü çözünürlüğü 500 x 500'den fazla piksel veya daha ayrıntılı modelleri Resnet-101 veya Resnet-152 gibi düşünün.

Tablodaki ilk satır, bilgisayar işleme paketin ayarlanmış varsayılan parametrelere karşılık gelir. Diğer tüm satırları (ilk sütununda gösterilen) eğitmek ancak daha uzun sürer, artırılmış doğruluk Avantajı (ikinci sütun için ortalama kesinlik üç iç veri kümeleri üzerinde bakın). Örneğin, son sıradaki parametre, ancak artan (ortalama) doğruluğu, %82.6 %92.8 üç iç test kümesi olarak sonuçlanan x 5-15 eğitmek için uzun alır.

Daha fazla DNN bellek derin modelleri ve daha yüksek giriş çözünürlüğünün yararlanın ve bu nedenle minibatch boyutu çıkış, bellek hatalarını önlemek için daha fazla model karmaşıklığı ile sınırlı gerekir. Aşağıdaki tabloda görüldüğü gibi aynı çarpan tarafından minibatch azalan boyut her iki faktörüyle öğrenme oranı azaltmak yararlıdır. Minibatch boyutu azaltıldı gerekebilir daha küçük miktarlarda bellek ile GPU'ları hakkında daha fazla.

| Eğitim süresini (kabaca bir tahmin) | Örnek doğruluğu | Minibatch boyutu (*mb_size*) | Öğrenme oranı (*lr_per_mb*) | Görüntü çözünürlüğü (*image_dims*) | DNN mimarisi (*base_model_name*) |
|------------- |:-------------:|:-------------:|:-----:|:-----:|:---:|
| 1 x (başvuru) | %82.6 | 32 | [0,05] \*7 + [0.005]\*7 + [0.0005]  | (3, 224, 224) | ResNet18_ImageNet_CNTK |
| 2-5 x    | %90.2 | 16 | [0,025] \*7 + [0.0025]\*7 + [0.00025] | (3, 500, 500) | ResNet18_ImageNet_CNTK |
| 2-5 x    | % 87,5 | 16 | [0,025] \*7 + [0.0025]\*7 + [0.00025] | (3, 224, 224) | ResNet50_ImageNet_CNTK |
| 5-15 x        | %92.8 |  8 | [0,01] \*7 + [0,001]\*7 + [0,0001]  | (3, 500, 500) | ResNet50_ImageNet_CNTK |


## <a name="next-steps"></a>Sonraki adımlar

Görüntü işleme için Azure Machine Learning paketi hakkında bilgi almak için:
+ Başvuru belgeleri inceleyin

+ Hakkında bilgi edinin [diğer Azure Machine Learning Python paketleri](reference-python-package-overview.md)