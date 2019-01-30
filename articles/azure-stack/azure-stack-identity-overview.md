---
title: Azure Stack için kimlik genel bakış | Microsoft Docs
description: Azure Stack ile kullanabileceğiniz kimlik sistemleri hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/14/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 9fc53fd2539a39de4f01758704765392cc7e98a8
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246975"
---
# <a name="overview-of-identity-for-azure-stack"></a>Azure Stack için kimlik genel bakış

Azure Stack, Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS), Active Directory kimlik sağlayıcısı tarafından desteklenen gerektirir. Sağlayıcı seçimi Azure Stack ilk kez dağıttığınızda oluşturan tek seferlik bir karardır. Kavramlar ve yetkilendirme ayrıntıları bu makalede arasında kimlik sağlayıcıları seçmenize yardımcı olabilir.

Tercih ettiğiniz ya da Azure AD veya AD FS, Azure Stack dağıttığınız modu tarafından belirlenir:

- Bağlı modunda dağıttığınızda, Azure AD veya AD FS kullanabilirsiniz.
- Yalnızca AD FS, bir internet bağlantısı olmadan bağlantısız bir modda dağıttığınızda desteklenir.

Azure Stack ortamınıza bağlıdır, seçenekleriniz hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- Azure Stack Dağıtım Seti: [Kimlik konuları](azure-stack-datacenter-integration.md#identity-considerations).
- Azure Stack tümleşik sistemleri: [Dağıtım Planlama kararları için Azure Stack tümleşik sistemleri](azure-stack-deployment-decisions.md).

## <a name="common-concepts-for-identity"></a>Kimlik için genel kavramlar

Sonraki bölümlerde, kimlik sağlayıcısı ve Azure Stack kullanımları hakkında genel kavramlar açıklanmaktadır.

![Kimlik sağlayıcıları terminolojisi](media/azure-stack-identity-overview/terminology.png)

### <a name="directory-tenants-and-organizations"></a>Dizin kiracıları ve kuruluşlar

Bir dizin hakkında bilgileri tutan bir kapsayıcıdır *kullanıcılar*, *uygulamaları*, *grupları*, ve *hizmet sorumluları*.

Bir dizin kiracısı bir *kuruluş*Microsoft veya kendi şirket gibi.

- Birden çok kiracının Azure AD destekler ve birden çok kuruluş, her biri kendi dizin destekleyebilir. Azure AD'yi kullanın ve birden fazla Kiracı varsa, uygulamaları ve kullanıcıların diğer kiracıları aynı dizinde bir kiracı erişim verebilirsiniz.
- AD FS yalnızca tek bir kiracının destekler ve bu nedenle, yalnızca tek bir kuruluş.

### <a name="users-and-groups"></a>Kullanıcılar ve gruplar

Kullanıcı hesapları (kimlik) kişiler bir kullanıcı kimliği ve parola kullanarak kimlik doğrulaması standart hesaplarıdır. Grupları, kullanıcıları veya diğer grupları ekleyebilirsiniz.

Kullandığınız kimlik çözümü nasıl oluşturabileceğinizi ve kullanıcıları ve grupları yönetmek bağlıdır.

Azure Stack kullanıcı hesapları:

- Oluşturulan *username@domain* biçimi. AD FS bir Active Directory örneğine kullanıcı hesaplarını eşler olsa da, AD FS kullanımını desteklemiyor  *\\ \<etki alanı >\\\<diğer adı >* biçimi.
- Çok faktörlü kimlik doğrulaması kullanacak şekilde ayarlanabilir.
- Kuruluşun dizin nerede bunlar önce kaydetmeniz, dizini için kısıtlanır.
- Şirket içi dizinlerinizi içeri aktarılabilir. Daha fazla bilgi için [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](/azure/active-directory/connect/active-directory-aadconnect).

Kuruluşunuzun Kiracı portalında oturum açtığınızda, kullandığınız *https://portal.local.azurestack.external* URL'si. Azure Stack kaydetmek için kullanılan bir başka etki alanlarından Azure Stack portalında oturum açtığınızda, Azure Stack kaydetmek için kullanılan etki alanı adı portala eklenmesi gereken url. Örneğin, Azure Stack ile Fabrikam.onmicrosoft.com adresli kayıtlı olup olmadığını ve oturum açma kullanıcı hesabı olan admin@contoso.com, kullanıcı portalında oturum açmak için kullanılacak url şu şekilde olacaktır: https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

### <a name="guest-users"></a>Konuk kullanıcılar

Konuk kullanıcıları, dizininizdeki kaynaklara erişim izni verilen diğer directory kiracısından kullanıcı hesaplarıdır. Konuk kullanıcıları desteklemek için Azure AD kullanma ve çok kiracılılık desteğini etkinleştirme. Desteği etkinleştirildiğinde, dış kuruluşlar ile kendi işbirliği sırayla sağlayan directory kiracınızda bulunan kaynaklara erişmek için konuk kullanıcılar davet edebilirsiniz.

Konuk kullanıcıları davet etmek için bulut operatörleri ve kullanıcıları kullanabilirsiniz [Azure AD B2B işbirliği](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Davet edilen kullanıcıları dizininizden belgelerin, kaynakları ve uygulamalara erişim elde ve kendi kaynakları ve veri üzerinde denetim sağlayın. 

Bir Konuk kullanıcı olarak, başka bir kuruluşun directory kiracısı ile oturum açabilir. Bunu yapmak için dizin adı, bir kuruluşun portala ekleme URL'si. Örneğin, Contoso kuruluşa ait ve Fabrikam dizine oturum açmasını istiyorsanız kullanmanız https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

### <a name="applications"></a>Uygulamalar

Uygulamalar Azure AD veya AD FS için kayıt ve ardından kuruluşunuzdaki kullanıcılara uygulamaları sunar.

Uygulamalar şunlardır:

- **Web uygulaması**: Azure portalı ve Azure Resource Manager verilebilir. Bunlar, Web API çağrılarını destekler.
- **Yerel istemci**: Azure PowerShell, Visual Studio ve Azure CLI örnekleri içerir.

Kiralama iki tür uygulamaları destekler:

- **Tek kiracılı**: Kullanıcılar ve hizmetler yalnızca uygulama burada kayıtlı aynı dizinden destekler.

  > [!NOTE]
  > AD FS yalnızca tek bir dizin desteklediğinden, bir AD FS topolojisi oluşturma, tasarım gereği, tek kiracılı uygulamalar uygulamalardır.

- **Çok kiracılı**: Kullanıcılar ve hizmetler burada uygulamanın kayıtlı dizin ve ek Kiracı dizinler destekler kullanın. Çok kiracılı uygulamaları, kullanıcıların başka bir kiracı dizinine (başka bir Azure AD kiracısı) can'ın, uygulamanız için oturum açın. 

  Çok kiracılı modeli hakkında daha fazla bilgi için bkz: [çok kiracılı modeli etkinleştirme](azure-stack-enable-multitenancy.md).

  Çok kiracılı uygulama geliştirme hakkında daha fazla bilgi için bkz. [çok kiracılı uygulamalar](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

Bir uygulamayı kaydettiğinizde, iki nesne oluşturun:

- **Uygulama nesnesi**: Tüm kiracılar genelinde uygulama genel gösterimi. Bu ilişki yazılım uygulama ile bire bir ve yalnızca uygulama ilk kayıtlı olduğu dizinde bulunmaktadır.

- **Hizmet sorumlusu nesnesi**: Uygulama ilk kayıtlı olduğu dizine bir uygulama için oluşturulan bir kimlik bilgisi. Bir hizmet sorumlusu da uygulama kullanıldığı her bir ek Kiracı dizininde oluşturulur. Bu ilişki tek-çok yazılım uygulamayla olabilir.

Uygulama ve hizmet sorumlusu nesneleri hakkında daha fazla bilgi için bkz: [uygulaması ve Azure Active Directory'de Hizmet sorumlusu nesneleri](/azure/active-directory/develop/active-directory-application-objects).

### <a name="service-principals"></a>Hizmet sorumluları

Bir hizmet sorumlusu kümesidir *kimlik bilgilerini* bir uygulama veya Azure Stack'te kaynaklara erişim hizmeti için. Bir hizmet sorumlusu kullanımını kullanıcısının izinleri uygulama izinleri uygulamanın ayırır.

Bir hizmet sorumlusu uygulama kullanıldığı her kiracıda oluşturulur. Hizmet sorumlusu oturum açma ve bu Kiracı tarafından güvenliği sağlanan kaynaklara (örneğin, kullanıcılar) erişimi için bir kimlik oluşturur.

- Tek kiracılı bir uygulama ilk oluşturulduğu dizin içinde yalnızca bir hizmet sorumlusu sahiptir. Bu hizmet sorumlusu oluşturulur ve onay için uygulama kaydı sırasında kullanılan.
- Çok kiracılı web uygulaması veya API her kiracıda oluşturulan hizmet sorumlusu burada bu kiracıda bir kullanıcı, uygulama kullanımını toplanmasına onay verir sahiptir.

Hizmet sorumluları için kimlik bilgileri Azure Portalı aracılığıyla oluşturulan bir anahtar veya bir sertifika olabilir. Sertifikaları anahtarlardan daha güvenli olarak kabul edilir çünkü bir sertifika kullanımını Otomasyon için uygundur. 

> [!NOTE]
> AD FS ile Azure Stack kullandığınızda, yalnızca yönetici hizmet sorumlusu oluşturabilirsiniz. AD FS ile hizmet sorumluları sertifika gerektiren ve ayrıcalıklı uç noktayı (CESARETLENDİRİCİ) oluşturulur. Daha fazla bilgi için [uygulamalar erişim sağlamak için Azure Stack](azure-stack-create-service-principals.md).

Azure Stack için hizmet sorumluları hakkında bilgi edinmek için bkz. [hizmet sorumluları oluşturma](azure-stack-create-service-principals.md).

### <a name="services"></a>Hizmetler

Kimlik sağlayıcısı ile etkileşim kuran Azure Stack Hizmetleri, kimlik sağlayıcısı ile uygulamalar olarak kaydedilir. Uygulamalar gibi kayıt kimlik sistemi ile kimlik doğrulaması bir hizmet sağlar.

Tüm Azure Hizmetleri [Openıd Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) protokolleri ve [JSON Web belirteçlerini](/azure/active-directory/develop/active-directory-token-and-claims) kendi kimliğini oluşturmak için. Çünkü Azure AD ve AD FS kullanma protokolleri tutarlı bir şekilde, kullanabileceğiniz [Azure Active Directory Authentication Library](/azure/active-directory/develop/active-directory-authentication-libraries) şirket içi kimlik doğrulaması (ADAL) veya azure'a (bağlı senaryosunda). ADAL ile Bulutlar arası ve şirket içi kaynak yönetimi için Azure PowerShell ve Azure CLI gibi araçları da kullanabilirsiniz.

### <a name="identities-and-your-identity-system"></a>Kimlikler ve kimlik sisteminizde

Azure Stack için kimlikleri, kullanıcı hesaplarını, grupları ve hizmet sorumlularını içerir.

Kimlik sağlayıcınız dizin kiracısında Azure Stack, birkaç yerleşik uygulamalar ve hizmetler otomatik olarak yüklediğinizde kaydedin. Kayıt bazı hizmetler yönetimi için kullanılır. Diğer Hizmetleri kullanıcıları için kullanılabilir. Varsayılan kayıtları hem birbirleriyle hem de daha sonra eklediğiniz kimlikleri ile etkileşim kurabilir Çekirdek Hizmetleri kimlikleri verin.

Azure AD ile çok kiracılı ayarlarsanız, bazı uygulamaları yeni dizinler yayar.

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme

### <a name="authentication-by-applications-and-users"></a>Uygulamalar ve kullanıcılar tarafından kimlik doğrulaması

![Azure Stack katmanlar arasında kimlik](media/azure-stack-identity-overview/identity-layers.png)

Uygulamalar ve kullanıcılar için Azure Stack mimarisinin dört katmanı tarafından tanımlanır. Bu Katmanlar arasındaki etkileşimleri farklı türde kimlik doğrulaması kullanabilirsiniz.

|Katman    |Katmanlar arasında kimlik doğrulaması  |
|---------|---------|
|Araçlar ve istemciler, Yönetim Portalı gibi     | Erişmek veya Azure Stack'te bir kaynağı değiştirmek için Araçlar ve istemcilerin kullandığı bir [JSON Web belirteci](/azure/active-directory/develop/active-directory-token-and-claims) Azure Resource Manager'a aramak için. <br>Azure Resource Manager JSON Web belirteci doğrular ve bakar *talep* verilen belirteç yetkilendirme düzeyini tahmin etmek için bu kullanıcı veya hizmet sorumlusu Azure Stack'te sahiptir. |
|Azure Resource Manager ve Çekirdek Hizmetleri     |Kaynak sağlayıcıları ile iletişimi kullanıcılardan aktarmak için Azure Resource Manager ile iletişim kurar. <br> Aktarır kullan *doğrudan kesinliği* çağrıları veya *bildirim temelli* aracılığıyla çağırır [Azure Resource Manager şablonları](/azure/azure-stack/user/azure-stack-arm-templates).|
|Kaynak sağlayıcıları     |Kaynak sağlayıcıları için geçirilen çağrıları, sertifika tabanlı kimlik doğrulaması ile korunur. <br>Azure Resource Manager ve kaynak sağlayıcısı sonra bir API aracılığıyla iletişimde kalın. Azure Resource Manager'dan alınan her çağrı için kaynak sağlayıcıya çağrı bu sertifika ile doğrular.|
|Altyapı ve iş mantığı     |Kaynak sağlayıcıları iş mantığı ve altyapı ile kendi tercih ettiğiniz bir kimlik doğrulama modunu kullanarak iletişim kurar. Azure Stack ile birlikte gelen varsayılan kaynak sağlayıcıları, bu iletişimin güvenliğini sağlamak için Windows kimlik doğrulaması kullanın.|

![Kimlik doğrulaması için gereken bilgileri](media/azure-stack-identity-overview/authentication.png)

### <a name="authenticate-to-azure-resource-manager"></a>Azure Resource Manager'a kimlik doğrulaması

Kimlik sağlayıcı ile kimlik doğrulaması ve bir JSON Web belirteci almak için aşağıdaki bilgilere sahip olmalıdır:

1. **Kimlik sistemi (yetkili) URL'sini**: Kimlik sağlayıcınız ulaşılabilir URL'si. Örneğin, *https://login.windows.net*.
2. **Uygulama Kimliği URI'si için Azure Resource Manager**: Azure kaynak kimliği sağlayıcınızdan kayıtlı Yöneticisi için benzersiz tanımlayıcısı. Ayrıca, her Azure Stack yüklemesine de benzersizdir.
3. **kimlik bilgileri**: Kimlik bilgisi, kimlik sağlayıcısı ile kimlik doğrulaması yapmak için kullanın.
4. **Azure Resource Manager için URL**: Azure Resource Manager hizmet konumu URL'dir. Örneğin, *https://management.azure.com* veya *https://management.local.azurestack.external*.

(Bir istemci, uygulama veya kullanıcı) sorumlu bir kaynağa erişmek için kimlik doğrulama isteği yaptığında, istek şunları içermelidir:

- Sorumlunun kimlik bilgileri.
- Uygulama Kimliği URI'SİNİN erişmeye sorumlusunu istediği kaynak.

Kimlik sağlayıcısı tarafından kimlik bilgilerinin doğrulanır. Kimlik sağlayıcısı ayrıca kayıtlı bir uygulama için uygulama kimliği URI'si olduğunu ve asıl bu kaynak için bir belirteç almak için doğru ayrıcalıklara sahip olduğunu doğrular. İstek geçerliyse bir JSON Web belirteci verilir.

Belirteç, ardından Azure Resource Manager için bir istek üst bilgisinde geçmelidir. Azure Resource Manager, belirli bir sıraya göre aşağıdakileri yapar:

- Doğrulama *veren* (ISS) talep belirteci doğru kimlik sağlayıcısından olduğunu doğrulayın.
- Doğrulama *İzleyici* (aud) talep belirteci Azure Resource Manager'a verildiğini doğrulamak için.
- Doğrulama için Azure Resource Manager JSON Web belirteci Openıd ile yapılandırılmış bir sertifikasıyla imzalanmasını bilinir.
- Gözden geçirme *zaman* (IAT) ve *sona erme* (exp) belirteci etkin olduğundan emin olmak talepleri ve kabul edilebilir.

Tüm Doğrulamalar tamamlandığında, Azure Resource Manager kullanan *eşitlemek* (OID) ve *grupları* talep sorumlusu erişebildiği kaynakları listesini oluşturmak.

![Belirteç değişimi Protokolü diyagramı](media/azure-stack-identity-overview/token-exchange.png)

> [!NOTE]
> Dağıtımdan sonra Azure Active Directory genel yönetici izni gerekli değildir. Ancak, bazı işlemler, genel yönetici kimlik bilgileri gerektirebilir. Örneğin, bir kaynak sağlayıcısı yükleyicisi betiği veya izin verilecek gerektiren yeni bir özelliktir. Geçici olarak hesap genel yönetici izinleri yeniden geri veya sahiplerinden biri olan ayrı bir genel yönetici hesabı kullanın *varsayılan sağlayıcı aboneliği*.

### <a name="use-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanma

Rol tabanlı erişim denetimi (RBAC), Azure Stack, Microsoft azure'da uygulama ile tutarlıdır. Kaynaklara erişim için kullanıcılara, gruplara ve uygulamalara uygun RBAC rolü atayarak yönetebilirsiniz.
Azure Stack ile RBAC kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama](/azure/role-based-access-control/overview).
- [Azure abonelik kaynaklarınıza erişimi yönetmek için rol tabanlı erişim denetimi kullanan](/azure/role-based-access-control/role-assignments-portal).
- [Azure rol tabanlı erişim denetimi için özel roller oluşturma](/azure/role-based-access-control/custom-roles).
- [Rol tabanlı erişim denetimini yönetme](azure-stack-manage-permissions.md) Azure Stack'te.

### <a name="authenticate-with-azure-powershell"></a>Azure PowerShell ile kimlik doğrulaması

Azure Stack ile kimlik doğrulaması için Azure PowerShell kullanma hakkında ayrıntılı bilgi bulunabilir [Azure Stack kullanıcının PowerShell ortamını yapılandırmak](azure-stack-powershell-configure-user.md).

### <a name="authenticate-with-azure-cli"></a>Azure CLI ile kimlik doğrulaması

Azure Stack ile kimlik doğrulaması için Azure PowerShell kullanma hakkında daha fazla bilgi için bkz: [yüklemek ve Azure Stack ile kullanmak için Azure CLI yapılandırma](/azure/azure-stack/user/azure-stack-connect-cli).

## <a name="next-steps"></a>Sonraki adımlar

- [Kimlik mimarisi](azure-stack-identity-architecture.md)
- [Veri Merkezi tümleştirmesi - kimlik](azure-stack-integrate-identity.md)
