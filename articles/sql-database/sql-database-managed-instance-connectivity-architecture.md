---
title: Azure SQL veritabanı yönetilen örneği için bağlantı mimarisi | Microsoft Docs
description: Bileşenleri doğrudan trafiğe yönetilen örneğe nasıl Azure SQL veritabanı yönetilen örneği iletişim ve bağlantı mimarisi hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: fasttrack-edit
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 02/26/2019
ms.openlocfilehash: f08b22f24dfde41646f56dc1ecd9777f267620ee
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58651321"
---
# <a name="connectivity-architecture-for-a-managed-instance-in-azure-sql-database"></a>Azure SQL veritabanı yönetilen örneği için bağlantı mimarisi 

Bu makalede, Azure SQL veritabanı yönetilen örneği'nde iletişim açıklanmaktadır. Ayrıca, bağlantı mimarisi ve bileşenleri doğrudan yönetilen örnek için trafiği nasıl açıklar.  

SQL veritabanı yönetilen örneği, Azure sanal ağı ve yönetilen örnekleri için ayrılmış bir alt ağ içinde yer alır. Bu dağıtım sağlar:

- Güvenli özel IP adresi.
- Yönetilen örnek için bir şirket içi ağ bağlanma özelliği.
- Bağlantılı bir sunucu veya başka bir yönetilen örnek bağlanmak için veri deposu şirket içi.
- Yönetilen örnek Azure kaynaklarına bağlama yeteneği.

## <a name="communication-overview"></a>İletişimine genel bakış

Aşağıdaki diyagramda, yönetilen bir örneğine bağlanmak varlıkları gösterir. Ayrıca, yönetilen örneği ile iletişim kurmak için gereken kaynaklar gösterilmektedir. Diyagramın alt kısmındaki iletişim sürecinde müşteri uygulama ve veri kaynağı olarak yönetilen örneğe bağlanma araçların temsil eder.  

