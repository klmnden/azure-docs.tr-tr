---
title: "Azure Analysis Services kimlik doğrulaması ve kullanıcı izinleri | Microsoft Docs"
description: "Azure Analysis Services kimlik doğrulama ve kullanıcı izinleri hakkında bilgi edinin."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 10/09/2017
ms.author: owend
ms.openlocfilehash: e7fdb55ba29fbdc2f3d89fbb19c8b77bf2c05795
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="authentication-and-user-permissions"></a>Kimlik doğrulaması ve kullanıcı izinleri
Azure Analysis Services Kimlik Yönetimi ve kullanıcı kimlik doğrulaması için Azure Active Directory (Azure AD) kullanır. Herhangi bir kullanıcı oluşturma, yönetme veya bir Azure Analysis Services'a bağlanma sunucu geçerli kullanıcı kimlik olmalıdır bir [Azure AD kiracısı](../active-directory/active-directory-administer.md) aynı abonelik.

Azure Analysis Services destekleyen [Azure AD B2B işbirliği](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md). B2B ile bir Azure AD dizinine Konuk kullanıcılar olarak kuruluşun dışındaki kullanıcıları davet edilebilirsiniz. Konuklar, başka bir Azure AD Kiracı dizini veya herhangi bir geçerli e-posta adresi olabilir. Bir kez davet ve davet gönderildi e-posta ile Azure'den, kullanıcı kimliği Kiracı dizine eklenen kullanıcı kabul eder. Bu kimlik, güvenlik grupları veya sunucu yöneticisi veya veritabanı rolü üyeleri olarak eklenebilir.

![Azure Analysis Services kimlik doğrulama mimarisi](./media/analysis-services-manage-users/aas-manage-users-arch.png)

## <a name="authentication"></a>Kimlik Doğrulaması
Bir veya daha fazla çözümleme hizmetleri tüm istemci uygulamaları ve araçları kullanmak [istemci kitaplıkları](analysis-services-data-providers.md) (bir sunucuya bağlanmak için AMO, MSOLAP, ADOMD). 

Tüm üç istemci kitaplıkları, hem Azure AD etkileşimli akış ve etkileşimli olmayan kimlik doğrulama yöntemlerini destekler. İki etkileşimli olmayan, Active Directory parolası ve Active Directory tümleşik kimlik doğrulaması yöntemleri AMOMD ve MSOLAP kullanan uygulamalarda kullanılabilir. Bu iki yöntem hiçbir zaman açılır iletişim kutularında neden.

