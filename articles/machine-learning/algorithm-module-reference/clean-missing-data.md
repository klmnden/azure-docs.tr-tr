---
title: 'Eksik verileri temizleme: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Eksik verileri temizleme modülü, Kaldır, Değiştir veya eksik değerler çıkarsamak için Azure Machine Learning hizmetinde kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: de81204219a102734f1820258a3c32e59a64c685
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028793"
---
# <a name="clean-missing-data-module"></a>Clean Missing Data Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Kaldır, Değiştir veya eksik değerler çıkarsamak için bu modülü kullanın. 

Veri bilimcileri, veri eksik değerler için sık sık kontrol edin ve sonra veri düzeltmek veya yeni değerler eklemek için çeşitli işlemleri gerçekleştirin. Temizleme gibi işlemleri amacı bir modeli eğitimindeki ortaya çıkabilecek eksik verilerin neden olduğu sorunları önlemektir. 

Bu modül, "dahil olmak üzere, eksik değerleri temizleme" için işlem birden çok türünü destekler:

+ Bir yer tutucu, ortalama veya başka bir değer eksik değerleri değiştirme
+ Satırları ve sütunları eksik değerleri olan tamamen kaldırma
+ İstatistiksel yöntemlere dayalı değerleri çıkarımını yapma


Bu modül kullanarak, kaynak veri kümesi değişmez. Bunun yerine, sonraki iş akışında kullanabileceğiniz çalışma alanınızda yeni bir veri kümesi oluşturur. Ayrıca, yeni, temizlenen veri kümesini yeniden kullanmak üzere kaydedebilirsiniz.

Bu modül, ayrıca bir tanımı eksik değerleri temizlemek için kullanılan Dönüşümü çıkarır. Bu dönüşüm kullanarak aynı şemaya sahip başka bir veri kümeleri üzerinde yeniden kullanabileceğiniz [uygulamak dönüştürme](./apply-transformation.md) modülü.  

## <a name="how-to-use-clean-missing-data"></a>Eksik verileri temizleme kullanma

Bu modül bir temizleme işlemi tanımlamanızı sağlar. Böylece, daha sonra yeni verilere uygulayan temizleme işlemi de kaydedebilirsiniz. Oluşturma ve Temizleme işleminin Kaydet açıklaması için aşağıdaki bağlantılara bakın: 
 
+ Eksik değerleri değiştirmek için
  
+ Yeni veri temizleme bir dönüştürme uygulamak için
 
> [!IMPORTANT]
> Eksik değerleri işlemek için kullandığınız yöntemin temizleme sonuçlarınız önemli ölçüde etkileyebilir. Farklı yöntemlerle denemeniz önerilir. Her iki gerekçe kullanmak için belirli bir yöntemi sonuç kalitesini ve göz önünde bulundurun.

### <a name="replace-missing-values"></a>Eksik değerleri Değiştir  

Uyguladığınız her zaman [eksik verileri temizleme](./clean-missing-data.md) modülü bir veri kümesi, aynı temizleme işlemi için seçtiğiniz tüm sütunlar için uygulanır. Farklı yöntemler kullanarak farklı sütunları temizlemek gerekiyorsa, bu nedenle, modül ayrı örneklerini kullanın.

1.  Ekleme [eksik verileri temizleme](./clean-missing-data.md) modülü, denemenize ve eksik değerleri olan bir veri kümesine bağlanın.  
  
2.  İçin **temizlenecek sütunları**, değiştirmek istediğiniz eksik değerler içeren sütunları seçin. Birden çok sütunu seçebilirsiniz, ancak tüm seçili olan sütunlardaki aynı değiştirme yöntemi kullanmanız gerekir. Bu nedenle, genellikle, dize sütunlarındaki ve sayısal sütunları ayrı olarak temizlemek gerekir.

    Örneğin, tüm sayısal sütunlara eksik değerleri denetlemek için şunu yazın:

    1. Sütun seçiciyi açın ve seçin **kuralları ile**.
    2. İçin **ŞUNUNLA Başla**seçin **Hayır sütunları**.

        Ayrıca tüm sütunları ile başlayın ve ardından sütunlarını hariç tutun. Başlangıçta, kurallar ilk tıklatırsanız gösterilmez **tüm sütunları**, ancak tıklayabilirsiniz **Hayır sütunları** ve ardından **tüm sütunları** yeniden tüm sütunları ile başlayın ve ardından filtrelemek için veri türü, adına göre (dahil değil) sütunları veya sütun dizini.

    3. İçin **INCLUDE**seçin **sütun türü** seçin ve açılan listeyi **sayısal**, ya da daha belirli bir sayısal tür. 
  
    Seçtiğiniz temizleme ya da değiştirme yöntemi uygulanabilir **tüm** sütun seçimi. Herhangi bir sütun verilerinde belirtilen işlemle uyumlu değilse, modül hata verir ve denemeyi durdurur.
  
