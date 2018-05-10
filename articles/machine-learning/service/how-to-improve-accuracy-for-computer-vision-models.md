---
title: Daha iyi doğruluğu, Azure Machine Learning bilgisayar görme & sınıflandırma modelleri
description: Bilgisayar görme görüntü Sınıflandırma, nesne algılama ve Azure Machine Learning paket için bilgisayar Vizyon kullanarak görüntü benzerlik modelleri doğruluğunu artırmak öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ms.openlocfilehash: e134e1e544c51d6756d5021fef8c049fe7d8afb0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="improve-the-accuracy-of-computer-vision-models"></a>Bilgisayar görme modelleri doğruluğunu artırmak

İle **bilgisayar görme için Azure Machine Learning paketi**, derleme ve görüntü sınıflandırma dağıtma nesne algılama ve görüntü benzerlik modeller. Bu paket ve nasıl yükleneceği hakkında daha fazla bilgi edinin.

Bu makalede, kuruluşların doğruluğunu artırmak için bu modeller ince ayar öğrenin. 

## <a name="accuracy-of-image-classification-models"></a>Görüntü sınıflandırma modelleri doğruluğunu

Bilgisayar görme paket çok çeşitli veri kümeleri için iyi sonuçlar vermek için gösterilir. Ancak, projeler öğrenme, en iyi olası alma çoğu makine için doğru olduğu gibi yeni bir veri kümesi için sonuçları gerektirir farklı tasarım kararlarına değerlendirme yanı sıra dikkatli parametresini ayarlama. Aşağıdaki bölümlerde verilen bir veri kümesi üzerinde doğruluğunu artırmak nasıl kılavuzluk, hangi parametreler diğer bir deyişle, ilk olarak, bu parametreleri için değerleri birini deneyin ve hangi Tuzaklar önlemek için en iyi duruma en taahhüdü.

Genel olarak bakıldığında, eğitim derin öğrenme modelleri ile birlikte gelen model doğruluğundan karşı eğitim saat arasında bir denge. Bilgisayar görme paket (aşağıdaki tabloda ilk satırına bakın) genellikle yüksek doğruluk modelleri üretilirken hızlı eğitim hızına odaklanmak önceden ayarlanmış varsayılan parametre yok. Bu doğruluğu genellikle daha fazla, örneğin, daha yüksek görüntü çözünürlüğü veya derin modeller, ancak kullanılarak geliştirilebilir 10 kat veya daha fazla faktörüyle eğitim süresini artırmak artması pahasına olur.

İlk olarak, varsayılan parametreler ile çalışma, bir modeli eğitmek, sonuçları inceleyin, gerektiği şekilde düzeltin başından başlayarak gerçekte ek açıklamaları ve ancak bundan sonra eğitim süresini yavaş parametreleri deneyin önerilir (önerilen parametre değerlerini aşağıdaki tabloya bakın). Bu parametreler gerekli teknik olmayan sırada bir anlayış ancak önerilir.


### <a name="best-practices-and-tips"></a>En iyi yöntemler ve ipuçları

* Veri Kalitesi: eğitim ve test kümeleri kaliteli olmalıdır. Diğer bir deyişle, görüntüleri açıklama doğru belirsiz görüntüleri kaldırıldı (görüntü tenis topu veya bir Limonlu gösteriyorsa örneğin İnsan göze belirsiz olduğu) ve öznitelikleri karşılıklı olarak birbirini dışlar (diğer bir deyişle, her görüntü için tek bir özniteliği ait).

* DNN daraltmayı önce SVM sınıflandırıcı önceden eğitilmiş ve sabit DNN featurizer kullanarak Eğitilecek. Bu bilgisayar görme paketinde desteklenir ve DNN değiştirilmeyen beri eğitmek için uzun gerektirmez. Basit bu yaklaşım, genellikle iyi accuracies erişir ve bu nedenle güçlü bir taban çizgisi temsil eder. Daha iyi doğruluk vermelidir DNN iyileştirmek için sonraki adım ise.

* Görüntüde ilgi, nesne küçükse, görüntü sınıflandırma yaklaşımlar düzgün çalışmıyor bilinmektedir. Böyle durumlarda, bir nesne algılama yaklaşım bilgisayar görme paketin daha hızlı R-CNN üzerinde Tensorflow esasında kullanmayı düşünün.

