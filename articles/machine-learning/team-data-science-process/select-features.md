---
title: Özellik Seçimi takım veri bilimi işleminde | Microsoft Docs
description: Özellik Seçimi amacını açıklayan ve machine learning veri geliştirme sürecinin içindeki rollerine örnekler sağlar.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 878541f5-1df8-4368-889a-ced6852aba47
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2017
ms.author: deguhath
ms.openlocfilehash: 2504d14bad3a7a3b3845e880e40034ef7e4c126b
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34838638"
---
# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Team Data Science Process’te (TDSP) özellik seçimi
Bu makalede özellik seçimi amaçları açıklar ve makine öğrenme veri geliştirme işleminde rolü örnekleri sağlar. Bu örnekler, Azure Machine Learning Studio'dan çizilir. 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

Mühendislik ve çeşitli özellikler, takım veri bilimi işlemi (makalesinde özetlenen TDSP) bir parçası olduğu [takım veri bilimi işlemi nedir?](overview.md). Özellik Mühendisliği ve seçim olan bölümleri **geliştirmek özellikleri** TDSP adımında.

* **özellik Mühendisliği**: Bu işlem veri varolan ham özelliklerinden ek ilgili özellikler oluşturmak ve öğrenme algoritmasında Tahmine dayalı güç artırmak için çalışır.
* **Özellik Seçimi**: Bu işlem özgün veri özelliklerinin anahtar kısmı eğitim sorun boyut azaltmak girişimi seçer.

Normalde **özellik Mühendisliği** ek özellikler oluşturmak için önce uygulanır ve daha sonra **özellik seçimi** adım ilgisiz, artık ya da son derece bağıntılı özellikleri ortadan kaldırmak için gerçekleştirilir.

## <a name="filter-features-from-your-data---feature-selection"></a>Filtre özellikleri verilerinizden - özellik seçimi
Özellik Seçimi sınıflandırma veya regresyon görevler gibi Tahmine dayalı modelleme görevleri için eğitim veri kümesi yapımı için yaygın olarak uygulanan bir işlemdir. Maksimum veri varyans temsil etmek için en az bir özellik kümesi kullanarak boyutlarıyla azaltın özgün veri kümesinden özelliklerinin bir alt kümesi seçmek için belirtilir. Bu alt özellikler kümesini modeli eğitmek için kullanılır. Özellik Seçimi iki ana amaca hizmet eder.

* İlk olarak, özellik seçimi sıklıkla ortadan kaldırarak ilgisiz, yedekli sınıflandırma doğruluğu artırır veya özellikleri son derece bağıntılı.
* İkinci olarak, model Eğitim işlemini daha verimli hale getirir özellikleri sayısını azaltır. Verimlilik gibi destek vektör makineler eğitmek pahalıdır öğrencileriyle özellikle önemlidir.

Özellik Seçimi modeli eğitmek için kullanılan veri kümesi özellikleri sayısını azaltmak arama rağmen bunu "boyut azaltma" terimi tarafından başvurmaz. Özellik Seçimi yöntemleri, bunları değiştirmeden veri özgün özelliklerinin bir kısmı ayıklayın.  Boyut azaltma yöntemleri özgün özellikler dönüştürmek ve bu nedenle bunlarda değişiklik tasarlanan özellikleri kullanın. Asıl bileşen analiz, kurallı bağıntı analiz ve tekil değer ayrıştırma boyut azaltma yöntemler örnekleridir.

Diğerleriyle birlikte, yaygın olarak uygulanan bir kategori denetimli bağlamda özellik seçimi yöntemlerin "filtre tabanlı özellik seçimi" adı verilir. Her bir özellik ve hedef öznitelik arasındaki bağıntıyı değerlendirerek bu yöntemlerin her bir özellik için bir puan atamak için istatistiksel bir ölçü uygulayın. Özellikleri, ardından tutma veya belirli bir özellik ortadan kaldırmak için eşiği ayarlamanıza yardımcı olması için kullanılabilir puana göre sıralanır. Kişi bağıntı, karşılıklı bilgileri ve Şi squared test bu yöntemlerinde kullanılan istatistiksel ölçümler örneklerindendir.

Azure Machine Learning Studio'daki modüller için özellik seçimi sağlanan vardır. Aşağıdaki çizimde gösterildiği gibi bu modülleri dahil [filtre tabanlı özellik seçimi] [ filter-based-feature-selection] ve [Fisher doğrusal Discriminant analiz] [ fisher-linear-discriminant-analysis].

![Özellik Seçimi örneği](./media/select-features/feature-Selection.png)

Örneğin, kullanımını düşünün [filtre tabanlı özellik seçimi] [ filter-based-feature-selection] modülü. Kolaylık olması için metni araştırma örneği kullanmaya devam et. 256 özellikler kümesi aracılığıyla oluşturduktan sonra bir regresyon modeli oluşturmak istediğinizi varsayalım [özellik karma] [ feature-hashing] modülü ve yanıt değişkeni "kitap İnceleme derecelendirmeleri içeriyor Col1" olduğunu 1 ile 5 arasında. "Puanlama yöntemi özellik" ayarlayarak "Pearson bağıntı", "hedef sütun" olacak şekilde "Col1" ve "istenen özelliklerini"sayısı 50. Ardından Modülü [filtre tabanlı özellik seçimi] [ filter-based-feature-selection] target özniteliği "Col1" ile birlikte 50 özellikleri içeren bir veri kümesini oluşturur. Aşağıdaki şekil, bu deneme ve giriş parametreleri akışını gösterir:

![Özellik Seçimi örneği](./media/select-features/feature-Selection1.png)

Aşağıdaki şekil, sonuçta ortaya çıkan veri kümeleri gösterir:

![Özellik Seçimi örneği](./media/select-features/feature-Selection2.png)

Her bir özelliğin tabanlı kendisi ve hedef öznitelik "Col1" arasında Pearson bağıntı üzerinde puanlanır. Üst puanları özelliklerle tutulur.

Seçilen özelliklerin karşılık gelen puanları aşağıdaki çizimde gösterilmektedir:

![Özellik Seçimi örneği](./media/select-features/feature-Selection3.png)

Bu uygulama tarafından [filtre tabanlı özellik seçimi] [ filter-based-feature-selection] modülü, 50 dışı özellikler "Col1" hedef değişkeni en bağıntılı özelliklerle sahip oldukları için seçili 256 dayalı Puanlama yöntemi "Pearson bağıntı".

## <a name="conclusion"></a>Sonuç
Özellik Mühendisliği ve özellik seçimi iki yaygın olarak tasarlanmıştır ve seçilen özellikler anahtar bilgileri ayıklamak için deneme verilerde bulunan eğitim işlem verimliliğini artırmak kümesidir. Bunlar ayrıca giriş verileri doğru şekilde sınıflandırmak ve sonuçları ilgi daha yerine tahmin etmek için bu modeller gücü geliştirin. Öğrenme daha pkı'ya tractable yapma özelliği mühendislik ve seçim birleştirebilirsiniz. Bunu arttırma ve ayarlama veya modeli eğitmek için gereken özellikleri sayısının azaltılması yapar. Matematiksel konuşarak, modeli eğitmek için seçilen özellikler en az bağımsız değişkenlerinin veri desenleri açıklar ve sonuçları başarıyla tahmin kümesidir.

Bu her zaman olmak zorunda özellik Mühendisliği veya özellik seçimi gerçekleştirmek için değildir. Veya gerekli olup olmadığını, toplanan veriler, seçili algoritması ve deneme amacı bağlıdır.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/

