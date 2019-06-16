---
title: 'Meta verileri Düzenle: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Meta verileri Düzenle modülü Azure Machine Learning hizmetinde bir veri kümesindeki sütunları ile ilişkili meta verileri değiştirmek için kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 282652adb917450c262e08bf10c3c6e537b829e7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072713"
---
# <a name="edit-metadata-module"></a>Meta veri modülü Düzenle

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir veri kümesindeki sütunları ile ilişkili meta verileri değiştirmek için Düzenle veri modülü kullanın. Değer ve veri türü veri kümesinin meta verileri Düzenle modülü kullanımdan sonra değişir.

Normal meta veriler değişiklikler şunlar olabilir:
  
+ Boole veya sayısal bir sütun kategorik değerler olarak kabul eder.
  
+ Hangi sütunda belirten **sınıfı** etiket veya kategorilere veya tahmin etmek istediğiniz değerleri içeriyor.
  
+ Sütun özellikleri işaretleniyor.
  
+ Tarih/saat değerleri sayısal değerler ya da tam tersi değiştiriliyor.
  
+ Sütunları yeniden adlandırma.
  
 Bir sütunun tanımı, bir aşağı akış modül gereksinimlerini karşılamak üzere genellikle değiştirmenize gerek zaman Düzenle meta verileri kullanın. Örneğin, bazı Modüller yalnızca belirli veri türleriyle çalışmak veya sütunları bayraklarını gibi gereken `IsFeature` veya `IsCategorical`.  
  
 Gerekli işlemi gerçekleştirdikten sonra meta verileri özgün durumuna sıfırlayabilirsiniz.
  
## <a name="configure-edit-metadata"></a>Düzenleme meta verileri yapılandırma
  
1. Azure Machine Learning denemenizi için meta verileri Düzenle modülünü ekleyin ve güncelleştirmek istediğiniz veri kümesini bağlanın. Veri kümesi altında bulabilirsiniz **veri dönüştürme** içinde **Değiştir** kategorisi.
  
1. Seçin **Sütun seçiciyi başlatın** ve çalışmak için sütun kümesini ve sütun seçin. Sütun adı veya dizin tarafından ayrı ayrı seçebilirsiniz veya sütun türüne göre bir grup seçebilirsiniz.  
  
1. Seçin **veri türü** Seçili sütunları için başka bir veri türüne atama gerekiyorsa seçeneği. Belirli işlemler için veri türünü değiştirmeniz gerekebilir. Metin olarak işlenen numaraları, kaynak veri kümesi varsa, örneğin, bunları bir sayısal veri türüne matematik işlemlerinden kullanmadan önce değiştirmeniz gerekir.

    + Desteklenen veri türleri **dize**, **tamsayı**, **çift**, **Boole**, ve **DateTime**.

    + Birden çok sütun seçerseniz, meta veri değişiklikleri uygulamalısınız *tüm* Seçili sütunlar. Örneğin, iki veya üç sayısal Sütunları Seç varsayalım. Tüm veri türüne dönüştürün ve bunları tek bir işlemde yeniden adlandır dizeye değiştirebilirsiniz. Ancak, bir tamsayı olarak bir float bir dize veri türünde bir sütun ve başka bir sütuna değiştiremezsiniz.
  
    + Yeni bir veri türü belirtmezseniz, sütun meta verileri değiştirilmez.

    + Meta verileri Düzenle işlemi gerçekleştirdikten sonra sütun türü ve değerlerini değiştirir. Sütun veri türü sıfırlamak için meta verileri Düzenle'ı kullanarak istediğiniz zaman orijinal veri türünü kurtarabilirsiniz.  

    > [!NOTE]
    > Herhangi bir türde bir sayıya değiştirirseniz **DateTime** yazın, bırakın **tarih/saat biçimi** alanını boş bırakın. Şu anda hedef veri biçimi belirtmek mümkün değildir.  

