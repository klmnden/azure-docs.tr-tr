---
title: "Veri Fabrikası kullanım örneği - ürün önerileri"
description: "Diğer hizmetlerin yanı sıra Azure Data Factory kullanarak uygulanan bir kullanım durumu hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6f1523c7-46c3-4b8d-9ed6-b847ae5ec4ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 04504d1e32243f752e488a24e04ec5ba73fbadc1
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="use-case---product-recommendations"></a>Kullanım Örneği - Ürün Önerileri
Azure Data Factory Çözüm Hızlandırıcıları Cortana Intelligence Suite uygulamak için kullanılan birçok hizmetlerden biridir.  Bkz: [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics) bu paketi hakkında ayrıntılar için sayfa. Bu belgede, Azure kullanıcıların zaten Çözüldü ve Azure Data Factory ve diğer Cortana Intelligence Bileşen Hizmetleri kullanılarak uygulanan ortak bir kullanım örneği açıklanmaktadır.

## <a name="scenario"></a>Senaryo
Çevrimiçi Perakendeciler genellikle büyük olasılıkla ilginizi olmasını ve bu nedenle büyük olasılıkla satın ürünleriyle sunarak ürünlerini satın müşterilerine ikna istiyorsunuz. Bunu gerçekleştirmek için çevrimiçi Perakendeciler, belirli bir kullanıcı için kişiselleştirilmiş ürün önerilerini kullanarak kendi kullanıcının çevrimiçi deneyimini özelleştirme gerekir. Kişiselleştirilmiş Bu öneriler, geçerli ve geçmiş davranışı verileri, ürün bilgilerini, yeni sunulan markalar ve ürün ve müşteri Segment veri alışveriş göre tabanlı yapılması üzeresiniz.  Ayrıca, bunlar birlikte, tüm kullanıcıların genel kullanım davranış analizini göre kullanıcı ürün önerilerini sağlayabilir.

Kullanıcı tıklatın satış dönüştürmeleri için en iyi duruma getirme ve daha yüksek satış gelirinin kazanmak için bu perakende belirtilir.  Bunlar, bu dönüştürme Müşteri ilgi alanları ve Eylemler dayalı olarak bağlamsal, davranış tabanlı ürün öneriler sunarak elde edin. Bu kullanım durumu için örnek olarak, müşterileri için en iyi duruma getirmek istediğiniz işletmeler çevrimiçi Perakendeciler kullanın. Ancak, bu ilkeler, müşterilerinin kendi mal ve hizmet geçici devreye ve müşterilerin satın alma kişiselleştirilmiş ürün önerilerini deneyiminizi geliştirmek için istediği herhangi bir işletme için geçerlidir.

## <a name="challenges"></a>Zorlukları
Birçok zorluklar mevcuttur, çevrimiçi Perakendeciler yüz bu tür bir kullanım örneği çalışırken. 

İlk olarak, farklı boyutlarda ve şekiller verilerin birden çok veri kaynaklarından alınan gerekir hem şirket içinde ve bulutta. Kullanıcının çevrimiçi perakende site gözatar olarak bu veriler ürün verilerini, geçmiş müşteri davranışı verileri ve kullanıcı verilerini içerir. 

İkinci, kişiselleştirilmiş ürün önerilerini makul ve doğru şekilde hesaplanan tahmin ve gerekir. Ürün, marka ve müşteri davranışı ve tarayıcı verilerini ek olarak, çevrimiçi Perakendeciler, ayrıca kullanıcı için en iyi ürün önerilerini belirlenmesi faktörü son alışveriş müşteri geri bildirimi eklemeniz gerekir. 

Üçüncü önerileri sorunsuz göz atma ve satın alma deneyimi sağlamak ve en son ve ilgili öneriler sunmak için kullanıcıya hemen teslim edilebilir olmalıdır. 

Son olarak, perakende genel yukarı satış izleyerek kendi yaklaşım etkisini ölçmek ve çapraz satış tıklatın dönüştürme satış başarı ve bunların gelecekteki önerileri ayarlamak gerekir.