Excel ve Power BI Desktop gibi istemci uygulamalar ve en son sürüme güncelleştirildiğinde kitaplıklarının en son sürümlerini SSMS ve SSDT gibi araçlar yükleyin. Power BI Desktop, SSMS ve SSDT aylık güncelleştirilir. Excel [Office 365 ile güncelleştirilmiş](https://support.office.com/en-us/article/When-do-I-get-the-newest-features-in-Office-2016-for-Office-365-da36192c-58b9-4bc9-8d51-bb6eed468516). Office 365 güncelleştirmelerini daha az sıklıkta ve anlamı güncelleştirmeleri yukarı üç ay için ertelenmiş olan, bazı kuruluşlar ertelenmiş kanal kullanın.

İstemci uygulaması veya kullandığınız aracı bağlı olarak kimlik doğrulaması ve oturum nasıl türü farklı olabilir. Her uygulama, Azure Analysis Services gibi bulut hizmetlerine bağlanmak için farklı özellikleri destekleyebilir.

Power BI Desktop, SSDT ve SSMS Active Directory Evrensel kimlik doğrulaması, ayrıca Azure multi-Factor Authentication (MFA) destekleyen bir etkileşimli yöntemi destekler. Azure MFA yardımcı koruma veri ve uygulamalarınıza erişmek için basit bir oturum açma işlemini sağlarken. Birkaç doğrulama seçeneklerini (telefon araması, SMS mesajı, akıllı kartlar ve PIN ya da mobil uygulama bildirimi) ile güçlü kimlik doğrulaması sunar. Azure AD ile etkileşimli MFA doğrulama için bir açılır iletişim kutusu neden olabilir. **Evrensel kimlik doğrulama önerilir**.

Bir Windows hesabı ve evrensel kimlik doğrulaması seçili kullanarak veya kullanılabilir (Excel), Azure'da oturum açma, [Active Directory Federasyon Hizmetleri (AD FS)](../active-directory/connect/active-directory-aadconnect-azure-adfs.md) gereklidir. Federasyon, Azure AD ile ve Office 365 kullanıcıları şirket içi kimlik bilgilerini kullanarak kimlik doğrulaması ve Azure kaynaklarına erişebilir.

### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)
Azure Analysis Services sunucuları desteği bağlantılarından [SSMS V17.1](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve Windows kimlik doğrulaması, Active Directory parola kimlik doğrulaması ve Active Directory Evrensel kimlik doğrulaması kullanarak daha yüksek. Genel olarak, çünkü Active Directory Evrensel kimlik doğrulaması kullanmanız önerilir:

*  Etkileşimli ve etkileşimli olmayan kimlik doğrulama yöntemini destekler.

*  Azure B2B Konuk kullanıcılar Azure AS Kiracı davet destekler. Bir sunucuya bağlanırken Konuk kullanıcılar Active Directory Evrensel kimlik doğrulaması sunucuya bağlanırken seçmeniz gerekir.

*  Çok faktörlü kimlik doğrulamasını (MFA) destekler. Azure MFA yardımcı koruma veri ve uygulamalara erişimi doğrulama seçeneklerini aralıklı: telefon araması, SMS mesajı, akıllı kartlar ve PIN ya da mobil uygulama bildirimi. Azure AD ile etkileşimli MFA doğrulama için bir açılır iletişim kutusu neden olabilir.

### <a name="sql-server-data-tools-ssdt"></a>SQL Server Veri Araçları (SSDT)
SSDT Azure Analysis Services için MFA desteği ile Active Directory Evrensel kimlik doğrulaması kullanarak bağlanır. Kullanıcılar ilk dağıtımı Azure oturumu açın istenir. Kullanıcılar Azure için dağıtma sunucunun Sunucu yönetici izinlerine sahip bir hesapla oturum gerekir. Azure'da ilk kez oturum açma, bir belirteç atanır. SSDT belirteci bellek içi gelecekteki yeniden bağlantılar için önbelleğe alır.

### <a name="power-bi-desktop"></a>Power BI Desktop
Power BI Desktop MFA desteği ile Active Directory Evrensel kimlik doğrulaması kullanarak Azure Analysis Services bağlanır. Kullanıcılar, Azure'a ilk bağlantıda oturum açmak için istenir. Kullanıcılar için Azure bir sunucu yöneticisi veya veritabanı rolü dahil bir hesapla oturum gerekir.

### <a name="excel"></a>Excel
Excel kullanıcılar, bir Windows hesabı, bir kuruluş kimliği (e-posta adresi) veya bir dış e-posta adresi kullanarak bir sunucuya bağlanabilir. Dış e-posta kimlikleri Konuk kullanıcı olarak Azure AD'de mevcut olması gerekir.

## <a name="user-permissions"></a>Kullanıcı izinleri

**Sunucu yöneticileri** Azure Analysis Services sunucusu örneğine özeldir. Azure portalı, SSMS ve veritabanları ekleme ve kullanıcı rollerini yönetme gibi görevleri gerçekleştirmek için SSDT gibi araçlarla bağlayın. Varsayılan olarak, sunucunun oluşturur Kullanıcı Analysis Services sunucu yönetici olarak otomatik olarak eklenir. Diğer yöneticiler, Azure portal veya SSMS kullanarak eklenebilir. Sunucu yöneticileri, aynı abonelikte Azure AD kiracısı bir hesabınızın olması gerekir. Daha fazla bilgi için bkz: [sunucu yöneticileri yönetmek](analysis-services-server-admins.md). 

**Veritabanı kullanıcıları** modeli veritabanları için Excel veya Power BI gibi istemci uygulamalarını kullanarak bağlanın. Veritabanı rolleri için kullanıcıların eklenmesi gerekir. Veritabanı rolleri, yönetici, işlem ya da bir veritabanı için Okuma izinleri tanımlar. Yönetici izinlerine sahip bir roldeki veritabanı kullanıcılar sunucu yöneticileri farklı olduğunu anlamak önemlidir. Ancak, varsayılan olarak, sunucu yöneticilerinin Veritabanı yöneticileri aynı zamanda değildir. Daha fazla bilgi için bkz: [veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md).

**Azure kaynak sahiplerine**. Kaynak sahiplerine bir Azure aboneliğine yönelik kaynakları yönetin. Kaynak sahiplerine ekleyebilirsiniz Azure AD kullanıcı kimlikleri sahibi veya katkıda bulunan rollerinin bir aboneliğin kullanarak **erişim denetimi** Azure portalında veya Azure Resource Manager şablonları ile. 

![Azure portalında erişim denetimi](./media/analysis-services-manage-users/aas-manage-users-rbac.png)

Bu düzeyde rolleri kullanıcı veya portalında veya Azure Resource Manager şablonları kullanarak tamamlanabilir görevleri gerçekleştirmek için gereken hesapları için geçerlidir. Daha fazla bilgi için bkz: [rol tabanlı erişim denetimi](../active-directory/role-based-access-control-what-is.md). 


## <a name="database-roles"></a>Veritabanı rolleri

 Tablo modeli için tanımlanan veritabanı rolleri rolleridir. Diğer bir deyişle, Azure AD kullanıcılarının oluşan üyeleri rolleri içerir ve bu üyeler eylemi tanımlamak özel izinleri olan güvenlik grupları bir model veritabanı üzerinde alabilir. Veritabanı rolü veritabanında ayrı bir nesne olarak oluşturulur ve bu rol oluşturulduğu veritabanına uygular.   
  
 Yeni bir tablo modeli projesi oluşturduğunuzda varsayılan olarak, herhangi bir rol modeli projesi yok. Roller, Rol Yöneticisi iletişim kutusuna SSDT kullanarak tanımlanabilir. Rolleri modeli proje tasarım sırasında tanımlandığında, bunlar yalnızca model çalışma alanı veritabanı uygulanır. Model dağıtıldığında, aynı roller dağıtılan modele uygulanır. Bir model dağıtıldıktan sonra rolleri ve üyeleri, sunucu ve Veritabanı yöneticileri SSMS kullanarak yönetebilirsiniz. Daha fazla bilgi için bkz: [veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md).
  


## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory grupları ile kaynaklara erişimi yönetme](../active-directory/active-directory-manage-groups.md)   
[Veritabanı rolleri ve kullanıcıları yönetme](analysis-services-database-users.md)  
[Sunucu yöneticilerini yönetme](analysis-services-server-admins.md)  
[Rol Tabanlı Access Control](../active-directory/role-based-access-control-what-is.md)  