---
title: 'Veri Normalleştir: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Bir veri kümesinde dönüştürmek için Azure Machine Learning hizmetinde Normalleştir veri modülü kullanmayı öğrenirsiniz *normalleştirme*...
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 95069bafa94770511c7ee40e82068960298fd6c5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029453"
---
# <a name="normalize-data-module"></a>Veri modülü normalleştirin

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir veri kümesinde dönüştürmek için bu modülü kullanın *normalleştirme*.

Normalleştirme, machine Learning veri hazırlama bir parçası olarak sıklıkla uygulanan bir tekniktir. Normalleştirme sayısal sütunlardaki değerleri veya kaybeden bilgileri aralıklarını deforme etme farklılıkları olmadan bir ortak ölçek kullanılacak veri kümesini değerlerini değiştirmek için hedefidir. Normalleştirme, verileri doğru şekilde modellemek bazı algoritmalar için de gereklidir.

Örneğin, 0 ile 1 arasında değişen değerler içeren bir sütun ve 10. 000 ' 100.000 arasında değişen değerlere sahip başka bir sütuna, giriş veri kümesi içerdiğini varsayalım. Büyük fark *ölçek* numaralarını değerleri sırasında modelleme Özellikleri birleştirmek denediğinizde sorunlara neden olabilir.

*Normalleştirme* modelinde kullanılan tüm sayısal sütunlarda uygulanan bir ölçek içindeki değerleri tanırken genel dağıtım ve kaynak verilerde oranlarını korumak yeni değerleri oluşturarak bu sorunları ortadan kaldırır.

Bu modül, sayısal verileri dönüştürme için çeşitli seçenekler sunar:

- Tüm değerleri 0-1 olarak değiştirebileceğiniz ölçeklendirin ya da değerlerini yüzdelik yerine mutlak değerlerini sıralar gibi bunları temsil ederek dönüştürün.
- Tek bir sütun veya aynı veri kümesindeki birden fazla sütuna normalleştirme uygulayabilirsiniz.
- Denemeyi yineleyin veya diğer verilerle aynı normalleştirme adımları uygulamak gerekiyorsa, adımları normalleştirme dönüşüm kaydedebilir ve aynı şemaya sahip başka bir veri kümeleri için geçerlidir.

> [!WARNING]
> Bazı algoritmalar, veri modeli eğitimi önce normalleştirilmiş gerektirir. Diğer algoritmalar, kendi veri ölçekleme veya normalleştirme gerçekleştirin. Bu nedenle, bir makine öğrenme algoritmasına Tahmine dayalı bir model oluşturmaya kullanmayı seçtiğinizde algoritması veri gereksinimleri için eğitim verilerini normalleştirme uygulamadan önce gözden geçirmek emin olun.

##  <a name="configure-normalize-data"></a>Yapılandırma verileri normalleştirin

Bu modül kullanılarak aynı anda yalnızca bir normalleştirme yöntemi uygulayabilirsiniz. Bu nedenle, aynı normalleştirme yöntemi, seçtiğiniz tüm sütunları uygulanır. Farklı bir normalleştirme yöntemini kullanmak için ikinci bir örneğini kullan **Normalleştir veri**.

1. Ekleme **Normalleştir veri** denemenizi modülü. ' % S'modülü, Azure Machine Learning altında bulabilirsiniz **veri dönüştürme**, **ölçek ve Reduce** kategorisi.

2. Tüm sayı en az bir sütunu içeren bir veri kümesine bağlanın.

3. Sütun seçiciyi'leri normalleştirmek için sayısal sütunları seçmek için kullanın. Varsayılan olarak tek tek sütun seçmezseniz **tüm** giriş sayısal tür sütunları dahil edilir ve aynı normalleştirme işlemi için seçili tüm sütunları uygulanır. 

    Normalleştirilmiş olmamalıdır sayısal sütunları içeriyorsa bu garip sonuçlara neden olabilir! Her zaman sütunları dikkatle gözden geçirin.

    Sayısal bir sütun yok algılanırsa, sütunun veri türünü desteklenen bir sayısal tür olduğunu doğrulamak için sütun meta verileri denetleyin.

    > [!TIP]
    > Belirli bir türdeki sütunların giriş olarak sağlandığından emin olmak için kullanmayı deneyin [kümesindeki sütunları seçme](./select-columns-in-dataset.md) önce Modülü **Normalleştir veri**.

