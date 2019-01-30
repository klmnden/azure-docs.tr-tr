---
title: Azure SQL veritabanı yönetilen örneği bağlantı mimarisi | Microsoft Docs
description: Bu makale, Azure SQL veritabanı yönetilen örneği iletişimine genel bakış sağlar ve bağlantı mimarisi yönetilen örneğe trafiği farklı bileşenleri işlev nasıl açıklar.
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
ms.date: 12/10/2018
ms.openlocfilehash: b709bbacce23a89b8c60b77a524018b50ca1ca5e
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245676"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Azure SQL veritabanı yönetilen örneği bağlantı mimarisi

Bu makale, Azure SQL veritabanı yönetilen örneği iletişimine genel bakış sağlar ve bağlantı mimarisi yönetilen örneğe trafiği farklı bileşenleri işlev nasıl açıklar.  

Azure SQL veritabanı yönetilen örneği, Azure sanal ağı ve alt ağ içinde yönetilen örnekler için adanmış yerleştirilir. Bu dağıtım aşağıdaki senaryolara olanak tanır: 
- Özel IP adresini güvenli hale getirin.
- Yönetilen örneği'ne bir şirket içi ağdan doğrudan bağlanma.
- Bağlantılı bir sunucu veya başka bir yönetilen örneğe bağlanma veri deposu şirket içi.
- Bir yönetilen örnek, Azure kaynaklarına bağlama.

## <a name="communication-overview"></a>İletişimine genel bakış

Aşağıdaki diyagramda, yönetilen örneği düzgün çalışması için ulaşmak için olan kaynaklar yanı sıra yönetilen örneğe bağlanma varlıkları gösterir.

