---
title: GRANT B2B kullanıcılar Azure AD'de erişim, şirket içi uygulamalarınızı | Microsoft Docs
description: Azure AD B2B işbirliği ile şirket içi uygulamalara bulut B2B kullanıcılara erişim vermek gösterilmiştir.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/20/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: f34bb9eaa04491dfbef8fac711690d1b19677d89
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications"></a>Şirket içi uygulamalarınıza Azure AD erişim ver B2B kullanıcılar

Azure AD iş ortağı kuruluşlardan konuk kullanıcılara davet etmek için Azure Active Directory (Azure AD) B2B işbirliği özelliklerinden kullanan kuruluş olarak, artık bu B2B kullanıcılara şirket içi uygulamalara erişim sağlayabilir. Bu şirket içi uygulamaları Kerberos Kısıtlı temsilci (KCD) ile SAML tabanlı kimlik doğrulaması veya tümleşik Windows kimlik doğrulaması (IWA) kullanabilirsiniz.

## <a name="access-to-saml-apps"></a>SAML uygulamalara erişim

Şirket içi uygulamanız SAML tabanlı kimlik doğrulaması kullanıyorsa, kolayca bu uygulamaları kullanılabilir Azure Portalı aracılığıyla Azure AD B2B işbirliği kullanıcılarınıza yapabilirsiniz.

Şunların ikisini de yapmanız gerekir:

- SAML uygulama Galerisi olmayan uygulaması şablonu kullanılarak açıklandığı gibi tümleştirmek [Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açma yapılandırma](../active-directory-saas-custom-apps.md). İçin kullandığınız not emin olun **oturum açma URL'si** değeri.
-  İle şirket içi uygulama yayımlamak için Azure AD uygulama proxy'si kullanma **Azure Active Directory** kimlik doğrulaması kaynağı olarak yapılandırılmış. Yönergeler için bkz: [Azure AD uygulama proxy'si ile uygulama yayımlama](../manage-apps/application-proxy-publish-azure-portal.md). 

   Yapılandırdığınızda **iç Url** ayarı, Galeri olmayan uygulama şablonunda belirtilen oturum açma URL'si kullanın. Bu şekilde, kullanıcılar uygulama kuruluş sınır dışında erişebilir. Uygulama proxy'si SAML çoklu oturum açma için şirket içi uygulama gerçekleştirir.
 
   ![Şirket içi uygulama ayarları iç URL ve kimlik doğrulama gösterir](media/hybrid-cloud-to-on-premises/OnPremAppSettings.PNG)

## <a name="access-to-iwa-and-kcd-apps"></a>IWA ve KCD uygulamalara erişim

B2B kullanıcıların tümleşik Windows kimlik doğrulaması ve Kerberos Kısıtlı temsilci ile güvenliği sağlanan şirket içi uygulamalara erişim sağlamak için aşağıdaki bileşenler gerekir:

