---
title: Azure kimlik ve erişim yönetimi temelleri nelerdir? -Azure Active Directory | Microsoft Docs
description: Azure Active Directory Premium sürümleri ile ek araçları ve gelişmiş koruma özellikleri hakkında bilgi edinin.
services: active-directory
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/13/2018
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: f7baa29c77ae4af9813bfc755a39cc07288a3ad2
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734684"
---
# <a name="what-are-the-fundamentals-of-azure-identity-and-access-management"></a>Azure kimlik ve erişim yönetimi temelleri nelerdir?
Azure AD Premium bir bulut tabanlı kimlik ve erişim yönetimi çözümü, gelişmiş koruma özellikleri ile gelir. Bu gelişmiş özellikler güvenli bir kimlik tüm uygulamalarınızı, kimlik koruması sağlanmasına yardımcı (ile Gelişmiş [Microsoft zeka security graph'i](https://www.microsoft.com/security/intelligence)), ve [Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md). Azure AD kullanıcılarınızın kimliklerini gerçek zamanlı olarak risk tabanlı oluşturmanıza yardımcı olur ve uyarlanabilir erişim ilkeleri, kuruluşunuzun verilerini korumaya yardımcı olur.

Azure AD kimlik yönetimi ve koruma özelliklerini anlatan bu kısa videoyu izleyin:
>[!VIDEO https://www.youtube.com/embed/9LGIJ2-FKIM]

Azure AD, güvenli, otomatikleştirin ve parolalar, kullanıcı ve Grup Yönetimi ve uygulama istekleri sıfırlama dahil, ortamınızı yönetmenize yardımcı olabilecek araçlar kümesi de sağlar. Azure AD de kullanıcıya ait cihazları ve erişim ve denetim yazılım olarak hizmet (SaaS) uygulamaları yönetmenize yardımcı olabilir.

Azure Active Directory Premium sürümleri ve ilişkili araçlar maliyetler hakkında daha fazla bilgi için bkz. [Azure Active Directory fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="connect-on-premises-active-directory-with-azure-ad-and-office-365"></a>Şirket içi Active Directory ile Azure AD ve Office 365 arasında bağlantı kurma
Şirket içi Active Directory uygulamanızı buluta şirket içi dizinlerinizi Azure AD ile tümleştirerek genişletme [karma Kimlik Yönetimi](https://aka.ms/aadframework). [Azure AD Connect](../connect/active-directory-aadconnect.md) Bu tümleştirme, kullanıcılarınız tek bir olanağı sağlar. böylece hem şirket içi kaynaklarınızı hem de Office 365 gibi bulut hizmetlerinizi çoklu oturum açma (SSO) kimlik ve erişim.

Azure AD Connect, DirSync ve Azure AD Sync, şirket içi ve Azure AD arasında kimlik eşitleme gereksinimlerinizi desteklemek için yardımcı olma gibi kimlik tümleştirme araçlarının eski sürümlerinin yerine geçer. Azure AD Connect eşitlemesi etkinleştirilir:

- **Eşitleme.** Kullanıcıları, grupları ve diğer nesnelerin oluşturulmasından sorumludur. Şirket içi kullanıcılarınız için kimlik bilgileri, Azure AD'de nedir eşleştiğinden emin olunması sorumludur. Parola geri yazma üzerinde kapatma Ayrıca kullanıcıların Azure AD'de parolaları güncelleştirdiğinizde, şirket içi dizinlerinizi eşitleyin yardımcı olur.

- kimlik doğrulaması. Doğru kimlik doğrulama yöntemini seçerek Azure AD karma kimlik çözümünüzü ayarlarken önemlidir. Bulut kimlik doğrulamasını seçebilirsiniz (parola karması eşitleme / geçişli kimlik doğrulaması) veya kuruluşunuz için Federasyon kimlik (AD FS). Kullanılabilir seçenekleri hakkında daha fazla bilgi için bkz. [, Azure Active Directory karma kimlik çözümü için doğru kimlik doğrulama yöntemini seçin](https://aka.ms/auth-options).

- **Sistem durumu izleme.** Azure AD Connect Health, izleme sağlar ve bu etkinliği görüntülemek için Azure portalında merkezi bir konum. Daha fazla bilgi için bkz. [Bulutta şirket içi kimlik altyapınızı ve eşitleme hizmetlerinizi izleme](../connect-health/active-directory-aadconnect-health.md).

## <a name="increase-productivity-and-reduce-helpdesk-costs-with-self-service-and-single-sign-on-experiences"></a>Self servis ve çoklu oturum açma deneyimleriyle üretkenliği artırın ve yardım masası maliyetlerini düşürün
Kullanıcılar, tek bir kullanıcı adı ve parola, tutarlı bir deneyim her cihaz üzerinde birlikte olduğunda zamandan tasarruf edin. Kullanıcılar ayrıca gibi Self Servis görevleri gerçekleştirerek zaman Kaydet[Unutulan parolayı sıfırlama](../user-help/active-directory-passwords-update-your-own-password.md) veya Yardım Masası Yardım beklemeden bir uygulamaya erişim isteme.

SSO ve tutarlı bir deneyim, Azure AD furthering [şirket içi Active Directory'nizde genişletir](../connect/active-directory-aadconnect.md) buluta, şirket kaynaklarına, kullanıcılarınıza kendi etki alanına katılmış cihazlarda birincil kuruluş hesabını kullanarak izin vererek ve tüm SaaS uygulamalarına ve web işin yapılması için kullanmanız gerekir. 

Ayrıca, uygulama erişimi otomatik olarak sağlanan (XML'deki sağlanan veya) grup üyeliklerini ve yardımcı bir kullanıcının çalışan durumu galeri uygulamaları veya geliştirilen ve üzerinden yayımlanan kendi şirket içi uygulamalara erişimi denetleme göre [Azure AD uygulama ara sunucusu](../manage-apps/application-proxy.md).

## <a name="manage-and-control-access-to-your-organizational-resources"></a>Yönetme ve kuruluş kaynaklarına erişimi denetleme
Microsoft kimlik ve erişim yönetimi çözümlerini kuruluşunuzun veri merkezi genelinde ve buluta uygulamalara ve kaynaklara erişimi korumaya yardımcı olur. Bu erişim yönetimi gibi ek doğrulama düzeylerini sağlamaya yardımcı olur [multi-Factor Authentication](../authentication/concept-mfa-howitworks.md) ve [koşullu erişim ilkeleri](../conditional-access/overview.md). Güvenlik raporlama, Denetim, Gelişmiş üzerinden şüpheli Etkinlik izleme ve uyarı da olası güvenlik sorunlarını gidermek için yardımcı olabilir.

Azure AD Premium koşullu erişim ilkeleri kullanarak tüm Azure AD bağlı uygulaması için SaaS uygulamaları bulutta veya şirket içinde veya web uygulamaları içinde çalışan özel iş uygulamaları gibi ilke tabanlı erişim kuralları oluşturmanıza olanak sağlar). Azure AD, bir kullanıcı bir uygulamaya erişmeye çalıştığında uygulamadan kurallarınızı gerçek zamanlı olarak değerlendirir. Azure kimlik koruma ilkeleri sağlar, otomatik olarak gereken işlemi (erişimi engelleme, çok faktörlü kimlik doğrulama zorlanıyor veya kullanıcı parolalarını sıfırlama) şüpheli etkinlik bulunursa.

## <a name="azure-active-directory-privileged-identity-management"></a>Azure Active Directory Privileged Identity Management
[Privileged Identity Management (PIM)](../privileged-identity-management/pim-getting-started.md), Azure Active Directory Premium 2 edition ile birlikte, bulmak, kısıtlamak ve yönetim hesapları ve Azure Active Directory ve diğer kaynaklarına erişimleri izlemenize yardımcı olur Microsoft Çevrimiçi Hizmetler. PIM Ayrıca, tam bir süre boyunca çok faktörlü kimlik doğrulaması, önceden yapılandırılmış bir süre için kendi ayrıcalıklarını geçici olarak yükseltilmesini talep etmek Yöneticiler izin verebileceğiniz anlamına gelir, gereken isteğe bağlı yönetim erişimi yönetmenize yardımcı olur bir normal kullanıcı durumunun hesaplarına döndürmeden önce.

## <a name="next-steps"></a>Sonraki adımlar
Azure AD mimarisi hakkında daha fazla bilgi için bkz: [Azure AD mimarisi nedir?](active-directory-architecture.md).
