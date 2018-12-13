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
ms.openlocfilehash: bf8b3ab62697857a636b7550376cfa0b6d4ebecd
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269541"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Azure SQL veritabanı yönetilen örneği bağlantı mimarisi

Bu makale, Azure SQL veritabanı yönetilen örneği iletişimine genel bakış sağlar ve bağlantı mimarisi yönetilen örneğe trafiği farklı bileşenleri işlev nasıl açıklar.  

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

Yönetim ve dağıtım hizmetlerini yönetilen örneği için kullanılacak bağlantı [yönetim uç noktası](sql-database-managed-instance-management-endpoint.md) dış yük dengeleyici ile eşler. Trafik, yalnızca önceden tanımlanmış birtakım özel olarak yönetilen örnek yönetim bileşenleri tarafından kullanılan bağlantı noktaları üzerinde aldıysanız düğümlere yönlendirilir. Düğümlerde yerleşik güvenlik duvarı, yalnızca belirli IP aralıkları Microsoft gelen trafiğe izin verecek şekilde yapılandırılır. Yönetim bileşenler ve yönetim düzlemi arasındaki tüm iletişim karşılıklı kimlik doğrulaması sertifikadır.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md)
- Sanal ağ yapılandırması hakkında daha fazla bilgi için bkz. [yönetilen örnek sanal ağ yapılandırması](sql-database-managed-instance-vnet-configuration.md).
- Yönetilen örnek oluşturma Hızlı Başlangıç için bkz:
  - Gelen [Azure portalı](sql-database-managed-instance-get-started.md)
  - kullanarak [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/)
  - kullanarak [Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)
  - kullanarak [Azure Resource Manager şablonu (Sıçrama kutusu dahil SSMS ile)](https://portal.azure.com/)