- **Azure AD uygulama proxy'si aracılığıyla kimlik doğrulaması**. B2B kullanıcıların şirket içi uygulamaya kimlik doğrulaması için olması gerekir. Bunu yapmak için Azure AD uygulama proxy'si aracılığıyla şirket içi uygulama yayımlamanız gerekir. Daha fazla bilgi için bkz: [uygulama ara sunucusu ile başlayın ve Bağlayıcısı'nı yüklemek](../manage-apps/application-proxy-enable.md) ve [Azure AD uygulama proxy'si ile uygulama yayımlama](../manage-apps/application-proxy-publish-azure-portal.md).
- **Şirket içi dizin B2B kullanıcı nesnesinde aracılığıyla yetkilendirme**. Uygulamanın kullanıcı erişim denetimleri gerçekleştirmek ve doğru kaynaklara erişim izni olması gerekir. IWA ve KCD şirket içi Windows Server Active Directory'de bu yetkilendirme tamamlamak için bir kullanıcı nesnesi gerektirir. Bölümünde açıklandığı gibi [nasıl çoklu oturum açma KCD works ile](../manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#how-single-sign-on-with-kcd-works), uygulama Proxy kullanıcının kimliğine bürün ve uygulama için bir Kerberos belirteci edinmek için bu kullanıcı nesnesi gerekir. 

   B2B kullanıcı senaryosu için şirket içi dizin yetkilendirme için gerekli olan Konuk kullanıcı nesneleri oluşturmak için kullanabileceğiniz iki yöntem vardır:

   - Microsoft Identity Manager (MIM) ve [MIM yönetim Aracısı Microsoft Graph için](#create-b2b-guest-user-objects-through-mim-preview). 
   - [Bir PowerShell Betiği](#create-b2b-guest-user-objects-through-a-script-preview). Komut dosyası kullanarak MIM gerektirmeyen daha basit bir çözümdür. 

Aşağıdaki diyagramda birlikte B2B kullanıcıların şirket içi IWA ve KCD uygulamalara erişim vermek için dizin iş nasıl Azure AD uygulama proxy'si üst düzey bir genel bakış ve şirket içi B2B kullanıcı nesnesinin oluşturulmasını sağlar. Aşağıdaki diyagramda ayrıntılı numaralı adımlar açıklanmaktadır.

![MIM ve B2B betik çözümleri diyagramı](media/hybrid-cloud-to-on-premises/MIMScriptSolution.PNG)

1.  Bir kullanıcı bir iş ortağı kuruluştan (Fabrikam Kiracı) Contoso Kiracı için davet.
2.  Bir Konuk kullanıcı nesnesi Contoso Kiracı içinde oluşturulur (örneğin, bir kullanıcı nesnesi guest_fabrikam.com#EXT# UPN ile@contoso.onmicrosoft.com).
3.  Fabrikam Konuk contoso B2B PowerShell Betiği veya MIM yoluyla alınır.
4.  Bir gösterimi veya Fabrikam Konuk kullanıcı nesnesinin (konuk EXT #) "alanını" Contoso.com, MIM veya B2B PowerShell komut dosyası aracılığıyla şirket içi dizin oluşturulur.
5.  Konuk kullanıcı erişim şirket içi uygulama, app.contoso.com.
6.  Kimlik doğrulama isteği kısıtlı Kerberos temsilcisi kullanarak uygulama proxy'si aracılığıyla yetkili. 
7.  Konuk kullanıcı nesnesi yerel olarak mevcut olduğundan, kimlik doğrulama başarılı olur.

### <a name="lifecycle-management-policies"></a>Yaşam döngüsü yönetimi ilkeleri

Şirket içi B2B kullanıcı nesneleri yaşam döngüsü yönetimi ilkeleri aracılığıyla yönetebilirsiniz. Örneğin:

- Uygulama proxy'si kimlik doğrulaması sırasında MFA kullanılmasını sağlamak amacıyla Konuk kullanıcı için çok faktörlü kimlik doğrulaması (MFA) ilkeleri ayarlayabilirsiniz. Daha fazla bilgi için bkz: [B2B işbirliği kullanıcılar için koşullu erişim](conditional-access.md).
- Tüm sponsorships, erişim incelemeleri, hesap Doğrulamalar, kullanıcı, şirket içi kullanıcılar için geçerlidir. bulut B2B üzerinde gerçekleştirilen vb. Örneğin, bulut kullanıcı yaşam döngüsü yönetimi ilkelerinizi silinirse, şirket içi kullanıcı de MIM eşitleme veya Azure AD Connect eşitleme yoluyla silinir. Daha fazla bilgi için bkz: [Azure AD erişim Konuk erişimi yönetme incelemeleri](../active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md).

### <a name="create-b2b-guest-user-objects-through-mim-preview"></a>MIM (Önizleme) aracılığıyla B2B Konuk kullanıcı nesneleri oluşturma

MIM 2016 Service Pack 1 ve MIM yönetim Aracısı Microsoft Graph için şirket içi dizinde Konuk kullanıcı nesneleri oluşturmak için nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Microsoft Identity ile Azure AD işletmeden işletmeye (B2B) işbirliği Azure uygulama proxy'si (genel Önizleme) Manager (MIM) 2016 SP1](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-graph-b2b-scenario).

### <a name="create-b2b-guest-user-objects-through-a-script-preview"></a>Komut dosyası (Önizleme) aracılığıyla B2B Konuk kullanıcı nesneleri oluşturma

Şirket içi Active Directory Konuk kullanıcı nesneleri oluşturmak için bir başlangıç noktası olarak kullanabileceğiniz bir PowerShell örnek betiği yoktur.

Komut dosyası ve benioku dosyasından yükleyebilirsiniz [Yükleme Merkezi'nden](https://www.microsoft.com/download/details.aspx?id=51495). Seçin **komut dosyası ve Benioku dosyasını Azure AD B2B kullanıcılar üzerinde-prem.zip çekmesini** dosya.

Komut dosyası kullanmadan önce ilişkili benioku dosyasında önemli noktalar ve önkoşulları gözden emin olun. Ayrıca, komut dosyası yalnızca bir örnek olarak sunulacağını anlayın. Geliştirme ekibiniz ya da bir iş ortağı gerekir özelleştirebilir ve çalıştırmadan önce komut dosyasını gözden geçirin.

## <a name="license-considerations"></a>Lisans konuları

Şirket içi uygulamalara erişim dış Konuk kullanıcılar için doğru istemci erişim lisansları (CAL) sahip olduğunuzdan emin olun. Daha fazla bilgi için "Dış bağlayıcılar" bölümüne bakın [istemci erişim lisansları ve yönetim lisansları](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx). Microsoft temsilcinize başvurun veya yerel bayinizle ilgili lisans gereksinimlerinizi.

## <a name="next-steps"></a>Sonraki adımlar

- [Karma kuruluşlar için Azure Active Directory B2B işbirliği](hybrid-organizations.md)

- Azure AD Connect genel bakış için bkz: [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../connect/active-directory-aadconnect.md).