## <a name="solution-overview"></a>Çözüme Genel Bakış
Bu örnek kullanım örneği Çözüldü ve Azure Data Factory ve de dahil olmak üzere diğer Cortana Intelligence component services kullanarak gerçek Azure kullanıcılar tarafından uygulanan [Hdınsight](https://azure.microsoft.com/services/hdinsight/) ve [Power BI](https://powerbi.microsoft.com/).

Çevrimiçi satıcısı kendi veri depolama seçenekleri iş akışı boyunca olarak Azure Blob Depolama, bir şirket içi SQL server, Azure SQL DB ve bir ilişkisel veri reyonu kullanır.  Blob deposu, müşteri bilgileri, müşteri davranışı verileri ve ürün bilgi verilerini içerir. Bir ürün kataloğu depolanmış şirket içi SQL veri ambarı ve ürün bilgileri verileri ürün marka bilgileri içerir. 

Tüm veriler birleştirilmiş ve müşteri ilgi alanları ve Eylemler, Ürün Kataloğu Web sitesi kullanıcı gözatar sırada dayalı olarak kişiselleştirilmiş öneriler sunmak için bir ürün öneri sistemine ssas'nin. Müşteriler, ayrıca tek bir kullanıcıya ilgili olmayan genel Web sitesi kullanım desenlerini bakarak ürün ilgili ürünler dayalı bakın.

![Kullanım örneği diyagramı](./media/data-factory-product-reco-usecase/diagram-1.png)

Ham web günlüğü dosyalarını gigabayt günlük olarak yarı yapılandırılmış dosyaları çevrimiçi satıcısında'nın Web sitesinden üretilir. Ham web günlük dosyaları ve müşteri ve ürün kataloğu bilgilerini alınan düzenli aralıklarla veri fabrikasının genel olarak dağıtılmış veri taşıma hizmeti olarak kullanarak bir Azure Blob depolama alanına. Gün için ham günlük dosyaları, uzun vadeli depolama için blob depolama birimindeki (yıl ve ay) bölümlenir.  [Azure Hdınsight](https://azure.microsoft.com/services/hdinsight/) blob deposu ham günlük dosyalarında bölüm ve Hive veya Pig betikleri kullanarak ölçekte alınan günlükleri işlemek için kullanılır. Verileri bölümlenen web günlüklerini öğrenme öneri sistemine kişiselleştirilmiş ürün öneri oluşturmak amacıyla bir makine için gerekli girişleri ayıklamak için işlenir.

Bu örnekte öğrenme makine için kullanılan öneri öneri platformundan öğrenme bir açık kaynak makine sistemidir [Apache Mahout](http://mahout.apache.org/).  Tüm [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) veya özel model senaryoya uygulanabilir.  Mahout modeli genel kullanım düzenlerini esas alarak Web sitesinde öğeleri arasında benzerlik tahmin etmek ve bireysel kullanıcıyı temel alarak kişiselleştirilmiş öneri oluşturmak amacıyla kullanılır.

Son olarak, kişiselleştirilmiş ürün önerilerini sonuç kümesini bir ilişkisel veri reyonuna tüketimi için satıcıya Web sitesi tarafından taşınır.  Sonuç kümesi ayrıca doğrudan blob depolama alanından başka bir uygulama tarafından erişilen, veya ek mağazaları diğer tüketicilerin ve kullanım örnekleri için taşınır.

## <a name="benefits"></a>Avantajlar
Kendi ürün öneri stratejisi en iyi duruma getirme ve iş hedeflerini ile hizalama çözümü çevrimiçi Satıcısı'nın ticaret ve hedefleri pazarlama karşılanır. Ayrıca, bunlar faaliyete ve ürün öneri iş akışı verimli, güvenilir ve uygun maliyetli bir şekilde yönetmek kullanabilirsiniz. Yaklaşım bunları kendi modeli güncelleştirmek ve satış tıklatın dönüştürme başarıları ölçüler üzerinde temel verimliliğinden ince ayar yapmak daha kolay hale. Azure Data Factory kullanarak, kullanıcılar kendi zaman alabilir ve pahalı el ile bulut kaynak yönetimi bırakın ve isteğe bağlı bulut kaynak yönetimi için taşıma. Bu nedenle, bunlar zaman, para tasarrufu ve çözüm dağıtımı için kendi süresini azaltabilir. Veri çizgileri görünümleri ve işletimsel hizmet durumu görselleştirmek ve sezgisel Data Factory izleme ve yönetim Azure portalından kullanılabilir kullanıcı Arabirimi ile ilgili sorunları giderme kolay hale geldi. Kendi çözüm şimdi zamanlanmış ve böylece bitmiş veri güvenilir bir şekilde üretilen ve kullanıcılara teslim ve verileri ve işleme bağımlılıkları insan etkileşimi olmadan otomatik olarak yönetilir yönetilir.

Bu kişiselleştirilmiş alışveriş deneyimi sağlayarak daha fazla rekabet çekici bir müşteri oluşturulan çevrimiçi satıcısında deneyimi ve bu nedenle satış ve genel müşteri memnuniyetini artırın.

