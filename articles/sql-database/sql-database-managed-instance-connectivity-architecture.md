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
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 04/16/2019
ms.openlocfilehash: fa19ea0c7ebeea0170822db0dae298f84e958983
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006140"
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

Azure depolama için yedeklemeleri, telemetri için Azure Event Hubs, Azure Active Directory kimlik doğrulaması için Azure Key Vault saydam veri şifrelemesi (TDE) gibi Azure Hizmetleri ve birkaç sağlayan Azure platform hizmetlerini yönetilen örnekleri bağlıdır. Güvenlik ve desteklenebilirliği özellikleri. Yönetilen örnekleri, bu hizmetlere yönelik bağlantılar sağlar.

Tüm iletişimler şifrelenir ve sertifika kullanılarak imzalanmış. Taraflar, yönetilen iletişim güvenilirliğini denetlemek için bu sertifikalar sertifika iptal listeleri aracılığıyla sürekli örnekleri doğrulayın. Sertifikaları iptal edilir, yönetilen örnek verileri korumak için bağlantıları kapatır.

## <a name="high-level-connectivity-architecture"></a>Üst düzey bağlantı mimarisi

Yüksek düzeyde, bir yönetilen örnek hizmet bileşenleri kümesidir. Bu bileşenler, müşterinin sanal ağ alt ağı içinde çalışan yalıtılmış sanal makineleri özel bir dizi üzerinde barındırılır. Bu makineler, sanal bir küme oluşturacak.

Bir sanal kümede birden fazla yönetilen örneği barındırabilir. Gerekirse, küme otomatik olarak genişletir veya müşteri alt sağlanan örnek sayısını değiştiğinde daraltır.

Müşteri uygulamaları yönetilen örneklerine bağlanabilirsiniz sorgulamak ve bir sanal ağ, eşlenen sanal ağ içindeki veritabanlarını güncelleştirmek veya VPN veya Azure ExpressRoute ile bağlı ağ. Bu ağ, bir uç nokta ve özel IP adresi kullanmanız gerekir.  

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
> Yönetilen örneğin bölgesi içinde olduğundan Azure hizmetlerine giden Traffice iyileştirilmiştir ve ilgili olmayan NATed manged örnek yönetim uç noktası ortak IP adresi için neden. IP tabanlı güvenlik duvarı kuralları, en yaygın olarak depolama için kullanmak istiyorsanız bu nedenle hizmet yönetilen örneğinden farklı bir bölgede olması gerekir.

## <a name="network-requirements"></a>Ağ gereksinimleri

Yönetilen örnek sanal ağ içinde ayrılmış bir alt ağ içinde dağıtın. Alt ağ, şu özelliklere sahip olmanız gerekir:

