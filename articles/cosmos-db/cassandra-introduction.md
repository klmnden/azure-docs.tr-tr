---
title: "Azure Cosmos DB Cassandra API giriş | Microsoft Docs"
description: "Nasıl \"yükseltme ve shift\" var olan uygulamaları için Azure Cosmos DB kullanın ve Cassandra Cassandra sürücüleri ve zaten aşina CQL kullanarak API'yi kullanarak yeni uygulamalar öğrenin."
services: cosmos-db
author: govindk
manager: jhubbard
documentationcenter: 
ms.assetid: 73839abf-5af5-4ae0-a852-0f4159bc00a0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2017
ms.author: govindk
ms.openlocfilehash: 62e56a611d3f720172ace83a4b407a962ec69ea8
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="introduction-to-apache-cassandra-api-for-azure-cosmos-db"></a>Azure Cosmos DB Apache Cassandra API için giriş

Azure Cosmos DB Cassandra API (Önizleme) için Apache Cassandra premium özellikleri gibi yazılan uygulamaları için sağlar:

* [Ölçeklenebilir depolama boyutu ve üretilen iş](partition-data.md).
* [Anahtar teslim genel dağıtım](distribute-data-globally.md)
* 99 yüzdebirlikte tek basamaklı milisaniyelik gecikme süresi.
* [Beş iyi tanımlanmış tutarlılık düzeyleri](consistency-levels.md)
* [Otomatik dizin oluşturma verileri](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) gerektirmeden şema ve dizin yönetimi ile ilgilidir. 
* Yüksek kullanılabilirlik, tarafından yedeklenen tüm garanti [endüstri lideri SLA'ları](https://azure.microsoft.com/support/legal/sla/cosmos-db/)

## <a name="what-is-the-azure-cosmos-db-apache-cassandra-api"></a>Azure Cosmos DB Apache Cassandra API nedir?

Azure Cosmos DB yazılmış uygulamalar için veri deposu olarak kullanılabilir [Apache Cassandra](https://cassandra.apache.org/), Apache Cassandra API'yi kullanarak. Varolan kullanarak buna [Apache lisanslı CQLv4 ile uyumlu sürücüleri](https://cassandra.apache.org/doc/latest/getting_started/drivers.html?highlight=driver), uygulamanızın Cassandra Azure Cosmos DB Cassandra API'si ile artık iletişim kurmak için yazılmış. Çoğu durumda, sadece bir bağlantı dizesi değiştirilerek Azure Cosmos veritabanı Apache Cassandra API kullanarak Apache Cassandra kullanarak geçiş yapabilirsiniz. Bu işlevini kullanarak, kolayca oluşturabilir ve çalışma Cassandra API veritabanı uygulamaları Azure bulut ile Azure Cosmos veritabanı genel dağıtım ve [kapsamlı endüstri lideri SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db), bilinen kullanmaya devam ederken becerileri ve araçları Cassandra API'si.

![Azure Cosmos DB Cassandra API](./media/cassandra-introduction/cosmosdb-cassandra.png)

Cassandra istemci sürücüleri zaten alışık olduğunuz ve Azure Cosmos (gibi CQLSH) Cassandra sorgu tabanlı dil araçları kullanarak veritabanında depolanan verileri ile etkileşim kurmak Cassandra API sağlar. Bu Microsoft Mechanics video sorumlu mühendislik Yöneticisi Kirill Gavrylyuk ile hakkında daha fazla bilgi edinin.

> [!VIDEO https://www.youtube.com/embed/1Sf4McGN1AQ]
>

## <a name="what-is-the-benefit-of-using-apache-cassandra-api-for-azure-cosmos-db"></a>Apache Cassandra API için Azure Cosmos DB kullanmanın faydası nedir?

**Operations management**: Gerçek hizmet tam olarak yönetilen olarak Azure Cosmos DB Cassandra API Yöneticiler yönetme ve işletim sistemi, JVM ve yaml dosyaları ve bunların Interplay arasında çok ayarları izleme hakkında endişelenmeniz gerekmez sağlar. Azure Cosmos DB verimlilik, gecikme, depolama ve kullanılabilirlik ve yapılandırılabilir uyarıları izleme sağlar. 

**Performans Yönetimi**: Azure Cosmos DB için 99 yazar ve yedeklenen SLA düşük gecikme süresi okuma sağlar. Kullanıcıların iyi okuma sağlamak ve SLA'ları yazmak için işlem yükünü çok hakkında endişelenmeniz gerekmez. Bu, genellikle silinmiş öğe, çiçek tomurcukları filtreler ayarlayarak ve çoğaltma gecikmelere yönetme sıkıştırma, zamanlama içerir. Bunlar yönetme endişe sorunları ve olanak sağlayan azure Cosmos DB alır hemen üzerinde uygulama sonuçlara odaklanın.

**Otomatik dizin oluşturma**: Azure Cosmos DB Cassandra API'si veritabanı tablosunun tüm sütunları otomatik olarak dizinler. Azure Cosmos DB sorgulamalarını hızlandırmak için ikincil dizinlerin oluşturulmasını gerektirmez. Bu, düşük gecikme süresi okuma ve yazma otomatik tutarlı dizin yaparken deneyimi sunar. 

**Var olan kodu ve araçlarını kullanma yeteneğini**: Azure Cosmos DB kablo protokolü düzeyi uyumluluğunu varolan SDK'lar ve araçlar sağlar. Bu uyumluluk varolan kullanabileceğiniz sağlar Önemsiz değişikliklerle Cassandra API, Azure Cosmos DB ile codebase.

**Üretilen iş ve depolama esneklik**: Azure Cosmos platform, basit portalı, PowerShell aracılığıyla bölgeler arasında garantili verimlilik esneklik sağlar veya CLI işlemleri. Uygulamanız büyüdükçe, Özellikler esnek Azure Cosmos DB tabloları tahmin edilebilir performansla sorunsuz bir şekilde ölçeklendirebilirsiniz. Azure Cosmos DB neredeyse sınırsız depolama boyutlarına ölçeklendirebilirsiniz Cassandra API tabloları destekler. 

**Genel dağıtım ve kullanılabilirlik**: Azure Cosmos DB kullanıcıların kullanılabilirlik sağlarken düşük gecikme süreli bir deneyim sağlamak için Azure bölgeleri verilerinin dağıtma olanağı sağlar. Bir bölge içinde % 99,99 kullanılabilirlik Azure Cosmos DB sağlar ve hiçbir işlem yüküyle bölgelere kullanılabilirlik %99.999 okuyun. Azure Cosmos DB kullanılabilir 30 + [Azure bölgeleri](https://azure.microsoft.com/regions/services/). Daha fazla bilgi edinin [verilerini genel dağıtmak](distribute-data-globally.md). 

**Tutarlılık seçimine**: Azure Cosmos DB beş iyi tanımlanmış tutarlılık düzeyleri tutarlılık ve performans arasında en iyi dengeyi elde etmek için seçimi sağlar. Güçlü, sınırlanmış eskime durumu, oturum, tutarlı önek bu tutarlılık düzeyleri olan ve son. Bu ayrıntılı ve iyi tanımlanmış tutarlılık düzeyleri ses dengelemeler tutarlılık, kullanılabilirlik ve gecikme süresi arasında yapmak geliştiriciye izin verir. Daha fazla bilgi için bkz. [Kullanılabilirlik ve performansı en üst düzeye çıkarmak için tutarlılık düzeylerini kullanma](consistency-levels.md). 

**Kurumsal düzeyde**: güvenli ve varsayılan – DB sağlayan Azure cosmos uyumlu [uyumluluk sertifikalarından](https://www.microsoft.com/trustcenter) kullanıcıların, uyumluluk sorunları hakkında endişelenmeden platform kullanabilir emin olmak için. Azure Cosmos DB şifreleme REST ve hareket, IP Güvenlik Duvarı'nı ve denetim düzlemi etkinlikler için denetim günlüklerini de sağlar.  

<a id="sign-up-now"></a>
## <a name="sign-up-now"></a>Hemen kaydolun 

Bir Azure aboneliğiniz zaten varsa, Cassandra API (Önizleme) programa katılmak kaydolabilirsiniz [Azure portal](https://aka.ms/cosmosdb-cassandra-signup).  Azure'da yeniyseniz, kaydolun bir [ücretsiz deneme sürümü](https://azure.microsoft.com/free) 12 ay ücretsiz Azure Cosmos DB erişimin nereden. Cassandra API (Önizleme) programı'na erişim istemek için aşağıdaki adımları tamamlayın.

1. İçinde [Azure portal](https://portal.azure.com), tıklatın **kaynak oluşturma** > **veritabanları** > **Azure Cosmos DB**. 

2. Yeni hesap sayfasında seçin **Cassandra** API kutusunda. 

3. İçinde **abonelik** kutusunda, bu hesap için kullanmak istediğiniz Azure aboneliğini seçin.

4. Tıklatın **önizlemeye hemen kaydolun**.

    ![Azure Cosmos DB Cassandra API](./media/cassandra-introduction/cassandra-sign-up.png)

3. Bugün bölmesinde önizlemek için kaydolun tıklatın **Tamam**. 

    İsteği gönderirseniz sonra durum değişikliklerini **onay bekleyen** yeni hesap bölmesinde. 

İsteği gönderdikten sonra isteğiniz onaylandıktan e-posta bildirimi için bekleyin. İsteği nedeniyle yüksek hacimli bir hafta içinde bildirim almanız gerekir. İsteği tamamlamak için bir destek bileti oluşturun gerekmez. Sıraya alınmış olan istekleri incelenecektir. 

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım
Önizleme programına katıldıktan sonra Cassandra API'sini kullanarak bir uygulama oluşturmak için Cassandra API quickstarts izleyin:

* [Hızlı Başlangıç: bir Cassandra Node.js ve Azure Cosmos DB ile web uygulaması oluşturma](create-cassandra-nodejs.md)
* [Hızlı Başlangıç: bir Cassandra Java ve Azure Cosmos DB ile web uygulaması oluşturma](create-cassandra-java.md)
* [Hızlı Başlangıç: bir Cassandra .NET ve Azure Cosmos DB ile web uygulaması oluşturma](create-cassandra-dotnet.md)
* [Hızlı Başlangıç: bir Cassandra Python ve Azure Cosmos DB ile web uygulaması oluşturma](create-cassandra-python.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB Cassandra API'si hakkında bilgi Azure Cosmos DB belgeler genel tümleştirilmiştir, ancak başlamanıza yardımcı olmak için birkaç işaretçi şunlardır:

* İzleyin [Quickstarts](create-cassandra-nodejs.md) Git örnek kullanarak bir hesap ve yeni bir uygulama oluşturmak için
* İzleyin [öğretici](tutorial-develop-cassandra-java.md) program aracılığıyla yeni bir uygulama oluşturmak için.
* İzleyin [alma Cassandra veri öğretici](cassandra-import-data.md) mevcut verilerinizi Azure Cosmos Veritabanına içeri aktarmak için.
* Okuma [SSS](faq.md#cassandra).
