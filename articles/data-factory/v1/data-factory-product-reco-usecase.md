---
title: Data Factory kullanım örneği - ürün önerileri
description: Azure Data Factory ile birlikte diğer hizmetleri kullanılarak uygulanan bir kullanım örneği hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 6f1523c7-46c3-4b8d-9ed6-b847ae5ec4ae
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 4a3d1c513bcfb6449ca73d873c0dd9831c6fe01d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60605694"
---
# <a name="use-case---product-recommendations"></a>Kullanım Örneği - Ürün Önerileri
Azure Data Factory Çözüm Hızlandırıcıları, Cortana Intelligence Suite uygulamak için kullanılan birçok hizmetlerden biridir.  Bkz: [Cortana Intelligence Suite](https://www.microsoft.com/cortanaanalytics) bu paketi hakkında daha fazla ayrıntı için. Bu belgede, Azure kullanıcılarının zaten çözülen ve Azure Data Factory ve diğer Cortana Intelligence Bileşen Hizmetleri kullanılarak uygulanan genel bir kullanım örneği açıklanmaktadır.

## <a name="scenario"></a>Senaryo
Çevrimiçi Perakendeciler yaygın olarak, müşterilerine büyük olasılıkla ilginizi olmasını ve bu nedenle büyük olasılıkla satın ürünleriyle sunarak ürün satın ikna istiyorsunuz. Bunu gerçekleştirmek için çevrimiçi Perakendeciler belirli bir kullanıcı için kişiselleştirilmiş ürün önerileri kullanarak kendi kullanıcı çevrimiçi deneyimini özelleştirmeniz gerekir. Bu kişiselleştirilmiş öneriler, geçerli ve geçmiş alışveriş davranış verilerini, ürün bilgilerini, yeni tanıtılan markalar ve ürün ve müşteri Segment veri tabanlı yapılması üzeresiniz.  Ayrıca, bunlar birlikte tüm kullanıcıları genel kullanım davranış analizini temel kullanıcı ürün önerileri sağlayabilir.

Bu Satıcıların kullanıcı tıklatın satış dönüştürmeleri için en iyi duruma getirmek ve daha yüksek satış gelir kazanın olmaktır.  Bunlar, bu dönüştürme Müşteri Vade farkı ve Eylemler göre bağlamsal, davranış tabanlı ürün önerileri sunarak elde edin. Bu kullanım vakası için örnek olarak, müşterileri için en iyi duruma getirmek istediğiniz işletmeler çevrimiçi Perakendeciler kullanın. Ancak, mal ve hizmet geçici olarak, müşterilerle etkileşim kurun ve kişiselleştirilmiş ürün önerileri müşterilerinin satın alma deneyiminizi geliştirmek isteyen tüm işletmeler için bu ilkeler geçerlidir.

## <a name="challenges"></a>Zorluklar
Vardır birçok güçlükle de bu çevrimiçi Perakendeciler yüz bu tür bir kullanım örneği çalışırken. 

İlk olarak, farklı boyut ve şekillerde verilerin birden çok veri kaynağından alınan gerekir hem şirket içi ve bulut. Bu veriler, çevrimiçi perakende sitesini kullanıcının gider gibi ürün verileri, geçmiş müşteri davranışı verileri ve kullanıcı verilerini içerir. 

İkinci, kişiselleştirilmiş ürün önerileri makul bir şekilde ve doğru bir şekilde hesaplanan tahmin edilen ve gerekir. Ürün, marka ve müşteri davranışı ve tarayıcı verilerinin yanı sıra çevrimiçi Perakendeciler ayrıca müşteri geri bildirimi, kullanıcı için en iyi ürün önerileri belirlenmesi etkimesi için geçmiş satın eklemeniz gerekir. 

Üçüncü olarak, öneriler sorunsuz gezinme ve satın alma deneyimi sağlar ve en son ve ilgili öneriler sağlamak için kullanıcıya hemen teslim edilebilir olması gerekir. 

Son olarak, genel yukarı satış izleyerek yaklaşımlarını verimliliğini ölçme ve çapraz satış tıklatın dönüştürme satış başarılar ve bunların geleceğe yönelik öneriler için ayarlamak Perakendeciler gerekir.

## <a name="solution-overview"></a>Çözüme genel bakış
Bu örnek kullanım örneği çözülen ve Azure Data Factory ve dahil olmak üzere diğer Cortana Intelligence Bileşen Hizmetleri'ni kullanarak gerçek Azure kullanıcılar tarafından uygulanan [HDInsight](https://azure.microsoft.com/services/hdinsight/) ve [Power BI](https://powerbi.microsoft.com/).

Çevrimiçi satış şirketi, iş akışı boyunca kendi veri depolama seçenekleri olarak Azure Blob Depolama, şirket içi SQL server, Azure SQL DB ve ilişkisel veri reyonu kullanır.  Blob depolama, müşteri bilgileri, müşteri davranış verilerini ve ürün bilgileri verileri içerir. Ürün bilgisi verileri ürün marka bilgilerini içeren ve bir ürün kataloğu depolanmış şirket içi bir SQL veri ambarı'nda. 

Tüm verileri birleştirilir ve müşteri vade farkı ve Eylemler, Ürün Kataloğu Web sitesi kullanıcı gözatar sırada göre kişiselleştirilmiş öneriler sunmak için bir ürün önerisi sisteme beslenir. Müşteriler ayrıca, herhangi bir kullanıcı için ilgili olmayan genel bir Web sitesi kullanım düzenlerini göz atan ürün ile ilgili ürünleri göre bakın.

![Kullanım durumu diyagramı](./media/data-factory-product-reco-usecase/diagram-1.png)

Ham web günlüğü dosyaları gigabayt, yarı yapılandırılmış dosyalar olarak çevrimiçi satış şirketi'nın Web sitesinden günlük oluşturulur. Ham web günlüğü dosyaları ve müşteri ve ürün kataloğu bilgilerini alınan düzenli olarak bir hizmet olarak Data Factory'nin küresel çapta dağıtılan veri hareketini kullanarak bir Azure Blob Depolama içine. Gün için ham günlük dosyaları, uzun vadeli depolama için blob depolama alanında (yıl ve aya göre) bölümlenir.  [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) ham günlük dosyaları blob depolama bölümlemek ve uygun ölçekte hem Hive ve Pig betiklerini kullanarak alınan günlükler işlemek için kullanılır. Bölümlenmiş web verilerini günlüğe kaydeder. bir machine learning öneri sistemin kişiselleştirilmiş ürün önerileri oluşturma için gerekli girişleri ayıklamak için işlenir.

Makine öğrenimi Bu örnekte kullanılan öneri açık kaynaklı bir makine öğrenme öneri platformdan sistemidir [Apache Mahout](https://mahout.apache.org/).  Tüm [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) veya senaryoya özel model uygulanabilir.  Mahout model, genel kullanım modellerini Web sitesindeki öğeleri arasında benzerlik tahmin etmek ve tek tek kullanıcıya bağlı kişiselleştirilmiş öneriler oluşturmak için kullanılır.

Son olarak, kişiselleştirilmiş ürün önerileri sonuç kümesi bir ilişkisel veri reyonuna tüketimleri perakendecisi Web sitesi tarafından taşınır.  Sonuç kümesi de doğrudan blob depolama alanından başka bir uygulama tarafından erişilen veya diğer tüketiciler ve kullanım örnekleri için ek mağazalarının taşındı.

## <a name="benefits"></a>Avantajlar
Kendi ürün önerisi strateji en iyi duruma getirme ve iş hedeflerine hizalama, çevrimiçi satış şirketi'nın mağazacılık ve hedefleri pazarlama çözümü karşılaştı. Ayrıca, bunlar kullanıma hazır hale getirme ve ürün önerisi iş akışı verimli, güvenilir ve uygun maliyetli bir şekilde yönetmek kullanabilirsiniz. Yaklaşım, modeli güncelleştirmek ve verimliliğinden satış tıklatın dönüştürme başarıları ölçümlerine üzerinde ince ayar için yaptığınız. Azure Data Factory kullanarak, bunlar, zaman alıcı ve pahalı el ile bulut kaynak yönetimi iptal ve isteğe bağlı bulut kaynak yönetimi için taşıma sunmayı başarabilseydiniz nasıl olurdu. Bu nedenle, bunlar zaman para kazanmak için Çözüm dağıtımı kendi süresini azaltmak kullanabilirsiniz. Veri kökenini görünümleri ve işletimsel hizmet durumu görselleştirmenize ve sezgisel Data factory'yi izleme ve Yönetimi Azure portalından kullanılabilir kullanıcı Arabirimi sorunlarını gidermek kolay hale geldi. Çözüm artık zamanlanan ve tamamlanan verileri güvenilir bir şekilde oluşturulur ve kullanıcılara sunulan ve veri ve işlem bağımlılıklarını insan müdahalesi olmadan otomatik olarak yönetilir yönetilir.

Bu kişiselleştirilmiş alışveriş deneyimi sağlayarak, çevrimiçi satış şirketi daha rekabetçi, ilgi çekici bir müşteri deneyimi ve bu nedenle satış ve genel müşteri memnuniyetini artırın.

