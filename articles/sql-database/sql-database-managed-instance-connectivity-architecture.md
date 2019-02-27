---
title: Azure SQL veritabanı yönetilen örneği bağlantı mimarisi | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı yönetilen örneği iletişimine genel bakış ve bağlantı mimarisi yönetilen örnek trafiği yönlendirmek için farklı bileşenleri işlev nasıl açıklar sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 02/26/2019
ms.openlocfilehash: 64e0444c85440a017872aa32017e7d1c47e44e89
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56889790"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Azure SQL veritabanı yönetilen örneği bağlantı mimarisi

Bu makalede, Azure SQL veritabanı yönetilen örneği iletişimine genel bakış ve bağlantı mimarisi yönetilen örnek trafiği yönlendirmek için farklı bileşenleri işlev nasıl açıklar sağlar.  

Azure SQL veritabanı yönetilen örneği, Azure sanal ağı ve yönetilen örnekleri için ayrılmış alt ağ içinde yer alır. Bu dağıtım aşağıdaki senaryolara olanak tanır:

- Özel IP adresini güvenli hale getirin.
- Yönetilen örnek için bir şirket içi ağdan doğrudan bağlanma.
- Bağlantılı bir sunucu veya başka bir yönetilen örnek bağlanan veri deposu şirket içi.
- Yönetilen örnek, Azure kaynaklarına bağlama.

## <a name="communication-overview"></a>İletişimine genel bakış

Yönetilen örnek kaynaklar yanı sıra yönetilen örneğe bağlanma varlıkları sahip düzgün çalışması için ulaşmak Aşağıdaki diyagramda gösterilmiştir.