- **Ayrılmış alt ağı:** Yönetilen örneğin alt ağ ile ilişkili herhangi bir bulut hizmeti içeremez ve bir ağ geçidi alt ağı olamaz. Herhangi bir kaynağa ancak yönetilen örnek alt içeremez ve daha sonra alt ağdaki kaynakları ekleyemezsiniz.
- **Ağ güvenlik grubu (NSG):** Sanal ağ ile ilişkilendirilmiş bir NSG tanımlamalıdır [gelen güvenlik kuralları](#mandatory-inbound-security-rules) ve [giden güvenlik kuralları](#mandatory-outbound-security-rules) herhangi diğer kurallardan önce. 1433 numaralı bağlantı noktasında trafiği filtreleyerek yönetilen Örneğin veri uç noktası erişimi denetlemek için bir NSG kullanabilirsiniz ve bağlantı noktaları 11000-yönetilen örnek için yapılandırıldığında 11999 bağlantıları yönlendirin.
- **Kullanıcı tanımlı yol (UDR) tablo:** Sanal ağ ile ilişkili bir UDR tablo belirli içermelidir [girişleri](#user-defined-routes).
- **Hizmet uç noktası yok:** Uç nokta yönetilen örneğin alt ağ ile ilişkili olmalıdır. Sanal ağ oluşturduğunuzda, hizmet uç noktaları seçeneğini devre dışı bırakıldığından emin olun.
- **Yeterli IP adresi:** Yönetilen örnek alt ağı, en az 16 IP adresleri olması gerekir. Önerilen en az 32 IP adresleri aralığıdır. Daha fazla bilgi için [yönetilen örnekleri için alt ağ boyutunu belirlemek](sql-database-managed-instance-determine-size-vnet-subnet.md). Yönetilen örnekleri dağıtabilirsiniz [mevcut ağ](sql-database-managed-instance-configure-vnet-subnet.md) karşılamak için yapılandırdıktan sonra [yönetilen örnekleri için ağ gereksinimleri](#network-requirements). Aksi takdirde, oluşturun bir [yeni ağ ve alt ağ](sql-database-managed-instance-create-vnet-subnet.md).

> [!IMPORTANT]
> Hedef alt şu özelliklere sahip değilse yeni bir yönetilen örnek dağıtamazsınız. Yönetilen örnek oluşturma, ağ kurulumu için uyumsuz değişiklikleri önlemek için alt ağda bir ağ hedefi ilke uygulanır. Son örneğini alt ağdan kaldırıldıktan sonra Ağ hedefi İlkesi de kaldırılır.

### <a name="mandatory-inbound-security-rules"></a>Zorunlu bir gelen güvenlik kuralları

| Name       |Bağlantı noktası                        |Protokol|Kaynak           |Hedef|Eylem|
|------------|----------------------------|--------|-----------------|-----------|------|
|yönetim  |9000, 9003, 1438, 1440, 1452|TCP     |Herhangi biri              |MI ALT AĞ  |İzin Ver |
|mi_subnet   |Herhangi biri                         |Herhangi biri     |MI ALT AĞ        |MI ALT AĞ  |İzin Ver |
|health_probe|Herhangi biri                         |Herhangi biri     |AzureLoadBalancer|MI ALT AĞ  |İzin Ver |

### <a name="mandatory-outbound-security-rules"></a>Zorunlu giden güvenlik kuralları

| Name       |Bağlantı noktası          |Protokol|Kaynak           |Hedef|Eylem|
|------------|--------------|--------|-----------------|-----------|------|
|yönetim  |80, 443, 12000|TCP     |MI ALT AĞ        |AzureCloud |İzin Ver |
|mi_subnet   |Herhangi biri           |Herhangi biri     |MI ALT AĞ        |MI ALT AĞ  |İzin Ver |

> [!IMPORTANT]
> 9003, yalnızca bir gelen kuralı 9000, bağlantı noktaları olduğundan emin olmak için bağlantı noktası 80, 443, 12000 1438, 1440, 1452 ve bir giden kuralı. Yönetilen örnek aracılığıyla Azure kaynak gelen ve giden kurallar her bağlantı için ayrı ayrı yapılandırılıp yapılandırılmadığını dağıtımlar başarısız olur Manager sağlama. Bu bağlantı noktalarını ayrı kurallarında kullanıyorsanız, dağıtım hata kodu ile başarısız olur `VnetSubnetConflictWithIntendedPolicy`

\* Form 10.x.x.x/y alt ağ için IP adresi aralığı mı alt ifade eder. Bu bilgi, alt ağ özelliklerini Azure portalında bulabilirsiniz.

> [!IMPORTANT]
> Gerekli gelen güvenlik kuralları, gelen trafiğe izin verse de _herhangi_ 9000 noktalarındaki kaynak, bu bağlantı noktaları 9003, 1438 1440 ve 1452, yerleşik bir güvenlik duvarı tarafından korunur. Daha fazla bilgi için [yönetim uç nokta adresini belirlemek](sql-database-managed-instance-find-management-endpoint-ip-address.md).
> [!NOTE]
> Yönetilen örnek işlem çoğaltma kullanma ve herhangi bir örnek veritabanı bir yayımcı ya da bir dağıtıcı olarak kullanıyorsanız, bağlantı noktası 445 (TCP Giden) alt ağ güvenliği kurallarının açın. Bu bağlantı noktası, Azure dosya paylaşımına erişim sağlar.

### <a name="user-defined-routes"></a>Kullanıcı tanımlı yollar

|Name|Adres ön eki|Sonraki atlama|
|----|--------------|-------|
|subnet_to_vnetlocal|MI ALT AĞ|Sanal ağ|
|mi-13-64-11-nexthop-internet|13.64.0.0/11|Internet|
|mi-13-96-13-nexthop-internet|13.96.0.0/13|Internet|
|mi-13-104-14-nexthop-internet|13.104.0.0/14|Internet|
|mı-20-8-nexthop-Internet|20.0.0.0/8|Internet|
|mi-23-96-13-nexthop-internet|23.96.0.0/13|Internet|
|mi-40-64-10-nexthop-internet|40.64.0.0/10|Internet|
|mi-42-159-16-nexthop-internet|42.159.0.0/16|Internet|
|mı-51-8-nexthop-Internet|51.0.0.0/8|Internet|
|mı-52-8-nexthop-Internet|52.0.0.0/8|Internet|
|mi-64-4-18-nexthop-internet|64.4.0.0/18|Internet|
|mi-65-52-14-nexthop-internet|65.52.0.0/14|Internet|
|mi-66-119-144-20-nexthop-internet|66.119.144.0/20|Internet|
|mi-70-37-17-nexthop-internet|70.37.0.0/17|Internet|
|mi-70-37-128-18-nexthop-internet|70.37.128.0/18|Internet|
|mi-91-190-216-21-nexthop-internet|91.190.216.0/21|Internet|
|mi-94-245-64-18-nexthop-internet|94.245.64.0/18|Internet|
|mi-103-9-8-22-nexthop-internet|103.9.8.0/22|Internet|
|mi-103-25-156-22-nexthop-internet|103.25.156.0/22|Internet|
|mi-103-36-96-22-nexthop-internet|103.36.96.0/22|Internet|
|mi-103-255-140-22-nexthop-internet|103.255.140.0/22|Internet|
|mi-104-40-13-nexthop-internet|104.40.0.0/13|Internet|
|mi-104-146-15-nexthop-internet|104.146.0.0/15|Internet|
|mi-104-208-13-nexthop-internet|104.208.0.0/13|Internet|
|mi-111-221-16-20-nexthop-internet|111.221.16.0/20|Internet|
|mi-111-221-64-18-nexthop-internet|111.221.64.0/18|Internet|
|mi-129-75-16-nexthop-internet|129.75.0.0/16|Internet|
|mi-131-253-16-nexthop-internet|131.253.0.0/16|Internet|
|mi-132-245-16-nexthop-internet|132.245.0.0/16|Internet|
|mi-134-170-16-nexthop-internet|134.170.0.0/16|Internet|
|mi-134-177-16-nexthop-internet|134.177.0.0/16|Internet|
|mi-137-116-15-nexthop-internet|137.116.0.0/15|Internet|
|mi-137-135-16-nexthop-internet|137.135.0.0/16|Internet|
|mi-138-91-16-nexthop-internet|138.91.0.0/16|Internet|
|mi-138-196-16-nexthop-internet|138.196.0.0/16|Internet|
|mi-139-217-16-nexthop-internet|139.217.0.0/16|Internet|
|mi-139-219-16-nexthop-internet|139.219.0.0/16|Internet|
|mi-141-251-16-nexthop-internet|141.251.0.0/16|Internet|
|mi-146-147-16-nexthop-internet|146.147.0.0/16|Internet|
|mi-147-243-16-nexthop-internet|147.243.0.0/16|Internet|
|mi-150-171-16-nexthop-internet|150.171.0.0/16|Internet|
|mi-150-242-48-22-nexthop-internet|150.242.48.0/22|Internet|
|mi-157-54-15-nexthop-internet|157.54.0.0/15|Internet|
|mi-157-56-14-nexthop-internet|157.56.0.0/14|Internet|
|mi-157-60-16-nexthop-internet|157.60.0.0/16|Internet|
|mi-167-220-16-nexthop-internet|167.220.0.0/16|Internet|
|mi-168-61-16-nexthop-internet|168.61.0.0/16|Internet|
|mi-168-62-15-nexthop-internet|168.62.0.0/15|Internet|
|mi-191-232-13-nexthop-internet|191.232.0.0/13|Internet|
|mi-192-32-16-nexthop-internet|192.32.0.0/16|Internet|
|mi-192-48-225-24-nexthop-internet|192.48.225.0/24|Internet|
|mi-192-84-159-24-nexthop-internet|192.84.159.0/24|Internet|
|mi-192-84-160-23-nexthop-internet|192.84.160.0/23|Internet|
|mi-192-100-102-24-nexthop-internet|192.100.102.0/24|Internet|
|mi-192-100-103-24-nexthop-internet|192.100.103.0/24|Internet|
|mi-192-197-157-24-nexthop-internet|192.197.157.0/24|Internet|
|mi-193-149-64-19-nexthop-internet|193.149.64.0/19|Internet|
|mi-193-221-113-24-nexthop-internet|193.221.113.0/24|Internet|
|mi-194-69-96-19-nexthop-internet|194.69.96.0/19|Internet|
|mi-194-110-197-24-nexthop-internet|194.110.197.0/24|Internet|
|mi-198-105-232-22-nexthop-internet|198.105.232.0/22|Internet|
|mi-198-200-130-24-nexthop-internet|198.200.130.0/24|Internet|
|mi-198-206-164-24-nexthop-internet|198.206.164.0/24|Internet|
|mi-199-60-28-24-nexthop-internet|199.60.28.0/24|Internet|
|mi-199-74-210-24-nexthop-internet|199.74.210.0/24|Internet|
|mi-199-103-90-23-nexthop-internet|199.103.90.0/23|Internet|
|mi-199-103-122-24-nexthop-internet|199.103.122.0/24|Internet|
|mi-199-242-32-20-nexthop-internet|199.242.32.0/20|Internet|
|mi-199-242-48-21-nexthop-internet|199.242.48.0/21|Internet|
|mi-202-89-224-20-nexthop-internet|202.89.224.0/20|Internet|
|mi-204-13-120-21-nexthop-internet|204.13.120.0/21|Internet|
|mi-204-14-180-22-nexthop-internet|204.14.180.0/22|Internet|
|mi-204-79-135-24-nexthop-internet|204.79.135.0/24|Internet|
|mi-204-79-179-24-nexthop-internet|204.79.179.0/24|Internet|
|mi-204-79-181-24-nexthop-internet|204.79.181.0/24|Internet|
|mi-204-79-188-24-nexthop-internet|204.79.188.0/24|Internet|
|mi-204-79-195-24-nexthop-internet|204.79.195.0/24|Internet|
|mi-204-79-196-23-nexthop-internet|204.79.196.0/23|Internet|
|mi-204-79-252-24-nexthop-internet|204.79.252.0/24|Internet|
|mi-204-152-18-23-nexthop-internet|204.152.18.0/23|Internet|
|mi-204-152-140-23-nexthop-internet|204.152.140.0/23|Internet|
|mi-204-231-192-24-nexthop-internet|204.231.192.0/24|Internet|
|mi-204-231-194-23-nexthop-internet|204.231.194.0/23|Internet|
|mi-204-231-197-24-nexthop-internet|204.231.197.0/24|Internet|
|mi-204-231-198-23-nexthop-internet|204.231.198.0/23|Internet|
|mi-204-231-200-21-nexthop-internet|204.231.200.0/21|Internet|
|mi-204-231-208-20-nexthop-internet|204.231.208.0/20|Internet|
|mi-204-231-236-24-nexthop-internet|204.231.236.0/24|Internet|
|mi-205-174-224-20-nexthop-internet|205.174.224.0/20|Internet|
|mi-206-138-168-21-nexthop-internet|206.138.168.0/21|Internet|
|mi-206-191-224-19-nexthop-internet|206.191.224.0/19|Internet|
|mi-207-46-16-nexthop-internet|207.46.0.0/16|Internet|
|mi-207-68-128-18-nexthop-internet|207.68.128.0/18|Internet|
|mi-208-68-136-21-nexthop-internet|208.68.136.0/21|Internet|
|mi-208-76-44-22-nexthop-internet|208.76.44.0/22|Internet|
|mi-208-84-21-nexthop-internet|208.84.0.0/21|Internet|
|mi-209-240-192-19-nexthop-internet|209.240.192.0/19|Internet|
|mi-213-199-128-18-nexthop-internet|213.199.128.0/18|Internet|
|mi-216-32-180-22-nexthop-internet|216.32.180.0/22|Internet|
|mi-216-220-208-20-nexthop-internet|216.220.208.0/20|Internet|
||||

Ayrıca, sanal ağ geçidi veya sanal ağ Gereci (NVA) üzerinden bir hedef olarak şirket içi özel IP aralıkları içeren trafiği yönlendirmek için rota tablosu girdileri ekleyebilirsiniz.

Sanal ağ özel DNS içeriyorsa, özel DNS sunucusunun ana bilgisayar adlarını çözümleyebilmesi gerekir \*. core.windows.net bölge. Azure AD kimlik doğrulaması ek FQDN çözümleme gerektirebilir gibi ek özellikler kullanıyor. Daha fazla bilgi için [özel bir DNS ayarlamalısınız](sql-database-managed-instance-custom-dns.md).

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [SQL veritabanı veri güvenliği Gelişmiş](sql-database-managed-instance.md).
- Bilgi nasıl [yeni bir Azure virtual Network ayarlama](sql-database-managed-instance-create-vnet-subnet.md) veya [mevcut Azure sanal ağına](sql-database-managed-instance-configure-vnet-subnet.md) yönetilen örnekleri dağıtabileceğiniz.
- [Alt ağ boyutunu hesaplamak](sql-database-managed-instance-determine-size-vnet-subnet.md) yönetilen örnekleri'ni dağıtmak istediğiniz.
- Yönetilen örnek oluşturma işlemleri gerçekleştirmeyi öğreneceksiniz:
  - Gelen [Azure portalında](sql-database-managed-instance-get-started.md).
  - Kullanarak [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md).
  - Kullanarak [bir Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/).
  - Kullanarak [(Sıçrama kutusu, dahil edilen SSMS ile kullanarak) bir Azure Resource Manager şablonu](https://portal.azure.com/). 
