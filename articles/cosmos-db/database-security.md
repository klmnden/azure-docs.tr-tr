---
title: Güvenlik - Azure Cosmos DB veritabanı | Microsoft Docs
description: Azure Cosmos DB verileriniz için veritabanı koruma ve veri güvenliği nasıl sağladığını öğrenin.
keywords: nosql veritabanı güvenlik, bilgi güvenliği, veri güvenliği, veritabanı şifreleme, veritabanı koruma, güvenlik ilkeleri, güvenlik test etme
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: a02a6a82-3baf-405c-9355-7a00aaa1a816
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: sngun
ms.openlocfilehash: 2b0cb1ed92694a7859912dfe0339ef719c0d15ef
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="azure-cosmos-db-database-security"></a>Azure Cosmos DB veritabanı güvenliği

Bu makalede, en iyi güvenlik uygulamalarını, veritabanı ve engellemenize, algılamanıza ve veritabanı ihlallerine yanıt yardımcı olması için Azure Cosmos DB tarafından sunulan temel özellikleri açıklanmaktadır.
 
## <a name="whats-new-in-azure-cosmos-db-security"></a>Yeni Azure Cosmos DB güvenlik nedir?

Bekleyen şifreleme belgeler ve Azure Cosmos DB tüm Azure bölgeleri içinde depolanan yedeklemeler için kullanıma sunulmuştur. Bekleyen şifreleme Bu bölgeler hem yeni hem de mevcut müşteriler için otomatik olarak uygulanır. Herhangi bir şey yapılandırmak için gerek yoktur; get aynı harika gecikme, işleme, kullanılabilirlik ve olarak önce verilerinizi bilmesinin avantajı ile bekleyen şifreleme ile güvenli bir işlevdir.

## <a name="how-do-i-secure-my-database"></a>Veritabanım güvenliğini nasıl sağlayabilirim? 

Veri güvenliği, müşteri ve veritabanı sağlayıcınız arasında paylaşılan bir sorumluluğundadır. Seçtiğiniz veritabanı sağlayıcısı bağlı olarak, yapmanız sorumluluk miktarını farklılık gösterebilir. Bir şirket içi çözüm seçerseniz, her şeyi kolay bir görev, donanım - fiziksel güvenlik için son nokta korumasını sağlamanız gerekir. Bir PaaS bulut veritabanı sağlayıcısı Azure Cosmos DB gibi seçerseniz, bölgenizdeki sorun önemli ölçüde küçültür. Aşağıdaki resimde, Microsoft'un Web sitesinden ödünç [bulut bilgi işlemin paylaşılan sorumlulukları](https://aka.ms/sharedresponsibility) teknik incelemesi gösterir nasıl Azure Cosmos DB gibi bir PaaS sağlayıcısı ile sizin sorumluluğunuzdadır azaltır.

![Müşteri ve veritabanı sağlayıcısı sorumlulukları](./media/database-security/nosql-database-security-responsibilities.png)

Önceki diyagramda üst düzey bulut güvenlik bileşenleri gösterir, ancak hangi öğeleri hakkında özellikle veritabanı çözümünüz için endişelenmenize gerek? Ve çözümleri birbirleriyle nasıl karşılaştırabilirsiniz? 

Aşağıdaki denetim listesinde, veritabanı sistemleri Karşılaştırılacak gereksinimlerinin öneririz:

- Ağ güvenliği ve güvenlik duvarı ayarları
- Kullanıcı kimlik doğrulaması ve hassas kullanıcı denetimleri
- Bölgesel arızalara için genel veri çoğaltma olanağı
- Bir veri merkezi'nden yük devretme işlemlerini gerçekleştirme özelliği
- Bir veri merkezi içinde yerel veri çoğaltma
- Otomatik veri yedeklemeleri
- Silinen veri yedeklerden geri yükleme
- Koruma ve hassas verileri yalıtma
- Saldırıları için izleme
- Saldırılara karşı yanıt
- Veri İdaresi kısıtlamaları için uygun şekilde coğrafi dilimi veri özelliği
- Korumalı veri merkezlerinde sunucuların fiziksel koruma