![bağlantı mimari varlıkları](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Diyagramın alt düzenlenmiş iletişim, müşteri uygulama ve araçların veri kaynağı olarak yönetilen örneğe bağlanma temsil eder.  

Yönetilen örnek platform-olarak-a-services (PaaS) olarak telemetri veri akışları üzerinde tabanlı otomatik aracıları (Yönetim, dağıtım ve bakım) kullanarak bu hizmet teklifi, Microsoft yönetir. Yönetilen örnek Yönetimi yalnızca Microsoft sorumluluk olduğundan, müşterilerin yönetilen örnek küme sanal makinelere RDP üzerinden erişmek alamıyoruz.

Yönetilen örneği'nın platformuyla etkileşim kurmak son kullanıcılar veya uygulamalar tarafından başlatılan bazı SQL Server işlemleri gerektirebilir. Yönetilen örnek veritabanı - portal, PowerShell ve Azure CLI sunulan bir kaynak oluşturulmasını bir durumdur.

Yönetilen örnek, diğer Azure Hizmetleri, kendi düzgün çalışması için (örneğin, Azure depolama için yedeklemeleri, telemetri için Azure Service Bus, Azure AD kimlik doğrulaması, Azure anahtar kasası için TDE vb.) bağlıdır ve bağlantıları onlara uygun şekilde başlatır.

Yukarıda belirtilen tüm iletişimler şifrelenir ve sertifika kullanılarak imzalanmış. İletişim kuran güvenildiğinden emin olmak için yönetilen örneği sürekli olarak bu sertifikaları bir sertifika yetkilisi ile iletişim kurarak doğrular. Sertifikaları iptal edilir ya da yönetilen örneği bunları doğrulanamadı, verileri korumak için bağlantıları kapatır.

## <a name="high-level-connectivity-architecture"></a>Üst düzey bağlantı mimarisi

Yüksek düzeyde, yönetilen örneği, hizmet bileşenleri, adanmış bir müşteri sanal ağ alt ağ içinde çalışan ve bir sanal küme oluşturacak yalıtılmış sanal makine kümesi üzerinde barındırılan bir kümesidir.

Birden çok yönetilen örneği, tek bir sanal kümede barındırılabilir. Küme otomatik olarak genişletilmiş veya müşteri alt sağlanan örnek sayısını değiştiğinde gerekirse sözleşmeleri yapılır.

Müşteri uygulamaları, yalnızca sanal ağ veya eşlenen sanal ağ veya VPN içinde çalıştırırsanız, yönetilen örneği, sorgu ve güncelleştirme veritabanlarına bağlanabileceği / Express Route ağ uç noktası ile özel IP adresini kullanarak bağlı.  

![bağlantı mimarisi diyagramı](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Yönetilen örneği ve Microsoft Hizmetleri arasında bağlantı uç noktaları ile genel IP adresleri üzerinden gider için sanal ağ dışında Microsoft Yönetim ve dağıtım hizmetlerini çalıştırın. Yönetilen örnek giden bağlantı oluşturduğunda, bu genel IP ağ adresi çevirisi (NAT) nedeniyle gelen alan tarafta görülüyor.

Yönetim trafiği, müşteri sanal ağı üzerinden akar. Bu sanal ağ altyapısının öğeleri etkiler ve potansiyel olarak zarar yönetim trafiğini neden hatalı durumuna girmesini ve kullanılamaz hale gelmesi örneği anlamına gelir.

> [!IMPORTANT]
> Müşteri deneyimini iyileştirmek ve hizmet kullanılabilirliği için Microsoft Ağ hedefi ilke yönetilen örneği çalışmasını etkileyebilecek Azure sanal ağ altyapısı öğelerde uygulanır. Bu, ağ yanlış yapılandırılmasının önlemek ve normal yönetilen örneği işlemleri sağlamak için asıl amacı olan son kullanıcılar için saydam bir şekilde ağ gereksinimlerini iletişim kurmak için bir platform mekanizmadır. Yönetilen örnek silme işlemi sırasında Ağ hedefi İlkesi de kaldırılır.

## <a name="virtual-cluster-connectivity-architecture"></a>Sanal küme bağlantı mimarisi

Yönetilen örnek bağlantı mimaride daha yakından bakın alalım. Sanal küme kavramsal düzeni Aşağıdaki diyagramda gösterilmiştir.

![bağlantı mimarisi diyagramı sanal küme](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

İstemcilerin bir biçimde konak adını kullanarak yönetilen örnek'e bağlanıp `<mi_name>.<dns_zone>.database.windows.net`. Genel DNS bölgesinde kaydedilir ve genel olarak çözümlenebilen rağmen bu ana bilgisayar adı için özel IP adresine çözümler. `zone-id` Küme oluşturulduğunda otomatik olarak oluşturulur. Yeni oluşturulan küme, ikincil bir yönetilen örnek barındırma, kendi bölge kimliği birincil küme ile paylaşır. Daha fazla bilgi için [otomatik yük devretme grupları](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets)

Özel IP adresi için yönetilen örneği iç yük dengeleyici (yönetilen örnek ağ geçidi'için (GW) trafiği yönlendiren ILB) aittir. Birden çok yönetilen örnekler aynı küme içinde potansiyel olarak çalıştırabileceğiniz gibi GW yönetilen örneği ana bilgisayar adı için doğru SQL altyapısı hizmet trafiği yönlendirmek için kullanır.

Yönetim ve dağıtım hizmetlerini yönetilen örneği için kullanılacak bağlantı [yönetim uç noktası](#management-endpoint) dış yük dengeleyici ile eşler. Trafik, yalnızca önceden tanımlanmış birtakım özel olarak yönetilen örnek yönetim bileşenleri tarafından kullanılan bağlantı noktaları üzerinde aldıysanız düğümlere yönlendirilir. Düğümlerde yerleşik güvenlik duvarı, yalnızca belirli IP aralıkları Microsoft gelen trafiğe izin verecek şekilde yapılandırılır. Yönetim bileşenler ve yönetim düzlemi arasındaki tüm iletişim karşılıklı kimlik doğrulaması sertifikadır.

## <a name="management-endpoint"></a>Yönetim uç noktası

Azure SQL veritabanı yönetilen örneği sanal küme, Microsoft'un yönetilen örneğe yönetmek için kullandığı bir yönetim uç noktası içerir. Yönetim uç noktası, ağ düzeyinde ve karşılıklı sertifika doğrulamayı uygulama düzeyinde yerleşik güvenlik duvarı ile korunur. Yapabilecekleriniz [yönetim uç noktası IP adresini bulmak](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Bağlantılar yönetilen örneğe içinde (Yedekleme, Denetim günlüğü) başlatılır, trafiği yönetim uç noktası genel IP adresinden kaynaklanan görünür. Güvenlik duvarı kuralları yalnızca yönetilen örnek IP adreslerine izin verecek şekilde ayarlayarak yönetilen örneğinden kamu hizmetleri için erişimi sınırlayabilirsiniz. Gerçekleştirebileceğiniz yöntemi hakkında daha fazla bilgi [yönetilen örneği yerleşik güvenlik duvarı doğrulayın](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> Bu, Azure platformu birlikte bulunan hizmetler arasında trafiğinin yönelik bir iyileştirme olarak, yönetilen örneği ile aynı bölgede bulunan Azure Hizmetleri için güvenlik duvarı kurallarını ayarlamak geçerli değildir.

## <a name="network-requirements"></a>Ağ gereksinimleri

Yönetilen örnek, aşağıdaki gereksinimlere uygun bir sanal ağ içinde ayrılmış bir alt ağda (yönetilen örnek alt) dağıtabilirsiniz:
- **Ayrılmış alt**: Yönetilen örnek alt kendisiyle ilişkilendirilmiş diğer bulut hizmeti içermemesi gerekir ve bir ağ geçidi alt ağı olmamalıdır. Yönetilen örnek haricinde kaynaklar içeren bir alt ağdaki bir yönetilen örnek oluşturma mümkün olmayacaktır ve daha sonra diğer kaynaklar alt ağdaki ekleyebilirsiniz değil.
- **Uyumlu ağ güvenlik grubu (NSG)**: Bir yönetilen örnek alt ağ ile ilişkilendirilmiş bir NSG kuralları önüne başka kurallar aşağıdaki tablolarda (zorunlu gelen güvenlik kuralları ve zorunlu giden güvenlik kuralları) gösterilen içermelidir. 1433 numaralı bağlantı noktasında trafiği filtreleyerek, tam olarak yönetilen örnek veri uç noktası erişimi denetlemek için bir NSG kullanabilirsiniz. 
- **Uyumlu kullanıcı tanımlı yol tablosu (UDR)**: Yönetilen örnek alt bir kullanıcı rota tablosuyla olmalıdır **0.0.0.0/0 sonraki atlama Internet** atanmış zorunlu UDR olarak. Ayrıca, bir hedef sanal ağ geçidi veya sanal ağ Gereci (NVA) üzerinden şirket içi özel IP aralıklarına sahip bu trafiği yönlendirir UDR ekleyebilirsiniz. 
- **İsteğe bağlı bir özel DNS**: Sanal ağda özel DNS belirtilirse, Azure'nın yinelemeli çözümleyici IP adresi (örneğin, 168.63.129.16) listeye eklenmelidir. Daha fazla bilgi için [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md). Özel DNS sunucusunun ana bilgisayar adlarını aşağıdaki etki alanları ve bunların alt çözümleyebilmesi gerekir: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com*, ve *microsoftonline-p.com*. 
- **Hizmet uç noktası yok**: Yönetilen örnek alt ilişkili bir hizmet uç noktası olmaması gerekir. Hizmet uç noktaları seçeneğini sanal ağ oluştururken, devre dışı emin olun.
- **Yeterli IP adresi**: Yönetilen örnek alt en az 16 IP adresi olmalıdır (en az 32 IP adresleri önerilir). Daha fazla bilgi için [yönetilen örnekler için alt ağ boyutunu belirlemek](sql-database-managed-instance-determine-size-vnet-subnet.md). Yönetilen örnekleri'nde dağıtabilirsiniz [mevcut ağ](sql-database-managed-instance-configure-vnet-subnet.md) karşılamak için yapılandırdıktan sonra [yönetilen ağ gereksinimleri örneği](#network-requirements), veya bir [yeni ağ ve alt ağ](sql-database-managed-instance-create-vnet-subnet.md).

> [!IMPORTANT]
> Hedef alt ağ, tüm bu gereksinimler ile uyumlu değilse, yeni bir yönetilen örnek dağıtmak mümkün olmayacaktır. Yönetilen bir örneği oluşturulduğunda bir *Ağ hedefi İlkesi* alt ağ yapılandırmasını uyumlu değişiklikleri önlemek için uygulanır. Son örnek bir alt ağdan kaldırıldıktan sonra *Ağ hedefi İlkesi* de kaldırılır

### <a name="mandatory-inbound-security-rules"></a>Zorunlu bir gelen güvenlik kuralları 

| Name       |Bağlantı noktası                        |Protokol|Kaynak           |Hedef|Eylem|
|------------|----------------------------|--------|-----------------|-----------|------|
|yönetim  |9000, 9003, 1438, 1440, 1452|TCP     |Herhangi biri              |Herhangi biri        |İzin Ver |
|mi_subnet   |Herhangi biri                         |Herhangi biri     |MI ALT AĞ        |Herhangi biri        |İzin Ver |
|health_probe|Herhangi biri                         |Herhangi biri     |AzureLoadBalancer|Herhangi biri        |İzin Ver |

### <a name="mandatory-outbound-security-rules"></a>Zorunlu giden güvenlik kuralları 

| Name       |Bağlantı noktası          |Protokol|Kaynak           |Hedef|Eylem|
|------------|--------------|--------|-----------------|-----------|------|
|yönetim  |80, 443, 12000|TCP     |Herhangi biri              |Internet   |İzin Ver |
|mi_subnet   |Herhangi biri           |Herhangi biri     |Herhangi biri              |MI ALT AĞ  |İzin Ver |

  > [!Note]
  > Form 10.x.x.x/y alt ağ için IP adresi aralığı mı alt ifade eder. Bu bilgiler (alt ağ özelliklerini) aracılığıyla Azure portalında bulunabilir.
  
  > [!Note]
  > Zorunlu bir gelen güvenlik kuralları, gelen trafiğe izin verse de _herhangi_ bağlantı noktalarını 9000, 9003, kaynak 1438, 1440, 1452 Bu bağlantı noktaları, yerleşik güvenlik duvarı tarafından korunur. Bu [makale](sql-database-managed-instance-find-management-endpoint-ip-address.md) yönetim uç noktası IP adresi Bul ve güvenlik duvarı kurallarını doğrulayın nasıl gösterir. 
  
  > [!Note]
  > Yönetilen örneği'nde işlemsel çoğaltma kullanarak ve herhangi bir veritabanı yönetilen örneğinde yayımcı ya da dağıtımcı gibi kullanılır, bağlantı noktası 445 (TCP Giden) ayrıca Azure dosya paylaşımına erişmek için alt ağ güvenliği kurallarının açılması gerekir.
  
## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md)
- Bilgi nasıl [yeni sanal ağ yapılandırma](sql-database-managed-instance-create-vnet-subnet.md) veya [var olan bir sanal ağ yapılandırma](sql-database-managed-instance-configure-vnet-subnet.md) yönetilen örnekler dağıtabileceğiniz.
- [Alt ağ boyutunu hesaplamak](sql-database-managed-instance-determine-size-vnet-subnet.md) yönetilen örnekler dağıtacağınız. 
- Yönetilen örnek oluşturma Hızlı Başlangıç için bkz:
  - Gelen [Azure portalı](sql-database-managed-instance-get-started.md)
  - kullanarak [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/)
  - kullanarak [Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)
  - kullanarak [Azure Resource Manager şablonu (Sıçrama kutusu dahil SSMS ile)](https://portal.azure.com/)
