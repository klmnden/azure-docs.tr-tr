---
title: Güvenlik - Azure Cosmos DB veritabanı
description: Azure Cosmos DB, verileriniz için veritabanı koruma ve veri güvenliğini nasıl sağladığını öğrenin.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: rimman
ms.openlocfilehash: d37f373e98835846c2d29130781706c400086c0e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60889543"
---
# <a name="security-in-azure-cosmos-db---overview"></a>Azure Cosmos DB'de - güvenlik genel bakış

Bu makalede, en iyi güvenlik uygulamaları, veritabanı ve önleyin, algılayın ve veritabanı ihlallerine yanıt yardımcı olması için Azure Cosmos DB tarafından sunulan temel özellikleri açıklanmaktadır.
 
## <a name="whats-new-in-azure-cosmos-db-security"></a>Azure Cosmos DB güvenlik yenilikler nelerdir?

Bekleme sırasında şifreleme belgeler ve tüm Azure bölgelerinde Azure Cosmos DB'de depolanan yedeklemeler için kullanıma sunulmuştur. Bekleme sırasında şifreleme, bu bölgede yeni ve mevcut müşteriler için otomatik olarak uygulanır. Herhangi bir şey yapılandırmak için gerek yoktur; aynı harika gecikme süresi, aktarım hızı, kullanılabilirlik alın ve olarak önce verilerinizi olduğunu bilmesinin avantajı ile bekleyen veri şifrelemesi ile güvenli bir işlevdir.

## <a name="how-do-i-secure-my-database"></a>Veritabanım güvenliğini nasıl sağlayabilirim? 

