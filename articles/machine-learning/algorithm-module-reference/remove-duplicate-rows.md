---
title: 'Yinelenen satırları kaldırır: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Yinelenen satırları Kaldır modülü Azure Machine Learning hizmetinde bir veri kümesinden olası yinelenenleri kaldırmak için kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: dce90d911085c1f7330a2e0952bb9576c1d765fa
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029303"
---
# <a name="remove-duplicate-rows-module"></a>Yinelenen satırları modülü kaldırıldı

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir veri kümesinden olası yinelenenleri kaldırmak için bu modülü kullanın.

Örneğin, verileriniz aşağıdakine benzer olacaktır ve hastalar için birden çok kayıt temsil varsayalım. 

| PatientID | Baş harfleri| Cinsiyet|Yaş|Kabul edilen|
|----|----|----|----|----|
|1|F.M.| M| 53| Oca|
|2| F.A.M.| M| 53| Oca|
|3| F.A.M.| M| 24| Oca|
|3| F.M.| M| 24| Şub|
|4| F.M.| M| 23| Şub|
| | F.M.| M| 23| |
|5| F.A.M.| M| 53| |
|6| F.A.M.| M| NaN| |
|7| F.A.M.| M| NaN| |

NET bir şekilde, bu örnek, potansiyel olarak yinelenen verilerle birden çok sütun içeriyor. Aslında çoğaltmaları olup olmadıkları bilginiz verilerin bağlıdır. 

+ Örneğin, birçok hastalara aynı ada sahip biliyor olabilirsiniz. Yalnızca hiçbir adı sütun kullanarak yinelemeleri ortadan mıydı **kimliği** sütun. Bu şekilde, yalnızca yinelenen kimliği değerlere sahip satırları, hastaların aynı ada veya olup bağımsız olarak filtrelenir.

+ Alternatif olarak, Kimliği alanında atlanmasına izin ver ve başka bir kombinasyonuna dosya adı, son adı, yaşı ve cinsiyet gibi benzersiz kayıtları bulmak için karar verebilirsiniz.  

Bir satır yinelenen olup olmamasına ölçütlerini ayarlamak için tek bir sütun veya sütunlar olarak kullanmak üzere bir dizi belirttiğiniz **anahtarları**. İki satır yinelenen olarak kabul edilir yalnızca değerlerde **tüm** anahtar sütunları eşit. Herhangi bir satır için eksik değer varsa **anahtarları**, yinelenen satırları değerlendirilmeyecek. Yukarıdaki tabloya, 6 ve 7 satır anahtarlarını yinelenen satırları verilen olmadığından cinsiyet ve yaş ayarlarsanız, örneğin, değeri eksik yaşı sahiptirler.

Modül çalıştırdığınızda, bir aday veri kümesi oluşturur ve belirtilen sütun kümesi arasında yinelemelere sahip satır kümesini döndürür.

> [!IMPORTANT]
> Kaynak veri kümesi değiştirilemez; Bu modül, çoğaltmaları, belirttiğiniz ölçütlere göre hariç tutmak için filtrelenmiştir yeni bir veri kümesi oluşturur.

## <a name="how-to-use-remove-duplicate-rows"></a>Yinelenen satırları Kaldır'ı kullanma

1. Modül için denemenizi ekleyin. Bulabilirsiniz **yinelenen satırları Kaldır** modülü altında **veri dönüştürme**, **işleme**.  

2. Yinelenen satırlar için kontrol etmek istediğiniz veri kümesine bağlanın.

3. İçinde **özellikleri** bölmesi altında **anahtar sütun seçimi filtre ifadesi**, tıklayın **Sütun seçiciyi Başlat**, çoğaltmaları tanımlanırken kullanılacak sütunları seçmek için.

    Bu bağlamda **anahtar** benzersiz bir tanımlayıcı anlamına gelmez. Sütun seçiciyi kullanarak seçtiğiniz tüm sütunları olarak atanan **anahtar sütunları**. Tüm seçili olmayanların haricindeki sütunları, anahtar olmayan sütun olarak kabul edilir. Kayıtların benzersiz anahtarlar olarak seçtiğiniz sütunları birleşimi belirler. (Bunu, birden çok equalities birleştirmeler kullanan bir SQL deyimi düşünün.)

    Örnekler:

    + "Kimlikleri benzersiz olduğundan emin olmak istiyorum": Yalnızca kimlik sütunu seçin.
    + "Ad, Soyadı ve kimliği birleşimi benzersiz olduğundan emin olmak istiyorum": Üç sütunu seçin.

4. Kullanım **tut ilk yinelenen satır** çoğaltmaları bulunduğunda döndürülecek satır belirtmek için onay kutusunu:

    + Seçtiyseniz, ilk satırda verilir ve diğerlerinin atıldı. 
    + Bu seçeneğin işaretini kaldırırsanız, sonuçlarda son yinelenen satır tutulur ve diğerleri yoksayılır. 

5. Denemeyi çalıştırın.

6. Sonuçları gözden geçirmek için modülün sağ tıklayın, **sonuçları veri kümesi**, tıklatıp **Görselleştir**. 

> [!TIP]
> Sonucu anlaşılması zor olan veya göz bazı sütunları dışlamak istiyorsanız, kullanarak sütunları kaldırabilirsiniz [kümesindeki sütunları seçme](./select-columns-in-dataset.md) modülü.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 