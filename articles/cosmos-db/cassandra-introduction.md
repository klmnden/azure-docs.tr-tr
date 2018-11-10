---
title: Azure Cosmos DB Cassandra API'ye giriş | Microsoft Belgeler
description: Mevcut uygulamaları "yükseltmek ve kaydırmak" için Azure Cosmos DB’yi ve yeni uygulamalar geliştirmek için zaten aşina olduğunuz Cassandra sürücüleri ve CQL ile Cassandra API’yi nasıl kullanabileceğinizi öğrenin.
services: cosmos-db
author: kanshiG
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.devlang: na
ms.topic: overview
ms.date: 09/24/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 1037f7921093d38d9020bafd9fd3597f27ca5011
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230259"
---
# <a name="introduction-to-the-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API’sine giriş

Azure Cosmos DB Cassandra API’si, [Apache Cassandra](https://cassandra.apache.org/) için yazılmış uygulamalara yönelik veri deposu olarak kullanılabilir. Başka bir deyişle, mevcut Cassandra uygulamanız artık CQLv4 ile uyumlu mevcut [Apache sürücülerini](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver) kullanarak Azure Cosmos DB Cassandra API’si ile iletişim kurabilir. Çoğu durumda yalnızca bir bağlantı dizesini değiştirerek Apache Cassandra’yı kullanmaktan Azure Cosmos DB’nin Cassandra API’sini kullanmaya geçiş yapabilirsiniz. 

Cassandra API’si; Cassandra Sorgu Dili (CQL), Cassandra tabanlı araçları (cqlsh gibi) ve zaten aşina olduğunuz Cassandra istemci sürücülerini kullanarak Azure Cosmos DB’de depolanan verilerle etkileşim kurmanızı sağlar.

## <a name="what-is-the-benefit-of-using-apache-cassandra-api-for-azure-cosmos-db"></a>Azure Cosmos DB için Apache Cassandra API'si kullanmanın avantajı nedir?

**İşletim yönetimi gerekmez**: Tam olarak yönetilen bulut hizmeti olarak Azure Cosmos DB Cassandra API’si, işletim sistemleri, JVM ve YAML dosyaları genelindeki sayısız ayarı ve bunların etkileşimlerini yönetme ve izleme yükünü ortadan kaldırır. Azure Cosmos DB; aktarım hızı, gecikme süresi, depolama, kullanılabilirlik için izleme ve yapılandırılabilir uyarılar sunar.

**Performans yönetimi**: Azure Cosmos DB, yüzde 99. dilim için SLA ile desteklenen, garantili düşük gecikme süreli okuma ve yazmalar sunar. Kullanıcıların yüksek performanslı ve düşük gecikme süreli okuma ve yazma işlemleri sağlamak için işletim yükünden endişelenmesi gerekmez. Başka bir deyişle, kullanıcıların zamanlama sıkıştırması, kaldırılmış uygulama kayıtlarının yönetilmesi, Bloom filtrelerinin ve çoğaltmaların ayarlanması ile ilgilenmesi gerekmez. Azure Cosmos DB, bu sorunları yönetme yükünü ortadan kaldırır ve uygulama mantığına odaklanmanıza olanak sağlar.

**Mevcut kodu ve araçları kullanma olanağı**: Azure Cosmos DB, mevcut Cassandra SDK’ları ve araçları ile kablo protokolü düzeyinde uyumluluk sunar. Bu uyumluluk, küçük değişikliklerle Azure Cosmos DB Cassandra API’si ile mevcut kod tabanınızı kullanabilmenizi sağlar.

**Aktarım hızı ve depolama esnekliği**: Azure Cosmos DB, tüm bölgelerde garantili aktarım hızı sağlar ve Azure portal, PowerShell veya CLI işlemleriyle sağlanan aktarım hızını ölçeklendirebilir. Tahmin edilebilir performansla gerektiğinde tablolarınız için depolama alanını ve aktarım hızını esnek şekilde ölçeklendirebilirsiniz.

**Genel dağıtım ve kullanılabilirlik**: Azure Cosmos DB, tüm Azure bölgelerinde verileri genel olarak dağıtma olanağı sağlar ve bir yandan düşük gecikme süreli veri erişimini ve yüksek kullanılabilirliği sağlarken diğer yandan yerel olarak verileri sunar. Azure Cosmos DB, ek işletim yükü getirmeden bir bölge içinde %99,99 gibi yüksek kullanılabilirlik ve bölgeler arasında %99,999'luk okuma ve yazma kullanılabilirliği sağlar. [Verileri genel olarak dağıtma](distribute-data-globally.md) makalesinden daha fazla bilgi edinin. 

**Seçimde tutarlılık**: Azure Cosmos DB, tutarlılık ve performans arasında en iyi oranı elde etmek için beş iyi tanımlanmış tutarlılık düzeyi seçeneği sunar. Bu tutarlılık düzeyleri güçlü, sınırlanmış eskime durumu, oturum, tutarlı önek ve son şeklindedir. Bu iyi tanımlanmış, pratik ve sezgisel tutarlılık düzeyleri, geliştiricilerin tutarlılık, kullanılabilirlik ve gecikme süresi arasında sağlam bir denge kurmasına olanak tanır. [Tutarlılık düzeyleri](consistency-levels.md) makalesinden daha fazla bilgi edinin. 

**Kurumsal düzey**: Azure Cosmos DB, kullanıcıların platformu güvenli bir şekilde kullanabilmesini sağlamak için [uyumluluk sertifikaları](https://www.microsoft.com/trustcenter) sunar. Azure Cosmos DB ayrıca durağan ve hareketli durumlarda şifreleme, IP güvenlik duvarı ve denetim düzlemi etkinlikleri için denetim günlükleri sunar.

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
