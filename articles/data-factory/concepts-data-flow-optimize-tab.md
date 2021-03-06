---
title: Azure veri fabrikası eşleme veri akışını iyileştirme sekmesi
description: Azure veri fabrikası eşleme verileri bölüm ayarları ile en iyi duruma getirme sekmesini kullanarak akışları en iyi duruma getirme
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 40023931b2a0b3788a583a5b5240e7916b187e34
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190653"
---
# <a name="mapping-data-flow-transformation-optimize-tab"></a>Eşleme veri akışı dönüştürme iyileştirmek sekmesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Her veri akışı dönüştürme bir "En iyi duruma getir" sekmesi vardır. En iyi duruma getir sekmesi, veri akışları için bölümleme düzenleri yapılandırmak için isteğe bağlı ayarları içerir.

<img src="media/data-flow/opt001.png" width="800">

"Geçerli bölümleme kullan" varsayılan ayardır. Azure Data Factory, Azure databricks'te Spark üzerinde çalışan veri akışları için yerel bölümleme düzeni kullanmak için geçerli bir bölümleme bildirir. Genel olarak, önerilen yaklaşım budur.

Bununla birlikte, burada bölümleme ayarlamak isteyebilirsiniz örnekler vardır. Dönüştürmelerinizi üzere gölü'nde tek bir dosyayı çıkış istiyorsanız, örneğin, ardından "tek bölüm" havuz dönüşümünde bölümleme için en iyi duruma getir sekmesinde seçtiniz.

Burada veri Bağlantılarınızdaki için kullanılan bölümleme düzenleri üzerinde denetim uygulamak isteyebilirsiniz başka bir durum, performans açısından geçerlidir. Veri bölümleme ayarlama, işlem düğümleri ve genel veri akış performansınızı hem pozitif hem de olumsuz etkileri olabilir veri konumu iyileştirmeler arasında dağıtım verileriniz üzerinde denetim düzeyini sağlar.

Herhangi bir dönüştürme bölümleme değiştirmek istiyorsanız, yalnızca en iyi duruma getir sekmesine tıklayın ve "Ayarlamak bölümleme" radyo düğmesini seçin. Ardından bir dizi bölümleme seçenekleri sunulur. En iyi yöntemi uygulamak için bölümleme veri birimleri, aday anahtar, null değerler ve kardinalite göre farklılık gösterir. En iyi uygulama, varsayılan bölümleme ile başlatın ve ardından farklı bölümleme seçenekleri deneyin sağlamaktır. Hata ayıklama işlem hattı çalıştırma ve bölüm kullanım izleme görünümünden yanı sıra gruplandırma her dönüştürme harcanan süre görünümünde kullanarak test edebilirsiniz.

<img src="media/data-flow/opt002.png" width="600">

### <a name="round-robin"></a>Hepsini Bir Kez Deneme

Hepsini bir kez otomatik olarak verileri bölümler arasında eşit şekilde dağıtan basit bölümdür. Kullanım düz, akıllı bir bölümleme stratejisi uygulamak için en iyi önemli adaylar olmadığında hepsini bir kez. Fiziksel bölüm sayısını ayarlayabilirsiniz.

### <a name="hash"></a>Karma

Azure Data Factory benzer değerlere sahip satırları aynı bölümde kalacak şekilde, Tekdüzen bölümler oluşturmak için sütunları karmasını oluşturur. Karma seçeneği kullanılırken, eğme olası bölüm için test edin. Fiziksel bölüm sayısını ayarlayabilirsiniz.

### <a name="dynamic-range"></a>Dinamik aralık

Dinamik aralık sütunları veya sağladığınız ifadeleri temel Spark dinamik aralıklarını kullanır. Fiziksel bölüm sayısını ayarlayabilirsiniz. 

### <a name="fixed-range"></a>Sabit aralık

Bölümlenmiş verileri sütunlarınızı içindeki değerleri için sabit bir aralık sağlayan bir ifade oluşturmanız gerekir. Bölüm eğriltme önlemek için bu seçeneği kullanmadan önce verilerinizin iyi anlamış olmanız gerekir. İfade için girdiğiniz değerleri bölümleme işlevi bir parçası olarak kullanılır. Fiziksel bölüm sayısını ayarlayabilirsiniz.

### <a name="key"></a>Anahtar

Anahtar bölümleme kardinalite verilerinizin iyi bir anlayışa sahip, iyi bölüm stratejisi olabilir. Anahtar bölümleme bölümler her bir benzersiz değer için, bir sütun oluşturur. Benzersiz değerler sayısı hesaplanır bölüm sayısı ayarlanamıyor.

## <a name="next-steps"></a>Sonraki adımlar

[Eşleme veri akışı performans kılavuzunu](concepts-data-flow-performance.md)
[veri akışı izleme](concepts-data-flow-monitoring.md)