3.  İçin **Minimum değer oranı eksik**, işlemin gerçekleştirilmesi için gerekli değerleri eksik sayısı alt sınırını belirtin.  
  
    Bu seçeneği ile birlikte kullanmanız **maksimum değer oranı eksik** altında bir temizleme işlemi gerçekleştirildi veri kümesinde koşulları tanımlayın. Değerleri eksik çok fazla veya çok az sayıda satır varsa, işlem gerçekleştirilemiyor. 
  
    Temsil girdiğiniz sayı **oranı** sütundaki tüm değerleri için eksik değer. Varsayılan olarak, **Minimum eksik değer oranı** özelliğini 0 olarak ayarlayın. Bu, eksik olan tek değer olsa bile, eksik değerlerin temizlenmesi anlamına gelir. 

    > [!WARNING]
    > Bu durum uygulamak belirtilen işlem için sırayla her sütun tarafından karşılanması gerekir. Örneğin, üç sütun seçilmedi ve eksik değerlerin minimum oranı (% 20).2 için ayarlayın, ancak yalnızca bir sütun gerçekten % 20 eksik değerler olduğunu varsayalım. Bu durumda, temizleme işlemi sütunu yalnızca % 20'den eksik değerleri ile geçerlidir. Bu nedenle diğer sütunları değişmeden olacaktır.
    > 
    > Eksik değerleri değiştirilip hakkında herhangi bir şüpheli varsa bu seçeneği seçin **eksik değer gösterge sütunu oluşturmak**. Bir sütun, her sütun için minimum ve maksimum aralıkları belirtilen ölçütleri karşılanıyorsa olup olmadığını belirtmek için veri kümesine eklenir.  
  
4. İçin **maksimum değer oranı eksik**, işlemin gerçekleştirilmesi için mevcut olabilecek eksik değerlerin maksimum sayısını belirtin.   
  
    Örneğin, yalnızca % 30 veya daha az satır eksik değerler içeren ancak değerleri olarak bırakırsanız eksik değer değiştirme gerçekleştirmek isteyebileceğiniz-% 30'den fazla satır eksik değerleri tanımlanmadığı durumdur.  
  
    Sütundaki tüm değerleri için eksik değerleri oranı olarak sayısını tanımlayın. Varsayılan olarak, **maksimum değer oranı eksik** 1 olarak ayarlayın. Bu sütundaki değerleri %100 eksik olsa bile, eksik değerleri temizlenir anlamına gelir.  
  
   
  
5. İçin **temizleme modu**değiştirmek için aşağıdaki seçeneklerden birini seçin veya eksik kaldırma değerleri:  
  
  
    + **Özel bir değiştirme değeri**: Tüm eksik değerleri için geçerli bir yer tutucu değerini (örneğin, 0 veya ad) belirtmek için bu seçeneği kullanın. Bunun yerine belirttiğiniz değer sütunun veri türü ile uyumlu olması gerekir.
  
    + **Ortalama ile değiştirin**: Sütun ortalamasını hesaplar ve ortalama, her sütun eksik bir değer değiştirme değeri kullanır.  
  
        Yalnızca Integer, Double sahip sütun veya Boole veri türleri için geçerlidir.  
  
    + **ORTANCA ile değiştirin**: Sütunun ORTANCA değeri hesaplar ve sütundaki eksik olan bir değer için değiştirme ORTANCA değer kullanır.  
  
        Integer veya Double veri türlerine sahip sütunlar için geçerlidir. 
  
    + **Modu ile değiştirmek**: Sütun modunu hesaplar ve her eksik sütun değeri değiştirme değeri modunu kullanır.  
  
        Integer, Double, Boole veya kategorik veri türlerine sahip sütunlar için geçerlidir. 
  
    + **Tüm satırı Kaldır**: Tamamen herhangi bir satır kümesindeki bir veya daha fazla eksik değerleri olan kaldırır. Eksik değeri rastgele olarak eksik kabul edilebilir değilse, bu yararlıdır.  
  
    + **Tümünü sütunu kaldırmak**: Tamamen herhangi bir sütuna sahip bir veya daha fazla eksik değerleri kümesinde kaldırır.  
  
    
  
