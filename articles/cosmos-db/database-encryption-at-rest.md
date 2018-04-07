---
title: 'Bekleyen veritabanı şifreleme: Azure Cosmos DB | Microsoft Docs'
description: Varsayılan şifreleme tüm verileri Azure Cosmos DB nasıl sağladığını öğrenin.
services: cosmos-db
author: voellm
manager: kfile
documentationcenter: ''
ms.assetid: 99725c52-d7ca-4bfa-888b-19b1569754d3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: voellm
ms.openlocfilehash: c32c85c560e9dfe587e79e77aa4b1262e347bed4
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="azure-cosmos-db-database-encryption-at-rest"></a>Bekleyen Azure Cosmos DB veritabanı şifreleme

Bekleyen şifreleme katı hal sürücüleri (SSD) gibi yaygın olarak kalıcı depolama cihazlarda verilerin şifrelenmesini başvurduğu bir tümceciği ve sabit disk sürücülerinin (HDD'ler) ' dir. Cosmos DB birincil veritabanlarını Ssd'de depolar. Medya ekler ve yedeklemeleri genellikle HDD tarafından yedeklenir Azure Blob Depolama alanında depolanır. Cosmos DB için bekleyen şifreleme sürümle birlikte, tüm veritabanları, medya ekler ve yedeklemeleri şifrelenir. Verilerinizi şimdi (ağ üzerinden) aktarım sırasında şifrelenir ve rest (kalıcı depolama) uçtan uca şifreleme sağlar.

Bir PaaS Cosmos DB kullanımı çok kolay hizmettir gibi. Cosmos DB içinde depolanan tüm kullanıcı verileri hem beklerken hem de Aktarım şifreli olduğundan, herhangi bir eylemde bulunmanız gerekmez. Bu koymak için bir başka yolu olan bekleyen şifreleme olan varsayılan olarak "açık". Kapatma veya açma için denetim yoktur. Karşılamaya devam ederken bu özellik sağladığımız bizim [kullanılabilirliğini ve performansını SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db).

## <a name="implementation-of-encryption-at-rest-for-azure-cosmos-db"></a>Azure Cosmos DB için bekleyen şifreleme uygulaması

Bekleyen şifreleme, güvenli anahtar depolama sistemleri, şifrelenmiş ağlar ve şifreleme API'leri dahil güvenlik teknolojileri çeşitli kullanılarak uygulanır. Anahtarları Yönet sistemleriyle iletişim kurmak, verileri işlemek ve şifresini çözmek sistemleri sahiptir. Diyagram nasıl şifrelenmiş verilerin depolanması ve anahtar yönetimi ayrılmış gösterir. 

![Tasarım diyagramı](./media/database-encryption-at-rest/design-diagram.png)

Bir kullanıcı isteği temel akışı aşağıdaki gibidir:
- Kullanıcı veritabanı hesabı hazır yapıldığında ve depolama anahtarları yönetim hizmeti kaynak sağlayıcısı için bir istek yoluyla alınır.
- Cosmos DB bağlantı HTTPS ve güvenli aktarımı ile bir kullanıcı oluşturur. (SDK'ları ayrıntıları soyut.)
- Kullanıcı daha önce oluşturulmuş güvenli bağlantı üzerinden depolanması için bir JSON belgesi gönderir.
- Kullanıcı dizin oluşturmayı devre dışı kapamış olduğu sürece JSON belgesini dizine alınır.
- JSON belgesini ve dizin veri depolama güvenli hale getirmek için yazılmıştır.
- Düzenli aralıklarla, veriler güvenli depolama alanından okuyun ve Azure şifreli Blob deposuna yedeklendi.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="q-how-much-more-does-azure-storage-cost-if-storage-service-encryption-is-enabled"></a>S: depolama hizmeti şifrelemesi etkinse, daha fazlasını nasıl Azure depolama maliyeti?
Y: yoktur ek ücret ödemeden.

### <a name="q-who-manages-the-encryption-keys"></a>S: şifreleme anahtarları yöneten?
Y: anahtarları Microsoft tarafından yönetilir.

### <a name="q-how-often-are-encryption-keys-rotated"></a>S: ne sıklıkta şifreleme anahtarları döndürülüp?
Y: Microsoft Cosmos DB izleyen şifreleme anahtar döndürme için iç Kılavuzlar kümesi vardır. Özel yönergeleri yayımlanmaz. Microsoft yayımlama [güvenlik geliştirme yaşam döngüsü (SDL)](https://www.microsoft.com/sdl/default.aspx), bir iç Kılavuzu alt görülen ve geliştiriciler için kullanışlı en iyi yöntemler vardır.

### <a name="q-can-i-use-my-own-encryption-keys"></a>S: kendi şifreleme anahtarları kullanmak?
A: biz sabit hizmeti kullanımı kolay tutmaya çalışılan ve cosmos DB bir PaaS hizmetidir. Bu soru genellikle PCI-DSS gibi bir uyumluluk gereksinimini karşılamak için bir proxy soru olarak sorulan fark ettim. Bu özellik oluşturmanın bir parçası olarak, biz Cosmos DB kullanan müşteriler kendilerini anahtarlarını yönetmek zorunda kalmadan kendi gereksinimleri karşıladığından emin olmak için Uyumluluk denetçiler çalışmıştır.

### <a name="q-what-regions-have-encryption-turned-on"></a>S: ne bölgeleri açık şifreleme var mı?
Y: tüm Azure Cosmos DB bölgeler için tüm kullanıcı verilerini açık şifreleme vardır.

### <a name="q-does-encryption-affect-the-performance-latency-and-throughput-slas"></a>S: şifreleme performans gecikme süresi ve verimlilik SLA'ları etkiler?
Y: etkisi veya yoktur değişiklikleri göre bekleyen şifreleme tüm mevcut ve yeni hesaplar için etkin SLA performans. Hakkında daha fazla bilgiyi [Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db) son garanti görmek için sayfayı.

### <a name="q-does-the-local-emulator-support-encryption-at-rest"></a>S: yerel öykünücüsü bekleyen şifreleme destekliyor mu?
Y: öykünücüsü bir tek başına geliştirme ve test aracıdır ve yönetilen Cosmos DB hizmetini kullanan anahtar yönetimi hizmetlerini kullanmaz. Bizim önerimiz, burada hassas öykünücüsü test veri depolayan sürücülerde BitLocker etkinleştirmektir. [Öykünücüsü destekleyen varsayılan veri dizinini değiştirme](local-emulator.md) iyi bilinen bir konum kullanarak yanı sıra.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB güvenlik ve en son geliştirmeleri genel bakış için bkz: [Azure Cosmos DB veritabanı güvenliği](database-security.md).
Microsoft sertifikalar hakkında daha fazla bilgi için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/en-us/support/trust-center/).
