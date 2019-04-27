---
title: Azure Analysis Services kimlik doğrulaması ve kullanıcı izinleri | Microsoft Docs
description: Azure Analysis Services kimlik doğrulaması ve kullanıcı izinleri hakkında bilgi edinin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 7673b115a3ad2e6ca7aec34b1cfabfb38d2a16f4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60859218"
---
# <a name="authentication-and-user-permissions"></a>Kimlik doğrulaması ve kullanıcı izinleri

Azure Analysis Services Kimlik Yönetimi ve kullanıcı kimlik doğrulaması için Azure Active Directory (Azure AD) kullanır. Herhangi bir kullanıcı oluşturma, yönetme veya bir Azure Analysis Services'e bağlanma sunucusu geçerli kullanıcı kimlik olmalı bir [Azure AD kiracısı](../active-directory/fundamentals/active-directory-administer.md) aynı abonelikte.

Azure Analysis Services destekler [Azure AD B2B işbirliği](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md). B2B ile bir kuruluş dışından kullanıcılar Azure AD dizini Konuk kullanıcı davet edebilir. Konuklar, başka bir Azure AD Kiracı dizini veya herhangi bir geçerli e-posta adresi olabilir. Bir kez davet ve kullanıcı davet gönderilen e-posta ile Azure'nın sunduğu kullanıcı kimliği Kiracı dizinine eklenir kabul eder. Bu kimlik, güvenlik grupları veya sunucu yöneticisi veya veritabanı rolünün üyeleri olarak eklenebilir.

![Azure Analysis Services kimlik doğrulama mimarisi](./media/analysis-services-manage-users/aas-manage-users-arch.png)

## <a name="authentication"></a>Kimlik Doğrulaması

Bir veya daha fazla Analysis Services'ın tüm istemci uygulamaları ve araçları kullanın [istemci kitaplıkları](analysis-services-data-providers.md) (bir sunucuya bağlanmak için MSOLAP, olan AMO ADOMD). 

İstemci kitaplıklarının üçünü hem Azure AD etkileşimli akışı ve etkileşimli olmayan kimlik doğrulama yöntemleri için destek. İki etkileşimli olmayan yöntemleri, Active Directory parolası ve Active Directory tümleşik kimlik doğrulaması yöntemleri AMOMD ve MSOLAP kullanan uygulamalarda kullanılabilir. Bu iki yöntem hiçbir zaman açılır iletişim kutularında neden.

