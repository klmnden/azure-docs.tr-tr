---
title: Azure PaaS veritabanlarının güvenliğini sağlama | Microsoft Docs
description: " Azure SQL veritabanı ve SQL veri ambarı güvenliği hakkında PaaS web ve mobil uygulamalarınızın güvenliğini sağlamak için en iyi adımları öğrenin. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 00b2b249f5889888f34d57fd1577ccfea776d00c
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347979"
---
# <a name="securing-paas-databases-in-azure"></a>Azure PaaS veritabanlarının güvenliğini sağlama

Bu makalede ele koleksiyonu [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) ve [SQL veri ambarı](https://azure.microsoft.com/services/sql-data-warehouse/) PaaS web ve mobil uygulamalarınızın güvenliğini sağlamak için en iyi güvenlik uygulamaları. Bu en iyi Azure ile deneyimimizi ve sizin gibi müşteri deneyimleri türetilmiştir.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Azure SQL veritabanı ve SQL veri ambarı
[Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md) ve [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) Internet tabanlı uygulamalarınız için bir ilişkisel veritabanı hizmeti sağlar. Azure SQL veritabanı ve SQL veri ambarı bir PaaS dağıtımda kullanılırken uygulamalarınızın ve verilerinizin korunmasına yardımcı olma Hizmetleri bakalım:

- Azure Active Directory kimlik doğrulaması (yerine SQL Server kimlik doğrulaması)
- Azure SQL güvenlik duvarı
- Saydam veri şifrelemesi (TDE)

## <a name="best-practices"></a>En iyi uygulamalar

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme için merkezi kimlik deposu kullanın.

Azure SQL veritabanları, iki tür kimlik doğrulaması kullanmak için yapılandırılabilir:

- **SQL kimlik doğrulaması** adı ve parola kullanır. Veritabanınıza ait mantıksal sunucuyu oluşturduktan sonra kullanıcı adı ve parola belirleyerek "sunucu yöneticisi" oturum açma bilgisi oluşturdunuz. Bu kimlik bilgilerini kullanarak, sunucu üzerindeki herhangi bir veritabanı veritabanı sahibi olarak doğrulayabilirsiniz.

- **Azure Active Directory kimlik doğrulaması** Azure Active Directory tarafından yönetilen kimlikleri kullanır ve yönetilen ve tümleşik etki alanları için desteklenir. Azure Active Directory kimlik doğrulamasını kullanmak için "Azure AD kullanıcılarını ve gruplarını yönetme izni olan Azure AD Yöneticisi" adlı başka bir sunucu yöneticisi oluşturmanız gerekir. Bu yönetici normal bir sunucu yöneticisinin gerçekleştirebileceği tüm işlemleri yapabilir.

[Azure Active Directory kimlik doğrulaması](../active-directory/develop/authentication-scenarios.md) kimlikleri Azure Active Directory (AD) kullanarak Azure SQL veritabanı ve SQL veri ambarına bağlanma bir mekanizmadır. Azure AD, kullanıcı kimliklerini çoğalan veritabanı sunucuları arasında durdurmak için SQL Server kimlik doğrulaması için bir alternatif sağlar. Azure AD kimlik doğrulaması veritabanı kullanıcıları ve tek bir merkezi konumda diğer Microsoft Hizmetleri kimliklerini merkezi olarak yönetmenizi sağlar. Merkezi kimlik yönetimi, veritabanı kullanıcıları yönetmek için tek bir yerde sağlar ve izin yönetimini kolaylaştırır.  

Azure AD kimlik doğrulaması yerine SQL kimlik doğrulaması kullanmanın avantajları şunlardır:

- Tek bir yerde parola dönüşü sağlar.
- Dış Azure kullanarak veritabanı izinleri yönetir AD grupları.
- Parolaları, tümleşik Windows kimlik doğrulaması ve diğer Azure AD tarafından desteklenen kimlik doğrulama etkinleştirerek ortadan kaldırır.
- Veritabanı düzeyinde kimlikleri doğrulamak için veritabanı kullanıcıları yer alan kullanır.
- SQL veritabanına bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
- Etki alanı eşitleme olmadan yerel bir Azure AD için ADFS (etki alanı Federasyon) veya yerel kullanıcı/parola kimlik doğrulamasını destekler.
- Active Directory Evrensel içeren kimlik doğrulaması kullanan SQL Server Management Studio bağlantılarından destekler [multi-Factor Authentication (MFA)](../active-directory/authentication/multi-factor-authentication.md). MFA, güçlü kimlik doğrulaması ile çeşitli kolay doğrulama seçenekleri içerir — telefon araması, SMS mesajı, akıllı kartlar ve PIN ya da mobil uygulama bildirimi. Daha fazla bilgi için [SQL veritabanı ve SQL veri ambarı ile Azure AD MFA için SSMS desteği](../sql-database/sql-database-ssms-mfa-authentication.md).

Azure AD kimlik doğrulaması hakkında daha fazla bilgi için bkz:

- [SQL Database'e veya SQL veri ambarı için Azure Active Directory kimlik doğrulamasını kullanarak bağlanma](../sql-database/sql-database-aad-authentication.md)
- [Azure SQL Veri Ambarı’nda kimlik doğrulama](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Azure AD kimlik doğrulamasını kullanarak Azure SQL DB için belirteç tabanlı kimlik doğrulama desteği](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)

> [!NOTE]
> Azure Active Directory ortamınız için uygun olmasını sağlamak için bkz: [Azure AD özellikleri ve sınırlamaları](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), özellikle ek hususlar.
>
>

### <a name="restrict-access-based-on-ip-address"></a>IP adresine göre erişimi kısıtlama
Kabul edilebilir IP adreslerinin aralıklarını belirten güvenlik duvarı kuralları oluşturabilirsiniz. Bu kurallar sunucu ve veritabanı düzeylerinde hedefleyebilir. Mümkün olduğunda güvenliği artırmak ve veritabanınızı daha taşınabilir yapmak için veritabanı düzeyinde güvenlik duvarı kuralları kullanmanızı öneririz. Sunucu düzeyinde güvenlik duvarı kuralları, Yöneticiler ve ne zaman aynı erişim gereksinimlerine sahip birçok veritabanınız varsa ancak her veritabanını ayrı ayrı yapılandırmaya zaman harcamak istemiyorsanız için en iyi şekilde kullanılır.

SQL veritabanı'nın varsayılan kaynak IP adresi sınırlamaları (diğer abonelikler ve kiracıları dahil) tüm Azure adresinden erişim sağlar. Bu yalnızca bir örneğine erişmek IP adreslerinizi izin verecek şekilde kısıtlayabilirsiniz. Hatta SQL güvenlik duvarınızdan ve IP adresi sınırlamaları ile güçlü kimlik doğrulaması yine de gereklidir. Bu makalenin önceki bölümlerinde yapılan öneriler bakın.

Azure SQL güvenlik duvarı ve IP kısıtlamaları hakkında daha fazla bilgi için bkz:

- [Azure SQL veritabanı erişim denetimi](../sql-database/sql-database-control-access.md)
- [Azure SQL veritabanı güvenlik duvarı kuralları - yapılandırma genel bakış](../sql-database/sql-database-firewall-configure.md)
- [Azure portalını kullanarak Azure SQL veritabanı sunucu düzeyinde güvenlik duvarı kuralı yapılandırma](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi
[Saydam veri şifrelemesi (TDE)](https://msdn.microsoft.com/library/azure/bb934049) varsayılan olarak etkindir. TDE şeffaf bir şekilde SQL Server, Azure SQL veritabanı ve Azure SQL veri ambarı veri ve günlük dosyaları şifreler. TDE, doğrudan erişim dosyalar ya da bunların yedekleme için bir güvenliğinin ihlallere karşı korur. Bu, mevcut uygulamalarınızı değiştirmeden kalan verileri şifrelemesini sağlar. TDE, her zaman etkin kalmalı; Ancak, bu normal erişim yolunu kullanarak bir saldırganın durdurmaz. TDE, birçok yasaları, düzenlemeleri ve çeşitli sektörlerde oluşturduğu yönergeleri ile uyumlu olanağı sağlar.

Azure SQL TDE için anahtar ilgili sorunlarını yönetir. TDE ile kurtarılabilirliği emin olmak için şirket içi özel dikkatli olunması gerekir ve veritabanları taşırken. Daha karmaşık senaryolarda, anahtarları açıkça Azure anahtar Kasası'nda Genişletilebilir anahtar yönetimi yönetilebilir (bkz [SQL Server kullanarak EKM TDE'yi etkinleştirme](/sql/relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm)). Bu ayrıca Getir bilgisayarınızı kendi anahtarını (BYOK için) Azure anahtar kasası BYOK yeteneği sağlar.

Azure SQL sütunlar için şifreleme sağlar [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Bu, gizli sütunlara yalnızca yetkili uygulamalar erişim sağlar. Bu tür bir şifreleme kullanarak SQL sorguları için değer eşitliği tabanlı şifrelenmiş sütunlar için sınırlar.

Uygulama düzeyinde şifreleme, ayrıca seçilen veriler için kullanılmalıdır. Veri egemenliği sorunları, bazen bir anahtarla doğru ülke tutulan verileri şifreleyerek azaltılabilir. Bu, bile yanlışlıkla veri aktarımı, güçlü bir algoritma (AES 256 gibi) kullanılan varsayılarak anahtarı içermeyen ve verilerin şifresini çözmek mümkün olduğundan soruna neden olmasını engeller.

Güvenli sistem tasarımı, gizli varlıkları şifreleme ve veritabanı sunucular etrafında bir güvenlik duvarı oluşturma gibi veritabanı güvenliğini sağlamaya yardımcı olmak için ek önlemler kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, PaaS web ve mobil uygulamalarınızın güvenliğini sağlamak için SQL veritabanı ve SQL veri ambarı en iyi güvenlik uygulamaları koleksiyonunu kullanıma sunuldu. PaaS dağıtımlarının güvenliğini sağlama hakkında daha fazla bilgi için bkz:

- [PaaS dağıtımlarının güvenliğini sağlama](security-paas-deployments.md)
- [PaaS web ve Azure App Services'ı kullanarak mobil uygulamalarının güvenliğini sağlama](security-paas-applications-using-app-services.md)
