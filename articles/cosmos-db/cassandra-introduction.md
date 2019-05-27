---
title: Azure Cosmos DB Cassandra API’sine giriş
description: Mevcut uygulamaları "yükseltmek ve kaydırmak" için Azure Cosmos DB’yi ve yeni uygulamalar geliştirmek için zaten aşina olduğunuz Cassandra sürücüleri ve CQL ile Cassandra API’yi nasıl kullanabileceğinizi öğrenin.
author: kanshiG
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: overview
ms.date: 05/21/2019
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 82ca7814f756a12005ee5802c3e8a7fd28f6d398
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968976"
---
# <a name="introduction-to-the-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API’sine giriş

Azure Cosmos DB Cassandra API’si, [Apache Cassandra](https://cassandra.apache.org/) için yazılmış uygulamalara yönelik veri deposu olarak kullanılabilir. Başka bir deyişle, mevcut Cassandra uygulamanız artık CQLv4 ile uyumlu mevcut [Apache sürücülerini](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver) kullanarak Azure Cosmos DB Cassandra API’si ile iletişim kurabilir. Çoğu durumda yalnızca bir bağlantı dizesini değiştirerek Apache Cassandra’yı kullanmaktan Azure Cosmos DB’nin Cassandra API’sini kullanmaya geçiş yapabilirsiniz. 

Cassandra API’si; Cassandra Sorgu Dili (CQL), Cassandra tabanlı araçları (cqlsh gibi) ve zaten aşina olduğunuz Cassandra istemci sürücülerini kullanarak Azure Cosmos DB’de depolanan verilerle etkileşim kurmanızı sağlar.

## <a name="what-is-the-benefit-of-using-apache-cassandra-api-for-azure-cosmos-db"></a>Azure Cosmos DB için Apache Cassandra API'si kullanmanın avantajı nedir?

**Hiçbir operations management**: Bir tam olarak yönetilen bulut hizmeti olan Azure Cosmos DB Cassandra API'SİNİN ayarlar'ın işletim sistemi, JVM ve yaml dosyaları ve bunların etkileşimler izleme ve yönetme ek yükünü kaldırır. Azure Cosmos DB; aktarım hızı, gecikme süresi, depolama, kullanılabilirlik için izleme ve yapılandırılabilir uyarılar sunar.

**Performans Yönetimi**: Azure Cosmos DB, garantili düşük gecikme süreli okuma ve SLA'lar ile desteklenen 99. yüzdebirlik yazma sağlar. Kullanıcıların yüksek performanslı ve düşük gecikme süreli okuma ve yazma işlemleri sağlamak için işletim yükünden endişelenmesi gerekmez. Başka bir deyişle, kullanıcıların zamanlama sıkıştırması, kaldırılmış uygulama kayıtlarının yönetilmesi, Bloom filtrelerinin ve çoğaltmaların ayarlanması ile ilgilenmesi gerekmez. Azure Cosmos DB, bu sorunları yönetme yükünü ortadan kaldırır ve uygulama mantığına odaklanmanıza olanak sağlar.

**Mevcut kodunuzu ve araçları kullanma yeteneğini**: Azure Cosmos DB, mevcut Cassandra SDK'ları ve araçları ile kablo protokolü düzeyinde uyumluluk sağlar. Bu uyumluluk, küçük değişikliklerle Azure Cosmos DB Cassandra API’si ile mevcut kod tabanınızı kullanabilmenizi sağlar.

**Aktarım hızı ve depolama esneklik**: Azure Cosmos DB, tüm bölgeler arasında aktarım hızı kesin ve Azure portalı, PowerShell veya CLI ile sağlanan aktarım hızını ölçeklendirebilirsiniz sağlar operations. Tahmin edilebilir performansla gerektiğinde tablolarınız için depolama alanını ve aktarım hızını esnek şekilde ölçeklendirebilirsiniz.

**Genel dağıtım ve kullanılabilirlik**: Azure Cosmos DB genel olarak tüm Azure bölgelerindeki veri dağıtmak ve düşük gecikmeli veri erişimi ve yüksek kullanılabilirlik sağlarken verileri yerel olarak hizmet olanağı sağlar. Azure Cosmos DB, ek işletim yükü getirmeden bir bölge içinde %99,99 gibi yüksek kullanılabilirlik ve bölgeler arasında %99,999'luk okuma ve yazma kullanılabilirliği sağlar. [Verileri genel olarak dağıtma](distribute-data-globally.md) makalesinden daha fazla bilgi edinin. 

**Seçim tutarlılık**: Azure Cosmos DB tutarlılık ve performans en iyi bir denge sağlamak için beş iyi tanımlanmış tutarlılık düzeyi sunar. Bu tutarlılık düzeyleri güçlü, sınırlanmış eskime durumu, oturum, tutarlı önek ve son şeklindedir. Bu iyi tanımlanmış, pratik ve sezgisel tutarlılık düzeyleri, geliştiricilerin tutarlılık, kullanılabilirlik ve gecikme süresi arasında sağlam bir denge kurmasına olanak tanır. [Tutarlılık düzeyleri](consistency-levels.md) makalesinden daha fazla bilgi edinin. 

**Kurumsal sınıf**: Azure cosmos DB sağlar [uyumluluk sertifikaları](https://www.microsoft.com/trustcenter) kullanıcıların kullanabilir platform güvenli bir şekilde sağlamak için. Azure Cosmos DB ayrıca durağan ve hareketli durumlarda şifreleme, IP güvenlik duvarı ve denetim düzlemi etkinlikleri için denetim günlükleri sunar.

## <a name="next-steps"></a>Sonraki adımlar

* Cassandra API verilerini oluşturmak ve yönetmek için aşağıdaki dile özgü uygulamaları derlemeye hızlıca başlayabilirsiniz:
  - [Node.js uygulaması](create-cassandra-nodejs.md)
  - [.NET uygulaması](create-cassandra-dotnet.md)
  - [Python uygulaması](create-cassandra-python.md)

* Java uygulaması kullanarak [Cassandra API hesabı, veritabanı ve tablo oluşturmaya](create-cassandra-api-account-java.md) başlama.

* Java uygulaması kullanarak [Cassandra API tablosuna örnek verileri yükleme](cassandra-api-load-data.md).

* Java uygulaması kullanarak [Cassandra API hesabından verileri sorgulama](cassandra-api-query-data.md).

* Azure Cosmos DB Cassandra API’si tarafından desteklenen Apache Cassandra özellikleri hakkında bilgi edinmek için [Cassandra desteği](cassandra-support.md) makalesine bakın.

* [Sık Sorulan Sorular](faq.md#cassandra)’ı okuyun.
