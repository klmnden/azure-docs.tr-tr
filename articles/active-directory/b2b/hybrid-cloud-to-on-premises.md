---
title: GRANT B2B kullanıcıları, şirket içi uygulamalarınıza - Azure Active Directory erişim | Microsoft Docs
description: Bulut üzerinde Azure AD B2B işbirliği ile şirket içi uygulama B2B kullanıcılara erişim izni vermek gösterilmektedir.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0e9536f009d478796bc9367a66630c02019dcd2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60412721"
---
# <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications"></a>GRANT B2B kullanıcıları Azure AD'de, şirket içi uygulamalarınıza erişim

Azure ad iş ortağı kuruluşlardan Konuk kullanıcıları davet etmek için Azure Active Directory (Azure AD) B2B işbirliği özelliklerini kullanan bir kuruluş, artık bu B2B kullanıcıları şirket içi uygulamalara erişim sağlayabilir. Bu şirket içi uygulamaları, Kerberos Kısıtlı temsilci (KCD) ile SAML tabanlı kimlik doğrulaması veya tümleşik Windows kimlik doğrulaması (IWA) kullanabilirsiniz.

## <a name="access-to-saml-apps"></a>SAML uygulamalara erişim

Şirket içi uygulamanız SAML tabanlı kimlik doğrulaması kullanıyorsa, kolayca bu uygulamaları kullanılabilir Azure portalından Azure AD B2B işbirliği kullanıcılarınıza yapabilirsiniz.

Şunların ikisini de yapmalısınız:

- SAML galeri dışı uygulaması şablonu kullanılarak açıklandığı uygulamalarıyla [Azure Active Directory Uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açma yapılandırma](../manage-apps/configure-single-sign-on-non-gallery-applications.md). İçin kullandığınız not aldığınızdan emin olun **oturum açma URL'si** değeri.
-  Azure AD uygulama ara sunucusu ile şirket içi uygulama yayımlamak için kullanmak **Azure Active Directory** kimlik kaynağı olarak yapılandırılır. Yönergeler için [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](../manage-apps/application-proxy-publish-azure-portal.md). 

   Yapılandırdığınızda **iç Url** ayarı, Galeri dışı uygulama şablonunda belirtilen oturum açma URL'sini kullanın. Bu şekilde, kullanıcıların kuruluş sınırının dışında uygulamadan erişebilirsiniz. Uygulama proxy'si şirket içi uygulama için SAML çoklu oturum açma gerçekleştirir.
 
   ![Şirket içi uygulama ayarları iç URL ve kimlik doğrulaması gösterir](media/hybrid-cloud-to-on-premises/OnPremAppSettings.PNG)

## <a name="access-to-iwa-and-kcd-apps"></a>IWA ve KCD uygulamalara erişim

B2B kullanıcıları tümleşik Windows kimlik doğrulaması ve Kerberos kısıtlanmış temsili ile güvenliği sağlanan şirket içi uygulamalara erişim sağlamak için aşağıdaki bileşenleri gerekir:

