---
title: Azure Active Directory ile çalışmaya başlama | Microsoft Docs
description: Lisansları almak, etki alanı adı ekleyin, özel oturum açma sayfası oluşturmak ve Self Servis parola sıfırlama Azure Active dizin ekleyin
keywords: ''
author: curtand
manager: mtillman
ms.author: curtand
ms.reviewer: jsnow
ms.date: 11/14/2017
ms.topic: article
ms.prod: ''
ms.service: active-directory
ms.workload: identity
ms.technology: ''
ms.assetid: ''
services: active-directory
ms.custom: it-pro
ms.openlocfilehash: eedcb80038179cf74666880816cb0b5416ac63fd
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="get-started-with-azure-ad"></a>Azure AD’yi kullanmaya başlama
Modern Kimlik Yönetimi, uygulamaların ve hizmetlerin yalnızca kimliği doğrulanmış kullanıcılar için kullanılabilirliği sağlamak için ölçeklenebilir, tutarlı reliablity gerektirir. Yeterli bir hizmet (SaaS) uygulamaları olarak onaylanan, Genel yazılım erişim sağlamak için bir yol kullanıcılar, BT ihtiyaçlarını kimlik yönetimi gereksinimlerini desteklemek için ana bilgisayar iç iş kolu uygulamaları için bir yol, geliştirmek için bile yolları şirket içi ve uygulama geliştirme ve kullanım. Tüm bu gereksinimlerin bir bulut tabanlı kimlik yönetimi çözümü gereksinimi için gelin.      

Azure Active Directory (Azure AD) olduğu Microsoft çok kiracılı, bulut tabanlı dizin ve Kimlik Yönetimi Hizmeti. Azure AD çekirdek Dizin Hizmetleri, Gelişmiş Kimlik Yönetimi ve uygulamaya erişim yönetimi birleştirir. Çok kiracılı, coğrafi olarak dağıtılan, yüksek kullanılabilirlik tasarımı, buna yönelik en kritik işletme ihtiyaçlarınıza bağlı Azure AD anlamına gelir.

Azure AD kimlik yönetimi özelliklerini şirket içi kaynak bilgileri, eşitleme özelliği de dahil olmak üzere, tam bir paketi içeren özelleştirilebilir şirket markası, basit lisans yönetimi ve hatta Self Servis parola yönetimi. Bu dağıtımı kolay yetenekleri yardımcı olabilecek Azure AD ile bulut tabanlı uygulamalar güvenli, BT işlemlerini kolaylaştırır, maliyetleri azaltmak için çalışmaya başlamak ve kurumsal uyumluluk hedefleri karşılandığından emin olmak.

![Azure AD ](./media/get-started-azure-ad/Azure_Active_Directory.png)

Bu makalenin geri kalanında Azure AD ile çalışmaya başlama anlatır. 

## <a name="sign-up-for-azure-active-directory-premium"></a>Azure Active Directory Premium'a kaydolma
İçin [Azure Active Directory (Azure AD) Premium ile çalışmaya başlama](active-directory-get-started-premium.md), lisansları satın almanız ve bunları Azure aboneliğinizle ilişkilendirin. Yeni bir Azure aboneliği oluşturursanız, lisans planı ve Azure AD hizmeti erişimini etkinleştirmeniz gerekir. 

Active Directory Premium'a kaydolmanız için birkaç seçenek sunulmaktadır: 

- Azure veya Office 365 aboneliğinizi kullanın
- Enterprise Mobility + Security lisans planı kullanın
- Microsoft Toplu Lisanslama planı kullanın

### <a name="verification-step"></a>Doğrulama adımı
Abonelik etkinleştirdikten sonra, hizmette oturum açabildiğinizden emin olun.