![Bağlantı mimari varlıkları](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Yönetilen örnek bir platform, olarak bir hizmet (PaaS) teklifidir. Microsoft Otomatik aracıları (Yönetim, dağıtım ve bakım) telemetri veri akışları üzerinde bu hizmeti yönetmek için kullanır. Microsoft yönetiminden sorumlu olduğu için müşteriler yönetilen örnek küme sanal makineleri Uzak Masaüstü Protokolü (RDP) üzerinden erişilemiyor.

Son kullanıcılar veya uygulamalar tarafından başlatılan işlemleri gerektirebilir bazı SQL Server yönetilen platformuyla etkileşim kurmak için örnekler. Yönetilen örnek veritabanı oluşturulmasını bir durumdur. Bu kaynak, Azure portalı, PowerShell, Azure CLI ve REST API kullanıma sunulur.

Azure depolama için yedeklemeleri, telemetri için Azure Service Bus, Azure Active Directory gibi Azure Hizmetleri için kimlik doğrulaması ve Azure anahtar kasası yönetilen örnekleri saydam veri şifrelemesi (TDE) bağlıdır. Yönetilen örnekleri, bu hizmetlere yönelik bağlantı kurun.

Tüm iletişimler, imzalama ve şifreleme için sertifikalar kullanır. Taraflar, yönetilen iletişim güvenilirliğini denetlemek için örnekleri sürekli olarak bu sertifikaları bir sertifika yetkilisi ile iletişim kurarak doğrulayın. Sertifikaları iptal edilir ya da doğrulanamıyor, yönetilen örnek verileri korumak için bağlantıları kapatır.

## <a name="high-level-connectivity-architecture"></a>Üst düzey bağlantı mimarisi

Yüksek düzeyde, bir yönetilen örnek hizmet bileşenleri kümesidir. Bu bileşenler, müşterinin sanal ağ alt ağı içinde çalışan yalıtılmış sanal makineleri özel bir dizi üzerinde barındırılır. Bu makineler, sanal bir küme oluşturacak.

Bir sanal kümede birden fazla yönetilen örneği barındırabilir. Gerekirse, küme otomatik olarak genişletir veya müşteri alt sağlanan örnek sayısını değiştiğinde daraltır.

Müşteri uygulamaları için yönetilen örnekleri yeniden bağlanabilir ve sorgulayabilirsiniz ve güncelleştirme veritabanları yalnızca sanal ağ içinde çalıştırırsanız eşlenen sanal ağ veya VPN veya Azure ExpressRoute ile bağlı ağ. Bu ağ, bir uç nokta ve özel IP adresi kullanmanız gerekir.  

![bağlantı mimarisi diyagramı](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Microsoft Yönetim ve dağıtım hizmeti, sanal ağ dışında çalıştırın. Yönetilen örnek ve Microsoft Hizmetleri genel IP adreslerine sahip uç noktaları bağlanır. Ne zaman bir yönetilen örnek bağlantı görünümlü, bu genel bir IP adresinden gelen ağ adresi çevirisi (NAT) yapar alan tarafta bir giden bağlantı oluşturur.

Yönetim trafiği, müşterinin sanal ağ üzerinden akar. Bu, başarısız ve kullanılamaz hale örneği sağlayarak yönetim trafiği sanal ağın altyapı öğelerini zarar emin anlamına gelir.

> [!IMPORTANT]
> Müşteri deneyimini iyileştirmek ve hizmet kullanılabilirliği için Microsoft Azure sanal ağ altyapısı öğelerde Ağ hedefi ilkesi uygulanır. İlke yönetilen örnek nasıl çalıştığını etkileyebilir. Bu platform mekanizması kullanıcılara ağ gereksinimlerine saydam bir şekilde iletişim kurar. İlkenin ana ağ yanlış yapılandırılmasının önlemek ve normal yönetilen örnek işlemlere hedeftir. Yönetilen örnek sildiğinizde, Ağ hedefi İlkesi de kaldırılır.

## <a name="virtual-cluster-connectivity-architecture"></a>Sanal küme bağlantı mimarisi

Derin Dalış bağlantı mimarisi yönetilen örnekleri için içine alalım. Sanal küme kavramsal düzeni Aşağıdaki diyagramda gösterilmiştir.

![Sanal küme bağlantı mimarisi](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

İstemciler, biçiminde olan bir ana bilgisayar adını kullanarak yönetilen bir örneğine bağlanır `<mi_name>.<dns_zone>.database.windows.net`. Bir ortak etki alanı adı sistemi (DNS) bölgesinde kaydedilir ve genel olarak çözümlenebilen rağmen bu ana bilgisayar adı için özel bir IP adresi çözümler. `zone-id` Kümeyi oluşturduğunuzda otomatik olarak oluşturulur. Yeni oluşturulan bir küme ikincil bir yönetilen örnek barındırıyorsa, kendi bölge kimliği birincil küme ile paylaşır. Daha fazla bilgi için [birden fazla veritabanının saydam ve Eşgüdümlü yük devretmeyi etkinleştirmek için otomatik yük devretme grupları kullanma](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets).

Özel IP adresi yönetilen örneğin iç load balancer'a ait. Yük Dengeleyici yönetilen örneğin ağ geçidi trafiği yönlendirir. Birden çok yönetilen örnek içinde aynı kümede çalıştığından ağ geçidi yönetilen örneğin ana bilgisayar adı için doğru SQL Altyapısı hizmeti trafiği yönlendirmek için kullanır.

Yönetim ve dağıtım hizmetlerini kullanarak bir yönetilen örneğe bağlanma bir [yönetim uç noktası](#management-endpoint) yük dengeleyici dış eşlenir. Yalnızca önceden tanımlanmış bir yönetilen örneğin Yönetimi bileşenlerini kullanan bağlantı noktalarının alınırsa düğümlere trafik yönlendirilir. Düğümlerde yerleşik bir güvenlik duvarı yalnızca Microsoft IP aralığını gelen trafiğe izin verecek şekilde ayarlanır. Sertifika Yönetimi bileşenlerini ve yönetim düzlemi arasındaki tüm iletişim karşılıklı kimlik doğrulaması.

## <a name="management-endpoint"></a>Yönetim uç noktası

Microsoft, yönetim uç noktasını kullanarak yönetilen örnek yönetir. Bu uç nokta örneğinin sanal kümesi içinde var. Yönetim uç noktası, ağ düzeyinde yerleşik bir güvenlik duvarı tarafından korunur. Uygulama düzeyi, onu karşılıklı sertifika doğrulama ile korunuyor. Uç noktanın IP adresini bulmak için bkz: [yönetim uç noktasının IP adresini belirlemek](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Zaman bağlantıları içinde yönetilen örnek (yedeklemeler ve Denetim günlükleri'te olduğu gibi ile) Başlat, trafiği görünür yönetim uç noktanın genel IP adresi başlatmak için. Güvenlik duvarı kurallarını yalnızca yönetilen örneğin IP adreslerine izin verecek şekilde ayarlayarak bir yönetilen örneğinden kamu hizmetleri için erişimi sınırlayabilirsiniz. Daha fazla bilgi için [yönetilen örneğin yerleşik güvenlik duvarı doğrulayın](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> İçinde yönetilen örneğinizi bağlantıları için güvenlik duvarı, bu hizmetler arasında giden trafik için iyileştirilmiş bir güvenlik duvarı yönetilen örneğin bölgesi içinde Azure hizmetleri vardır.

## <a name="network-requirements"></a>Ağ gereksinimleri

Yönetilen örnek sanal ağ içinde ayrılmış bir alt ağ içinde dağıtın. Alt ağ, şu özelliklere sahip olmanız gerekir:

- **Ayrılmış alt ağı:** Yönetilen örneğin alt ağ ile ilişkili herhangi bir bulut hizmeti içeremez ve bir ağ geçidi alt ağı olamaz. Herhangi bir kaynağa ancak yönetilen örnek alt içeremez ve daha sonra alt ağdaki kaynakları ekleyemezsiniz.
- **Ağ güvenlik grubu (NSG):** Sanal ağ ile ilişkilendirilmiş bir NSG tanımlamalıdır [gelen güvenlik kuralları](#mandatory-inbound-security-rules) ve [giden güvenlik kuralları](#mandatory-outbound-security-rules) herhangi diğer kurallardan önce. 1433 numaralı bağlantı noktasında trafiği filtreleyerek yönetilen Örneğin veri uç noktası erişimi denetlemek için bir NSG kullanabilirsiniz.
- **Kullanıcı tanımlı yol (UDR) tablo:** Sanal ağ ile ilişkili bir UDR tablo belirli içermelidir [girişleri](#user-defined-routes).
- **Hizmet uç noktası yok:** Uç nokta yönetilen örneğin alt ağ ile ilişkili olmalıdır. Sanal ağ oluşturduğunuzda, hizmet uç noktaları seçeneğini devre dışı bırakıldığından emin olun.
- **Yeterli IP adresi:** Yönetilen örnek alt ağı, en az 16 IP adresleri olması gerekir. Önerilen en az 32 IP adresleri aralığıdır. Daha fazla bilgi için [yönetilen örnekleri için alt ağ boyutunu belirlemek](sql-database-managed-instance-determine-size-vnet-subnet.md). Yönetilen örnekleri dağıtabilirsiniz [mevcut ağ](sql-database-managed-instance-configure-vnet-subnet.md) karşılamak için yapılandırdıktan sonra [yönetilen örnekleri için ağ gereksinimleri](#network-requirements). Aksi takdirde, oluşturun bir [yeni ağ ve alt ağ](sql-database-managed-instance-create-vnet-subnet.md).

> [!IMPORTANT]
> Hedef alt şu özelliklere sahip değilse yeni bir yönetilen örnek dağıtamazsınız. Yönetilen örnek oluşturma, ağ kurulumu için uyumsuz değişiklikleri önlemek için alt ağda bir ağ hedefi ilke uygulanır. Son örneğini alt ağdan kaldırıldıktan sonra Ağ hedefi İlkesi de kaldırılır.

### <a name="mandatory-inbound-security-rules"></a>Zorunlu bir gelen güvenlik kuralları

| Ad       |Bağlantı Noktası                        |Protokol|Kaynak           |Varış Adresi|Eylem|
|------------|----------------------------|--------|-----------------|-----------|------|
|yönetim  |9000, 9003, 1438, 1440, 1452|TCP     |Herhangi              |Herhangi        |İzin ver |
|mi_subnet   |Herhangi                         |Herhangi     |MI ALT AĞ        |Herhangi        |İzin ver |
|health_probe|Herhangi                         |Herhangi     |AzureLoadBalancer|Herhangi        |İzin ver |

### <a name="mandatory-outbound-security-rules"></a>Zorunlu giden güvenlik kuralları

| Ad       |Bağlantı Noktası          |Protokol|Kaynak           |Varış Adresi|Eylem|
|------------|--------------|--------|-----------------|-----------|------|
|yönetim  |80, 443, 12000|TCP     |Herhangi              |AzureCloud  |İzin ver |
|mi_subnet   |Herhangi           |Herhangi     |Herhangi              |MI ALT *  |İzin ver |

> [!IMPORTANT]
> 9003, yalnızca bir gelen kuralı 9000, bağlantı noktaları olduğundan emin olmak için bağlantı noktası 80, 443, 12000 1438, 1440, 1452 ve bir giden kuralı. Giriş ve çıkış kuralları her bağlantı için ayrı olarak yapılandırılmışsa, ARM dağıtımları yönetilen örneğini sağlama başarısız olur. Bu bağlantı noktalarını ayrı kurallarında kullanıyorsanız, dağıtım hata kodu ile başarısız olur `VnetSubnetConflictWithIntendedPolicy`

\* Form 10.x.x.x/y alt ağ için IP adresi aralığı mı alt ifade eder. Bu bilgi, alt ağ özelliklerini Azure portalında bulabilirsiniz.

> [!IMPORTANT]
> Gerekli gelen güvenlik kuralları, gelen trafiğe izin verse de _herhangi_ 9000 noktalarındaki kaynak, bu bağlantı noktaları 9003, 1438 1440 ve 1452, yerleşik bir güvenlik duvarı tarafından korunur. Daha fazla bilgi için [yönetim uç nokta adresini belirlemek](sql-database-managed-instance-find-management-endpoint-ip-address.md).

> [!NOTE]
> Yönetilen örnek işlem çoğaltma kullanma ve herhangi bir örnek veritabanı bir yayımcı ya da bir dağıtıcı olarak kullanıyorsanız, bağlantı noktası 445 (TCP Giden) alt ağ güvenliği kurallarının açın. Bu bağlantı noktası, Azure dosya paylaşımına erişim sağlar.

### <a name="user-defined-routes"></a>Kullanıcı tanımlı yollar

|Ad|Adres ön eki|Sonraki atlama|
|----|--------------|-------|
|subnet_to_vnetlocal|[mi_subnet]|Sanal ağ|
|mi-0-5-Next-Hop-internet|0.0.0.0/5|İnternet|
|mı-11-8-nexthop-Internet|11.0.0.0/8|İnternet|
|mı-12-6-nexthop-Internet|12.0.0.0/6|İnternet|
|mı-128-3-nexthop-Internet|128.0.0.0/3|İnternet|
|mı-16-4-nexthop-Internet|16.0.0.0/4|İnternet|
|mı-160-5-nexthop-Internet|160.0.0.0/5|İnternet|
|mı-168-6-nexthop-Internet|168.0.0.0/6|İnternet|
|mı-172-12-nexthop-Internet|172.0.0.0/12|İnternet|
|mi-172-128-9-nexthop-internet|172.128.0.0/9|İnternet|
|mi-172-32-11-nexthop-internet|172.32.0.0/11|İnternet|
|mi-172-64-10-nexthop-internet|172.64.0.0/10|İnternet|
|mı-173-8-nexthop-Internet|173.0.0.0/8|İnternet|
|mı-174-7-nexthop-Internet|174.0.0.0/7|İnternet|
|mı-176-4-nexthop-Internet|176.0.0.0/4|İnternet|
|mi-192-128-11-nexthop-internet|192.128.0.0/11|İnternet|
|mi-192-160-13-nexthop-internet|192.160.0.0/13|İnternet|
|mi-192-169-16-nexthop-internet|192.169.0.0/16|İnternet|
|mi-192-170-15-nexthop-internet|192.170.0.0/15|İnternet|
|mi-192-172-14-nexthop-internet|192.172.0.0/14|İnternet|
|mi-192-176-12-nexthop-internet|192.176.0.0/12|İnternet|
|mi-192-192-10-nexthop-internet|192.192.0.0/10|İnternet|
|mı-192-9-nexthop-Internet|192.0.0.0/9|İnternet|
|mı-193-8-nexthop-Internet|193.0.0.0/8|İnternet|
|mı-194-7-nexthop-Internet|194.0.0.0/7|İnternet|
|mı-196-6-nexthop-Internet|196.0.0.0/6|İnternet|
|mı-200-5-nexthop-Internet|200.0.0.0/5|İnternet|
|mı-208-4-nexthop-Internet|208.0.0.0/4|İnternet|
|mı-224-3-nexthop-Internet|224.0.0.0/3|İnternet|
|mı-32-3-nexthop-Internet|32.0.0.0/3|İnternet|
|mı-64-2-nexthop-Internet|64.0.0.0/2|İnternet|
|mı-8-7-nexthop-Internet|8.0.0.0/7|İnternet|
||||

Ayrıca, sanal ağ geçidi veya sanal ağ Gereci (NVA) üzerinden bir hedef olarak şirket içi özel IP aralıkları içeren trafiği yönlendirmek için rota tablosu girdileri ekleyebilirsiniz.

Sanal ağ özel DNS içeriyorsa (168.63.129.16 gibi) Azure özyinelemeli çözümleyici IP adresi için bir giriş ekleyin. Daha fazla bilgi için [özel bir DNS ayarlamalısınız](sql-database-managed-instance-custom-dns.md). Özel DNS sunucusunun ana bilgisayar adlarını bu etki alanları ve bunların alt çözümleyebilmesi gerekir: *microsoft.com*, *windows.net*, *windows.com*,  *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com*, ve *microsoftonline-p.com*.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [SQL veritabanı veri güvenliği Gelişmiş](sql-database-managed-instance.md).
- Bilgi nasıl [yeni bir Azure virtual Network ayarlama](sql-database-managed-instance-create-vnet-subnet.md) veya [mevcut Azure sanal ağına](sql-database-managed-instance-configure-vnet-subnet.md) yönetilen örnekleri dağıtabileceğiniz.
- [Alt ağ boyutunu hesaplamak](sql-database-managed-instance-determine-size-vnet-subnet.md) yönetilen örnekleri'ni dağıtmak istediğiniz.
- Yönetilen örnek oluşturma işlemleri gerçekleştirmeyi öğreneceksiniz:
  - Gelen [Azure portalında](sql-database-managed-instance-get-started.md).
  - Kullanarak [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md).
  - Kullanarak [bir Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/).
  - Kullanarak [(Sıçrama kutusu, dahil edilen SSMS ile kullanarak) bir Azure Resource Manager şablonu](https://portal.azure.com/).
