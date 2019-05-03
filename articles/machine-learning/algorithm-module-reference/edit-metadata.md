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
ms.openlocfilehash: cfee607aca155b6cf68e5bddc40eb9c752df5e34
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028823"
---
# <a name="edit-metadata-module"></a>Meta veri modülü Düzenle

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir veri kümesindeki sütunları ile ilişkili meta verileri değiştirmek için bu modülü kullanın. Değer ve veri türü veri kümesinin kullandıktan sonra değiştirilecek **meta verileri Düzenle** modülü. 
 
Normal meta veriler değişiklikler şunlar olabilir:
  
+ Boole veya sayısal bir sütun kategorik değerler olarak kabul eder.  
  
+ Hangi sütunda belirten *sınıfı* etiket veya kategorilere veya tahmin etmek istediğiniz değerleri  
  
+ Sütunları özellikleri işaretleme
  
+ Sayısal bir değer veya tam tersi değiştirme tarih/saat değerleri  
  
+ Sütunları yeniden adlandırma
  
 Kullanım [Düzenle bir sütunun tanımı, bir aşağı akış modül gereksinimlerini karşılamak üzere genellikle değiştirmenize gerek herhangi bir zamanda meta verileri. Örneğin, bazı Modüller yalnızca belirli veri türleriyle çalışmak veya sütunları bayraklarını gibi gereken `IsFeature` veya `IsCategorical`.  
  
 Gerekli işlemi gerçekleştirdikten sonra meta verileri özgün durumuna sıfırlayabilirsiniz. 
  
## <a name="configure-edit-metadata"></a>Düzenleme meta verileri yapılandırma
  
1.  Azure Machine Learning'de ekleme [meta verileri Düzenle](./edit-metadata.md) modülü, denemenize ve güncelleştirmek istediğiniz veri kümesine bağlanın. Bunun altında bulabilirsiniz **veri dönüştürme**, **Değiştir** kategorisi.
  
2.  Tıklayın **Sütun seçiciyi başlatın** ve çalışmak için sütun kümesini ve sütun seçin. Ada veya dizine göre tek tek sütun komutunu seçebilir veya sütunların türüne göre bir grup seçebilirsiniz.  
  
3.  Seçin **veri türü** Seçili sütunları için başka bir veri türüne atama gerekiyorsa seçeneği. Verileri değiştirme türü belirli işlemler için gerekli: metin olarak işlenen numaraları, kaynak veri kümesi varsa, örneğin, bunları bir sayısal veri türüne matematik işlemlerinden kullanmadan önce değiştirmeniz gerekir. 

    + Desteklenen veri türleri `String`, `Integer`, `Double`, `Boolean`, `DateTime`. 

    + Birden çok sütun seçiliyse, meta veri değişiklikleri uygulamalısınız **tüm** Seçili sütunlar. Örneğin, 2-3 sayısal sütunları seçtiğiniz varsayalım. Bir dize veri türünde tüm bunları değiştirmek ve bunları tek bir işlemde yeniden adlandır. Ancak, bir tamsayı olarak bir float bir dize veri türünde bir sütun ve başka bir sütuna değiştiremezsiniz.
  
    + Yeni bir veri türü belirtmezseniz sütun meta verileri değiştirilmez. 
    
    + Sütun türünü ve değerleri değiştirilecek sonra gerçekleştirmek [meta verileri Düzenle](./edit-metadata.md) işlemi. Özgün veri türü kullanarak herhangi bir zamanda kurtarabilirsiniz [meta verileri Düzenle](./edit-metadata.md) sütun veri türü sıfırlanır.  

    > [!NOTE]
    > Herhangi bir türde bir sayıya değiştirirseniz **DateTime** yazın, bırakın **tarih/saat biçimi** alanını boş bırakın. Şu anda, hedef veri biçimi belirtmek mümkün değildir.  

      
4.  Seçin **kategorik** seçili olan sütunlardaki değerleri kategori olarak değerlendirilmesi gerektiğini belirtmek için seçeneği. 

    Örneğin, 0,1 ve 2 sayıları içeren bir sütuna sahip ancak sayıları gerçekten "Smoker", "Non-smoker" ve "Bilinmeyen" ortalama bildirin. Bu durumda, sütunun kategorik olarak işaretliyor tarafından değerleri yalnızca verileri gruplandırmak için sayısal hesaplamalarında kullanılmaz emin olabilirsiniz. 
  
5.  Kullanım **alanları** Azure Machine Learning bir modeldeki verileri kullanma şeklini değiştirmek istiyorsanız seçeneği.

    + **Özellik**: Yalnızca özellik sütunlarda çalışan modülleri ile kullanmak için bir özellik olarak bir sütun bayrağı için bu seçeneği kullanın. Varsayılan olarak, tüm sütunları başlangıçta özellikleri kabul edilir.  
  
    + **Etiket**: Etiket (diğer adıyla tahmin edilebilir öznitelik veya hedef değişkeni) işaretlemek için bu seçeneği kullanın. En az bir (ve tek) çok sayıda modülleri gerektiren etiket sütun kümesinde mevcut olabilir. 
    
        Çoğu durumda, Azure Machine Learning sınıf etiket sütunu içeriyor, ancak bu meta veriler ayarlayarak sütunu doğru tanımlanmamış sağlayabilirsiniz çıkarabilir. Bu ayar yalnızca bazı makine öğrenimi algoritmaları verileri işleyecek şekilde veri değerlerini değiştirmez.
  

  
    > [!TIP]
    >  Bu kategoriye uymayan veri var mı?  Örneğin, Veri kümenizi değişkenleri olarak kullanışlı olmayan benzersiz tanımlayıcıları gibi değerler içerebilir. Bazen kimlikleri bir modelde kullanıldığında sorunlara neden olabilir. 
    >   
    >  Neyse ki tür sütunları veri kümesinden Sil zorunda kalmamak için "perde" Azure Machine Learning, verilerinizi korur. Bazı özel sütun kümesini işlemleri gerektiğinde, yalnızca diğer tüm sütunlar geçici olarak kullanarak kaldırma [kümesindeki sütunları seçme](./select-columns-in-dataset.md) modülü. Dataset nesnesine kullanarak sütunları daha sonra birleştirebilirsiniz [Sütun Ekle](./add-columns.md) modülü.  
  
6. Önceki seçimleri Temizle ve meta verileri varsayılan değerlere geri yüklemek için aşağıdaki seçenekleri kullanın.  
  
    + **Açık özellik**: Özellik bayrağını kaldırmak üzere bu seçeneği kullanın.  
  
         Tüm sütunları başlangıçta özellikleri, matematiksel işlemler gerçekleştiren modüllerinin işlendiğinden değişkenler olarak kabul sayısal sütunları önlemek için bu seçeneği kullanmak gerekebilir.
  
    + **Açık etiketi**: Kaldırmak için bu seçeneği kullanın **etiket** belirtilen sütun meta verileri.  
  
    + **Puan Temizle**: Kaldırmak için bu seçeneği kullanın **puanı** belirtilen sütun meta verileri.  
  
         Şu anda özelliği açıkça bir sütun bir puan olarak işaretlemek için Azure Machine Learning'de kullanılamıyor. Ancak, bazı işlemler bir puan dahili olarak işaretlenmiş bir sütunda neden. Ayrıca, özel bir R modülü puanı değerleri çıkış.
  
  
7.  İçin **yeni sütun adları**, seçili olan sütunda veya sütun yeni adı yazın.  
  
    + Sütun adları, UTF-8 tarafından desteklenen karakterler kullanabilir kodlama. Boş dizeler, null ya da tamamen boşluklardan oluşan adları izin verilmez.  
  
    + Birden çok sütunu yeniden adlandırmak için sütun dizinleri sırasına göre virgülle ayrılmış bir liste olarak adlarını yazın.  
  
    + Seçili tüm sütunları yeniden adlandırılması gerekir. Atlayın veya sütunları Atla değiştirilemez.  
  
  
8.  Denemeyi çalıştırın.  

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 