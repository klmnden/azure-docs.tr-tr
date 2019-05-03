---
title: 'Veri kümesindeki sütunları seçin: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Aşağı Akış işlemlerinde kullanılacak sütunların bir alt kümesini seçmek için Azure Machine Learning hizmeti modülünde Dataset Sütunları Seç kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: e7407f62bd3401411d56076b298bd8cd134ece62
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028103"
---
# <a name="select-columns-in-dataset-module"></a>Veri kümesi modülünde sütunları seçin

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Aşağı Akış işlemlerinde kullanılacak sütunların bir alt kümesini seçmek için bu modülü kullanın. Modül fiziksel kaynak kümesinden sütunları kaldırmaz; Bunun yerine, bir veritabanı gibi bir sütun alt kümesi oluşturur *görünümü* veya *projeksiyon*.

Bu modül, bir aşağı akış işlem için kullanılabilir sütunları sınırlamak gerektiğinde veya gereksiz sütunları kaldırarak veri kümesi boyutunu küçültmek istiyorsanız kullanışlıdır.

Farklı bir sırada belirtmiş olsanız da, veri kümesindeki sütunları çıktısı özgün veriler olduğu gibi aynı sırayla alınır.

## <a name="how-to-use"></a>Nasıl kullanılır

Bu modül, hiç parametre yok. Sütun seçiciyi dahil etmek veya hariç tutmak için sütunları seçmek için kullanın.

### <a name="choose-columns-by-name"></a>Sütun adına göre seçme

Sütun adına göre seçmek modülünde birden çok seçenek vardır: 

+ Filtre ve arama

    Tıklayın **ada göre** seçeneği.

    Kullanılabilir sütunlar listesi zaten doldurulmuş bir veri kümesi bağlandıysanız görüntülenmesi gerekir. Hiçbir sütun görünüyorsa, sütun listesinde görüntülemek üzere Yukarı Akış modülleri çalıştırmak gerekebilir.

    Listeyi filtrelemek için arama kutusuna yazın. Örneğin, yazdığınız harfi `w` arama kutusuna, listede harfi içeren sütun adları gösterecek şekilde filtrelenir `w`.

    Sütun seçin ve listenin sağ bölmede seçili sütunları taşımak için sağ ok düğmesine tıklayın.

    + Sütun adları sürekli aralığı seçmek için basın **SHIFT + tıklatma**.
    + Tek tek sütun seçimi eklemek için basın **Ctrl + tıklama**.

    Kaydedip kapatmak için onay işareti düğmesine tıklayın.

+ Diğer kurallarla birlikte adları kullanma

    Tıklayın **kuralları ile** seçeneği.
    
    Gibi belirli veri türünde sütunlar gösteren bir kural seçin.

    Ardından adıyla seçim listesine eklenecek sütunları bu tür tek tek tıklayın.

+ Sütun adlarının virgülle ayrılmış bir listesini yapıştırın veya yazın

    Veri kümeniz genişse, dizinleri daha kolay olabilir veya tek tek seçme sütunlar yerine adları listesi oluşturulan. Hazırladığınız listede önceden varsayılarak:

    1. Tıklayın **kuralları ile** seçeneği. 
    2. Seçin **hiçbir sütun**seçin **INCLUDE**ve ardından kırmızı bir ünlem işareti metin kutusunun içine tıklayın. 
    3. Daha önce doğrulanmış sütun adlarının virgülle ayrılmış bir listesini yazın ya da yapıştırın. Herhangi bir sütun geçersiz bir ada sahipse, modülü şekilde önceden adları kontrol ettiğinizden emin olamaz.
    
    Bu yöntem, dizin değerlerini kullanarak bir sütun listesini belirtmek için de kullanabilirsiniz. 

### <a name="choose-by-type"></a>Türüne göre seçin

Kullanırsanız **kuralları ile** seçeneği, birden çok koşul sütunu seçimlere uygulayabilirsiniz. Örneğin, yalnızca bir sayısal veri türünde sütunlar özelliği almak gerekebilir.

**ŞUNUNLA Başla** seçeneği, başlangıç noktanız belirler ve sonuçları anlamak için önemlidir. 

+ Seçerseniz **tüm sütunları** seçeneği, tüm sütunlar listesine eklenir. Daha sonra kullanmanız gerekir **hariç** seçeneğini *Kaldır* belirli koşullara uyan sütunları. 

    Örneğin, tüm sütunları ile başlayın ve ardından ada veya türe göre sütunları kaldırın.

+ Seçerseniz **Hayır sütunları** seçeneği, sütun listesi boş başlatır. Koşulları ardından belirttiğiniz *Ekle* sütun listesi. 

    Birden çok kural uygularsanız, her durumdur **eklenebilir**. Örneğin, hiçbir sütun ile başlayın ve ardından tüm sayısal sütunları elde etmek üzere bir kural eklemek varsayalım. Otomobil fiyat kümesinde, 16 sütunlarında sonuçlanır. Ardından, tıkladığınız **+** yeni koşul Ekle ve oturum **tüm özellikler**. Sonuç veri kümesini, tüm sayısal sütunları yanı sıra bazı dize özelliği sütunlar dahil olmak üzere tüm özellik sütunları içerir.

### <a name="choose-by-column-index"></a>Sütun dizini tarafından seçin

Özgün veri kümesi içindeki sütun sıra sütun dizini belirtir.

+ Sütunları sırayla 1'den başlayarak numaralandırılır.  
+ Bir dizi sütunları almak için bir kısa çizgi kullanın. 
+ Gibi açık uçlu belirtimleri `1-` veya `-3` izin verilmez.
+ Yinelenen dizin değerleri (veya sütun adları) izin verilmez ve hataya neden.

Örneğin, veri kümenizin en az sekiz sütuna sahip olduğu varsayılırsa, birden çok bitişik olmayan sütunları döndürmek için aşağıdaki örnekleri hiçbirinde yapıştırabilirsiniz: 

+ `8,1-4,6`
+ `1,3-8`
+ `1,3-6,4` 

Son örnek hatayla sonuçlanmaz; Ancak, tek bir sütun örneği döndürür `4`.



### <a name="change-order-of-columns"></a>Sütunların sırasını değiştirme

Seçenek **izin yinelemeleri ve sütun sırasını seçimdeki korumak** boş bir liste ile başlar ve belirttiğiniz ada veya dizine göre sütunları ekler. Her zaman, "doğal sıra" sütunları döndürür, diğer seçenekleri aksine bu seçeneği sütunları sırayla isimlendirdiğiniz çıkarır veya onları listeleyin. 

Örneğin, Sütun1, Sütun2, Col3 ve Sütun4 sütunları içeren bir veri kümesinde, sütunların sırasını ters ve sütun 2,, aşağıdaki listelerden birini belirterek bırakın:

+ `Col4, Col3, Col1`
+ `4,3,1`


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 