* Daha fazla eğitim verileri daha iyi. Bir kural-in-Flash, şunlardan en az 100 örnekler her sınıf için diğer bir deyişle, 100 görüntüleri "köpek", "kat", vb. için 100 görüntüleri olmalıdır. Daha az yansıma ile bir model eğitim mümkündür, ancak iyi sonuçlar doğurabilir değil.

* Eğitim görüntüleri GPU makinede yerel olarak bulunan ve bir SSD sürücüsü (HDD değil) olması gerekir. Aksi takdirde, görüntü okuma gelen gecikme eğitim hızı (faktörüyle bile 100 x) büyük ölçüde azaltabilir.


### <a name="parameters-to-optimize"></a>En iyi duruma getirmek için parametreleri

Bu parametreler için en uygun değerleri bulma önemlidir ve genellikle doğruluğunu önemli ölçüde iyileştirebilen:
* Öğrenme hızı (`lr_per_mb`): sağ almak için tartışmaya açık bir şekilde en önemli parametre. ~ %5, DNN iyileştirme tamamlandıktan sonra büyük olasılıkla öğrenme oranı ya da çok yüksek ise eğitim doğruluğu ayarlarsanız veya çok düşük eğitim dönemlerinde sayısı. Eğitim verileri aşırı sığacak, ancak bu test için iyi modelleri götürür uygulamada ayarlamak için özellikle küçük veri kümeleri ile DNN eğilimi gösterir. Genellikle 15 dönemlerinde burada ilk öğrenme oranı iki kez azaltılmış kullanıyoruz; Daha fazla dönemler kullanarak eğitim bazı durumlarda, performansı geliştirebilir.

* Giriş çözünürlük (`image_dims`): varsayılan görüntü çözünürlüğü 224 x 224 pikseldir. Daha yüksek görüntü çözünürlüğü, örneğin, 500 x 500 kullanarak piksel veya 1000 x 1000 piksel doğruluğunu önemli ölçüde iyileştirebilen ancak DNN iyileştirme yavaşlatır. Bilgisayar görme paket, 3 (kırmızı yeşil mavi bantları) ayarlamak renk kanal sayısını sahip olduğu, (3, 224, 224), örneğin giriş çözümlemesinin bir tanımlama grubu (renk kanalları, görüntü genişliği, Görüntü yüksekliği) olması için bekliyor.

* Model mimarisi (`base_model_name`): ResNet 34 ya da ResNet 50 gibi daha derin DNNs yerine varsayılan ResNet 18 modelini kullanmayı deneyin. Resnet 50 model yalnızca daha derin değil, ancak çıktısını sondan katmanın boyutu 2048 float (vs. 512 float ResNet 18 ve ResNet 34 modellerin). Bu artan boyut sabit ve bunun yerine bir SVM sınıflandırıcı eğitim DNN tutma özellikle yararlı olabilir.

* Minibatch boyutu (`mb_size`): yüksek minibatch boyutları sağlama daha hızlı eğitim süresi için ancak bir artan DNN bellek tüketimi ödün verme pahasına. Bu nedenle, daha derin modelleri (örneğin, ResNet-50 ResNet 18 karşı) ve/veya daha yüksek görüntü çözünürlüğü seçerken (500\*224 karşı 500 piksel\*224 piksel), genellikle varsa bellek yetersiz hatalarını önlemek için minibatch boyutunu azaltma. Minibatch boyutu değiştirirken, genellikle ayrıca öğrenme oranı aşağıdaki tabloda görüldüğü şekilde ayarlanması gerekir.
* Açılan kullanıma oranı (`dropout_rate`) ve L2 regularizer (`l2_reg_weight`): (varsayılan değer 0,5 bilgisayar görme paketindeki) 0,5 ya da daha fazla düşme oranını ve regularizer ağırlık artırarak DNN atlayarak Sığdırma'nin indirgenebilir (varsayılan olarak bilgisayarın görme 0.0005 Paketi). Ancak özellikle küçük veri kümeleri ile DNN sabit ve önlemek genellikle imkansız atlayarak sığdırma olduğunu unutmayın.


### <a name="parameter-definitions"></a>Parametre tanımları

