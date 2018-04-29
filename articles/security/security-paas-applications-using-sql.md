---
title: Azure PaaS veritabanlarında güvenli hale getirme | Microsoft Docs
description: " Azure SQL Database ve SQL veri ambarı güvenliği hakkında bilgi edinme PaaS web ve mobil uygulamaların güvenliğini sağlamaya yönelik en iyi uygulamalar. "
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
ms.openlocfilehash: 3e7dc4dfba001228a4d11e2b21cdeed8e7af45ac
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="securing-paas-databases-in-azure"></a>Azure PaaS veritabanlarında güvenliğini sağlama

Bu makalede, bir koleksiyonu aşağıdakiler ele [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) ve [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/) PaaS web ve mobil uygulamaların güvenliğini sağlamaya yönelik en iyi güvenlik uygulamaları. Bu en iyi uygulamaları Azure ile deneyimi bizim ve kendiniz gibi müşterilerin deneyimleri türetilir.

## <a name="azure-sql-database-and-sql-data-warehouse"></a>Azure SQL Database ve SQL veri ambarı
[Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md) ve [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) , Internet tabanlı uygulamalar için bir ilişkisel veritabanı hizmeti sağlar. Azure SQL Database ve SQL Data Warehouse dağıtımı bir PaaS kullanırken, uygulamaları ve verileri korumaya yardımcı olmak Hizmetleri bakalım:

- Azure Active Directory kimlik doğrulaması (yerine SQL Server kimlik doğrulaması)
- Azure SQL güvenlik duvarı
- Saydam veri şifreleme (TDE)

## <a name="best-practices"></a>En iyi uygulamalar

### <a name="use-a-centralized-identity-repository-for-authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme için merkezi kimlik deposu kullanın

Azure SQL veritabanları, iki tür kimlik doğrulaması birini kullanacak şekilde yapılandırılabilir:

- **SQL kimlik doğrulaması** bir kullanıcı adı ve parola kullanır. Veritabanınıza ait mantıksal sunucuyu oluşturduktan sonra kullanıcı adı ve parola belirleyerek "sunucu yöneticisi" oturum açma bilgisi oluşturdunuz. Bu kimlik bilgilerini kullanarak, sunucu üzerindeki herhangi bir veritabanı veritabanı sahibi olarak doğrulayabilirsiniz.

- **Azure Active Directory kimlik doğrulaması** Azure Active Directory tarafından yönetilen kimlikleri kullanır ve yönetilen ve tümleşik etki alanları için desteklenir. Azure Active Directory kimlik doğrulaması kullanmak için "Azure AD kullanıcıları ve grupları yönetmek için izin verilen Azure AD yönetim" adlı başka bir sunucu yöneticisinin oluşturmanız gerekir. Bu yönetici normal bir sunucu yöneticisinin gerçekleştirebileceği tüm işlemleri yapabilir.

[Azure Active Directory kimlik doğrulaması](../active-directory/develop/active-directory-authentication-scenarios.md) kimlikleri Azure Active Directory (AD,) kullanarak Azure SQL Database ve SQL Data Warehouse bağlayan bir mekanizmadır. Azure AD kullanıcı kimlikleri artışı veritabanı sunucuları arasında durdurmak için SQL Server kimlik doğrulaması için bir alternatif sağlar. Azure AD kimlik doğrulaması, veritabanı kullanıcıları ve diğer Microsoft hizmetlerini tek bir merkezi konumda kimlikleri merkezi olarak yönetmenizi sağlar. Merkezi kimlik yönetimi, veritabanı kullanıcıları yönetmek için tek bir yer sağlar ve izin Yönetimi basitleştirir.  

SQL kimlik doğrulaması yerine Azure AD kimlik doğrulaması kullanmanın avantajları şunlardır:

- Tek bir yerde parola döndürme sağlar.
- Dış Azure kullanarak veritabanı izinlerini yönetir AD grupları.
- Depolama parolalar, tümleşik Windows kimlik doğrulaması ve Azure AD tarafından desteklenen kimlik doğrulama başka biçimlerde etkinleştirerek ortadan kaldırır.
- Veritabanı düzeyinde kimlikleri doğrulamak için veritabanı kullanıcıları bulunan kullanır.
- SQL veritabanına bağlanma uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
- Etki alanı eşitleme olmadan yerel bir Azure AD için ADFS (etki alanı Federasyonu) veya yerel kullanıcı/parola kimlik doğrulamasını destekler.
- Active Directory Evrensel içeren kimlik doğrulaması kullanan SQL Server Management Studio bağlantılarını destekler [çok faktörlü kimlik doğrulama (MFA)](../active-directory/authentication/multi-factor-authentication.md). MFA kolay doğrulama seçeneklerini aralıklı güçlü kimlik doğrulaması içerir — telefon araması, SMS mesajı, akıllı kartlar ve PIN ya da mobil uygulama bildirimi. Daha fazla bilgi için bkz: [SSMS desteklemek için SQL Database ve SQL Data Warehouse ile Azure AD MFA](../sql-database/sql-database-ssms-mfa-authentication.md).

Azure AD kimlik doğrulaması hakkında daha fazla bilgi için bkz:

