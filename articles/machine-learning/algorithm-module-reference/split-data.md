---
title: 'Verileri bölme: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Verileri bölme modülünün Azure Machine Learning hizmetinde bir veri kümesi iki ayrı kümeleri halinde bölmek için kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: a7395280ed9a2e9dcb94a081f0b3bf10a28da719
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029408"
---
# <a name="split-data-module"></a>Verileri bölme Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

İki farklı kümeye bir veri kümesini ayırmak için bu modülü kullanın.

Bu modül, verileri eğitim ve test etme halinde ayırmak ihtiyacınız olduğunda özellikle yararlıdır. Veriler de bölünür şekilde özelleştirebilirsiniz. Bazı seçenekler rastgele veri desteği: diğer belirli bir veri türü veya model türü için uyarlanabilir.

## <a name="how-to-configure"></a>Yapılandırma

> [!TIP]
> Bölme modu seçmeden önce gereksinim duyduğunuz bölünmüş türünü belirlemek için tüm seçenekleri okuyun.
> Bölme modu değiştirirseniz, diğer tüm seçenekler sıfırlayabiliyor.

1. Ekleme **verileri bölme** denemenizde arabirimi modülü. Bu modül altında bulabilirsiniz **veri dönüştürme**, **örnek ve bölme** kategorisi.

2. **Bölme modu**: Sahip olduğunuz veriler ve onu bölmek istiyorsanız nasıl türüne bağlı olarak aşağıdaki modlarından birini seçin. Her ayırma modu farklı seçenekler vardır. Ayrıntılı yönergeler ve örnekler için aşağıdaki konulara tıklayın. 

    - **Satırları bölün**: Verileri iki parçalara bölmek istiyorsanız bu seçeneği kullanın. Her bölmedeki koymak için verilerin yüzde belirtebilirsiniz, ancak varsayılan olarak, 50 50 veriler bölünür.

        Ayrıca, her grupta satır seçimi rastgele yap ve stratified örnekleme kullanabilirsiniz. Stratified örnekleme değerleri iki sonucu veri kümeleri arasında eşit olarak apportioned istediğiniz verilerin tek bir sütun seçmelisiniz.  

    - **Normal ifade bölünmüş** tek bir sütunda bir değer için test ederek kümeniz bölmek istiyorsanız bu seçeneği belirleyin.

        Örneğin, yaklaşımı analiz, metin alanındaki belirli bir ürün adının varlığını denetleyin ve satırları hedef ürün adı ve olmayanlar ile veri kümesi bölün.

    - **Göreli ifade bölünmüş**:  Bir sayı sütunu için bir koşul uygulamak istediğinizde bu seçeneği kullanın. Sayı, tarih/saat alanı, bir sütun içeren yaş veya tutarlara veya bile yüzde olabilir. Örneğin, Veri kümenizi gruba yaş aralıklara göre veya bir takvim tarihi olarak ayrı veri öğeleri maliyetini bağlı olarak bölmek isteyebilirsiniz.

### <a name="split-rows"></a>Satırları bölün
1.  Ekleme [verileri bölme](./split-data.md) denemenizde arabirimi modülüne ve bölmek istediğiniz veri kümesine bağlanın.
  
2.  İçin **bölme modu**, seçin **satırları bölün**. 

3.  **İlk çıkış veri kümesinde satır kesiri**. Satır sayısı (soldaki) ilk çıkış Git belirlemek için bu seçeneği kullanın. Diğer tüm satırları, ikinci (sağdaki) çıktıyı geçer.

    Oran satırları ilk çıkış veri kümesi için 0 ile 1 arasında ondalık bir sayı yazmalısınız böylece gönderilen yüzdesini temsil eder.
     
     0,75 değeri olarak yazın, örneğin, veri kümesini ilk çıkış veri kümesine gönderilen satırların %75 ve % 25'İkinci çıktı veri kümesi için gönderilen bir 75:25 oranı kullanılarak ayrılır.
  
4. Seçin **Randomized bölünmüş** iki gruba veri seçimi rastgele seçmek istiyorsanız seçeneği. Eğitim ve test veri kümesi oluştururken tercih edilen seçenek budur.

5.  **Rastgele bir tohum**: Sözde rastgele dizi kullanılacak örnekleri başlatmak için bir negatif olmayan tamsayı değer girin. Bu varsayılan çekirdek rastgele sayılar üretin tüm modüllerdeki kullanılır. 

     Bir çekirdek belirtme sonuçları genellikle yeniden üretilebilen yapar. Bölme işlemi sonuçlarını yinelemek gerekirse, rastgele sayı üretici için bir çekirdek belirtmeniz gerekir. Aksi halde rastgele bir tohum varsayılan ilk çekirdek değeri sistem saatinden elde edilen anlamına gelen 0 olarak ayarlanır. Sonuç olarak, verilerin dağılımını bölme gerçekleştirdiğiniz her zaman biraz farklı olabilir. 

6. **Stratified bölünmüş**: Bu seçenek kümesine **True** iki çıkış veri kümeleri için temsili bir örnek değerler içerdiğinden emin olmak için *strata sütun* veya *stratification anahtar sütunu*. 

    Her bir çıkış veri kümesi kabaca her hedef değer aynı yüzdesini alır, veri stratified örnekleme ile ayrılmıştır. Örneğin, eğitim ve test etme ve sonuca göre kabaca dengelendiğinden emin olmak istiyorsanız veya ile cinsiyet gibi diğer bazı sütun ot algıla.

7. Denemeyi çalıştırın.


## <a name="regular-expression-split"></a>Normal ifade Böl

1.  Ekleme [verileri bölme](./split-data.md) modülü, denemenize ve bölmek istediğiniz veri kümesinin girdi olarak bağlanın.  
  
2.  İçin **bölme modu**seçin **normal ifade bölünmüş**.

3. İçinde **normal ifade** geçerli bir normal ifade yazın. 
  
   Normal ifade Python normal ifade söz dizimini izlemeniz gerekir.


4. Denemeyi çalıştırın.

    Sağladığınız normal ifadeye göre veri kümesini iki satır kümeleri halinde bölünmüştür: ifade ve kalan tüm satırların aynı değerleri içeren satır. 

## <a name="relative-expression-split"></a>Göreli ifade bölün.

1. Ekleme [verileri bölme](./split-data.md) modülü, denemenize ve bölmek istediğiniz veri kümesinin girdi olarak bağlanın.
  
2. İçin **bölme modu**seçin **göreli ifade bölünmüş**.
  
3. İçinde **ilişkisel ifade** metin kutusuna bir tek bir sütun üzerinde bir karşılaştırma işlemi gerçekleştirdiğinde ifade yazın:


 - Sayısal sütun:
    - Tarih/saat veri türleri dahil olmak üzere, herhangi bir sayısal veri türü sayıda sütun içeriyor.

    - İfade, bir sütun adı en fazla başvurabilirsiniz.

    - Karakterini kullanın (&) kanal ve işlem ve kullanımı için veya işleci (|) karakter.

    - Aşağıdaki işleçleri desteklenir: `<`, `>`, `<=`, `>=`, `==`, `!=`

    - İşlemleri kullanarak gruplandıramazsınız `(` ve `)`.

 - Dize sütunu: 
    - Aşağıdaki işleçleri desteklenir: `==`, `!=`



4. Denemeyi çalıştırın.

    İfade bir veri kümesi iki satır kümeleri halinde böler: koşula uyan değerlere sahip satırları ve tüm kalan satırlar.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 