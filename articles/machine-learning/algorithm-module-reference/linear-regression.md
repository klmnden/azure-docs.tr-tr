---
title: 'Doğrusal regresyon: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Doğrusal regresyon modülü Azure Machine Learning hizmetinde bir doğrusal regresyon modeli kullanmak için bir deneme oluşturmak için kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 040f726a703d34a95bae7d5b7cdd766655c62a4e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029723"
---
# <a name="linear-regression-module"></a>Doğrusal regresyon Modülü
Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir doğrusal regresyon modeli kullanmak için bir deneme oluşturmak için bu modülü kullanın.  Doğrusal regresyon, bir veya daha fazla bağımsız değişken ve sayısal sonucu veya bağımlı değişken arasında doğrusal bir ilişki kurmak çalışır. 

Bir doğrusal regresyon yöntemi tanımlamak için bu modülü kullanın ve ardından etiketlenmiş bir veri kümesi kullanan bir modeli eğitin. Eğitilen modelin ardından tahminlerde bulunmak üzere kullanılabilir.

## <a name="about-linear-regression"></a>Doğrusal regresyon hakkında

Doğrusal regresyon machine learning'de benimsenen ve uydurur ve hata ölçmek için birçok yeni yöntem ile geliştirilmiş bir ortak istatistiksel yöntemidir. En temel anlamda regresyon sayısal hedefi tahmin anlamına gelir. Basit bir model için temel bir Tahmine dayalı görevi istediğinizde doğrusal regresyon yine de iyi bir seçimdir. Doğrusal regresyon, ayrıca de yüksek boyutlu, seyrek veri karmaşıklığı eksik kümeleri üzerinde çalışmaya eğilimi gösterir.

Azure Machine Learning, regresyon modelleri, doğrusal regresyon yanı sıra çeşitli destekler. Ancak, "regresyon" terimi gevşek yorumlanabilir ve gerileme diğer araçlarla sağlanan bazı türleri desteklenmez.

+ Klasik regresyon problemi tek bir bağımsız değişken ve bağımlı bir değişken gerektirir. Bu adlandırılır *basit regresyon*.  Bu modül, basit bir regresyon destekler.

+ *Birden çok doğrusal regresyon* iki veya daha fazla bağımsız katkıda bulunan tek bir bağımsız değişkenin değişken içerir. İçinde birden fazla giriş tek bir sayısal sonucu tahmin etmek için kullanılan sorunları da verilir *çok değişkenli doğrusal regresyon*.

    **Doğrusal regresyon** modülü, bu sorunları çözebilir, çoğu diğer regresyon modülü gibi.

+ *Birden çok etiket regresyon* tek bir modelde birden çok bağımlı değişkenler hakkında tahminde görevdir. Örneğin, birden çok etiket Lojistik regresyon, bir örnek için birden çok farklı etiketler atanabilir. (Bu bir tek sınıf değişkeni içinde birden çok düzeyi tahmin görevini farklıdır.)

    Bu tür bir gerileme, Azure Machine Learning'de desteklenmiyor. Birden fazla değişken tahmin etmek için tahmin etmek istediğiniz her bir çıktı için ayrı bir learner oluşturun.

Yıl boyunca istatistikçiler regresyon için giderek daha fazla gelişmiş yöntemleri geliştirme. Bu, doğrusal regresyon için bile geçerlidir. Bu modül hatası ölçmek ve regresyon satırın sığması için iki yöntemi destekler: normal kareler yöntemi ve bir gradyan düşüşü.

- **Gradyan düşüşü** model eğitim işleminin her adımında bir hata miktarı en aza indiren bir yöntemdir. Gradyan düşüşü birçok çeşidi vardır ve kendi en iyi duruma getirme çeşitli öğrenme sorunları için kapsamlı bir şekilde eğitim. Bu seçeneği seçerseniz **çözüm yöntemi**, adım boyutunu denetlemek için parametreleri çeşitli öğrenme oranı ayarlayın ve VS. Bu seçenek, aynı zamanda bir tümleşik bir parametre tarama kullanımını destekler.

- **Sıradan kareler** doğrusal regresyon en sık kullanılan teknikleri biridir. Örneğin, kareler çözümleme araç takımı Microsoft Excel için kullanılan yöntemin olur.

    Sıradan kareler karesi alınmış hata en aza indirerek model uyarlama ve gerçek değer mesafe tahmin edilen satırına karesini toplamı olarak hata hesaplar kaybı işlevi başvurur. Bu yöntem, giriş ve bağımlı değişkenin arasında güçlü bir doğrusal ilişki varsayar.

## <a name="configure-linear-regression"></a>Doğrusal regresyon yapılandırın

Bu modül, farklı seçeneklerle bir regresyon modeli sığdırma için iki yöntemi destekler:

+ [Çevrimiçi bir gradyan düşüşü kullanarak bir regresyon modeli oluşturun](#bkmk_GradientDescent)

    Gradyan düşüşü, daha karmaşık olan veya çok küçük bir eğitim veri değişken sayısı verilen olan modeller için daha iyi bir kaybı işlevidir.



+ [Sıradan kareler kullanarak bir regresyon modeli Sığdır](#bkmk_OrdinaryLeastSquares)

    Küçük veri kümeleri için sıradan kareler seçmek en iyisidir. Bu, Excel'e benzer sonuçlar vermesi gerekir.

## <a name="bkmk_OrdinaryLeastSquares"></a> Sıradan kareler kullanarak bir regresyon modeli oluşturun

1. Ekleme **doğrusal regresyon modeli** denemenizde arabirimi modülü.

    Bu modülde bulabilirsiniz **Machine Learning** kategorisi. Genişletin **modeli Başlat**, genişletin **regresyon**ve ardından sürükleyin **doğrusal regresyon modeli** denemenizi modülü.

2. İçinde **özellikleri** bölmesinde, **çözüm yöntemi** açılan listesinden **sıradan kareler**. Bu seçenek, regresyon çizgisini bulmak için kullanılan hesaplama yöntemini belirtir.

3. İçinde **L2 Kurallaştırma ağırlık**, ağırlık L2 Kurallaştırma için kullanılacak değeri yazın. Overfitting önlemek için sıfır olmayan bir değer kullanmanızı öneririz.

     Düzenli hale getirme modeli sığdırma nasıl etkilediği hakkında daha fazla bilgi için bu makaleye bakın: [L1 ve Machine Learning için L2 düzenleme](https://msdn.microsoft.com/magazine/dn904675.aspx)

4. Seçeneğini **INCLUDE ıntercept terimi**, kesme noktası için kullanım dönemi görüntülemek istiyorsanız.

    Regresyon formülü gözden geçirmeniz gerekmiyorsa, bu seçeneğin işaretini kaldırın.

5. İçin **rastgele sayı doldurma**, isteğe bağlı olarak modeli tarafından kullanılan rastgele sayı üreticisinin sağlamak için bir değer yazın.

    Bir çekirdek değerini kullanarak, farklı aynı denemenin çalıştırmaları arasında aynı sonuçları korumak istiyorsanız kullanışlıdır. Aksi takdirde, varsayılan sistem saatinden bir değer kullanmaktır.


7. Ekleme [modeli eğitme](./train-model.md) modülü, denemenize ve etiketlenmiş bir veri kümesine bağlanın.

8. Denemeyi çalıştırın.

## <a name="results-for-ordinary-least-squares-model"></a>Sıradan kareler model için sonuçları

Alıştırma tamamlandıktan sonra:

+ Modelin parametreleri görüntülemek için trainer çıkış sağ tıklayıp **Görselleştir**.

+ Tahminde bulunmak için eğitilen modele bağlanmak [Score Model](./score-model.md) modülüyle bir veri kümesi yeni değerler. 


## <a name="bkmk_GradientDescent"></a> Çevrimiçi bir gradyan düşüşü kullanarak bir regresyon modeli oluşturun

1. Ekleme **doğrusal regresyon modeli** denemenizde arabirimi modülü.

    Bu modülde bulabilirsiniz **Machine Learning** kategorisi. Genişletin **modeli Başlat**, genişletme **regresyon**, sürükleyin **doğrusal regresyon modeli** denemenizi Modülü

2. İçinde **özellikleri** bölmesinde, **çözüm yöntemi** açılan listesinde **çevrimiçi bir gradyan düşüşü** regresyon çizgisini bulmak için kullanılan hesaplama yöntemi olarak.

3. İçin **Oluştur trainer modu**, veya önceden tanımlanmış bir parametre kümesi ile modeli eğitmek istediğiniz bir parametre tarama kullanarak model en iyi duruma getirmek isteyip istemediğinizi belirtin.

    + **Tek bir parametre**: Nasıl doğrusal regresyon ağı yapılandırmak istediğinizi biliyorsanız, bağımsız değişken olarak belirli bir değerler kümesi sağlayabilirsiniz.

   
4. İçin **öğrenme oranı**, stokastik iyileştirici için öğrenme oranı belirtin.

5. İçin **eğitim dönemlerinde sayısı**, algoritma kaç kez gösteren bir değer örneklerle yineleme türü. Az sayıda örnek veri kümeleri için bu sayı yakınsama ulaşana kadar büyük olmalıdır.

6. **Özellikleri Normalleştir**: Modeli eğitmek için kullanılan sayısal verileri zaten normalleştirilmiş, bu seçeneğin işaretini kaldırın. Varsayılan olarak, tüm sayısal girişler 0 ile 1 arasında bir aralığa modülü normalleştirir.

    > [!NOTE]
    > 
    > Yeni verilerle Puanlama için kullanılan aynı normalleştirme yöntemi uygulanacağını unutmayın.

7. İçinde **L2 Kurallaştırma ağırlık**, ağırlık L2 Kurallaştırma için kullanılacak değeri yazın. Overfitting önlemek için sıfır olmayan bir değer kullanmanızı öneririz.

    Düzenli hale getirme modeli sığdırma nasıl etkilediği hakkında daha fazla bilgi için bu makaleye bakın: [L1 ve Machine Learning için L2 düzenleme](https://msdn.microsoft.com/magazine/dn904675.aspx)


9. Seçeneğini **azaltma öğrenme oranı**yinelemeler ilerlemesi azaltmak için öğrenme oranı istiyorsanız.  

10. İçin **rastgele sayı doldurma**, isteğe bağlı olarak modeli tarafından kullanılan rastgele sayı üreticisinin sağlamak için bir değer yazın. Bir çekirdek değerini kullanarak, farklı aynı denemenin çalıştırmaları arasında aynı sonuçları korumak istiyorsanız kullanışlıdır.


12. Etiketlenmiş bir veri kümesi ve eğitim modüllerinden birini ekleyin.

    Bir parametre tarama kullanmıyorsanız kullanın [modeli eğitme](train-model.md) modülü.

13. Denemeyi çalıştırın.

## <a name="results-for-online-gradient-descent"></a>Çevrimiçi bir gradyan düşüşü sonuçları

Alıştırma tamamlandıktan sonra:

+ Tahminde bulunmak için eğitilen modele bağlanmak [Score Model](./score-model.md) modülü, yeni giriş verileriyle birlikte.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 