- [Azure Active Directory kimlik doğrulaması kullanarak SQL veritabanı veya SQL Data Warehouse'a bağlanma](../sql-database/sql-database-aad-authentication.md)
- [Azure SQL Veri Ambarı’nda kimlik doğrulama](../sql-data-warehouse/sql-data-warehouse-authentication.md)
- [Azure AD kimlik doğrulaması kullanarak Azure SQL DB belirteç tabanlı kimlik doğrulama desteği](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/)

> [!NOTE]
> Azure Active Directory ortamınız için uygun olmanın olduğundan emin olmak için bkz: [Azure AD özelliklerini ve sınırlamalarını](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations), özellikle ek hususlar.
>
>

### <a name="restrict-access-based-on-ip-address"></a>IP adresine göre erişimi kısıtlama
Kabul edilebilir IP adreslerinin aralıklarını belirtmek güvenlik duvarı kuralları oluşturabilirsiniz. Bu kurallar sunucu ve veritabanı düzeylerinde hedeflenebilir. Veritabanı düzeyinde güvenlik duvarı kuralları mümkün olduğunca güvenliğini artırmak ve veritabanınızı daha taşınabilir sağlamak için kullanmanızı öneririz. Sunucu düzeyinde güvenlik duvarı kuralları, Yöneticiler ve aynı erişim gereksinimleri olan birçok veritabanı sahip ancak her veritabanı ayrı ayrı yapılandırma süre beklemesini istemediğiniz durumlarda için en iyi şekilde kullanılır.

SQL veritabanı'nın varsayılan kaynak IP adresi sınırlamaları (diğer abonelikler ve kiracılar dahil) tüm Azure adresinden erişime izin ver. Bu örnek erişmek IP adreslerinizi yalnızca izin verecek şekilde kısıtlayabilirsiniz. Hatta, SQL güvenlik duvarı ve IP adresi sınırlamaları ile güçlü kimlik doğrulaması hala gereklidir. Bu makalenin önceki bölümlerinde önerilerin bakın.

Azure SQL güvenlik duvarı ve IP kısıtlamaları hakkında daha fazla bilgi için bkz:

- [Azure SQL veritabanına erişim denetimi](../sql-database/sql-database-control-access.md)
- [Azure SQL veritabanı güvenlik duvarı kurallarını - yapılandırma genel bakış](../sql-database/sql-database-firewall-configure.md)
- [Azure Portalı'nı kullanarak bir Azure SQL veritabanı sunucu düzeyinde güvenlik duvarı kuralı yapılandırma](../sql-database/sql-database-configure-firewall-settings.md)

### <a name="encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi
[Saydam veri şifreleme (TDE)](https://msdn.microsoft.com/library/azure/bb934049) varsayılan olarak etkindir. TDE, SQL Server, Azure SQL Database ve Azure SQL Data Warehouse veri ve günlük dosyalarını saydam şifreler. TDE dosyalar ya da kendi yedekleme doğrudan erişim güvenliğinin korur. Bu, uygulamalarınız değiştirmeden kalan verileri şifrelemesini sağlar. TDE, her zaman etkin kalmalı; Ancak, bu normal erişim yolunu kullanarak bir saldırganın durdurmaz. TDE birçok yasalar, düzenlemeler ve çeşitli sektörün belirlenen talimatları uyması olanağı sağlar.

Azure SQL TDE'nin için anahtar ilgili sorunlar yönetir. Kurtarılabilirliği emin olmak için özellikle dikkatli şirket içi izlenmelidir TDE olarak ve veritabanlarını taşıma. Daha karmaşık senaryolarda anahtarları açıkça Azure anahtar kasası Genişletilebilir anahtar yönetimi yönetilebilir (bkz [etkinleştirmek TDE SQL Server kullanarak EKM üzerinde](/security/encryption/enable-tde-on-sql-server-using-ekm)). Bu ayrıca Getir bilgisayarınızı kendi anahtarını (BYOK için) Azure anahtar kasası BYOK yeteneği sağlar.

Azure SQL sütunlar için şifreleme sağlar [her zaman şifreli](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Bu hassas sütunları yalnızca yetkili uygulamalar erişim sağlar. Bu tür bir şifreleme kullanarak SQL sorgularını eşitlik tabanlı değerlere şifrelenmiş sütunlar için sınırlar.

Uygulama düzeyinde şifreleme için seçmeli verileri de kullanılması gerekir. Veri egemenliği sorunları bazen doğru ülke tutulur bir anahtarla verileri şifreleyerek azaltılabilir. Bu, hatta yanlışlıkla veri aktarımı (AES 256 gibi) güçlü bir algoritma kullanılır varsayarak anahtarı olmaksızın verileri şifrelemek mümkün olduğundan bir soruna neden önler.

Güvenli bir sistemde tasarlama, gizli varlıklar şifreleme ve veritabanı sunucuları geçici bir güvenlik duvarı oluşturma gibi veritabanını güvenli hale getirmek için ek güvenlik önlemleri kullanın.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede bir koleksiyonuna PaaS web ve mobil uygulamaların güvenliğini sağlamak için SQL Database ve SQL Data Warehouse en iyi güvenlik uygulamalarını kullanıma sunuldu. PaaS dağıtımları güvenliğini sağlama hakkında daha fazla bilgi için bkz:

- [PaaS dağıtımlarının güvenliğini sağlama](security-paas-deployments.md)
- [PaaS web ve mobil uygulamaları Azure App Services kullanarak güvenli hale getirme](security-paas-applications-using-app-services.md)