- **Öğrenme hızı**: adım gradyan düşüşü öğrenme sırasında kullanılan boyutu. Çok yüksek kümesi sonra model için iyi bir çözüm birleşir değil, çok düşük kümesi sonra modeli eğitmek için birçok dönemlerinde alacaksa. Genellikle bir zamanlama öğrenme oranı dönemlerinde belirli bir sayıda sonra burada azalır kullanılır. E.For örnek öğrenme oranı zamanlama `[0.05]*7 + [0.005]*7 + [0.0005]` için ilk yedi dönemlerinde 0,05 ilk öğrenme oranı kullanarak karşılık gelen için başka bir yedi dönemlerinde 0.005 azaltılmış öğrenme hızı x 10 arkasından ve son olarak tek bir dönem için model hassas ayar yapma 100 x ile 0.0005 öğrenme hızı azaltma.

- **Minibatch boyutu**: GPU hesaplama hızlandırmak için paralel birden fazla görüntü işleyebilir. Bu paralel işlenen görüntüler minibatch da adlandırılır. Yüksek hızlı eğitim minibatch boyutu, ancak olacak bir artan DNN bellek tüketimi ödün verme pahasına.

### <a name="recommended-parameter-values"></a>Önerilen parametre değerleri

Aşağıdaki tabloda çok çeşitli görüntü sınıflandırma görevleri yüksek doğruluk modellerinde üretmek için gösterilen farklı parametre kümesi sağlar. En iyi parametreleri belirli veri kümesi ve kullanılan tam GPU bağlı, bu nedenle tablo yalnızca bir kılavuz olarak görülmelidir. Bu parametreler denendikten sonra görüntü çözünürlüğü 500 x 500'den fazla piksel ya da Resnet 101 veya Resnet 152 gibi daha derin modelleri de göz önünde bulundurun.

Tablodaki ilk satır bilgisayar görme paketin içinde ayarlanan varsayılan parametreleri karşılık gelir. Diğer tüm satırların ancak (ilk sütununda gösterilen) eğitmek için daha uzun sürer en yüksek doğruluk yararı (üç iç veri kümeleri üzerinde ortalama doğruluğu için ikinci sütununa bakın). Örneğin, son satırını parametrelerinde, ancak %82.6 %92.8 kadar olan üç iç sınama kümeleri üzerinde artan (ortalama) doğruluk ile sonuçlandı 5-15 x eğitmek için daha uzun olur.

Daha fazla DNN bellek daha derin modelleri ve daha yüksek giriş çözünürlüğü alın ve bu nedenle minibatch boyutunu genişletme, bellek hatalarını önlemek için artan modeli karmaşıklık ile sınırlı gerekir. Aşağıdaki tabloda görüldüğü gibi minibatch azalan boyut her tarafından aynı çarpanı iki faktörüyle öğrenme oranı azaltmak faydalıdır. Minibatch boyutu azaltılmış gerekebilecek küçük miktarda bellek ile GPU üzerinde daha fazla.

| Eğitim süresini (kaba tahmin) | Örnek doğruluğu | Minibatch boyutu (*mb_size*) | Öğrenme hızı (*lr_per_mb*) | Resim çözünürlüğü (*image_dims*) | DNN mimarisi (*base_model_name*) |
|------------- |:-------------:|:-------------:|:-----:|:-----:|:---:|
| 1 x (Başvurusu) | %82.6 | 32 | [0,05] \*7 + [0.005]\*7 + [0.0005]  | (3, 224, 224) | ResNet18_ImageNet_CNTK |
| 2-5 x    | %90.2 | 16 | [0.025] \*7 + [0.0025]\*7 + [0.00025] | (3, 500, 500) | ResNet18_ImageNet_CNTK |
| 2-5 x    | % 87,5 | 16 | [0.025] \*7 + [0.0025]\*7 + [0.00025] | (3, 224, 224) | ResNet50_ImageNet_CNTK |
| 5-15 x        | %92.8 |  8 | [0,01] \*7 + [0,001]\*7 + [0.0001]  | (3, 500, 500) | ResNet50_ImageNet_CNTK |


## <a name="next-steps"></a>Sonraki adımlar

Bilgisayar görme için Azure Machine Learning paketi hakkında bilgi almak için:
+ Out başvuru belgelerini denetleyin

+ Hakkında bilgi edinin [diğer Python paketlerini Azure Machine Learning için](reference-python-package-overview.md)