- **Azure AD uygulama ara sunucusu üzerinden kimlik doğrulaması**. B2B kullanıcıları şirket içi uygulamaya kimlik doğrulaması için olmalıdır. Bunu yapmak için şirket içi uygulama Azure AD uygulama ara sunucusu üzerinden yayımlamanız gerekir. Daha fazla bilgi için [bağlayıcıyı yükleyin ve uygulama ara sunucusu ile çalışmaya başlama](../manage-apps/application-proxy-enable.md) ve [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](../manage-apps/application-proxy-publish-azure-portal.md).
- **B2B kullanıcı nesnesinde şirket içi dizini ile yetkilendirme**. Uygulamanın kullanıcı erişim denetimleri gerçekleştirmek ve doğru kaynaklara erişim izni olması gerekir. IWA ve KCD, şirket içi Windows Server Active Directory'de bu yetkilendirme tamamlamak için bir kullanıcı nesnesi gerektirir. Bölümünde anlatıldığı gibi [nasıl çoklu oturum açma ile KCD works](../manage-apps/application-proxy-configure-single-sign-on-with-kcd.md#how-single-sign-on-with-kcd-works), uygulama proxy'si, kullanıcının kimliğine bürünmek ve uygulama için bir Kerberos belirteç almak için bu kullanıcı nesnesi gerekiyor. 

   B2B kullanıcı senaryosu için şirket içi dizin yetkilendirme için gerekli olan Konuk kullanıcı nesneleri oluşturmak için kullanabileceğiniz iki yöntem vardır:

   - Microsoft Identity Manager (MIM) ve Microsoft Graph için MIM yönetim Aracısı. 
   - [Bir PowerShell Betiği](#create-b2b-guest-user-objects-through-a-script-preview). Betik kullanarak MIM gerektirmeyen daha basit bir çözümdür. 

Aşağıdaki diyagramda, dizin iş birlikte B2B kullanıcıları şirket içi IWA ve KCD uygulamalarınıza erişim vermek için Azure AD uygulama ara sunucusu üst düzey bir genel bakış ve şirket içi B2B kullanıcı nesnesinin oluşturulmasını sağlar. Numaralandırılmış adımları aşağıdaki diyagramda ayrıntılı açıklanmıştır.

![MIM ve B2B betik çözüm diyagramı](media/hybrid-cloud-to-on-premises/MIMScriptSolution.PNG)

1.  Bir kullanıcı bir iş ortağı kuruluştan (Fabrikam Kiracı), Contoso kiracıya davet edilmiş.
2.  Contoso kiracıya bir Konuk kullanıcı nesnesi oluşturulur (örneğin, bir kullanıcı nesnesi guest_fabrikam.com#EXT# UPN'si ile@contoso.onmicrosoft.com).
3.  Fabrikam Konuk şubeden MIM veya B2B PowerShell Betiği aracılığıyla içeri aktarılır.
4.  Bir gösterimi veya "ayak" Fabrikam Konuk kullanıcı nesnesinin (konuk EXT #) Contoso.com, MIM veya B2B PowerShell Betiği aracılığıyla şirket içi dizin oluşturulur.
5.  Konuk kullanıcı erişimi şirket içi uygulama, app.contoso.com.
6.  Kerberos kısıtlanmış temsili aracılığıyla uygulama proxy'si aracılığıyla yetkili kimlik doğrulama isteği. 
7.  Konuk kullanıcı nesnesi yerel olarak bulunduğundan, kimlik doğrulaması başarılı olur.

### <a name="lifecycle-management-policies"></a>Yaşam döngüsü yönetimi ilkeleri

Şirket içi B2B kullanıcı nesnelerinin yaşam döngüsü yönetimi ilkeleri aracılığıyla yönetebilirsiniz. Örneğin:

- Uygulama proxy'si kimlik doğrulaması sırasında MFA kullanılmasını sağlamak amacıyla Konuk kullanıcı için çok faktörlü kimlik doğrulaması (MFA) ilkeleri ayarlayabilirsiniz. Daha fazla bilgi için [B2B işbirliği kullanıcıları için koşullu erişim](conditional-access.md).
- Tüm sponsorships, erişim gözden geçirmeleri, hesap doğrulamaları vb. kullanıcı şirket içi kullanıcılar için geçerliyse bulutta B2B gerçekleştirilen. Örneğin, bulut kullanıcı yaşam döngüsü yönetimi ilkelerinizi silinirse, şirket içi kullanıcı de MIM eşitleme veya Azure AD Connect eşitleme yoluyla silinir. Daha fazla bilgi için [Konuk erişimi yönetme ile Azure AD erişim gözden geçirmeleri](../governance/manage-guest-access-with-access-reviews.md).

### <a name="create-b2b-guest-user-objects-through-mim"></a>B2B Konuk kullanıcı nesneleri MIM ile oluşturma

MIM 2016 Hizmet Paketi 1 ve MIM yönetim aracısı için Microsoft Graph şirket içi dizinde Konuk kullanıcı nesneleri oluşturmak için nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure AD ile Microsoft Identity işletmeden işletmeye (B2B) işbirliği Azure uygulama proxy'si ile Manager (MIM) 2016 SP1](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-graph-b2b-scenario).

### <a name="create-b2b-guest-user-objects-through-a-script-preview"></a>Bir betik (Önizleme) ile B2B Konuk kullanıcı nesneleri oluşturma

Şirket içi Active Directory'nizde Konuk kullanıcı nesneleri oluşturmak için başlangıç noktası olarak kullanabileceğiniz bir PowerShell örnek betiği yoktur.

Betik ve gelen Benioku dosyasını indirebilirsiniz [İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=51495). Seçin **betik ve Benioku dosyasını Azure AD B2B kullanıcıları üzerinde-prem.zip çekmek için** dosya.

Komut dosyası kullanmadan önce önkoşulları ve önemli noktalar ve ilişkili benioku dosyasındaki gözden geçirme emin olun. Ayrıca, komut dosyası yalnızca bir örnek olarak sunulacağını anlayın. Geliştirme ekibiniz veya bir iş ortağı gerekir özelleştirebilir ve çalıştırmadan önce betiği gözden geçirin.

## <a name="license-considerations"></a>Lisans konuları

Dış Konuk kullanıcılar şirket içi uygulamalara erişmek için doğru istemci erişim lisansları (CAL) sahip olduğunuzdan emin olun. Daha fazla bilgi için "Harici bağlayıcılar" bölümüne bakın. [istemci erişim lisansları ve yönetim lisansları](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx). Microsoft temsilcinize başvurun veya yerel bayinizle ilgili lisans gereksinimlerinizi.

## <a name="next-steps"></a>Sonraki adımlar

- [Hibrit kuruluşlar için Azure Active Directory B2B işbirliği](hybrid-organizations.md)

- Azure AD Connect genel bakış için bkz. [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md).

