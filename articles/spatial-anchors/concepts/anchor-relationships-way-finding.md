---
title: Bağlantı ilişkilerini ve Azure uzamsal yer işaretleri, yol bulma | Microsoft Docs
description: Bağlantı ilişkilerini arkasında kavramsal model açıklanmaktadır. Boşluk ve bir şekilde bulma senaryo karşılamak üzere yakında API kullanma işlemi içinde yer işaretleri bağlanma işlemi açıklanmaktadır. Kavramsal model açıklanması sonra geliştiricilerin kendi uygulamalarında bu senaryoyu uygulamaya başlayabilirsiniz, yakındaki yapmak bizim örnek uygulamaları gelin.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: ramonarguelles
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 56f59a76ac3d11677d5b1f76904cf74e933fa7f6
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753345"
---
# <a name="anchor-relationships-and-way-finding-in-azure-spatial-anchors"></a>Bağlantı ilişkilerini ve Azure uzamsal yer işaretleri, şekilde bulma

Bağlantı ilişkilerini bağlı bağlayıcılarını boşluk oluşturup ardından bunları gibi hakkında sorular sormak izin ver:

* Yakındaki bağlantıların yer işaretleri var mı?
* Bunlar nasıl uzakta misiniz?

## <a name="examples"></a>Örnekler

Bazı örnek kullanım bağlı çıpasıyla etkinleştirebilirsiniz örnekleri şunlardır:

1. Bir çalışan bir endüstriyel fabrikası çeşitli konumlarda ziyaret içeren bir yordamı yürütmek gerekir. Fabrika uzamsal bağlayıcılarını her sitede yordamı katılan yerleştirdi. Çalışan bir konumdan diğerine Kılavuzu, HoloLens veya mobil uygulamanıza yardımcı olur. Yakında ve ardından sonraki konuma çalışan Kılavuzu uzamsal bağlayıcıları için ilk sorduğu. Uygulama genel yönünü ve uzaklık hakkında görsel göstergeleri görevini tamamlamak için sonraki konuma görüntüler.

2. Bir müze birlikte "Essential genel görüntüler bir saatlik Turu" gibi müze aracılığıyla belirli bir tur oluşturma genel görüntüler, uzamsal yer işaretleri oluşturur. Ziyaretçiler bir ortak görüntü olduğunuzda, bunlar mobil cihazlarında müze'nın karma gerçeklik uygulamasını açabilirsiniz. Ardından, bunlar telefonlarını alanı etrafında bir kamera akış aracılığıyla noktası ve genel yönünü ve diğer genel görüntüler uzaklığı Turun bakın. Genel görüntüler birini yürütmek kullanıcı başlattığında uygulama genel yönünü ve uzaklık Kılavuzu kullanıcılara yardımcı olmak için aşamalı olarak güncelleştirir.

## <a name="way-finding"></a>Yol-bulma

Uygulamayı "satırı görüş" yön ve rehberlik ipuçları kullanıcılara sunmak için yer işaretleri arasındaki uzaklığı kullanarak düşünün. Bu genel senaryoya yol-bulma olarak diyoruz. Yol-bulma Aç tarafından Aç Gezinti bölmesinden farklı olduğuna dikkat edin önemlidir. Bırakma tarafından Aç Gezinti bölmesinde kullanıcılar etrafında duvarlar, kapılar ve Katlar arasında gerçekleşir. Yol-bulma ile kullanıcı hedef genel yönünü konusunda ipuçları sağlanır. Ancak kullanıcının çıkarım ya da alanı bilgi aynı zamanda hedef yapısı arasında gezinmek için yardımcı olur.

Yolu bulma deneyimi oluştururken bir alanı deneyimini hazırlama ve son kullanıcıların etkileşim kuracağı bir uygulama geliştiren içerir. Kavramsal adımlar şunlardır:

1. Alanı planlama: Yol-bulma deneyimini katılan konumları alanı içinde belirler. Önceki örneklerde, bu etkinlik Fabrika gözetmen veya müze turu Düzenleyicisi tarafından tamamlanabilir.
2. Yer işaretleri bağlanma: Birisi seçilen konumlarını ziyaret eder ve var. uzamsal yer işaretleri oluşturur. Bu görevi son kullanıcı veya farklı bir uygulaması, Yönetici modu ile tamamen yapılabilir. Bu süreçte, her bağlantı bağlı veya başkalarına ilgili. Bu ilişkiler hizmette korunur.
3. Son kullanıcı deneyimi başlatılıyor: Son kullanıcılar için ilk adım seçtiğiniz konumlardan herhangi birinde olabilir uygulamasını kullanarak yer işaretleri birini bulmaktır. Son kullanıcıların deneyimini girebileceğiniz konumları belirleme, genel deneyimini tasarlama parçasıdır.
4. Bağlayıcılarını bulma: Kullanıcı bir yer işareti bulunduktan sonra uygulamanın yakındaki bağlantıların yer işaretleri isteyebilir. Bu yordam, cihaz ve bu yer işaretleri arasında bir poz döndürür.
5. Kullanıcı kılavuzluk: Uygulama poz her genel yönünü ve uzaklık ilgili faydalı bir kılavuz ipuçları işlemek için bu yer işaretleri yararlanabilirsiniz. Örneğin, bir simge olabilir ve aşağıdaki görüntüde gösterildiği gibi her bir potansiyel hedefine temsil eden mobil uygulamasında bir kamera oku akış.
6. Kılavuz iyileştirme: Kullanıcı gezer gibi uygulamayı düzenli olarak yeni bir poz arasında cihaz ve hedef bağlantı hesaplayabilirsiniz. Uygulama, hedefte geldiğinde kullanıcı Yardımı rehberi ipuçları iyileştirmek devam eder.

![Toplantı nokta](./media/meeting-spot.png)

## <a name="connecting-anchors"></a>Yer işaretleri bağlanma

Bir şekilde bulma deneyimi oluşturmak için seçtiğiniz bağlı bağlayıcılarını yere gerekir. Aşağıda uygulamanın bir yönetici tarafından bu çalışmanın varsayacağız.

### <a name="connecting-anchors-in-a-single-session"></a>Yer işaretleri, tek bir oturumda bağlanma

Yer işaretleri bağlanırken adımlar şunlardır:

1. Yönetim bir yer işareti oluşturur ve ilk konuma gösterir bir CloudSpatialAnchorSession kullanma.
2. Yönetici kullanıcıyı izlemek temel alınan MR/AR platformu devam ederken ikinci bir konuma yol gösterir.
3. Yönetim bağlantı B ile aynı CloudSpatialAnchorSession oluşturur. A ve B bağlayıcıları artık bağlıdır ve bu ilişki uzamsal bağlayıcılarını Azure hizmeti tarafından korunur.
4. Yordamı, bağlanmak istediğiniz tüm bağlayıcıları için devam edin.

### <a name="multiple-sessions"></a>Birden çok oturumu

Uzamsal yer işaretleri birden çok oturumu da bağlanabilirsiniz. Bu yöntem, bazı bağlantıları aynı anda bağlanma ve daha sonra oluşturulur ve daha fazla yer işaretleri bağlanmak olanak tanır. Yer işaretleri ile birden çok oturumu bağlanmak için:

1. Uygulama içinde bir CloudSpatialAnchorSession bazı yer işaretleri oluşturur.
2. Daha sonra örneği için başka bir gün, uygulamayı yeni bir CloudSpatialAnchorSession (örneğin, bağlantı A) bu çıpasıyla birini bulur.
3. Kullanıcıyı izlemek temel alınan MR/AR platformu devam ederken yeni bir konuma kullanıcı size yol gösterir.
4. Kullanıcı bağlantı C'ye yer işaretleri oluşturur aynı CloudSpatialAnchorSession kullanarak, A, B ve C artık bağlı ve bu ilişkiyi Azure uzamsal bağlayıcılarını tarafından korunur.
5. Zaman içinde daha fazla yer işaretleri ve daha fazla oturumları için bu yordamı devam edebilirsiniz.

### <a name="verifying-anchor-connections"></a>Yer işareti bağlantıları doğrulama

Uygulamanın yakındaki bağlantıları için bir sorgu yayınlayarak iki çıpasının bağlandığınızdan emin doğrulayabilirsiniz. Sorgu sonucu, istenen hedef bağlantı içeriyorsa, uygulama bağlayıcılarını bağlandığınızı onay sahiptir. Bağlı değilseniz, uygulama bağlantı yordamı yeniden deneyebilirsiniz. Neden bağlayıcılarını bağlanamayabilir bazı nedenler şunlardır:

1. Temel alınan MR/AR İzleyicisi İzleme bağlayıcılarını bağlanma işlemi sırasında kaybolur.
2. Uzamsal bağlayıcılarını Azure hizmetiyle iletişim kurulurken bir ağ hatası oluştu ve bağlantı bağlantı kalıcı uygulanamadı.

### <a name="sample-code"></a>Örnek kod

Yer işaretleri bağlanın ve sorguların yakındaki bağlantıların nasıl oluşturulduğunu gösteren örnek kodunu görebilirsiniz. Github'da Azure uzamsal bağlayıcılarını örnek uygulamaları'na bakın.