6. Seçenek **değiştirme değeri** seçeneği belirlendiğinde kullanılabilir **özel değiştirme değeri**. Sütundaki tüm eksik değerler için değiştirme değeri kullanılacak yeni bir değer yazın.  
  
    Bu seçenek yalnızca Integer, Double, Boole veya tarih veri türlerine sahip sütunlarda kullanmanız gerektiğini unutmayın. Tarih sütunları için değiştirme değeri de 100 nanosaniyelik tıklarının sayısını 1/1/0001 beri girilebilir 12: 00'da  
  
7. **Eksik değer gösterge sütunu oluşturmak**: Sütundaki değerleri eksik değerlerin temizlenmesi için ölçüt uyulup bazı göstergesini çıkış istiyorsanız bu seçeneği belirleyin. Olduğunda, yeni bir temizleme işlemi ayarlama ve tasarlandığı gibi çalıştığından emin olmak istiyorsanız, bu seçenek özellikle yararlıdır.
  
8. Denemeyi çalıştırın.

### <a name="results"></a>Sonuçlar

Modül iki çıkışı döndürür:  

-   **Temizlenen veri kümesini**: Bu seçeneği seçtiğinizde Seçili sütunlar eksik değerleri oluşan bir veri kümesi olarak belirtilen bir gösterge sütunu birlikte işlenir.  

    Temizleme işlemi için seçili olmayan sütunları da "ile geçirilen".  
  
-  **Dönüştürme Temizleme**: Çalışma alanına kaydedilir ve daha sonra yeni verilere uygulanan temizlemek için kullanılan bir veri dönüştürme.

### <a name="apply-a-saved-cleaning-operation-to-new-data"></a>Kaydedilmiş bir temizleme işlemi yeni verilere uygulayan  

Temizleme işlemleri genellikle yinelemek gerekirse, da, Yemek tarifi olarak veri temizleme için kaydetmenizi öneririz bir *dönüştürme*, aynı veri kümesiyle birlikte yeniden kullanmak için. Sık sık yeniden içeri aktarın ve ardından aynı şemaya sahip verileri temizledik, temizleme dönüştürme kaydetme özellikle yararlıdır.  
      
1.  Ekleme [uygulamak dönüştürme](./apply-transformation.md) denemenizi modülü.  
  
2.  Temizlemek istediğiniz veri kümesini eklemek ve veri kümesine sağ giriş bağlantı noktasına bağlanın.  
  
3.  Genişletin **dönüştüren** arabiriminin sol bölmesinden grubu. Kaydedilen dönüşümü bulun ve alanına sürükleyin.  
  
4.  Kaydedilen dönüşümü sol giriş bağlantı noktasına bağlayın [uygulamak dönüştürme](./apply-transformation.md). 

    Kaydedilmiş bir dönüşüm uyguladığınızda, dönüştürme uygulanır sütun seçilebilir. Dönüştürme zaten tanımlanmış olan ve özgün işleminde belirtilen sütunları otomatik olarak uygulandığı olmasıdır.

    Ancak, bir sayısal bir sütun alt kümesi üzerinde bir dönüştürme oluşturduğunuz düşünün. Yalnızca eşleşen sayısal sütunlarında bulunan eksik değerleri değiştiğinden bir hata yükseltmeden, bu dönüşümü karma sütun türleri için bir veri kümesi uygulayabilirsiniz.

6.  Denemeyi çalıştırın.  

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 