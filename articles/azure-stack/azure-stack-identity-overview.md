---
title: "Azure yığın kimliğini genel bakış | Microsoft Docs"
description: "Azure yığın ile kullanabileceğiniz kimlik sistemleri hakkında bilgi edinin."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/22/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: deebe5d8ff4c35c4507d2daf5c15025a1810d865
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="overview-of-identity-for-azure-stack"></a>Azure yığın kimliğini genel bakış

Azure yığını, Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS), Active Directory kimlik sağlayıcısı tarafından desteklenen gerektirir. Bir sağlayıcı Azure yığın ilk kez dağıttığınızda yaptığınız bir kerelik bir karardır seçimdir. Kavramları ve bu makalede yetkilendirme ayrıntılarını arasında kimlik sağlayıcıları seçmenize yardımcı olabilir. 

Tercih ettiğiniz ya da Azure AD veya AD FS Azure yığın dağıtmak modu tarafından belirlenen: 
- Bağlı bir modda dağıttığınızda, Azure AD ya da AD FS kullanabilirsiniz. 
- İnternet bağlantısı olmadan bağlantısı kesilmiş bir modda dağıtırken yalnızca AD FS desteklenir.

Azure yığın ortamınıza bağlıdır, seçenekleriniz hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
- Azure yığın Dağıtım Seti: [kimlik konuları](azure-stack-datacenter-integration.md#identity-considerations).
- Azure yığın tümleşik sistemler: [dağıtım planlama kararları Azure yığınının tümleşik sistemleri](azure-stack-deployment-decisions.md).

 
## <a name="common-concepts-for-identity"></a>Kimliği için ortak kavramlar
Sonraki bölümlerde, kimlik sağlayıcısı ve bunların kullanılması Azure yığınında hakkında genel kavramlar açıklanmaktadır.

![Kimlik sağlayıcılar terminolojisi](media/azure-stack-identity-overview/terminology.png)

### <a name="directory-tenants-and-organizations"></a>Dizin kiracılar ve kuruluşlar
Bir dizin hakkında bilgileri tutan bir kapsayıcıdır *kullanıcılar*, *uygulamaları*, *grupları*, ve *hizmet sorumluları*.
 
Bir dizin Kiracı bir *kuruluş*Microsoft veya kendi şirket gibi. 
- Azure AD birden çok kiracıya destekler ve birden çok kuruluş, her biri kendi dizin destekleyebilir. Azure AD kullanın ve birden çok kiracıya varsa, uygulamaları ve diğer kiracılar aynı dizin, bir kiracı erişimden kullanıcılara erişim izni verebilir.
- AD FS yalnızca tek bir kiracı destekler ve bu nedenle, yalnızca tek bir kuruluş. 

### <a name="users-and-groups"></a>Kullanıcılar ve gruplar
Kullanıcı hesapları (Kimlikler) kişiler bir kullanıcı kimliği ve parola kullanarak kimlik doğrulaması standart hesaplarıdır. Grupları, kullanıcıları veya diğer grupları dahil edebilirsiniz. 

Kullandığınız kimlik çözümü nasıl oluşturmak ve kullanıcıları ve grupları yönetin bağlıdır. 

Azure yığınında kullanıcı hesapları: 
- Oluşturulan  *username@domain*  biçimi. AD FS kullanıcı hesaplarını Active Directory örneğine eşlemeleri rağmen AD FS kullanımını desteklemez  *\<etki alanı >\<diğer adı >* biçimi. 
- Çok faktörlü kimlik doğrulaması kullanacak şekilde ayarlanabilir. 
- Kuruluşun dizin olan burada bunlar ilk kaydetmek, dizine kısıtlanır.
- Şirket içi dizinlerinizi içeri aktarılabilir. Daha fazla bilgi için bkz: [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](/azure/active-directory/connect/active-directory-aadconnect). 

Kuruluşunuzun Kiracı portalında oturum açtığınızda, kullandığınız  *https://portal.local.azurestack.external*  URL. 

### <a name="guest-users"></a>Konuk kullanıcılar
Konuk kullanıcılar, diğer dizin kiracıdan dizininizde kaynaklara erişim izni verilen kullanıcı hesaplarıdır. Konuk kullanıcılar desteklemek için Azure AD kullanın ve çoklu kiracı için desteği etkinleştirir. Destek etkinleştirildiğinde, dış kuruluşlar kendi işbirliğiyle sırayla sağlayan directory kiracınızda bulunan kaynaklara erişmek için konuk kullanıcıları davet edebilirsiniz. 

Bulut operatörleri ve kullanıcıların Konuk kullanıcılar davet etmek için sonuna kullanabilirsiniz [Azure AD B2B işbirliği](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Davet edilen kullanıcılar belgeleri, kaynaklar ve uygulamalar dizininizden erişmek ve kendi kaynakları ve veri üzerinde denetimi korumak. 

Konuk kullanıcı olarak, başka bir kuruluşun dizin Kiracı oturum açabilir. Bunu yapmak için bir kuruluşun dizin adı portalına komut satırına URL. Örneğin, Contoso kuruluşa ait ve Fabrikam dizinine oturum açmak istiyorsanız, kullandığınız https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

### <a name="applications"></a>Uygulamalar
Uygulamaları Azure AD veya AD FS için kaydolun ve ardından, kuruluşunuzdaki kullanıcılara uygulama sunar. 

Uygulamalar şunlardır:
- **Web uygulaması**: örnekler Azure portalı ve Azure Resource Manager. Web API çağrıları destekledikleri. 
- **Yerel istemci**: Azure PowerShell, Visual Studio ve Azure CLI örnekleri içerir.

Kiracı iki tür uygulamaları destekler: 
- **Tek Kiracı**: Kullanıcıları ve Hizmetleri yalnızca, uygulama kayıtlı aynı dizinden destekler. 

  > [!NOTE]    
  > AD FS yalnızca tek bir dizin desteklediğinden, bir AD FS topolojisinde oluşturmak, tasarım gereği, tek Kiracı uygulamaları uygulamalardır.

- **Çok kiracılı**: kullanıcıların ve hizmetlerin, uygulama kayıtlı dizin ve ek Kiracı dizinleri kullanım destekler. Çok kiracılı uygulamalarla başka bir kiracı directory (başka bir Azure AD kiracısı) can kullanıcılarının uygulamanıza oturum açın. 

  Çoklu kiracı hakkında daha fazla bilgi için bkz: [etkinleştirmek çoklu kiracı](azure-stack-enable-multitenancy.md). 

  Çok kiracılı uygulama geliştirme hakkında daha fazla bilgi için bkz: [çok kiracılı uygulamalara](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

Bir uygulamayı kaydetme, iki nesne oluşturun:

- **Uygulama nesnesi**: uygulama tüm kiracılar arasında genel gösterimi. Bu ilişki yazılım uygulamasıyla bire bir ve yalnızca, uygulama ilk kayıtlı dizini yok.

- **Hizmet sorumlusu nesnesi**: uygulamanın, uygulamayı ilk kayıtlı dizininde oluşturulan bir kimlik bilgisi. Bir hizmet sorumlusu da bu uygulama kullanıldığı her bir ek Kiracı dizinde oluşturulur. Bu ilişki bire çok yazılım uygulamaya sahip olabilir. 

Uygulama ve hizmet asıl nesneleri hakkında daha fazla bilgi için bkz: [uygulama ve hizmet asıl nesneler Azure Active Directory'de](/azure/active-directory/develop/active-directory-application-objects). 

### <a name="service-principals"></a>Hizmet asıl adı 
Bir hizmet sorumlusu kümesidir *kimlik bilgileri* bir uygulama veya Azure yığınında kaynaklara erişim izni hizmet için. Bir hizmet sorumlusu kullanımını kullanıcının izinleriyle uygulama izinleri uygulamanın ayırır.

Bir hizmet sorumlusu uygulama kullanıldığı her bir kiracı içinde oluşturulur. Hizmet sorumlusu oturum açma ve erişim, Kiracı tarafından güvenliği sağlanan kaynakları (örneğin, kullanıcılar) için bir kimliği oluşturur. 

- Tek kiracılı uygulama ilk oluşturulduğu dizinde yalnızca bir hizmet sorumlusu sahiptir. Bu hizmet sorumlusu oluşturulur ve izin uygulama kaydı sırasında kullanılan için. 
- Bir çok kiracılı web uygulaması veya API her bir kiracı oluşturulduğunda bir hizmet sorumlusu nerede Kiracı kullanıcıdan uygulamanın kullanımına izin vardır. 

Hizmet asıl adı için kimlik bilgileri, Azure portalı üzerinden oluşturulan bir anahtar ya da bir sertifika olabilir. Sertifikaları anahtarlara göre daha güvenli olarak kabul edilir çünkü bir sertifika kullanımını Otomasyon için uygundur. 


> [!NOTE]    
> AD FS ile Azure yığın kullandığınızda, yalnızca yönetici hizmet asıl adı oluşturabilirsiniz. AD FS ile hizmet asıl adı sertifika gerektiren ve ayrıcalıklı uç noktası aracılığıyla (CESARETLENDİRİCİ) oluşturulur. Daha fazla bilgi için bkz: [Azure yığın uygulama erişim sağlayan](azure-stack-create-service-principals.md).

Azure yığınının hizmet sorumluları hakkında bilgi edinmek için [hizmet sorumluları oluşturmak](azure-stack-create-service-principals.md).


### <a name="services"></a>Hizmetler
Kimlik sağlayıcısı ile etkileşim Hizmetleri Azure yığınında kimlik sağlayıcısı ile uygulamalar olarak kaydedilir. Uygulamalar gibi kimlik sistemi ile kimlik doğrulaması bir hizmet kaydını etkinleştirir. 

Tüm Azure hizmetlerine [Openıd Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) protokolleri ve [JSON Web belirteçlerini](/azure/active-directory/develop/active-directory-token-and-claims) kendi kimliğini oluşturmak için. Nedeniyle Azure AD ve AD FS kullanma protokolleri tutarlı bir şekilde, kullanabileceğiniz [Azure Active Directory kimlik doğrulama Kitaplığı](/azure/active-directory/develop/active-directory-authentication-libraries) (şirket içi kimlik doğrulaması için ADAL) veya Azure (bir senaryoda bağlı). ADAL ile Azure PowerShell ve Azure CLI gibi araçları arası Bulut ve şirket içi kaynak yönetimi için de kullanabilirsiniz. 


### <a name="identities-and-your-identity-system"></a>Kimlik ve kimlik sistemi 
Azure yığını için kimlikleri, kullanıcı hesaplarını, grupları ve hizmet asıl adı içerir. 

Azure yığın yüklediğinizde, birkaç yerleşik uygulamaları ve Hizmetleri otomatik olarak directory kiracısında, kimlik sağlayıcısı ile kaydedin. Kayıt bazı hizmetler yönetimi için kullanılır. Diğer hizmetler kullanıcıları için kullanılabilir. Varsayılan kayıtlar hem birbirleriyle ve daha sonra eklediğiniz kimlikleri etkileşime Çekirdek Hizmetleri kimlikleri verin.

Azure AD çoklu kiracı ile ayarladıysanız, bazı uygulamaları yeni dizinler yayılır. 

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme
 

### <a name="authentication-by-applications-and-users"></a>Uygulamalar ve kullanıcılar tarafından kimlik doğrulaması
  
![Azure yığınının katmanlar arasında kimlik](media/azure-stack-identity-overview/identity-layers.png)

Uygulamalar ve kullanıcılar için Azure yığın mimarisi dört katmanı tarafından tanımlanır. Bu katmanlardan her arasındaki etkileşimler farklı tür kimlik doğrulaması kullanabilirsiniz.


|Katman    |Katmanlar arasında kimlik doğrulaması  |
|---------|---------|
|Araçlar ve Yönetim Portalı gibi istemcileri     | Erişmek veya kaynak Azure yığınında değiştirmek için Araçlar ve istemcilerin kullandığı bir [JSON Web belirteci](/azure/active-directory/develop/active-directory-token-and-claims) için Azure Resource Manager aramak için. <br>Azure Resource Manager JSON Web belirteci doğrular ve iletiye göz atar *talep* verilen belirteç yetkilendirme düzeyini tahmin etmek için o kullanıcı ya da hizmet asıl Azure yığına sahip. |
|Azure Kaynak Yöneticisi ve Çekirdek Hizmetleri     |Azure Resource Manager iletişim kullanıcılardan aktarmak için kaynak sağlayıcıları ile iletişim kurar. <br> Aktarır kullanım *doğrudan kesinliği* çağrıları veya *bildirim temelli* aracılığıyla çağırır [Azure Resource Manager şablonları](/azure/azure-stack/user/azure-stack-arm-templates.md).|
|Kaynak sağlayıcıları     |Kaynak sağlayıcıları için geçirilen çağrıları sertifika tabanlı kimlik doğrulaması ile güvenli hale getirilir. <br>Azure Resource Manager ve kaynak sağlayıcısı, bir API aracılığıyla iletişim sonra kalır. Azure Kaynak Yöneticisi'nden alınan her çağrı için kaynak sağlayıcısı bu sertifikayı çağrısıyla doğrular.|
|Altyapı ve iş mantığı     |Kaynak sağlayıcıları kendi seçtikleri bir kimlik doğrulama modu kullanarak iş mantığı ve altyapı ile iletişim kurar. Azure yığını ile birlikte gelen varsayılan kaynak sağlayıcıları bu iletişimin güvenliğini sağlamak için Windows kimlik doğrulaması kullanın.|

![Kimlik doğrulaması için gereken bilgileri](media/azure-stack-identity-overview/authentication.png)


### <a name="authenticate-to-azure-resource-manager"></a>Azure kaynak yöneticisi için kimlik doğrulaması
Kimlik sağlayıcısı ile kimlik doğrulaması ve bir JSON Web belirteci almak için aşağıdaki bilgileri olması gerekir: 
1.  **Kimlik sistemi (yetkilisi) URL'sini**: URL, kimlik sağlayıcısı ulaşılabilir. Örneğin,  *https://login.windows.net* . 
2.  **Uygulama Kimliği URI'si için Azure Resource Manager**: Azure kaynak kimlik sağlayıcınız ile kayıtlı Yöneticisi için benzersiz tanımlayıcı. Ayrıca, her Azure yığın yüklemesine de benzersizdir.
3.  **Kimlik bilgileri**: kimlik doğrulaması için kullandığınız kimlik sağlayıcısı ile kimlik bilgileri. 
4.  **URL için Azure Resource Manager**: URL, Azure Kaynak Yöneticisi hizmeti konumdur. Örneğin,  *https://management.azure.com*  veya  *https://management.local.azurestack.external* .

(Bir istemci, uygulama veya kullanıcı) sorumlu bir kaynağa erişmek için kimlik doğrulama isteği yaptığında, istek şunları içermelidir:
- Sorumlunun kimlik bilgileri.
- Uygulama Kimliği URI'SİNİN kaynağının asıl erişmek istiyor. 

Kimlik bilgileri, kimlik sağlayıcısı tarafından doğrulanır. Kimlik sağlayıcısı Ayrıca uygulama kimliği URI'si için kayıtlı bir uygulama olur ve asıl bu kaynak için bir belirteç elde etmek için doğru ayrıcalıklara sahip olduğunu doğrular. İstek geçerliyse, bir JSON Web belirteci verilir. 

Belirteç sonra Azure Resource Manager için bir istek üstbilgisinde geçmesi gerekir. Azure Resource Manager belirli bir sırayla aşağıdakileri yapar:
- Doğrulama *veren* (ISS) talep belirteci doğru kimlik sağlayıcısı'ndan olduğunu doğrulayın. 
- Doğrulama *İzleyici* (aud) talep belirteci Azure Resource Manager için verilmiş olduğunu onaylayın. 
- Doğrulama Azure Resource Manager JSON Web belirteci Openıd ile yapılandırılmış bir sertifika ile imzalandığını denir. 
- Gözden geçirme *zaman* (IAT) ve *sona erme* (exp) belirteç etkin olduğunu doğrulamak talepleri ve kabul edilebilir. 

Tüm doğrulamaları tamamlandığında, Azure Resource Manager kullanan *ait nesneleri* (OID) ve *grupları* asıl erişebileceği kaynakları listesini oluşturmak talep. 

![Belirteç değişimi Protokolü diyagramı](media/azure-stack-identity-overview/token-exchange.png)


### <a name="use-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanma  
Rol tabanlı erişim denetimi (RBAC) Azure yığınında, Microsoft Azure uygulamasında tutarlıdır. Kullanıcılar, gruplar ve uygulamalara uygun RBAC rolü atayarak kaynaklara erişimi yönetebilir. RBAC Azure yığın ile kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
- [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](/azure/active-directory/role-based-access-control-what-is).
- [Azure aboneliği kaynaklarınıza erişimi yönetmek için rol tabanlı erişim denetimi kullanmak](/azure/active-directory/role-based-access-control-configure).
- [Azure rol tabanlı erişim denetimi için özel roller oluşturmanızı](/azure/active-directory/role-based-access-control-custom-roles).
- [Rol tabanlı erişim denetimini yönetmesine](azure-stack-manage-permissions.md) Azure yığınında.


### <a name="authenticate-with-azure-powershell"></a>Azure PowerShell ile kimlik doğrulaması  
Azure yığın ile kimlik doğrulaması için Azure PowerShell kullanma hakkında ayrıntılı bilgi adresinde bulunabilir [Azure yığın kullanıcının PowerShell ortamını yapılandırmak](azure-stack-powershell-configure-user.md).

### <a name="authenticate-with-azure-cli"></a>Azure CLI ile kimlik doğrulaması
Azure yığın ile kimlik doğrulaması için Azure PowerShell'i kullanma hakkında daha fazla bilgi için bkz: [yükleyin ve Azure yığını ile kullanmak için Azure CLI yapılandırma](/azure/azure-stack/user/azure-stack-connect-cli.md).

## <a name="next-steps"></a>Sonraki adımlar
- [Kimlik mimarisi](azure-stack-identity-architecture.md)   
- [Veri Merkezi tümleştirmesi - kimliği](azure-stack-integrate-identity.md)