Veri güvenliği, müşterinin ve veritabanı sağlayıcınız arasında paylaşılan bir sorumluluğundadır. Seçtiğiniz veritabanı sağlayıcıya bağlı olarak, yürütmek sorumluluk miktarını farklılık gösterebilir. Şirket içi bir çözüm seçerseniz, hiçbir kolay bir görevdir, donanım - fiziksel güvenlik için uç nokta koruması kadar her şeyi vermeniz gerekir. Azure Cosmos DB gibi PaaS bulut veritabanı sağlayıcısı seçerseniz, bölgenizdeki sorunlu önemli ölçüde küçültür. Aşağıdaki görüntüde, ödünç Microsoft [bulut bilgi işlem için paylaşılan sorumlulukları](https://aka.ms/sharedresponsibility) Teknik İnceleme de gösterir nasıl bir Azure Cosmos DB gibi PaaS sağlayıcısıyla sizin sorumluluğunuzdadır azaltır.

![Müşteri ve veritabanı sağlayıcısı sorumlulukları](./media/database-security/nosql-database-security-responsibilities.png)

Önceki şemada, üst düzey bulut güvenlik bileşenleri gösterilmektedir, ancak hangi öğeleri özellikle veritabanı çözümünüz için yükleme konusunda endişelenmenize gerek? Ve çözümleri birbirleriyle nasıl karşılaştırabilirsiniz? 

Gereksinimleri, veritabanı sistemleri karşılaştırmak aşağıdaki denetim listesindeki adımları öneririz:

- Ağ güvenliği ve güvenlik duvarı ayarları
- Kullanıcı kimlik doğrulaması ve hassas kullanıcı denetimleri
- Bölgesel hatalar için genel veri çoğaltma olanağı
- Bir veri merkezinden yük devretme gerçekleştirme becerisi
- Bir veri merkezinde yerel veri çoğaltma
- Otomatik veri yedekleme
- Silinen verileri yedeklerden geri yükleme
- Hassas verileri yalıtmak ve korumak
- Saldırıları için izleme
- Saldırılara karşı yanıt
- Coğrafi sınır veri veri İdaresi kısıtlamaları uyması olanağı
- Korumalı veri merkezlerindeki sunucular fiziksel koruma
- Sertifikalar

Ve bunu görünse de açık, son [büyük ölçekli veritabanı ihlallerini](https://thehackernews.com/2017/01/mongodb-database-security.html) bize aşağıdaki gereksinimleri basit ancak kritik önemini hatırlat:
- Güncel tutulduğu sunucuları düzeltme eki
- HTTPS varsayılan/SSL şifrelemesi
- Yönetim hesapları ile güçlü parolalar

## <a name="how-does-azure-cosmos-db-secure-my-database"></a>Azure Cosmos DB veritabanı güvenliğini nasıl?

Önceki listesini yeniden gözden geçirelim - bu güvenlik gereksinimleri kaç Azure Cosmos DB sağlar mı? Tek tek her.

Şimdi her birini ayrıntılı sayesinde çalışın.

|Güvenlik gereksinimi|Azure Cosmos DB'nin güvenlik yaklaşımı|
|---|---|
|Ağ güvenliği|İlk veritabanınızı güvenli hale getirmek için koruma katmanı bir IP Güvenlik Duvarı'nı kullanmaktır. Azure Cosmos DB, ilke temelli IP tabanlı erişim denetimlerini gelen güvenlik duvarı desteği için destekler. Geleneksel veritabanı sistemleri tarafından kullanılan güvenlik duvarı kuralları için IP tabanlı erişim denetimlerine benzerdir, ancak bir Azure Cosmos DB veritabanı hesabını yalnızca onaylanmış bir makine veya Bulut hizmetlerinizi kümesinden erişilebilir olması genişletilir. <br><br>Azure Cosmos DB, belirli bir IP adresi (168.61.48.0), bir IP aralığı (168.61.48.0/8) ve IP aralıkları ve birleşimleri etkinleştirmek sağlar. <br><br>Bu izin verilen liste dışındaki makinelerden gelen tüm istekler, Azure Cosmos DB tarafından engellenir. Onaylanan machines ve cloud services ardından gelen istekleri kaynaklara erişim denetimi verilecek kimlik doğrulama sürecini tamamlamanız gerekir.<br><br>Daha fazla bilgi [Azure Cosmos DB güvenlik duvarı desteği](firewall-support.md).|
|Yetkilendirme|Azure Cosmos DB yetkilendirme için karma tabanlı ileti kimlik doğrulama kodu (HMAC) kullanır. <br><br>Her isteğin gizli hesap anahtarı kullanarak karma uygulanır ve sonraki base-64 kodlanmış karma her çağrı için Azure Cosmos DB ile gönderilir. İstek doğrulamak için Azure Cosmos DB hizmetini doğru gizli anahtarı ve özelliklerini bir karma değer oluşturmak için kullanır ve ardından istekteki bir değerle karşılaştırır. İki değer eşleşen, işlemi başarıyla yetkili ve isteği işleyen, aksi takdirde bir Yetkilendirme hatası yoktur ve istek reddedilir.<br><br>Kullanabilirsiniz bir [ana anahtarı](secure-access-to-data.md#master-keys), veya bir [kaynak belirtecini](secure-access-to-data.md#resource-tokens) ayrıntılı erişim gibi bir belge bir kaynağa izin verme.<br><br>Daha fazla bilgi [Azure Cosmos DB kaynaklarına erişimi güvenli hale getirme](secure-access-to-data.md).|
|Kullanıcıları ve izinleri|Ana anahtarı için bir hesap kullanarak, kullanıcı kaynakları ve veritabanı başına izin kaynakları oluşturabilirsiniz. Kaynak belirtecini bir veritabanında bir izinle ilişkili ve kullanıcının olup olmadığını belirler. veritabanında uygulama kaynağı için erişim (okuma-yazma, salt okunur veya erişim yok). Kapsayıcı, belgeler, ekleri, saklı yordamlar, tetikleyiciler ve UDF'ler uygulama kaynaklarını içerir. Kaynak belirteci sağlayın ya da kaynağa erişimi reddetmek için kimlik doğrulaması sırasında sonra kullanılır.<br><br>Daha fazla bilgi [Azure Cosmos DB kaynaklarına erişimi güvenli hale getirme](secure-access-to-data.md).|
|Active directory ile tümleştirme (RBAC)| Ayrıca, bu tablonun altındaki ekran görüntüsünde gösterildiği gibi Azure portalında, erişim denetimi (IAM) kullanarak veritabanı hesabı erişim sağlayabilir. IAM rol tabanlı erişim denetimi sağlar ve Active Directory ile tümleşir. Bireyler ve aşağıdaki görüntüde gösterildiği gibi gruplar için yerleşik bir rol veya özel roller kullanabilirsiniz.|
|Genel çoğaltma|Azure Cosmos DB, verilerinizi, Azure'un dünya çapındaki veri merkezlerinden herhangi birine bir düğmeye tıklayarak çoğaltmayı olanak tanıyan anahtar teslim küresel dağıtım sunar. Genel çoğaltma küresel olarak ölçeklendirin ve düşük gecikme süreli erişim için verilerinizi dünyanın dört bir yanındaki sağlamanıza olanak tanır.<br><br>Güvenlik bağlamında, küresel çoğaltma bölgesel hatalarına karşı verileri koruma sağlar.<br><br>[Verileri küresel olarak dağıtma](distribute-data-globally.md) bölümünde daha fazlasını öğrenin.|
|Bölgesel yük devretme|Verilerinizi birden fazla veri merkezinde çoğaltıldığından, Azure Cosmos DB bölgesel veri merkezi çevrimdışı duruma işlemlerinizi otomatik olarak yapar. Yük devretme bölge içinde verilerinizi çoğaltılır bölgeleri kullanma öncelikli listesi oluşturabilirsiniz. <br><br>Daha fazla bilgi [Azure Cosmos DB'de bölgesel yük devretme](high-availability.md).|
|Yerel çoğaltma|Bile içinde tek bir veri merkezi, Azure Cosmos DB veri seçimi vererek yüksek kullanılabilirlik için otomatik olarak çoğaltır [tutarlılık düzeyleri](consistency-levels.md). Bu % 99,99 oranında garanti eder [kullanılabilirlik SLA'sı](https://azure.microsoft.com/support/legal/sla/cosmos-db) tek bölgeli tüm hesaplar ve çok bölgeli tüm hesaplar tutarlılıkla ve kullanılabilirliğine çok bölgeli tüm veritabanı hesaplarında % 99,999 okuyun.|
|Otomatik çevrimiçi yedeklemeler|Azure Cosmos DB veritabanları düzenli olarak yedeklenen ve georedundant deposunda depolanır. <br><br>Daha fazla bilgi [otomatik çevrimiçi yedekleme ve geri yükleme Azure Cosmos DB ile](online-backup-and-restore.md).|
|Silinmiş öğeleri geri yükleme verileri|Otomatik çevrimiçi yedeklemeler, yanlışlıkla olayından sonra yaklaşık 30 güne kadar sildiğinizi verileri kurtarmak için kullanılabilir. <br><br>Daha fazla bilgi [otomatik çevrimiçi yedekleme ve geri yükleme işlemi Azure Cosmos DB](online-backup-and-restore.md)|
|Hassas verileri yalıtmak ve korumak|Tüm veri bölgelerinde yenilikler listelenir? Şimdi, bekleme sırasında şifrelenir.<br><br>Belirli kullanıcılara yalnızca okuma erişimi sınırlandırılabilir veya kişisel veri ve diğer gizli verileri belirli bir kapsayıcı ve okuma-yazma için ayrılmış olabilir.|
|Saldırıları için İzleyici|Kullanarak [denetim günlüğü ve etkinlik günlükleri](logging.md), hesabınız için normal ve anormal etkinlik izleyebilirsiniz. Hangi işlemleri işlemi gerçekleştiğinde, işlem ve çok daha olarak bu tablodan sonraki ekran görüntüsünde gösterilen durum işlemi başlatan kaynaklarınız üzerinde gerçekleştirilen görüntüleyebilirsiniz.|
|Saldırılara karşılık vermek|Olası bir saldırı raporlamak üzere Azure desteğine görüştüğünüz sonra bir 5 adımlı olayı yanıt süreci başlatılır. 5 adımlı işlem amacı, bir sorun algılandı ve araştırma başlatıldıktan sonra normal hizmet güvenliği ve işlemleri mümkün olan en kısa sürede geri yüklemektir.<br><br>Daha fazla bilgi [bulutta Microsoft Azure güvenlik yanıtı](https://aka.ms/securityresponsepaper).|
|Şirketin coğrafı|Azure Cosmos DB bağımsız bölgeler (örneğin, Almanya, Çin, ABD Devleti) için veri yönetimi sağlar.|
|Korunan tesislerde|Azure Cosmos DB'de veriler, Azure'un korumalı veri merkezlerinde ssd'lerde depolanır.<br><br>Daha fazla bilgi [Microsoft küresel veri merkezleri](https://www.microsoft.com/en-us/cloud-platform/global-datacenters)|
|HTTPS/SSL/TLS şifrelemesi|Tüm istemci hizmeti Azure Cosmos DB etkileşimleri SSL/TLS 1.2 özellikli olan. Ayrıca, tüm içi veri merkezi ve çapraz veri merkezine çoğaltma SSL/TLS 1.2 zorlanan vardır.|
|Bekleme sırasında şifreleme|Azure Cosmos DB'ye depolanan tüm veriler bekleme durumundayken şifrelenir. Daha fazla bilgi [Azure Cosmos DB bekleme sırasında şifreleme](./database-encryption-at-rest.md)|
|Düzeltme eki uygulama sunucuları|Yönetilen bir veritabanı Azure Cosmos DB, sizin için otomatik olarak yapılır yönetin ve sunucuları, düzeltme eki ihtiyacını ortadan kaldırır.|
|Yönetim hesapları ile güçlü parolalar|Biz bile bu gereksinim bahsetmeniz gerekir, ancak bazı rakiplerimizin, bir yönetici hesabı parolası ile Azure Cosmos DB'de olması mümkün değildir geldiğimize inanmak.<br><br> Güvenliği SSL ve HMAC gizli tabanlı kimlik doğrulaması aracılığıyla varsayılan olarak yerleşik.|
|Güvenlik ve veri koruma sertifikaları|Sertifikaları veri listesi kadar en genel görmek için [Azure uyumluluk site](https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings) aktardığınızdan yanı sıra [Azure uyumluluk belge](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) tüm sertifikaları edinerek (Cosmos arayın). 25 Nisan 2018'de post kullanıma daha odaklı okumak için [Azure #CosmosDB: Güvenli, özel, uyumlu](https://azure.microsoft.com/blog/azure-cosmosdb-secure-private-compliant/) içeren SOC 1/2 Tür 2, HITRUST, PCI DSS düzey 1, ISO 27001, HIPAA, FedRAMP yüksek ve diğer birçok.

Azure portalında erişim denetimi (IAM) kullanarak Active directory tümleştirmesi (RBAC) aşağıdaki ekran görüntüsünde gösterilmektedir: ![Veritabanı güvenliği gösteren Azure portalı - erişim denetimi (IAM)](./media/database-security/nosql-database-security-identity-access-management-iam-rbac.png)

Aşağıdaki ekran görüntüsünde, hesabınız izlemek için Denetim günlüğü ve etkinlik günlüklerini nasıl kullanacağınızı gösterir: ![Azure Cosmos DB için etkinlik günlükleri](./media/database-security/nosql-database-security-application-logging.png)

## <a name="next-steps"></a>Sonraki adımlar

Ana anahtarları ve kaynak belirteçleri hakkında daha fazla ayrıntı için [Azure Cosmos DB verilere erişimin güvenliğini sağlama](secure-access-to-data.md).

Denetim günlüğü hakkında daha fazla ayrıntı için bkz. [Azure Cosmos DB tanılama günlüğüne kaydetme](logging.md).

Microsoft sertifikaları hakkında daha fazla ayrıntı için bkz. [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/).
