---
title: Team Data Science Process içinde özellik seçimi
description: Özellik Seçimi amacını açıklar ve makine öğrenimi veri geliştirme sürecinin içindeki rollerine örnekler sağlar.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/21/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: a74f2c21746deb16372174d4a769f9abb825a1cd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60809573"
---
# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Team Data Science Process’te (TDSP) özellik seçimi
Bu makalede, özellik seçimi amaçları açıklanır ve machine Learning veri geliştirme sürecinde rolü örnekleri sağlar. Bu örnekler, Azure Machine Learning Studio'dan çizilir. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

Özellik Seçimi ve mühendislik bir parçası olan Team Data Science işlem (makalesinde özetlenen TDSP) olduğu [Team Data Science Process nedir?](overview.md). Özellik Mühendisliği ve seçimi olan bölümleri **geliştirme özellikleri** TDSP. adımı.

* **özellik Mühendisliği**: Bu işlem verilerinde mevcut ham özelliklerinden ilgili ek özellikler oluşturma ve Tahmine dayalı öğrenme algoritmasını güç artırmak için çalışır.
* **Özellik Seçimi**: Bu işlem özgün veri özellikleri anahtar kısmını eğitim sorunun işlenemez azaltmak girişimi seçer.

Normalde **özellik Mühendisliği** ek özellikler oluşturmak için öncelikle uygulanır ve ardından **özellik seçimi** adım, ilgisiz, yedekli ya da son derece bağıntılı özellikleri ortadan kaldırmak için gerçekleştirilir.

## <a name="filter-features-from-your-data---feature-selection"></a>Filtre özellikleri verilerinizi - özellik seçimi
Özellik Seçimi eğitim veri kümeleri sınıflandırma veya regresyon görevler gibi Tahmine dayalı modelleme görevlerin oluşumu için yaygın olarak uygulanan bir işlemdir. Hedef veri varyans en uzun süreyi temsil etmek için en az bir özellik kümesi kullanarak boyutlar azaltılmasına özgün veri kümesinden alınan özelliklerinin bir alt kümesi seçmektir. Bu alt özellikler kümesini modeli eğitmek için kullanılır. Özellik Seçimi iki ana amaca hizmet eder.

* İlk olarak, özellik seçimi sıklıkla ortadan kaldırarak ilgisiz, yedekli sınıflandırma doğruluğu artırır veya özellikleri son derece bağıntılı.
* İkinci olarak, model eğitim işleminden daha verimli hale getirir özellikleri sayısını azaltır. Verimliliği gibi destek vektör makineler eğitmek pahalı öğrencileriyle için özellikle önemlidir.

Özellik Seçimi modeli eğitmek için kullanılan veri kümesi özellikleri sayısını azaltmak arama olsa da, bunun için "boyut düzeyi azaltma" terimi tarafından başvurmaz. Özellik Seçimi yöntemleri veri özgün özelliklerinin bir alt kümesi, değişiklik yapmadan ayıklayın.  Boyut düzeyi azaltma yöntemleri orijinal özellikler dönüştürebilir ve bu nedenle bunlarda değişiklik mühendislik uygulanan özellikleri kullanın. Asıl bileşen analizi, kurallı bağıntı analiz ve tekil değer ayrıştırma boyut düzeyi azaltma yöntemleri örnekleridir.

Diğerleriyle birlikte, denetlenen bir bağlamda özellik seçimi yöntemleri yaygın olarak uygulanan bir kategorisi "filtre tabanlı özellik seçimi" adı verilir. Her özellik ve hedef öznitelik arasındaki bağıntıyı değerlendirerek bu yöntemler bir puan her bir özellik atamak için istatistiksel bir ölçü uygulayın. Özellikler, ardından tutma veya belirli bir özelliği ortadan eşiği ayarlamanıza yardımcı olması için kullanılabilir puana göre sıralanır. Bu yöntemleri kullanılan istatistiksel ölçüleri kişi bağıntı, karşılıklı bilgileri ve Chi karesini test örneklerindendir.

Azure Machine Learning Studio modülleri için özellik seçimi sağlanan vardır. Aşağıdaki resimde gösterildiği gibi bu modülleri dahil edildi [özellik seçimi süzgeç tabanlı] [ filter-based-feature-selection] ve [Fisher doğrusal Discriminant analiz] [ fisher-linear-discriminant-analysis].

![Özellik Seçimi modülleri](./media/select-features/feature-Selection.png)

Örneğin, kullanımını düşünün [özellik seçimi süzgeç tabanlı] [ filter-based-feature-selection] modülü. Kolaylık olması için metin madenciliği örneği kullanarak devam edin. 256 özellikler kümesi aracılığıyla oluşturulan sonra bir regresyon modeli derler istediğini varsayın [özellik karma] [ feature-hashing] modülü ve yanıt değişkeni "kitap gözden geçirme derecelendirme içeren Col1" olduğunu 1 ile 5 arasında olabilir. "Puanlama yöntemi özelliği" ayarlayarak "Pearson bağıntı", "hedef sütun" olacak "Col1" ve "sayısı istenen özellikleri" 50 olarak. Modül [özellik seçimi süzgeç tabanlı] [ filter-based-feature-selection] target özniteliği "Col1" ile birlikte 50 özellikleri içeren bir veri kümesi üretir. Aşağıdaki şekil bu deneme ve giriş parametrelerini akışı gösterilmektedir:

![Özellik seçimi temel modül özellikleri Filtrele](./media/select-features/feature-Selection1.png)

Aşağıdaki şekil, oluşturulan veri kümelerini gösterir:

![Sonuç veri kümesini filtre temel özellik seçimi Modülü](./media/select-features/feature-Selection2.png)

Her bir özellik kendisi ve hedef öznitelik "Col1" arasında Pearson bağıntı göre puanlanır. Üst puanları özelliklerle tutulur.

Seçili özellikleri karşılık gelen puanları, aşağıdaki şekilde gösterilmiştir:

![Filtre temel özellik seçimi modülü için puanları](./media/select-features/feature-Selection3.png)

Bu uygulama tarafından [özellik seçimi süzgeç tabanlı] [ filter-based-feature-selection] modülü, 50 tanesi özellikleri "Col1" hedef değişkeni en bağıntılı özelliklerle sahip oldukları seçili 256 dayalı Puanlama yöntemi "Pearson bağıntı".

## <a name="conclusion"></a>Sonuç
Özellik Mühendisliği ve özellik seçimi iki yaygın olarak tasarlanmıştır ve seçilen özellikler anahtar bilgileri ayıklamak için deneme verilerde bulunan eğitim işleminin verimliliğini artırmak kümesidir. Bunlar ayrıca giriş verileri doğru bir şekilde sınıflandırmak için ve ilgilendiğiniz sonuçları yerine daha tahmin etmek üzere bu modellerinin gücünden geliştirin. Öğrenme hesaplama açısından daha tractable yapmak için özellik Mühendisliği ve seçimi birleştirebilirsiniz. Bunu geliştirmek ve ardından ayarlamak veya bir model eğitip için gereken özellikleri sayısını azaltmayı yapar. Matematiksel anlamda, modeli eğitmek için seçilen en az sayıda bağımsız değişkenlere, veri desenlerinde açıklamak ve sonuçlar'ı başarıyla tahmin özellikleridir.

Her zaman mutlaka özellik Mühendisliği veya özellik seçimi gerçekleştirmek için değil. Toplanan veriler, seçili algoritması ve deneyde amaç veya gerekli olup olmadığını bağlıdır.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

