---
title: 'Sinir ağı regresyon: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Sinir ağı regresyon modülü özelleştirilebilir sinir ağı algoritması kullanarak bir regresyon modeli oluşturmak için Azure Machine Learning hizmetinde kullanmayı öğrenin...
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: bc6a7505ab09e929e5add61eea687f871aedf242
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029318"
---
# <a name="neural-network-regression-module"></a>Sinir ağı regresyon Modülü

*Bir sinir ağı algoritması kullanarak bir regresyon modeli oluşturur*  
  
 Kategori: Makine öğrenimi / Model Başlat / regresyonu
  
## <a name="module-overview"></a>Modülüne genel bakış  

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Özelleştirilebilir sinir ağı algoritması kullanarak bir regresyon modeli oluşturmak için bu modülü kullanın.
  
 Sinir ağları yaygın olarak kullanılmak üzere derin öğrenme ve görüntü tanıma gibi karmaşık sorunlarını modellemekten bilinse de, gerileme sorunları kolayca uyarlanmış. Uyarlamalı ağırlıkları kullanın ve doğrusal olmayan işlevleri kendi girişlerinin yaklaşık istatistiksel modellerin herhangi bir sınıf bir sinir ağı terimiyle gösterilen. Bu nedenle sinir ağı regresyon daha geleneksel bir regresyon modeli bir çözüm burada sığamıyorsa sorunları için uygundur.
  
 Sinir ağı regresyon denetimli öğrenme yöntemidir ve bu nedenle bir *etiketli veri kümesini*, bir etiket sütun içerir. Etiket sütunu, bir regresyon modeli tahmin eden bir sayısal değer olduğundan, bir sayısal veri türü olması gerekir.  
  
 Girdi olarak model ve etiketli veri kümesini sağlayarak modeli eğitmek [modeli eğitme](./train-model.md). Eğitim modeli, ardından yeni giriş örnekleri için değerleri tahmin etmek için kullanılabilir.  
  
## <a name="configure-neural-network-regression"></a>Sinir ağı regresyon yapılandırın 

Sinir ağları kapsamlı olarak özelleştirilebilir. Bu bölüm iki yöntemi kullanarak bir model oluşturmayı açıklar:
  
+ [Varsayılan mimariyi kullanarak bir sinir ağı model oluşturma](#bkmk_DefaultArchitecture)  
  
    Varsayılan sinir ağı mimarisi kabul ederseniz, kullanın **özellikleri** bölmesi gibi gizli katman, öğrenme oranı ve normalleştirme düğüm sayısını sinir ağı davranışını denetleyen parametrelerini ayarlamak için.

    Sinir ağları yeniyseniz buradan başlayın. Modül, birçok özelleştirmeler yanı sıra model, derin sinir ağları bilginiz dışında ayarlama destekler. 

+ Bir sinir ağı için özel bir mimari tanımlayın 

    Çok gizli katmanları ekleyin veya tam ağ mimarisi, bağlantılar ve etkinleştirme işlevleri özelleştirmek istiyorsanız bu seçeneği kullanın.
    
    Bu sinir ağları ile biraz bilginiz varsa en iyi seçenektir. Net # dil ağ mimarisi tanımlamak için kullanın.  

##  <a name="bkmk_DefaultArchitecture"></a> Varsayılan mimariyi kullanarak bir sinir ağı model oluşturma
  
1.  Ekleme **sinir ağı regresyon** denemenizde arabirimi modülü. Bu modül altında bulabilirsiniz **Machine Learning**, **başlatmak**, **regresyon** kategorisi. 
  
2. Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.  
  
    -   **Tek bir parametre**: Model yapılandırmak istediğiniz nasıl zaten biliyorsanız, bu seçeneği belirleyin.  

3.  İçinde **katman belirtimi gizli**seçin **çalışması'tam olarak bağlı**. Bu seçenek, bu özniteliklere sahip bir sinir ağı regresyon modeli için varsayılan sinir ağı mimarisi kullanarak bir model oluşturur:  
  
    + Ağ tam olarak bir gizli katman vardır.
    + Çıkış katmanı tam olarak gizli katmana bağlı ve gizli katmanın tam olarak giriş katmana bağlı.
    + Gizli katmandaki düğüm sayısını, kullanıcı tarafından ayarlanabilir (varsayılan değer: 100).  
  
    Giriş katmandaki düğüm sayısını eğitim verilerini özellikleri sayısı tarafından belirlenir çünkü bir regresyon modeli olabilir yalnızca tek bir düğüme çıkış katmanda.  
  
4. İçin **gizli düğüm sayısını**, gizli düğüm sayısını yazın. 100 düğümü olan bir gizli katmanın varsayılandır. (Bu seçenek Net # kullanarak özel bir mimari tanımlarsanız kullanılabilir değildir.)
  
5.  İçin **öğrenme oranı**, düzeltme önce her bir yineleme sırasında gerçekleştirilen adım tanımlayan bir değeri yazın. Öğrenme oranı için daha büyük bir değer daha hızlı yakınsanmasını modeli neden olabilir, ancak yerel en düşük değerleri overshoot.

6.  İçin **sayısı, yinelemeler öğrenme**, algoritma eğitim durumları işleyen saatleri maksimum sayısını belirtin.

7.  İçin ** öğrenme ağırlık verme çapında, bir düğüm ağırlıkları öğrenme sürecinin başlangıcında belirleyen bir değer yazın.

8.  İçin **İtici Güç**, düğümlerde ağırlık olarak öğrenme sırasında önceki yinelemelerin uygulamak için bir değer yazın.

10. Seçeneğini **karışık örnekler**yinelemeleri arasındaki çalışmalarının sırasını değiştirmek için. Bu seçeneğin işaretini kaldırırsanız, servis talepleri denemeyi çalıştırma her zaman tam olarak aynı sırada işlenir.
  
11. İçin **rastgele sayı doldurma**, isteğe bağlı olarak temel kullanılacak bir değer yazın. Aynı deneyde çalıştırmaları arasında yinelenebilirliği sağlamak istediğinizde bir çekirdek belirten değer yararlı olur.
  
13. Bir eğitim veri kümesi ve bir bağlama [eğitim modülleri](module-reference.md): 
  
    -   Ayarlarsanız **Oluştur trainer modu** için **tek parametre**, kullanın [modeli eğitme](./train-model.md).  
  
   
14. Denemeyi çalıştırın.  

## <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra:

+ Ağırlıkları öğrenilen eğitim ve diğer parametreleri sinir ağı modelinin parametreler, bu özellik bir özetini görmek için çıkışını sağ tıklayın [modeli eğitme](./train-model.md)seçip **Görselleştir**.  

+ Eğitilen model anlık görüntüsünü kaydetmek için sağ **Trained modeli** seçin ve çıkış **eğitilen modeli olarak Kaydet**. Bu model, aynı denemede bunu izleyen çalışır güncelleştirilmez.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 