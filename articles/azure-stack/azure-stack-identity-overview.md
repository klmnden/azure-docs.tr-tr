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
ms.openlocfilehash: 609ea3300806f3cfed4a3285a096f59be491c684
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="overview-of-identity-for-azure-stack"></a>Azure yığın kimliğini genel bakış

Active Directory federe Hizmetleri (AD FS) tarafından Active Directory (AD) kimlik sağlayıcısı desteklenen veya Azure AD Azure yığın gerektirir. Aşağıdaki kavramlar ve yetkilendirme ayrıntıları kimlik sağlayıcılar arasında seçim yapmanıza yardımcı olabilir. Bir sağlayıcı seçimi Azure yığın ilk dağıttığınızda yaptığınız tek seferlik bir karardır.  

Tercih ettiğiniz Azure arasında AD ve AD FS sınırlı Azure yığın dağıtmak modu tarafından: 
- Bağlı bir modda dağıttığınızda, Azure AD veya AD FS kullanabilirsiniz. 
- Internet bağlantısı olmadan bağlantısız modda dağıttığınızda, yalnızca AD FS desteklenir.

Bu seçenekler hakkında daha fazla bilgi için Azure yığın ortamınıza bağlı olarak aşağıdaki makalelere bakın:
- Bkz: Azure yığın Deployment Kit [kimlik konuları](azure-stack-datacenter-integration.md#identity-considerations).
- Azure tümleşik yığını sistemleri Bkz [dağıtım planlama kararları Azure yığınının tümleşik sistemleri](azure-stack-deployment-decisions.md).

 
## <a name="common-concepts-for-identity"></a>Kimliği için ortak kavramlar
Aşağıdaki bölümlerde, kimlik sağlayıcısı ve Azure yığınında bunların kullanılması için ortak kavramlar açıklanmaktadır.
![Kimlik sağlayıcılar terminolojisi](media/azure-stack-identity-overview/terminology.png)


### <a name="directories-tenants-and-organizations"></a>Dizinleri kiracılar ve kuruluşlar
Bir dizin hakkında bilgileri tutan bir kapsayıcıdır *kullanıcılar*, *uygulamaları*, *grupları*, ve *hizmet sorumluları*.
 
Bir dizin Kiracı bir *kuruluş*Microsoft veya kendi şirket gibi. 
- Azure AD birden çok kiracıya destekler ve birden çok kuruluş, her kendi dizininde destekleyebilir. Azure AD kullanın ve birden çok kiracıya varsa, uygulamaları ve diğer kiracılar aynı dizin, bir kiracı erişimden kullanıcılara erişim izni verebilir.
- AD FS, yalnızca tek bir kiracı ve bu nedenle tek bir kurumun destekler. 

### <a name="users-and-groups"></a>Kullanıcılar ve gruplar
Kullanıcı hesapları (Kimlikler) kişiler bir kullanıcı kimliği ve parola kullanarak kimlik doğrulaması standart hesaplarıdır. Grupları, kullanıcıları veya diğer grupları dahil edebilirsiniz. 

Kullandığınız kimlik çözümü nasıl oluşturmak ve kullanıcıları ve grupları yönetin bağlıdır. 

Azure yığınında kullanıcı hesapları: 
- Oluşturulan  *username@domain*  biçimi. AD FS bir Active Directory kullanıcı hesapları eşlemeleri rağmen AD FS kullanımını desteklemez  _&lt;etki alanı >\<diğer adı >_ biçimi. 
- Çok faktörlü kimlik doğrulaması kullanacak şekilde ayarlanabilir. 
- Kuruluşların dizinlerini olan burada bunlar ilk kaydetmek, dizine kısıtlanır.
- Şirket içi dizinlerinizi içeri aktarılabilir. Daha fazla bilgi için bkz: [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](/azure/active-directory/connect/active-directory-aadconnect) Azure belgelerinde.  

Kuruluşların Kiracı portalında oturum açın, https://portal.local.azurestack.external kullanın. 

### <a name="guest-users"></a>Konuk kullanıcılar
Konuk kullanıcılar, diğer dizin kiracıdan dizininizde kaynaklara erişim izni verilen kullanıcı hesaplarıdır. Konuk kullanıcı desteklemek için Azure AD kullanın ve çoklu kiracı desteğini etkinleştirmeniz gerekir. Desteklendiğinde, dış kuruluşlarla işbirliği etkinleştirir directory kiracınızda bulunan kaynaklara erişmek için Konuk kullanıcı davet edebilirsiniz. 

Bulut operatörleri ve kullanıcıların Konuk kullanıcılar davet etmek için sonuna kullanabilirsiniz [Azure AD B2B işbirliği](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Kendi kaynakları ve veri üzerinde denetim bakım yaparken dizininizden kullanıcılara get erişim belgeleri, kaynaklar ve uygulamalar için davet. 

Konuk kullanıcı, başka bir kuruluş dizin Kiracı oturum açabilir. Bunu yapmak için kuruluşlar dizin adı portalına eklemeniz gerekir URL.  Örneğin contoso.com etki alanına ait, ancak Fabrikam dizine günlüğe kaydetmek istediğiniz https://portal.local.azurestack.external/fabrikam.onmicrosoft.com kullanın.

### <a name="applications"></a>Uygulamalar
Uygulamaları Azure AD veya AD FS için kaydolun ve bunları, kuruluşunuzdaki kullanıcılara sunmak. 

Uygulamalar şunlardır:
- **Web uygulaması** – örnekler Azure portalı ve Azure Resource Manager.  Web API çağrıları destekledikleri. 
- **Yerel istemci** – örnekler Azure PowerShell, Visual Studio ve Azure komut satırı arabirimi (CLI).

Kiracı iki tür uygulamaları destekler: 
- **Tek Kiracı** uygulamalar, kullanıcıların ve hizmetlerin yalnızca, uygulama kayıtlı aynı dizinden destekler. 

  > [!NOTE]    
  > AD FS yalnızca tek bir dizin desteklediğinden, bir AD FS topolojisinde oluşturduğunuz tasarım gereği, tek Kiracı uygulamaları uygulamalardır.
- **Çok kiracılı** uygulamaları desteği kullanarak kullanıcıların ve hizmetlerin, uygulama kayıtlı her iki dizininden ve ek Kiracı dizinleri.  Çok kiracılı uygulamalarla başka bir kiracı directory (başka bir Azure AD kiracısı) can kullanıcılarının uygulamanıza oturum açın.     

  Çoklu kiracı hakkında daha fazla bilgi için bkz: [etkinleştirmek çoklu kiracı](azure-stack-enable-multitenancy.md).  

  Çok kiracılı uygulama geliştirme hakkında daha fazla bilgi için bkz: [çok kiracılı uygulamalara](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

Bir uygulamayı kaydetme, iki nesne oluşturulur:
- **Uygulama nesnesi** – uygulama nesnesi genel uygulama tüm kiracılar gösterimidir. Bu ilişki yazılım uygulamasıyla bire bir ve yalnızca uygulama ilk yazmaçlar bulunduğu dizini yok.

 
   
- **Hizmet sorumlusu nesnesi** – bir hizmet sorumlusu, uygulamanın ilk kayıtlı dizininde bir uygulama için oluşturulan bir kimlik bilgisi değil. Bir hizmet sorumlusu da bu uygulama kullanıldığı her bir ek Kiracı dizinde oluşturulur. Bu ilişki bir bir-çok yazılım uygulamaya sahip olabilir.   

Uygulama ve hizmet asıl nesneleri hakkında daha fazla bilgi için bkz: [uygulama ve hizmet asıl nesneler Azure Active Directory'de (Azure AD)](/azure/active-directory/develop/active-directory-application-objects). 

### <a name="service-principals"></a>Hizmet asıl adı 
Bir hizmet sorumlusu kümesidir *kimlik bilgileri* bir uygulama veya Azure yığınında kaynaklara erişim izni hizmet için. Bir hizmet sorumlusu kullanımını uygulamanın kullandığı kullanıcı izinlerini uygulama izinleri ayırır.

Bir hizmet sorumlusu uygulama kullanıldığı her bir kiracı içinde oluşturulur. Hizmet sorumlusu oturum açma ve bu Kiracı tarafından güvenliği sağlanan kaynaklarına (ör. kullanıcılar) erişimi için bir kimlik oluşturur.   

- Tek kiracılı uygulama ilk oluşturulduğu dizinde yalnızca bir hizmet sorumlusu sahiptir.  Bu hizmet sorumlusu oluşturulur ve uygulama kaydı sırasında kullanılacak izin. 
- Bir çok kiracılı Web uygulaması veya API her bir kiracı oluşturulan bir hizmet sorumlusu burada uygulamayı kullanmak için bir kiracı kullanıcıdan izin vardır.  

Hizmet asıl adı için kimlik bilgilerini (Azure portalı üzerinden oluşturulur) anahtar veya bir sertifika ya da olabilir.  Sertifikaları anahtarları kullanımını daha fazla güvenli kabul edildiği için bir sertifika kullanımını Otomasyon için uygundur. 


> [!NOTE]    
> AD FS ile Azure yığın kullandığınızda, yalnızca yönetici hizmet asıl adı oluşturabilirsiniz. AD FS ile hizmet asıl adı sertifika gerektiren ve ayrıcalıklı uç noktası aracılığıyla (CESARETLENDİRİCİ) oluşturulur. Daha fazla bilgi için bkz: [Azure yığın uygulama erişim sağlayan](azure-stack-create-service-principals.md).

Azure yığınının hizmet sorumluları hakkında bilgi edinmek için [hizmet sorumluları oluşturmak](azure-stack-create-service-principals.md).


### <a name="services"></a>Hizmetler
Kimlik sağlayıcısı ile etkileşim Hizmetleri Azure yığınında kimlik sağlayıcısı ile uygulamalar olarak kayıtlı. Uygulamalar gibi kimlik sistemi ile kimlik doğrulaması bir hizmet kaydını etkinleştirir. 

Tüm Azure hizmetlerine [Openıd Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) protokolleri ve [JSON Web belirteçlerini](/azure/active-directory/develop/active-directory-token-and-claims) kendi kimliğini oluşturmak için (JWT). Azure AD tarafından kurallarının tutarlı kullanımı nedeniyle ve AD FS, Azure kullanabilir [Active Directory kimlik doğrulama Kitaplığı](/azure/active-directory/develop/active-directory-authentication-libraries) (şirket içi kimlik doğrulaması için ADAL) veya Azure (bir senaryoda bağlı). ADAL ayrıca Azure PowerShell ve CLI gibi araçlar için çapraz bulut kullanmanıza olanak tanır ve şirket içi kaynak yönetimi. 


### <a name="identities-and-your-identity-system"></a>Kimlik ve kimlik sistemi 
Azure yığını için kimlikleri, kullanıcı hesaplarını, grupları ve hizmet asıl adı içerir. Azure yığın yüklediğinizde, birkaç yerleşik uygulamaları ve Hizmetleri otomatik olarak directory kiracısında, kimlik sağlayıcısı ile kaydedin. Kayıt bazı hizmetler yönetimi için kullanılır. Diğer hizmetler kullanıcıları için kullanılabilir. Varsayılan kayıtlar birbiriyle etkileşim kurabilen Çekirdek Hizmetleri kimlikleri verin ve daha sonra kimliklerle ekleyin.
Azure AD çoklu kiracı ile ayarladıysanız, bazı uygulamaları yeni dizinler yayılır.  

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme
 

### <a name="authentication-by-applications-and-users"></a>Uygulamalar ve kullanıcılar tarafından kimlik doğrulaması
  
![Azure yığınının katmanlar arasında kimlik](media/azure-stack-identity-overview/identity-layers.png)

Uygulamalar ve kullanıcılar için Azure yığın mimarisi dört katmanı tarafından tanımlanır. Bu katmanlardan her arasındaki etkileşimler farklı tür kimlik doğrulaması kullanabilirsiniz.


|Katman    |Katmanlar arasında kimlik doğrulaması  |
|---------|---------|
|Araçlar ve istemcileri gibi Yönetici portalı     | Erişmek veya kaynak Azure yığınında değiştirmek için Araçlar ve istemcilerin kullandığı bir [JSON Web belirteçlerini](/azure/active-directory/develop/active-directory-token-and-claims) Azure Resource Manager için bir çağrı yerleştirmek için (JWT).  <br><br> Azure Resource Manager JWT değerini doğrular ve iletiye göz atar *talep* verilen belirteç yetkilendirme düzeyini tahmin etmek için o kullanıcı ya da hizmet asıl Azure yığına sahip.        |
|Azure Kaynak Yöneticisi ve Çekirdek Hizmetleri     |Azure Resource Manager iletişim kullanıcılardan aktarmak için kaynak sağlayıcıları ile iletişim kurar. <br><br> Aktarır kullanım *doğrudan kesinliği* çağrıları veya *bildirim temelli* aracılığıyla çağırır [Azure Resource Manager şablonları](/azure/azure-stack/user/azure-stack-arm-templates.md).|
|Kaynak Sağlayıcılar     |Kaynak sağlayıcıları için geçirilen çağrıları sertifika tabanlı kimlik doğrulaması ile güvenli hale getirilir. <br><br>Azure Resource Manager ve kaynak sağlayıcısı, bir API aracılığıyla iletişim sonra kalır. Azure Kaynak Yöneticisi'nden alınan her çağrı için kaynak sağlayıcısı bu sertifikayı çağrısıyla doğrular.|
|Altyapı ve iş mantığı     |Kaynak sağlayıcıları kendi seçtikleri bir kimlik doğrulama modu kullanarak iş mantığı ve altyapı ile iletişim kurar. Varsayılan kaynak sağlayıcıları Azure yığını ile birlikte, bu iletişimin güvenliğini sağlamak için Windows kimlik doğrulaması kullanın.|

![Kimlik doğrulaması için gereken bilgileri](media/azure-stack-identity-overview/authentication.png)


### <a name="authenticate-to-azure-resource-manager"></a>Azure kaynak yöneticisi için kimlik doğrulaması
Kimlik sağlayıcısı ile kimlik doğrulaması ve JWT almak için aşağıdaki bilgileri olması gerekir: 
1.  **Kimlik sistemi (yetkilisi) URL'sini** – hangi kimlik sağlayıcınızı ulaşılabilir URL. Örneğin,  *&lt;https://login.windows.net>*. 
2.  **Uygulama Kimliği URI'si için Azure Resource Manager** – Azure kaynak kimlik sağlayıcısı ile kaydedilen ve her Azure yığın yükleme için benzersiz olan Yöneticisi için benzersiz tanımlayıcı.
3.  **Kimlik bilgileri** – bu kullanmakta olduğunuz kimlik doğrulaması için kimlik sağlayıcısı ile kimlik bilgileri.  
4.  **URL için Azure Resource Manager** – URL, Azure Kaynak Yöneticisi hizmeti konumdur. Örneğin,  *&lt;https://management.azure.com>* veya  *&lt;https://management.local.azurestack.external>*.

(Bir istemci, uygulama veya kullanıcı) sorumlu bir kaynağa erişmek için kimlik doğrulama isteği yaptığında, bu istek şunları içermelidir:
- Sorumlunun kimlik bilgileri.
- Erişmek istediğiniz kaynağı uygulama kimliği URI'si.  

Kimlik bilgileri, kimlik sağlayıcısı tarafından doğrulanır. Kimlik sağlayıcısı Ayrıca uygulama kimliği URI'si kayıtlı bir uygulama için olduğunu ve asıl bu kaynak için bir belirteç almak üzere doğru ayrıcalıklara sahip olduğunu doğrular. İstek geçerliyse, bir JSON Web belirteci verilir. 

Belirteç sonra Azure Resource Manager için bir istek üstbilgisinde geçmesi gerekir. Azure Resource Manager belirli bir sırayla şu doğrulamaların yapar:
- Doğrulama *veren* (ISS) talep belirteci doğru kimlik sağlayıcısı'ndan olduğunu doğrulayın. 
- Doğrulama *İzleyici* (aud) talep belirteci için Azure Resource Manager verilmiş olduğunu onaylayın. 
- JWT Openıd yapılandırılmış ve Azure Resource Manager için bilinen bir sertifika ile imzalı olduğunu doğrulayın. 
- Gözden geçirme *zaman* (IAT) ve *süre sonu* (exp) belirteç etkin olduğunu doğrulamak talepleri ve kabul edilebilir. 

Tüm doğrulamaları tamamlandığında, Azure Resource Manager kullanan *ait nesneleri* (OID) ve *grupları* asıl erişebileceği kaynakları listesini oluşturmak talep. 

![Belirteç değişimi Protokolü diyagramı](media/azure-stack-identity-overview/token-exchange.png)


### <a name="use-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanma  
Rol tabanlı erişim denetimi (RBAC) Azure yığınında, Microsoft Azure uygulamasında tutarlıdır.  Kullanıcılar, gruplar ve uygulamalara uygun RBAC rolü atayarak kaynaklara erişimi yönetebilir. RBAC Azure yığın ile kullanma hakkında daha fazla bilgi için aşağıdaki makaleleri gözden geçirin:
- [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](/azure/active-directory/role-based-access-control-what-is).
- [Azure aboneliği kaynaklarınıza erişimi yönetmek için rol tabanlı erişim denetimi kullanmak](/azure/active-directory/role-based-access-control-configure).
- [Azure rol tabanlı erişim denetimi için özel roller oluşturmanızı](/azure/active-directory/role-based-access-control-custom-roles).
- [Rol tabanlı erişim denetimini yönetmesine](azure-stack-manage-permissions.md) Azure yığınında.


### <a name="authenticate-with-azure-powershell"></a>Azure PowerShell ile kimlik doğrulaması  
Azure yığın ile kimlik doğrulaması için Azure PowerShell kullanma hakkında ayrıntılı bilgi adresinde bulunabilir [Azure yığın kullanıcının PowerShell ortamını yapılandırmak](azure-stack-powershell-configure-user.md).

### <a name="authenticate-with-azure-cli"></a>Azure CLI ile kimlik doğrulaması
Azure yığın ile kimlik doğrulaması için Azure PowerShell kullanma hakkında ayrıntılı bilgi adresinde bulunabilir [yükleyin ve Azure yığını ile kullanmak için CLI yapılandırma](/azure/azure-stack/user/azure-stack-connect-cli.md).

## <a name="next-steps"></a>Sonraki adımlar
- [Kimlik mimarisi](azure-stack-identity-architecture.md)   
- [Veri Merkezi tümleştirmesi - kimliği](azure-stack-integrate-identity.md)