Excel ve Power BI Desktop gibi istemci uygulamalar ve SSMS'yi ve ssdt'yi yükleme gibi araçları en son sürüme güncelleştirildiğinde kitaplıklarının en son sürümlerini yükler. Power BI Desktop, SSMS'yi ve SSDT'yi aylık güncelleştirilir. Excel [Office 365 ile güncelleştirilmiş](https://support.office.com/article/When-do-I-get-the-newest-features-in-Office-2016-for-Office-365-da36192c-58b9-4bc9-8d51-bb6eed468516). Office 365 güncelleştirmeleri daha az sıklıkta ve anlamı güncelleştirmeleri yedeklemek için üç ay ertelenmiş, bazı kuruluşlar ertelenmiş kanal kullanın.

İstemci uygulamanız veya kullandığınız aracı bağlı olarak kimlik doğrulaması ve oturum açma türü farklı olabilir. Her uygulama, Azure Analysis Services gibi bulut hizmetlerine bağlanmak için farklı özellikleri desteklemiyor olabilir.

Power BI Desktop, SSDT ve SSMS Active Directory Evrensel kimlik doğrulaması, Azure multi-Factor Authentication (MFA) destekleyen etkileşimli bir yöntemi destekler. Azure MFA erişimi korumaya yardımcı olur ve basit bir oturum açma işlemi sağlarken uygulamalarınıza. Bu güçlü kimlik doğrulaması (telefon araması, SMS mesajı, akıllı kart PIN ya da mobil uygulama bildirimi ile) birçok doğrulama seçeneği sunar. Azure AD ile etkileşimli MFA doğrulama için bir açılır iletişim kutusu neden olabilir. **Evrensel kimlik doğrulaması önerilir**.

Bir Windows hesabı ve evrensel kimlik doğrulaması seçilmemiş kullanarak veya kullanılabilir (Excel), Azure'da oturum açma, [Active Directory Federasyon Hizmetleri (AD FS)](../active-directory/hybrid/how-to-connect-fed-azure-adfs.md) gereklidir. Federasyon, Azure AD ile Office 365 kullanıcıları, şirket içi kimlik bilgilerini kullanarak kimlik doğrulaması ve Azure kaynaklarına erişebilirsiniz.

### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)

Azure Analysis Services sunucuları desteği bağlantılarından [SSMS V17.1](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve Windows kimlik doğrulaması, Active Directory parola kimlik doğrulaması ve Active Directory Evrensel kimlik doğrulaması'nı kullanarak daha yüksek. Genel olarak, çünkü'de Active Directory Evrensel kimlik doğrulaması kullanmanız önerilir:

*  Etkileşimli ve etkileşimli olmayan kimlik doğrulama yöntemleri destekler.

*  Azure B2B Konuk kullanıcılar Azure AS kiracısına davet destekler. Bir sunucuya bağlanırken Konuk kullanıcıları Active Directory Evrensel kimlik doğrulaması sunucuya bağlanırken seçmeniz gerekir.

*  Çok faktörlü kimlik doğrulaması (MFA) destekler. Azure MFA erişimi korumaya yardımcı olur ve doğrulama seçenekleri ile uygulamalarınıza: telefon araması, SMS mesajı, akıllı kartlar ve PIN ya da mobil uygulama bildirimi. Azure AD ile etkileşimli MFA doğrulama için bir açılır iletişim kutusu neden olabilir.

### <a name="sql-server-data-tools-ssdt"></a>SQL Server Veri Araçları (SSDT)

SSDT, Azure Analysis Services için MFA desteği ile Active Directory Evrensel kimlik doğrulaması kullanarak bağlanır. Kullanıcı ilk dağıtımı Azure'da oturum açmanız istenir. Kullanıcıların azure'a dağıtım yaptığınız sunucuyu Sunucu Yöneticisi izinlerine sahip bir hesapla oturum açmalısınız. Azure'a ilk kez oturum açarken bir belirteç atanır. SSDT belirteci bellek içi gelecekteki yeniden bağlantılar için önbelleğe alır.

### <a name="power-bi-desktop"></a>Power BI Desktop

Power BI Desktop Azure Analysis Services MFA desteği ile Active Directory Evrensel kimlik doğrulaması kullanarak bağlanır. Kullanıcılar, ilk bağlantı Azure'da oturum açmanız istenir. Kullanıcıların Azure'a bir sunucu yöneticisi veya veritabanı rolüne dahil edilen bir hesapla oturum açmalısınız.

### <a name="excel"></a>Excel

Excel kullanıcılar, bir Windows hesabı, bir kuruluş kimliği (e-posta adresi) veya bir dış e-posta adresi kullanarak bir sunucuya bağlanabilir. Dış e-posta kimlikleri, bir Konuk kullanıcı olarak Azure AD'de mevcut olması gerekir.

## <a name="user-permissions"></a>Kullanıcı izinleri

**Sunucu yöneticileri** Azure Analysis Services sunucu örneğine özgüdür. Azure portalı, SSMS ve veritabanları ekleme ve kullanıcı rollerini yönetme gibi görevleri gerçekleştirmek için SSDT gibi araçlarla bağlanın. Varsayılan olarak, sunucunun oluşturan kullanıcı otomatik olarak bir Analysis Services sunucusu yöneticisi olarak eklenir. Diğer yöneticiler, Azure portal'ı veya SSMS kullanarak eklenebilir. Sunucu yöneticileri, aynı abonelikte bulunan Azure AD kiracısında bir hesabınızın olması gerekir. Daha fazla bilgi için bkz. [sunucu yöneticilerini yönetme](analysis-services-server-admins.md). 

**Veritabanı kullanıcıları** modeli veritabanları için Excel veya Power BI gibi istemci uygulamalarını kullanarak bağlanın. Kullanıcılar, veritabanı rollerine eklenmelidir. Veritabanı rolleri, yönetici, işlem veya bir veritabanı için Okuma izinleri tanımlayın. Veritabanı kullanıcıları yönetici izinlerine sahip bir roldeki Yöneticiler farklı olduğunu anlamak önemlidir. Bununla birlikte, varsayılan olarak, sunucu yöneticilerinin ayrıca veritabanı yöneticilerdir. Daha fazla bilgi için bkz. [veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md).

**Azure kaynak sahiplerinin**. Kaynak sahiplerinin, Azure aboneliğinin kaynaklarını yönetme. Kaynak sahiplerinin ekleyebilirsiniz Azure AD kullanıcı kimlikleri sahibi veya katkıda bulunan rol için bir abonelik dahilinde kullanarak **erişim denetimi** Azure portalında veya Azure Resource Manager şablonları ile. 

![Azure portalında erişim denetimi](./media/analysis-services-manage-users/aas-manage-users-rbac.png)

Kullanıcılara veya portalında veya Azure Resource Manager şablonları kullanarak tamamlanabilir görevleri gerçekleştirmek için gereken hesaplar rolleri bu düzeyde uygulanır. Daha fazla bilgi için bkz. [rol tabanlı erişim denetimi](../role-based-access-control/overview.md). 

## <a name="database-roles"></a>Veritabanı rolleri

 Bir tablosal model için tanımlanmış veritabanı rolleri rolleridir. Diğer bir deyişle, rolleri Azure AD kullanıcılarının oluşan üyeleri içerir ve güvenlik gruplarını, bu üyeler eylemi tanımlayan belirli izinlere sahip bir model veritabanında alabilir. Veritabanı rolü, veritabanında ayrı bir nesne olarak oluşturulur ve yalnızca bu rolün oluşturulduğu veritabanı için geçerli olur.   
  
 Yeni bir tablosal model projesi oluşturduğunuzda varsayılan olarak, model projesi herhangi bir rolü yok. Rolleri, Rol Yöneticisi iletişim kutusunda SSDT kullanarak tanımlanabilir. Rolleri modeli proje tasarım sırasında tanımlandığında, bunlar yalnızca model çalışma alanı veritabanına uygulanır. Model dağıtıldığında, aynı rol dağıtılan modele uygulanır. Bir model dağıtıldıktan sonra rolleri ve üyeleri, sunucu ve Veritabanı yöneticileri SSMS kullanarak yönetebilirsiniz. Daha fazla bilgi için bkz. [veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md).
  
## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory grupları ile kaynaklara erişimi yönetme](../active-directory/fundamentals/active-directory-manage-groups.md)   
[Veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md)  
[Sunucu yöneticilerini yönetme](analysis-services-server-admins.md)  
[Rol Tabanlı Access Control](../role-based-access-control/overview.md)  