![bağlantı mimari varlıkları](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Diyagramın alt düzenlenmiş iletişim, müşteri uygulama ve araçların veri kaynağı olarak yönetilen örneğe bağlanma temsil eder.  

Yönetilen örnek, bir platform olarak-a-sunan hizmetler (PaaS) olduğundan, Microsoft tabanlı telemetri veri akışları üzerinde otomatik aracıları (Yönetim, dağıtım ve bakım) kullanarak bu hizmeti yönetir. Yönetilen örnek Yönetimi yalnızca Microsoft'un sorumluluk, müşteriler RDP yönetilen örnek küme sanal makinelere erişmek mümkün değildir.

Yönetilen platformuyla etkileşim kurmak için örnekler bazı SQL Server Son kullanıcılar veya uygulamalar tarafından başlatılan işlemleri gerektirebilir. Yönetilen örnek veritabanı - Azure portalı, PowerShell, Azure CLI ve REST API kullanıma sunulan bir kaynak oluşturulmasını bir durumdur.

Yönetilen örnek, diğer Azure Hizmetleri, kendi düzgün çalışması için (örneğin, Azure depolama için yedeklemeleri, telemetri için Azure Service Bus, Azure AD kimlik doğrulaması, Azure anahtar kasası için TDE vb.) bağlıdır ve bağlantıları onlara uygun şekilde başlatır.

Yukarıda belirtilen tüm iletişimler şifrelenir ve sertifika kullanılarak imzalanmış. İletişim kuran güvenildiğinden emin olmak için yönetilen örneği sürekli olarak bu sertifikaları bir sertifika yetkilisi ile iletişim kurarak doğrular. Sertifikaları iptal edilir ya da yönetilen örnek bunları doğrulanamadı, verileri korumak için bağlantıları kapatır.

## <a name="high-level-connectivity-architecture"></a>Üst düzey bağlantı mimarisi

Yüksek düzeyde, bir yönetilen örnek adanmış bir müşteri sanal ağ alt ağ içinde çalışan ve bir sanal küme oluşturacak yalıtılmış sanal makine kümesi üzerinde barındırılan, hizmet bileşenleri kümesidir.

Birden çok yönetilen örneği, tek bir sanal kümede barındırılabilir. Küme otomatik olarak genişletilmiş veya müşteri alt sağlanan örnek sayısını değiştiğinde gerekirse sözleşmeleri yapılır.

Müşteri uygulamaları için yönetilen örnekleri bağlanabileceği, sorgu ve güncelleştirme, yalnızca sanal ağ içinde çalıştırırsanız veritabanları veya sanal ağ veya VPN eşlenmiş / Express Route ağ uç noktası ile özel IP adresini kullanarak bağlı.  

![bağlantı mimarisi diyagramı](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Yönetilen örnek ve Microsoft Hizmetleri arasında bağlantılar gelecek şekilde uç noktaları ile genel IP adresleri üzerinden sanal ağ dışında Microsoft Yönetim ve dağıtım hizmetlerini çalıştırın. Yönetilen örnek giden bağlantı oluşturduğunda, bu genel IP ağ adresi çevirisi (NAT) nedeniyle gelen son alınmasına görülüyor.

Yönetim trafiği, müşteri sanal ağı üzerinden akar. Bu sanal ağ altyapısının öğeleri etkiler ve potansiyel olarak zarar yönetim trafiğini neden hatalı durumuna girmesini ve kullanılamaz hale gelmesi örneği anlamına gelir.

> [!IMPORTANT]
> Müşteri deneyimini iyileştirmek ve hizmet kullanılabilirliği için Microsoft Ağ hedefi ilke yönetilen örnek çalışmasını etkileyebilecek Azure sanal ağ altyapısı öğelerde uygulanır. Bu, ağ yanlış yapılandırılmasının önlemek ve normal yönetilen örnek işlemleri sağlamak için asıl amacı olan son kullanıcılar için saydam bir şekilde ağ gereksinimlerini iletişim kurmak için bir platform mekanizmadır. Yönetilen örnek silme işlemi sırasında Ağ hedefi İlkesi de kaldırılır.

## <a name="virtual-cluster-connectivity-architecture"></a>Sanal küme bağlantı mimarisi

Yönetilen örnek bağlantı mimaride daha yakından bakın alalım. Sanal küme kavramsal düzeni Aşağıdaki diyagramda gösterilmiştir.

![bağlantı mimarisi diyagramı sanal küme](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

İstemciler bir biçimde konak adını kullanarak yönetilen örneğe bağlanma `<mi_name>.<dns_zone>.database.windows.net`. Genel DNS bölgesinde kaydedilir ve genel olarak çözümlenebilen rağmen bu ana bilgisayar adı için özel IP adresine çözümler. `zone-id` Küme oluşturulduğunda otomatik olarak oluşturulur. Yeni oluşturulan küme, ikincil bir yönetilen örnek barındırma, kendi bölge kimliği birincil küme ile paylaşır. Daha fazla bilgi için [otomatik yük devretme grupları](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets)

Özel IP adresi, yönetilen örnek (GW) ağ geçidi trafiği yönlendiren yönetilen örnek iç yük dengeleyici (ILB) aittir. Birden çok yönetilen örnekleri aynı küme içinde potansiyel olarak çalıştırabileceğiniz gibi GW yönetilen örnek ana bilgisayar adı için doğru SQL altyapısı hizmet trafiği yönlendirmek için kullanır.

Yönetim ve dağıtım hizmetlerini bağlama kullanarak bir yönetilen örnek bir [yönetim uç noktası](#management-endpoint) yük dengeleyici dış eşlenir. Trafik, yalnızca önceden tanımlanmış birtakım özel olarak yönetilen örnek yönetim bileşenleri tarafından kullanılan bağlantı noktaları üzerinde aldıysanız düğümlere yönlendirilir. Düğümlerde yerleşik güvenlik duvarı, yalnızca belirli IP aralıkları Microsoft gelen trafiğe izin verecek şekilde yapılandırılır. Yönetim bileşenler ve yönetim düzlemi arasındaki tüm iletişim karşılıklı kimlik doğrulaması sertifikadır.

## <a name="management-endpoint"></a>Yönetim uç noktası

Yönetilen örnek sanal küme, Microsoft'un yönetilen örnek yönetmek için kullandığı bir yönetim uç noktası içerir. Yönetim uç noktası, ağ düzeyinde ve karşılıklı sertifika doğrulamayı uygulama düzeyinde yerleşik güvenlik duvarı ile korunur. Yapabilecekleriniz [yönetim uç noktası IP adresini bulmak](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Bağlantılar içinde yönetilen örnek (Yedekleme, Denetim günlüğü) başlatılır, trafiği yönetim uç noktası genel IP adresinden kaynaklanan görünür. Güvenlik duvarı kurallarını yalnızca yönetilen örnek IP adreslerine izin verecek şekilde ayarlayarak yönetilen örneğinden kamu hizmetleri için erişimi sınırlayabilirsiniz. Gerçekleştirebileceğiniz yöntemi hakkında daha fazla bilgi [yönetilen örnek yerleşik güvenlik duvarı doğrulayın](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> Bu, Azure platformu birlikte bulunan hizmetler arasında trafiğinin yönelik bir iyileştirme olarak, yönetilen örnek ile aynı bölgede bulunan Azure Hizmetleri için güvenlik duvarı kurallarını ayarlamak geçerli değildir.

## <a name="network-requirements"></a>Ağ gereksinimleri

Aşağıdaki gereksinimlere uygun bir sanal ağ içinde ayrılmış bir alt ağ (yönetilen örnek alt ağ) yönetilen bir örneğinde dağıtın:

- **Ayrılmış alt**: Yönetilen örnek alt ağ ile ilişkilendirilmiş diğer bulut hizmeti içermemesi gerekir ve bir ağ geçidi alt ağı olmamalıdır. Yönetilen örnek haricinde kaynaklar içeren bir alt ağdaki yönetilen örnek oluşturma mümkün olmayacaktır ve daha sonra diğer kaynaklar alt ağdaki ekleyebilirsiniz değil.
- **Ağ güvenlik grubu (NSG)**: Sanal ağ ile ilişkilendirilmiş bir NSG bu tanımlı zorunlu içermelidir [gelen güvenlik kuralları](#mandatory-inbound-security-rules) ve [giden güvenlik kuralları](#mandatory-outbound-security-rules) önüne başka kurallar. 1433 numaralı bağlantı noktasında trafiği filtreleyerek, tam olarak yönetilen örnek veri uç noktası erişimi denetlemek için bir NSG kullanabilirsiniz.
- **Kullanıcı tanımlı yol tablosu (UDR)**: Bu sanal ağ ile ilişkilendirilmiş bir kullanıcı tanımlı yol tablosu olmalıdır [girişleri](#user-defined-routes) bir kullanıcı tanımlı yol tablosundaki.
- **Hizmet uç noktası yok**: Yönetilen örnek alt ağ ile ilişkili bir hizmet uç noktası olmaması gerekir. Hizmet uç noktaları seçeneğini sanal ağ oluştururken, devre dışı emin olun.
- **Yeterli IP adresi**: Yönetilen örnek alt en az 16 IP adresi olmalıdır (en az 32 IP adresleri önerilir). Daha fazla bilgi için [yönetilen örnekleri için alt ağ boyutunu belirlemek](sql-database-managed-instance-determine-size-vnet-subnet.md). Yönetilen örnekleri dağıtabilirsiniz [mevcut ağ](sql-database-managed-instance-configure-vnet-subnet.md) karşılamak için yapılandırdıktan sonra [yönetilen örnek ağ gereksinimlerini](#network-requirements), veya oluşturma bir [yeni ağ ve alt ağ](sql-database-managed-instance-create-vnet-subnet.md).

> [!IMPORTANT]
> Hedef alt ağ, tüm bu gereksinimler ile uyumlu değilse, yeni bir yönetilen örnek dağıtmak mümkün olmayacaktır. Yönetilen bir örneği oluşturulduğunda bir *Ağ hedefi İlkesi* alt ağ yapılandırmasını uyumlu değişiklikleri önlemek için uygulanır. Son örnek bir alt ağdan kaldırıldıktan sonra *Ağ hedefi İlkesi* de kaldırılır

### <a name="mandatory-inbound-security-rules"></a>Zorunlu bir gelen güvenlik kuralları

| Ad       |Bağlantı noktası                        |Protokol|Kaynak           |Hedef|Eylem|
|------------|----------------------------|--------|-----------------|-----------|------|
|yönetim  |9000, 9003, 1438, 1440, 1452|TCP     |Herhangi biri              |Herhangi biri        |İzin Ver |
|mi_subnet   |Herhangi biri                         |Herhangi biri     |MI ALT AĞ        |Herhangi biri        |İzin Ver |
|health_probe|Herhangi biri                         |Herhangi biri     |AzureLoadBalancer|Herhangi biri        |İzin Ver |

### <a name="mandatory-outbound-security-rules"></a>Zorunlu giden güvenlik kuralları

| Ad       |Bağlantı noktası          |Protokol|Kaynak           |Hedef|Eylem|
|------------|--------------|--------|-----------------|-----------|------|
|yönetim  |80, 443, 12000|TCP     |Herhangi biri              |Internet   |İzin Ver |
|mi_subnet   |Herhangi biri           |Herhangi biri     |Herhangi biri              |MI ALT *  |İzin Ver |

\* Form 10.x.x.x/y alt ağ için IP adresi aralığı mı alt ifade eder. Bu bilgiler (alt ağ özelliklerini) aracılığıyla Azure portalında bulunabilir.

> [!IMPORTANT]
> Zorunlu bir gelen güvenlik kuralları, gelen trafiğe izin verse de _herhangi_ bağlantı noktalarını 9000, 9003, kaynak 1438, 1440, 1452 Bu bağlantı noktaları, yerleşik güvenlik duvarı tarafından korunur. Bu [makale](sql-database-managed-instance-find-management-endpoint-ip-address.md) yönetim uç noktası IP adresi Bul ve güvenlik duvarı kurallarını doğrulayın nasıl gösterir.
> [!NOTE]
> Yönetilen örnek işlemsel çoğaltma kullanarak ve herhangi bir örnek veritabanı bir yayımcı ya da bir dağıtıcı olarak kullanılır, bağlantı noktası 445 (TCP Giden) ayrıca Azure dosya paylaşımına erişmek için alt ağ güvenliği kurallarının açılması gerekir.

### <a name="user-defined-routes"></a>Kullanıcı tanımlı yollar

|Ad|Adres ön eki|NET atlama|
|----|--------------|-------|
|subnet_to_vnetlocal|[mi_subnet]|Sanal ağ|
|mi-0-5-Next-Hop-internet|0.0.0.0/5|Internet|
|mı-11-8-nexthop-Internet|11.0.0.0/8|Internet|
|mı-12-6-nexthop-Internet|12.0.0.0/6|Internet|
|mı-128-3-nexthop-Internet|128.0.0.0/3|Internet|
|mı-16-4-nexthop-Internet|16.0.0.0/4|Internet|
|mı-160-5-nexthop-Internet|160.0.0.0/5|Internet|
|mı-168-6-nexthop-Internet|168.0.0.0/6|Internet|
|mı-172-12-nexthop-Internet|172.0.0.0/12|Internet|
|mi-172-128-9-nexthop-internet|172.128.0.0/9|Internet|
|mi-172-32-11-nexthop-internet|172.32.0.0/11|Internet|
|mi-172-64-10-nexthop-internet|172.64.0.0/10|Internet|
|mı-173-8-nexthop-Internet|173.0.0.0/8|Internet|
|mı-174-7-nexthop-Internet|174.0.0.0/7|Internet|
|mı-176-4-nexthop-Internet|176.0.0.0/4|Internet|
|mi-192-128-11-nexthop-internet|192.128.0.0/11|Internet|
|mi-192-160-13-nexthop-internet|192.160.0.0/13|Internet|
|mi-192-169-16-nexthop-internet|192.169.0.0/16|Internet|
|mi-192-170-15-nexthop-internet|192.170.0.0/15|Internet|
|mi-192-172-14-nexthop-internet|192.172.0.0/14|Internet|
|mi-192-176-12-nexthop-internet|192.176.0.0/12|Internet|
|mi-192-192-10-nexthop-internet|192.192.0.0/10|Internet|
|mı-192-9-nexthop-Internet|192.0.0.0/9|Internet|
|mı-193-8-nexthop-Internet|193.0.0.0/8|Internet|
|mı-194-7-nexthop-Internet|194.0.0.0/7|Internet|
|mı-196-6-nexthop-Internet|196.0.0.0/6|Internet|
|mı-200-5-nexthop-Internet|200.0.0.0/5|Internet|
|mı-208-4-nexthop-Internet|208.0.0.0/4|Internet|
|mı-224-3-nexthop-Internet|224.0.0.0/3|Internet|
|mı-32-3-nexthop-Internet|32.0.0.0/3|Internet|
|mı-64-2-nexthop-Internet|64.0.0.0/2|Internet|
|mı-8-7-nexthop-Internet|8.0.0.0/7|Internet|
||||

Ayrıca, bir hedef sanal ağ geçidi veya sanal ağ Gereci (NVA) üzerinden şirket içi özel IP aralıklarına sahip trafiği yönlendirmek için rota tablosu girdileri ekleyebilirsiniz.

- **İsteğe bağlı bir özel DNS**: Sanal ağda özel DNS belirtilirse, Azure'nın yinelemeli çözümleyici IP adresi (örneğin, 168.63.129.16) listeye eklenmelidir. Daha fazla bilgi için [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md). Özel DNS sunucusunun ana bilgisayar adlarını aşağıdaki etki alanları ve bunların alt çözümleyebilmesi gerekir: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com*, ve *microsoftonline-p.com*.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md)
- Bilgi nasıl [yeni sanal ağ yapılandırma](sql-database-managed-instance-create-vnet-subnet.md) veya [var olan bir sanal ağ yapılandırma](sql-database-managed-instance-configure-vnet-subnet.md) yönetilen örnekleri dağıtabileceğiniz.
- [Alt ağ boyutunu hesaplamak](sql-database-managed-instance-determine-size-vnet-subnet.md) yönetilen örnekleri dağıtacağınız.
- Yönetilen örnek oluşturma için hızlı başlangıçlar, bakın:
  - Gelen [Azure portalı](sql-database-managed-instance-get-started.md)
  - kullanarak [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/)
  - kullanarak [Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)
  - kullanarak [Azure Resource Manager şablonu (Sıçrama kutusu dahil SSMS ile)](https://portal.azure.com/)