## <a name="add-a-custom-domain-name"></a>Özel bir etki alanı adı ekleme
Bir ilk etki alanı adı biçiminde her Azure AD dizini birlikte *domainname*. onmicrosoft.com. İlk etki alanı adı değiştirilmiş ya da silinemez, ancak ayrıca [şirket etki alanı adınızı Azure AD'ye ekleme](add-custom-domain.md). Örneğin, kuruluşunuzun iş ve şirket etki alanı adınızı kullanarak oturum açan kullanıcıların yapmak için kullanılan diğer etki alanı adlarını büyük olasılıkla vardır. Azure AD ile özel etki alanı adları ekleme gibi kullanıcılarınız için tanıdık dizindeki kullanıcı adları atama olanak tanır 'alice@contoso.com.' yerine 'alice@.onmicrosoft.com'. Bu basit bir işlemdir:

1. Dizine özel etki alanı adı ekleyin
2. Etki alanı adı kayıt şirketinize etki alanı adı için bir DNS girişi ekleyin
3. Azure AD'de özel etki alanı adını doğrulayın

### <a name="verification-step"></a>Doğrulama adımı
Özel bir etki alanı eklendikten sonra sahip olduğundan emin olun **doğrulandı** görüntülenen durumu **özel etki alanı adları** Azure AD portalı dikey.

## <a name="add-company-branding-to-your-sign-in-page"></a>Şirket, oturum açma sayfasına markası ekleme 
Birçok şirket, karışıklığı önlemek için yönettikleri hizmetlerde ve web sitelerinde tutarlı bir genel görünüm uygulamak ister. Azure Active Directory (Azure AD), izin vererek bu yeteneği sağlar [şirket Logonuzla ve özel renk düzenleri ile oturum açma sayfası görünüşünü özelleştirme](customize-branding.md). Oturum açma sayfası, Office 365 veya Azure AD, kimlik sağlayıcısı olarak kullanan diğer web tabanlı uygulamalar için oturum açtığınızda görüntülenen sayfadır. Kimlik bilgilerinizi girmek için bu sayfayı ile etkileşim.

### <a name="verification-step"></a>Doğrulama adımı
Azure portalında oturum açın ve özelleştirilmiş oturum açma sayfanız ve ek dil özelleştirmeler doğru şekilde yapılandırıldığından emin olun. 

## <a name="add-new-users"></a>Yeni kullanıcı ekle
Yapabilecekleriniz [yeni kullanıcılar eklemek için kuruluşunuzun Azure AD](add-users-azure-active-directory.md) bir Azure portalını kullanarak bir zamanda veya şirket içi Windows Server AD kaynak verilerinizi eşitleyerek. Doğrudan Azure AD Portalı'ndan bulut tabanlı kullanıcılar eklemek veya şirket içi kullanıcı bilgilerini eşitleyin.

Azure AD ile şirket içi kimlik eşitlemesini etkinleştirmek için [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect)’i kuruluşunuzdaki bir sunucuya yükleyip yapılandırmanız gerekir. Bu uygulama, var olan kimlik kaynağınızdan Azure AD kiracınıza kullanıcı ve grupları eşitleme işlemini gerçekleştirir.

### <a name="verification-step"></a>Doğrulama adımı
Oluşturma veya yeni kullanıcılar eşitleme sonrasında, Azure AD içinde görünür olduğundan emin olun.

## <a name="assign-licenses"></a>Lisans atama
Bir aboneliği elde Ücretli özellikleri yapılandırmak için gereken her şeyi olsa da, hala gerekir [kullanıcı lisansları atama](license-users-groups.md) özellikleri Ücretli Azure AD Premium için. Kimin erişiminiz veya bir Azure üzerinden, yönetilen kimin herhangi bir kullanıcı bir lisans özelliği Ücretli AD atanmış. Lisans atama, kullanıcı ve Azure AD Premium, Basic veya Enterprise Mobility + Security gibi bir satın alınan hizmet arasındaki bir eşlemedir.

Aşağıdaki örneklerde olduğu gibi kuralları ayarlamak için Grup tabanlı lisans atamasını kullanabilirsiniz:

- Dizininizdeki tüm kullanıcılara otomatik olarak bir lisans alın
- Uygun iş unvanı herkesle bir lisans alır
- (Self Servis Grup kullanarak) kuruluştaki diğer yöneticiler kararını devredebilirsiniz.

### <a name="verification-step"></a>Doğrulama adımı
Atanan gözden geçirme ve altında kullanılabilir lisans **Azure Active Directory** > **lisansları** > **tüm ürünleri**.

## <a name="configure-self-service-password-reset"></a>Kendi kendine parola sıfırlamayı yapılandırın
[Self Servis parola sıfırlama (SSPR)](authentication/quickstart-sspr.md) basit bir yol sıfırlamak veya bunların parolaları veya hesaplarının kilidini açmak kullanıcıların BT yöneticilerine sunmaktadır. Sistem, kullanıcıların sistemi kullanması sırasında kötüye kullanım veya uygunsuz kullanım konusunda uyaran bildirimlerle birlikte izlemeye yönelik ayrıntılı raporlama içerir.

### <a name="verification-step"></a>Doğrulama adımı
Gözden geçirme etkin SSPR özellikleri altında **Azure Active Directory** > **parola sıfırlama** uygun kullanıcı ve Grup atamaları yapıldı emin olmak için. 


## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory Hizmeti sayfası](https://azure.microsoft.com/services/active-directory/)

[Azure Active Directory fiyatlandırma bilgileri sayfası](https://azure.microsoft.com/pricing/details/active-directory/)