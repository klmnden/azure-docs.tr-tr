---
title: Azure SQL veritabanı yönetilen örneği'ı yapılandırma | Microsoft Docs
description: Yapılandırma ve Azure SQL veritabanı yönetilen örneği yönetme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 12/14/2018
ms.openlocfilehash: 10a33ac09b5cfa588bef88e6c1587d67036b1aef
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53440495"
---
# <a name="how-to-use-managed-instance"></a>Yönetilen örnek kullanma

Bu bölümde çeşitli kılavuzları, betikler ve - Azure SQL veritabanı yönetilen örneği yönetip size yardımcı olacak bir açıklama bulabilirsiniz.

## <a name="migration"></a>Geçiş

- [Yönetilen örneğe geçirme](sql-database-managed-instance-migrate.md) – yönetilen örneğe geçiş araçları ve önerilen geçiş işlemi hakkında bilgi edinin.

- [TDE cert yönetilen örneğe geçirme](sql-database-managed-instance-migrate-tde-certificate.md) – SQL Server veritabanınıza saydam veri şifrelemesi (TDE) ile korunuyorsa, yönetilen örneği, Azure'da geri yüklemek istediğiniz yedekleme şifresini çözmek için kullanabileceğiniz bir sertifika geçirmek gerekir.

## <a name="network-configuration"></a>Ağ yapılandırması

- [Yönetilen örnek alt boyutunu belirlemek](sql-database-managed-instance-determine-size-vnet-subnet.md) – yönetilen örneği yerleştirilir, kaynakları içine ekledikten sonra yeniden boyutlandırılamaz alt görüntülemeye ayırır. Bu nedenle, hangi IP adresleri aralığı sayısını ve türlerini alt ağdaki dağıtmak istediğiniz örnek bağlı olarak alt ağ için gerekli olacak hesaplamak gerekir.
- [Yeni VNet ve alt ağı, yönetilen örneği için oluşturma](sql-database-managed-instance-create-vnet-subnet.md) – göre Azure VNet ve alt ağı, yönetilen örneklerinizi dağıtmak istediğiniz yapılandırılmalıdır [ağ burada açıklanan gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Bu kılavuzda, yeni VNet ve düzgün bir şekilde yönetilen örnekler için yapılandırılmış olan alt ağ oluşturmak için en kolay yolu bulabilirsiniz.
- [Yönetilen örnek için mevcut bir VNet ve alt ağ yapılandırma](sql-database-managed-instance-configure-vnet-subnet.md) –, mevcut bir VNet ve alt ağ içinde yönetilen örnek dağıtmak için yapılandırmak istiyorsanız, denetleyen betiğin burada bulabilirsiniz [ağ gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements) ve Oluştur, alt ağınızın gereksinimlerine göre yapılandırır.
- [Özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md) – db e-posta profilleri, bağlı sunucu aracılığıyla yönetilen Örneğinize özel etki alanları üzerinde dış kaynaklara erişmek istiyorsanız, özel DNS yapılandırmanız gerekir.
- [Eşitleme ağı yapılandırması](sql-database-managed-instance-sync-network-configuration.md) -Bu, olsa da gerçekleşebilir, [uygulamanızı bir Azure sanal ağı ile tümleşik](../app-service/web-sites-integrate-with-vnet.md), şunları yapabilirsiniz&#39;t yönetilen örnek bağlantı kurun. Ağ yapılandırması, hizmet planınız için bir şey deneyebilirsiniz yenilemektir.
- [Yönetim uç noktası IP adresini bulmak](sql-database-managed-instance-find-management-endpoint-ip-address.md) – yönetilen örnek genel uç nokta yönetim-yalnızca amacıyla kullanır. Burada açıklanan betiğini kullanarak yönetim uç noktasının IP adresini belirleyebilirsiniz.
- [Yerleşik güvenlik duvarı koruması doğrulayın](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md) – yönetilen örneği, yalnızca gerekli bağlantı noktalarında trafiğe izin veren yerleşik güvenlik duvarı ile korunur. Denetleyin ve bu kılavuzda açıklanan betiğini kullanarak yerleşik bir güvenlik duvarı kurallarını doğrulayın.
- [Uygulamaları bağlama](sql-database-managed-instance-connect-app.md) – yönetilen örneği, kendi özel Azure sanal ağ özel IP adresiyle yerleştirilir. Uygulamaları yönetilen Örneğinize bağlanmak için farklı yaklaşımlar hakkında bilgi edinin.

## <a name="feature-configuration"></a>Özelliği yapılandırması

- [İşlem çoğaltma](replication-with-sql-database-managed-instance.md) yönetilen örnekleri arasında veya şirket içi SQL Server yönetilen örneği, verilerinizi çoğaltmanıza olanak sağlar ve bunun tersi de geçerlidir. Daha fazla bilgi kullanın ve bu kılavuzdaki işlem çoğaltma yapılandırma bulun.
- [Tehdit algılamayı yapılandırma](sql-database-managed-instance-threat-detection.md) – [tehdit algılama](sql-database-threat-detection-overview.md) SQL ekleme veya şüpheli konumlardan erişim gibi çeşitli olası saldırıları algılayan yerleşik bir Azure SQL veritabanı özelliğidir. Bu kılavuzda etkinleştirmek ve yapılandırmak nasıl edinebilirsiniz [tehdit algılama](sql-database-threat-detection-overview.md) yönetilen örneği için.

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [nasıl yapılır kılavuzları, tek veritabanı](sql-database-howto-single-database.md)