Ve onu gibi görünse de belirgin, son [büyük ölçekli veritabanı ihlallerini](http://thehackernews.com/2017/01/mongodb-database-security.html) bize aşağıdaki gereksinimleri basit ancak kritik önemi hatırlat:
- Güncel tutulduğundan sunucuları düzeltme eki uygulandı
- HTTPS varsayılan/SSL şifrelemesi
- Güçlü parolalar ile yönetici hesapları

## <a name="how-does-azure-cosmos-db-secure-my-database"></a>Azure Cosmos DB Veritabanım güvenliğini nasıl?

Önceki listeyi geri bakalım - bu güvenlik gereksinimleri kaç Azure Cosmos DB sağlar mı? Tek tek her.

Her birini ayrıntılı içine şimdi edinebilirsiniz.

|Güvenlik gereksinimi|Azure Cosmos DB'ın güvenlik yaklaşımı|
|---|---|---|
|Ağ güvenliği|Bir IP Güvenlik Duvarı'nı kullanarak, veritabanınızın güvenliğini sağlamak için koruma ilk katmanıdır. Azure Cosmos DB gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini güdümlü İlkesi destekler. IP tabanlı erişim denetimlerini geleneksel veritabanı sistemleri tarafından kullanılan güvenlik duvarı kurallarına benzer, ancak böylece Azure Cosmos DB veritabanı hesabı yalnızca onaylanan bir makine veya Bulut Hizmetleri kümesinden erişilebilir genişletilir. <br><br>Azure Cosmos DB belirli bir IP adresi (168.61.48.0), bir IP aralığı (168.61.48.0/8) ve IP aralıkları ve birleşimleri etkinleştirmenize olanak sağlar. <br><br>Bu izin verilenler dışında makinelerden kaynaklanan tüm istekler Azure Cosmos DB tarafından engellendi. Onaylanan machines ve cloud services ardından gelen istekleri kaynaklara erişim denetimi verilecek kimlik doğrulama işlemini tamamlamalısınız.<br><br>Daha fazla bilgi edinin [Azure Cosmos DB Güvenlik Duvarı](firewall-support.md).|
|Yetkilendirme|Azure Cosmos DB yetkilendirme için karma tabanlı ileti kimlik doğrulama kodu (HMAC) kullanır. <br><br>Her istek gizli hesap anahtarı kullanarak karma uygulanır ve sonraki base-64 kodlanmış karma her çağrı Azure Cosmos DB ile gönderilir. İstek doğrulamak için Azure Cosmos DB hizmeti doğru gizli anahtar ve özellikleri karma oluşturmak için kullanır ve ardından istek bir değerle karşılaştırır. İki değer eşleşen, işlemi başarıyla yetkili ve istek işlenir, aksi takdirde bir Yetkilendirme hatası yoktur ve isteği reddedildi.<br><br>Kullanabilirsiniz bir [ana anahtar](secure-access-to-data.md#master-keys), veya bir [kaynak belirteci](secure-access-to-data.md#resource-tokens) hassas bir belge gibi bir kaynağa erişim sağlar.<br><br>Daha fazla bilgi edinin [Azure Cosmos DB kaynaklarına erişimi güvenli hale getirme](secure-access-to-data.md).|
|Kullanıcılar ve izinler|Kullanarak [ana anahtar](#master-key) hesap için kullanıcı ve veritabanı başına izin kaynaklarının oluşturabilirsiniz. A [kaynak belirteci](#resource-token) bir veritabanında bir izinle ilişkili olan ve kullanıcı olup olmadığını belirler veritabanındaki Uygulama kaynağı için erişim (okuma-yazma, salt okunur veya erişimi yok). Uygulama kaynaklarını koleksiyonlar, belgeler, ekleri, saklı yordamlar, tetikleyiciler ve UDF'ler içerir. Sonra kaynak belirteci kimlik doğrulaması sırasında sağlamak veya kaynak erişimini engellemek için kullanılır.<br><br>Daha fazla bilgi edinin [Azure Cosmos DB kaynaklarına erişimi güvenli hale getirme](secure-access-to-data.md).|
|Active directory ile tümleştirme (RBAC)| Ayrıca bu tabloda aşağıdaki ekran görüntüsünde gösterildiği gibi Azure portalında erişim denetimi (IAM) kullanarak veritabanı hesabı için erişim sağlayabilir. IAM rol tabanlı erişim denetimi sağlar ve Active Directory ile tümleştirir. Kişiler ve aşağıdaki görüntüde gösterildiği gibi grupları için yerleşik roller veya özel roller kullanabilirsiniz.|
|Genel çoğaltma|Azure Cosmos DB ve bu sayede verilerinizi Azure'nın dünya çapında veri merkezleri herhangi biri bir düğmeyi tıklatarak ile çoğaltmak anahtar teslimi genel dağıtım sunar. Genel çoğaltma küresel olarak ölçeklendirmeyi ve verilerinize dünyanın düşük gecikmeli erişim sağlamanıza olanak tanır.<br><br>Güvenlik bağlamında genel çoğaltma bölgesel arızalara karşı veri koruma sağlar.<br><br>Daha fazla bilgi edinin [verilerini genel dağıtmak](distribute-data-globally.md).|
|Bölgesel yük devretme|Verilerinizi birden fazla veri merkezinde çoğaltıldığından, Azure Cosmos DB bölgesel veri merkezi çevrimdışı duruma işlemlerinizin otomatik olarak yapar. Verilerinizi çoğaltılan bölgeleri kullanarak yük devretme bölge öncelikli listesi oluşturabilirsiniz. <br><br>Daha fazla bilgi edinin [bölgesel yük devretme işlemlerini Azure Cosmos veritabanı](regional-failover.md).|
|Yerel çoğaltma|Bile içinde tek bir veri merkezi, Azure Cosmos DB verilerini seçimi vererek yüksek kullanılabilirlik için otomatik olarak çoğaltır [tutarlılık düzeylerini](consistency-levels.md). Bu % 99,99 garanti [kullanılabilirlik SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db) tüm tek bölge ve tüm bölgeli hesapları ile rahat tutarlılık ve %99.999 okuma tüm bölgeli veritabanı hesaplarda kullanılabilirlik.|
|Çevrimiçi yedeklemeleri otomatik|Azure Cosmos DB veritabanları düzenli olarak yedeklenen ve georedundant deposunda depolanır. <br><br>Daha fazla bilgi edinin [otomatik çevrimiçi yedekleme ve geri yükleme Azure Cosmos DB ile](online-backup-and-restore.md).|
|Silinmiş verileri geri yüklemek|Otomatik çevrimiçi yedeklemeler yanlışlıkla olayından sonra yaklaşık 30 güne kadar sildiğiniz verileri kurtarmak için kullanılabilir. <br><br>Daha fazla bilgi edinin [otomatik çevrimiçi yedekleme ve geri yükleme ile Azure Cosmos DB](online-backup-and-restore.md)|
|Koruma ve hassas verileri yalıtma|Listelenen bölgelerde tüm verileri [yenilikler nelerdir?](#whats-new) şimdi bekleme sırasında şifrelenir.<br><br>PII ve diğer gizli veriler belirli koleksiyonlar ve okuma-yazma yalıtılmış olabilir veya salt okunur erişimi belirli kullanıcılarla sınırlı olabilir.|
|İzleme saldırıları için|Kullanarak [denetim günlüğü ve etkinlik günlükleri](logging.md), hesabınızı normal ve anormal etkinliğinin izleyebilirsiniz. Hangi işlemlerin işlemi oluştuğunda, işlem ve çok daha gösterildiği gibi bu tablodan sonraki ekran durumunu kimin işlemini başlattı kaynaklarınızı gerçekleştirilen görüntüleyebilirsiniz.|
|Saldırılara karşı yanıt|Olası bir saldırı bildirmek için Azure desteğine temas sonra 5-adım olay yanıtlama süreci başlayacağı zamana devre dışı. Mümkün olan en kısa sürede normal hizmet güvenliği ve işlemleri bir sorun algıladı ve bir araştırma başlatıldıktan sonra geri yüklemek için 5-adım işlemi belirtilir.<br><br>Daha fazla bilgi edinin [bulutta Microsoft Azure Security Response](https://aka.ms/securityresponsepaper).|
|Coğrafi yalıtma|Veri Yönetimi ve uyumluluk sovereign bölgeler (örneğin, Almanya, Çin, BİZE kamu) için Azure Cosmos DB sağlar.|
|Korumalı özellikleri|Veriler Azure Cosmos veritabanı Azure'nın korumalı veri merkezlerinde SSD depolanır.<br><br>Daha fazla bilgi edinin [Microsoft küresel veri merkezleri](https://www.microsoft.com/en-us/cloud-platform/global-datacenters)|
|HTTPS/SSL/TLS şifrelemesi|Tüm istemci hizmeti Azure Cosmos DB etkileşimler SSL/TLS 1.2 özellikli ' dir. Ayrıca, tüm içi veri merkezi ve çapraz veri merkezine çoğaltma SSL/TLS 1.2 zorlanan vardır.|
|Bekleme sırasında şifreleme|Bekleyen Azure Cosmos Veritabanına depolanan tüm veriler şifrelenir. Daha fazla bilgi edinin [bekleyen Azure Cosmos DB şifreleme](.\database-encryption-at-rest.md)|
|Düzeltme eki sunucuları|Yönetilen bir veritabanı olarak Azure Cosmos DB sizin için otomatik olarak yapılır, yönetmek ve sunucular, düzeltme eki için ihtiyacını ortadan kaldırır.|
|Güçlü parolalar ile yönetici hesapları|Bu gereksinim anlatılması biz bile gerekir, ancak bazı bizim rakiplere, bir yönetici hesabı parolası olmayan Azure Cosmos DB'de sahip mümkün değildir düşünüyorsanız zordur.<br><br> SSL ve HMAC gizli tabanlı kimlik doğrulaması aracılığıyla güvenlik varsayılan olarak mı baked.|
|Güvenlik ve veri koruma sertifikaları|Azure Cosmos DB sahip [ISO 27001](https://www.microsoft.com/en-us/TrustCenter/Compliance/ISO-IEC-27001), [Avrupa modeli yan tümceleri (EUMC)](https://www.microsoft.com/en-us/TrustCenter/Compliance/EU-Model-Clauses), ve [HIPAA](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) sertifikalar. Ek sertifikalar sürüyor.|

Azure portalında erişim denetimi (IAM) kullanarak Active directory tümleştirme (RBAC) aşağıdaki ekran gösterilir: ![veritabanı güvenliği gösteren Azure portalı - erişim denetimi (IAM)](./media/database-security/nosql-database-security-identity-access-management-iam-rbac.png)

Aşağıdaki ekran görüntüsünde hesabınız izlemek için Denetim günlüğü ve etkinlik günlüklerini nasıl kullanabileceğinizi gösterir: ![etkinlik günlükleri için Azure Cosmos DB](./media/database-security/nosql-database-security-application-logging.png)

## <a name="next-steps"></a>Sonraki adımlar

Ana anahtarları ve kaynak belirteçleri hakkında daha fazla ayrıntı için [Azure Cosmos DB verilere erişimin güvenliğini sağlama](secure-access-to-data.md).

Denetim günlüğü hakkında daha fazla ayrıntı için bkz: [Azure Cosmos DB tanılama günlük](logging.md).

Microsoft sertifikalar hakkında daha fazla ayrıntı için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/).