1. Seçin **kategorik** seçili olan sütunlardaki değerleri kategori olarak değerlendirilmesi gerektiğini belirtmek için seçeneği.

    Örneğin, 0, 1 ve 2 sayıları içeren bir sütuna sahip ancak sayı aslında "Smoker," anlama bilmeniz "Non-smoker" ve "Bilinmeyen". Bu durumda, sütunun kategorik olarak işaretliyor tarafından değerleri yalnızca verileri gruplandırmak için alan ve sayısal hesaplamalar kullanıldığından emin olun.
  
1. Kullanım **alanları** Azure Machine Learning bir modeldeki verileri kullanma şeklini değiştirmek istiyorsanız seçeneği.

    + **Özellik**: Yalnızca özellik sütunlarda çalışan modülleri bir özellik olarak bir sütun bayrağı için bu seçeneği kullanın. Varsayılan olarak, tüm sütunları başlangıçta özellikleri kabul edilir.  
  
    + **Etiket**: Tahmin edilebilir öznitelik veya hedef değişkeni olarak da bilinen olan etiketi işaretlemek için bu seçeneği kullanın. Bu tam olarak bir etiket sütun kümesinde mevcut olduğundan fazla modül gerektirir.

        Çoğu durumda, bir sütunu bir sınıf etiket içeren Azure Machine Learning çıkarabilir. Bu meta veriler ayarlayarak sütunu doğru tanımlanmamış emin olabilirsiniz. Bu ayar, veri değerlerini değiştirmez. Bu, yalnızca bazı makine öğrenimi algoritmaları verileri işlemek şeklini değiştirir.
  
    > [!TIP]
    > Bu kategoriye uymayan veri var mı? Örneğin, Veri kümenizi değişkenleri olarak faydalı olmayan benzersiz tanımlayıcıları gibi değerler içerebilir. Bazen bu tür kimlikleri bir modelde kullanıldığında sorunlara neden olabilir.
    >
    > Neyse ki, veri kümesinden tür sütunları silmek zorunda kalmazsınız Azure Machine Learning, verilerinizin tamamını tutar. Bazı özel sütun kümesini işlemleri gerektiğinde, yalnızca diğer tüm sütunlar geçici olarak kullanarak kaldırma [kümesindeki sütunları seçme](select-columns-in-dataset.md) modülü. Dataset nesnesine kullanarak sütunları daha sonra birleştirebilirsiniz [Sütun Ekle](add-columns.md) modülü.  
  
1. Önceki seçimleri Temizle ve meta verileri varsayılan değerlere geri yüklemek için aşağıdaki seçenekleri kullanın.  
  
    + **Açık özellik**: Özellik bayrağını kaldırmak üzere bu seçeneği kullanın.  
  
         Tüm sütunları, başlangıçta özellikleri kabul edilir. Matematiksel işlemler gerçekleştiren modüllerinin için değişkenler olarak kabul sayısal sütunları engellemek için bu seçeneği kullanmak gerekebilir.
  
    + **Açık etiketi**: Kaldırmak için bu seçeneği kullanın **etiket** belirtilen sütun meta verileri.  
  
    + **Puan Temizle**: Kaldırmak için bu seçeneği kullanın **puanı** belirtilen sütun meta verileri.  
  
         Şu anda açıkça bir sütun Azure Machine learning'de bir puan olarak işaretlenemiyor. Ancak, bazı işlemler bir puan dahili olarak işaretlenmiş bir sütunda neden. Ayrıca, özel bir R modülü puanı değerleri çıkış.

1. İçin **yeni sütun adları**, seçili olan sütunda veya sütun yeni bir ad girin.  
  
    + Sütun adları, UTF-8 tarafından desteklenen karakterler kullanabilir kodlama. Boş dizeler, null ya da tamamen boşluklardan oluşamaz adları izin verilmez.  
  
    + Birden çok sütunu yeniden adlandırmak için sütun dizinleri sırasına göre virgülle ayrılmış bir liste olarak adlarını girin.  
  
    + Seçili tüm sütunları yeniden adlandırılması gerekir. Atlayın veya sütunları Atla değiştirilemez.  
  
1. Denemeyi çalıştırın.  

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmeti için.
