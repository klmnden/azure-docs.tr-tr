---
title: Yapılandırma Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Yapılandırma ve Azure SQL veritabanı yönetilen örneği yönetme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlr
manager: craigg
ms.date: 04/16/2019
ms.openlocfilehash: 886f06e8640891ac09d1e4624335a7bfebcd3def
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60009387"
---
# <a name="how-to-use-a-managed-instance-in-azure-sql-database"></a>Azure SQL veritabanı'nda bir yönetilen örnek kullanma

Bu makalede çeşitli kılavuzları, betikler ve yönetmek ve yönetilen Örneğinize yapılandırmanıza yardımcı olacak bir açıklama bulabilirsiniz.

## <a name="migration"></a>Geçiş

- [Bir yönetilen örneğe geçiş](sql-database-managed-instance-migrate.md) – yönetilen örneğe geçiş araçları ve önerilen geçiş işlemi hakkında bilgi edinin.

- [TDE cert bir yönetilen örneğe geçiş](sql-database-managed-instance-migrate-tde-certificate.md) – SQL Server veritabanınıza saydam veri şifrelemesi (TDE) ile korunuyorsa, yönetilen örnek Azure'da geri yüklemek istediğiniz yedekleme şifresini çözmek için kullanabileceğiniz bir sertifika geçirmek gerekir.

## <a name="network-configuration"></a>Ağ yapılandırması

- [Yönetilen örnek alt ağı boyutunu belirlemek](sql-database-managed-instance-determine-size-vnet-subnet.md) – yönetilen örnek yerleştirilir, kaynakları içine ekledikten sonra yeniden boyutlandırılamaz alt görüntülemeye ayırır. Bu nedenle, hangi IP adresleri aralığı sayısını ve türlerini alt ağdaki dağıtmak istediğiniz örnek bağlı olarak alt ağ için gerekli olacak hesaplamak gerekir.
- [Yeni VNet ve alt ağ için bir yönetilen örnek oluşturma](sql-database-managed-instance-create-vnet-subnet.md) – göre Azure VNet ve alt ağı, yönetilen örneklerinizi dağıtmak istediğiniz yapılandırılmalıdır [ağ burada açıklanan gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Bu kılavuzda, yeni VNet ve düzgün bir şekilde yönetilen örnekleri için yapılandırılmış olan alt ağ oluşturmak için en kolay yolu bulabilirsiniz.
- [Mevcut bir VNet ve alt ağ için bir yönetilen örnek yapılandırma](sql-database-managed-instance-configure-vnet-subnet.md) –, mevcut bir VNet ve alt ağ içinde yönetilen örnek dağıtmak için yapılandırmak istiyorsanız, denetleyen betiğin burada bulabilirsiniz [ağ gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements) ve olun, alt ağ gereksinimlerine göre yapılandırır.
- [Özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md) – yönetilen örneğiniz db e-posta profilleri bağlı sunucu aracılığıyla özel etki alanları üzerindeki dış kaynaklara erişmek istiyorsanız, özel DNS yapılandırmanız gerekir.
- [Eşitleme ağı yapılandırması](sql-database-managed-instance-sync-network-configuration.md) -Bu, olsa da gerçekleşebilir, [uygulamanızı bir Azure sanal ağı ile tümleşik](../app-service/web-sites-integrate-with-vnet.md), şunları yapabilirsiniz&#39;t bir yönetilen örnek bağlantı kurun. Ağ yapılandırması, hizmet planınız için bir şey deneyebilirsiniz yenilemektir.
- [Yönetim uç noktası IP adresini bulmak](sql-database-managed-instance-find-management-endpoint-ip-address.md) – yönetilen örnek genel uç nokta yönetim amacıyla kullanır. Burada açıklanan betiğini kullanarak yönetim uç noktasının IP adresini belirleyebilirsiniz.
- [Yerleşik güvenlik duvarı koruması doğrulayın](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md) – yönetilen örneği, yalnızca gerekli bağlantı noktalarında trafiğe izin veren yerleşik güvenlik duvarı ile korunur. Denetleyin ve bu kılavuzda açıklanan betiğini kullanarak yerleşik bir güvenlik duvarı kurallarını doğrulayın.
- [Uygulamaları bağlama](sql-database-managed-instance-connect-app.md) – yönetilen örnek, kendi özel Azure sanal ağ özel IP adresiyle yerleştirilir. Uygulamaları yönetilen Örneğinize bağlanmak için farklı yaklaşımlar hakkında bilgi edinin.

## <a name="feature-configuration"></a>Özelliği yapılandırması

- [İşlem çoğaltma](replication-with-sql-database-managed-instance.md) verilerinizi şirket içi SQL Server'dan bir yönetilen örnek veya yönetilen örnekleri arasında çoğaltmanıza olanak sağlar ve bunun tersi de geçerlidir. Daha fazla bilgi kullanın ve bu kılavuzdaki işlem çoğaltma yapılandırma bulun.
- [Tehdit algılamayı yapılandırma](sql-database-managed-instance-threat-detection.md) – [tehdit algılama](sql-database-threat-detection-overview.md) SQL ekleme veya şüpheli konumlardan erişim gibi çeşitli olası saldırıları algılayan yerleşik bir Azure SQL veritabanı özelliğidir. Bu kılavuzda etkinleştirmek ve yapılandırmak nasıl edinebilirsiniz [tehdit algılama](sql-database-threat-detection-overview.md) yönetilen örnek için.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [tek veritabanları için nasıl yapılır kılavuzları](sql-database-howto-single-database.md)