4. **İşaretlendiğinde, sabit sütunların için 0 kullanın**:  Tek bir değişmeyen değer herhangi bir sayısal sütun içeriyorsa bu seçeneği belirleyin. Bu tür sütunlar normalleştirme işlemleri kullanılmaz sağlar.

5. Gelen **dönüştürme yöntemi** açılan listesinde, seçili tüm sütunları uygulamak için tek bir matematiksel işlev seçin. 
  
    - **Zscore**: Tüm değerleri bir z puan dönüştürür.
    
      Sütundaki değerleri aşağıdaki formül kullanılarak dönüştürülür:  
  
      ![z kullanarak normalleştirme&#45;puanları](media/module/aml-normalization-z-score.png)
  
      Mean ve standart sapma hesaplanır her sütun için ayrı olarak. Popülasyon standart sapması kullanılır.
  
    - **MinMax**: Min-maks normalizer doğrusal olarak her bir özellik [0,1] aralığında yeniden ölçeklenir.
    
      [0,1] aralığının ölçeklendirme, en düşük değeri 0'dır, her bir özellik değerlerini değiştirme ve ardından (olan özgün düzeyde ve minimum değerler arasındaki farkın) yeni düzeyde değerine göre ayırma tarafından gerçekleştirilir.
      
      Sütundaki değerleri aşağıdaki formül kullanılarak dönüştürülür:  
  
      ![min kullanarak normalleştirme&#45;işlevi en fazla](media/module/aml-normalization-minmax.png "AML_normalization minmax")  
  
    - **Lojistik**: Sütundaki değerleri aşağıdaki formül kullanılarak dönüştürülür:

      ![normalleştirme Lojistik işlevi tarafından formülü](media/module/aml-normalization-logistic.png "AML_normalization Lojistik")  
  
    - **LogNormal**: Bu seçenek, tüm değerlerin lognormal kadar bir ölçekte dönüştürür.
  
      Sütundaki değerleri aşağıdaki formül kullanılarak dönüştürülür:
  
      ![Formül günlük&#45;normal dağıtım](media/module/aml-normalization-lognormal.png "AML_normalization lognormal")
    
      Türü veriler her sütun için en fazla olasılığını tahmin ayrı olarak hesaplanma dağıtım parametrelerini μ ve σ aşağıda verilmiştir.  
  
    - **TanH**: Tüm değerler için bir hiperbolik tanjantı dönüştürülür.
    
      Sütundaki değerleri aşağıdaki formül kullanılarak dönüştürülür:
    
      ![tanh işlevi kullanarak normalleştirme](media/module/aml-normalization-tanh.png "AML_normalization tanh")

6. Denemeyi çalıştırın ya da çift **Normalleştir veri** modülü ve select **Seçileni**. 

## <a name="results"></a>Sonuçlar

**Normalleştir veri** modül iki çıkışı üretir:

- Dönüştürülmüş değerleri görüntülemek için modülü sağ tıklayın, **dönüştürülmüş veri kümesi**, tıklatıp **Görselleştir**.

    Varsayılan olarak, değerleri yerinde dönüştürülür. Özgün değerlerine dönüştürülen değerler karşılaştırmak istediğiniz kullanırsanız [Sütun Ekle](./add-columns.md) rahatça veri kümeleri ve yan yana sütunları görüntülemek için modülü.

- Başka bir benzer veri kümesine aynı normalleştirme yöntemi uygulayabilmesi dönüşümü kaydetmek için modülün sağ tıklayın, **dönüştürme işlevi**, tıklatıp **dönüştürme Kaydet**.

    Ardından gelen kaydedilmiş dönüştürmeleri yükleyebilir **dönüştüren** sol gezinti bölmesinde, Grup ve aynı şemaya sahip bir veri kümesi kullanarak geçerli [. / dönüştürme uygulamak](apply-transformation.md).  


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 