---
title: Karma Azure Active Directory join uygulaması Azure Active Directory'de (Azure AD) nasıl | Microsoft Docs
description: Hibrit Azure Active Directory'ye katılmış cihazları elle nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2019
ms.author: joflore
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64dd8067654246f7c9a077d027c068df820f439d
ms.sourcegitcommit: 6932af4f4222786476fdf62e1e0bf09295d723a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66688706"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Nasıl Yapılır: Hibrit Azure Active Directory join uygulamanızı planlama

Benzer şekilde bir kullanıcı, bir cihaz korumak ve dilediğiniz zaman ve herhangi bir konumdan kaynaklarınızı korumak için kullanmak istediğiniz başka bir çekirdek kimliktir. Bu hedefe getiren ve aşağıdaki yöntemlerden birini kullanarak Azure AD'de cihaz kimliklerini yönetme görevleri gerçekleştirebilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Cihazlarınızı Azure AD'ye taşıyarak, çoklu oturum açma (SSO) özelliği sayesinde bulut ve şirket içi kaynaklarınız genelinde kullanıcılarınızın üretkenliğini en üst düzeye çıkarırsınız. Ayrıca, [koşullu erişim](../active-directory-conditional-access-azure-portal.md) ile bulut ve şirket içi kaynaklarınıza erişimin güvenliği sağlayabilirsiniz.

Bir şirket içi Active Directory (AD) ortamınız varsa ve, AD etki alanına katılmış bilgisayarları Azure AD'ye istiyorsanız, bunu hibrit Azure AD'ye katılım'ı yaparak gerçekleştirebilirsiniz. Bu makalede, ortamınızda katılın, hibrit Azure AD'ye uygulamak için ilgili adımları sağlar. 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, aşina olduğunuzu varsayar [Azure Active Directory'de cihaz kimlik yönetimine giriş](../device-management-introduction.md).

> [!NOTE]
> Windows 10 hibrit Azure AD'ye katılma Windows Server 2008 R2 için gereken en düşük etki alanı işlevsel orman işlev düzeyleri.

## <a name="plan-your-implementation"></a>Uygulamanızı planlama

Karma Azure AD uygulamanız planlamak için ile kendinizi alıştırın:

|   |   |
| --- | --- |
| ![Onay][1] | Cihazları gözden geçir desteklenir |
| ![Onay][1] | Gözden geçirme bilmeniz gerekenler |
| ![Onay][1] | Hibrit Azure AD'ye katılma denetimli doğrulama gözden geçirin |
| ![Onay][1] | Temel kimlik altyapınızı senaryonuzu seçin |
| ![Onay][1] | Şirket içinde AD UPN'sini desteklemek için hibrit Azure AD'ye katılım gözden geçirme |

## <a name="review-supported-devices"></a>Cihazları gözden geçir desteklenir

Hibrit Azure AD'ye katılım geniş Windows cihazları destekler. Yapılandırmanın daha eski Windows sürümleri çalıştıran cihazlar için ek veya bunlardan farklı adımlar gerektiğinden, desteklenen cihazları iki kategorilere ayrılır:

### <a name="windows-current-devices"></a>Windows cihazları

- Windows 10
- Windows Server 2016
- Windows Server 2019

Windows masaüstü işletim sistemini çalıştıran cihazlar için desteklenen bir sürümü bu makalede listelenen [Windows 10 sürüm bilgileri](https://docs.microsoft.com/windows/release-information/). En iyi uygulama, Microsoft, Windows 10 'un en son sürüme yükseltme önerir.

### <a name="windows-down-level-devices"></a>Windows alt düzey cihazları

- Windows 8.1
- Windows 7. Windows 7 ilgili destek bilgileri için lütfen bu makaleyi gözden geçirin [desteklemek için Windows 7 tarihinde sona eriyor](https://www.microsoft.com/en-us/windowsforbusiness/end-of-windows-7-support)
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

İlk planlama adım, ortamınızı gözden geçirin ve Windows alt düzey cihazları desteklemek gerekli olup olmadığını belirleyin.

## <a name="review-things-you-should-know"></a>Gözden geçirme bilmeniz gerekenler

Ortamınızı kimlik verilerini birden fazla Azure AD kiracınıza eşitlemek, tek bir AD ormanında oluşuyorsa hibrit Azure AD'ye katılımı şu anda desteklenmiyor.

Hibrit Azure AD'ye katılımı, Sanal Masaüstü Altyapısı (VDI) kullanırken, şu anda desteklenmiyor.

Hibrit Azure AD'ye FIPS uyumlu TPM'ler için desteklenmiyor. FIPS uyumlu TPM'ler cihazlarınız varsa, bunları hibrit Azure AD'ye katılma devam etmeden önce devre dışı bırakmanız gerekir. Microsoft, TPM üreticisine bağımlı olduğu FIPS modundayken TPM'ler için devre dışı bırakmak için herhangi bir aracı sağlamaz. Lütfen OEM donanımınız için desteğe başvurun.

Hibrit Azure AD'ye katılım etki alanı denetleyicisi (DC) rolünü çalıştıran Windows Server için desteklenmiyor.

Hibrit Azure AD'ye katılma dolaşımı veya gezici kullanıcı profili kullanırken Windows alt düzey cihazları üzerinde desteklenmiyor.

Sistem hazırlığı Aracı (Sysprep) FQDN'yi ve kullanıyorsanız bir **öncesi 10 1809** görüntü yükleme için o yansıma değil bir CİHAZDAN Azure AD ile hibrit Azure AD'ye katılma olarak zaten kayıtlı olduğundan emin olun.

Ek sanal makineler oluşturmak için bir sanal makine (VM) üzerinde anlık görüntü FQDN'yi kullanıyorsanız, bu anlık görüntü zaten Azure AD ile hibrit Azure AD'ye katılma olarak kayıtlı bir VM'den olmadığından emin olun.

Windows 10 etki alanına katılmış ise zaten cihazlardır [kayıtlı Azure AD](https://docs.microsoft.com/azure/active-directory/devices/overview#azure-ad-registered-devices) kiracınız için hibrit Azure AD'ye katılma etkinleştirmeden önce bu duruma kaldırılması önerilir. Windows 10 1809 yayından çift bu durumu önlemek için aşağıdaki değişiklikler yapılmıştır:

- Hibrit Azure AD'ye katılmış cihaz olduktan sonra herhangi bir mevcut Azure AD kayıtlı durumu otomatik olarak kaldırılması.
- Etki alanına katılmış cihaz Azure AD'ye bu kayıt defteri anahtarı - HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin, "BlockAADWorkplaceJoin" ekleyerek kayıtlı olmasını engelleyebilir = DWORD: 00000001.
- Bu değişiklik ile uygulanan KB4489894 için Windows 10, 1803 sürümü kullanıma sunulmuştur. Windows iş için Hello yapılandırılmış varsa, ancak kullanıcı re-Windows iş için Hello ikili durum sonra temizleme kurulumu için gereklidir.


## <a name="review-controlled-validation-of-hybrid-azure-ad-join"></a>Hibrit Azure AD'ye katılma denetimli doğrulama gözden geçirin

Tüm önkoşulların yerinde olduğundan, Azure AD kiracınızda cihaz olarak Windows cihazları otomatik olarak kaydeder. Bu cihaz kimliklerini Azure AD'de durumunu hibrit Azure AD'ye katılım adlandırılır. Bu makalede ele alınan kavramları hakkında daha fazla bilgi makalelerde bulunabilir [Azure Active Directory'de cihaz kimlik yönetimine giriş](overview.md) ve [, hibrit Azure Active Directory katılım'ı planlama Uygulama](hybrid-azuread-join-plan.md).

Kuruluşlar, tek seferde tüm kuruluşlarındaki etkinleştirmeden önce hibrit Azure AD'ye katılım denetimli bir doğrulama yapmak isteyebilirsiniz. Makalesini gözden geçirin [denetlenen hibrit Azure AD'ye katılma doğrulaması](hybrid-azuread-join-control.md) bunu yerine getirmeyi anlamak için.


## <a name="select-your-scenario-based-on-your-identity-infrastructure"></a>Temel kimlik altyapınızı senaryonuzu seçin

Hibrit Azure AD'ye katılma hem yönetilen hem de Federasyon ortamlar ile çalışır.  

### <a name="managed-environment"></a>Yönetilen ortam

Yönetilen bir ortam olabilir aracılığıyla dağıtılan [parola karması eşitleme (PHS)](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-phs) veya [geçirmek aracılığıyla kimlik doğrulaması (PTA)](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta) ile [sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso).

Bu senaryolar, bir federasyon sunucusu kimlik doğrulaması için yapılandırmak gerekli değildir.

### <a name="federated-environment"></a>Federasyon ortamı

Bir Federasyon ortam aşağıdaki gereksinimleri destekleyen bir kimlik sağlayıcısına sahip olmalıdır:

- **WS-Trust protokolü:** Bu Windows kimlik doğrulaması için gerekli bir protokoldür geçerli hibrit Azure AD'ye katılmış cihazların Azure AD ile.
- **WIAORMULTIAUTHN talep:** Bu talep, hibrit Azure AD'ye katılımı için Windows alt düzey cihazları yapmak için gereklidir.

Active Directory Federasyon Hizmetleri (AD FS) kullanarak bir Federasyon ortamı varsa, yukarıdaki gereksinimleri zaten desteklenir.

> [!NOTE]
> Azure AD, yönetilen etki alanlarında akıllı kartlar veya sertifikaları desteklemez.

1.1.819.0 sürümünden itibaren Azure AD Connect hibrit Azure AD'ye katılımı yapılandırmak için bir sihirbaz sağlar. Sihirbaz, yapılandırma işlemini önemli ölçüde basitleştirebilmenizi sağlar. Azure AD Connect gerekli sürümünü yüklemek sizin için bir seçenek değilse, bkz. [el ile cihaz kaydını yapılandırmak nasıl](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual). 

Eşleşen kimlik altyapınızı senaryo temel alınarak, bakın:

- [Hibrit Azure Active Directory join Federasyon ortamı için yapılandırma](hybrid-azuread-join-federated-domains.md)
- [Hibrit Azure Active Directory join yönetilen ortam için yapılandırma](hybrid-azuread-join-managed-domains.md)



## <a name="review-on-premises-ad-upn-support-for-hybrid-azure-ad-join"></a>Gözden geçirme şirket içi hibrit Azure AD'ye katılma AD UPN'sini desteği

Bazı durumlarda, şirket içi AD UPN, Azure AD UPN farklı olabilir. Bu gibi durumlarda, Windows 10 hibrit Azure AD'ye katılma sınırlı destek için şirket içi AD UPN göre sağlar [kimlik doğrulama yöntemi](https://docs.microsoft.com/azure/security/azure-ad-choose-authn), etki alanı türü ve Windows 10 sürümü. Ortamınızda bulunabilir AD UPN şirket içi iki tür vardır:

- UPN yönlendirilebilir: UPN yönlendirilebilir bir etki alanı kayıt şirketi ile kayıtlı bir geçerli doğrulanmış etki sahiptir. Örneğin, Azure AD'de birincil etki alanı contoso.com ise şirket içi birincil etki alanı contoso.org olan Contoso tarafından sahip olunan bir AD ve [Azure AD'de doğrulanmış](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)
- Yönlendirilemeyen UPN: Yönlendirilemeyen bir UPN doğrulanmış bir etki alanı yok. Yalnızca kuruluşunuzun özel ağına içinde geçerlidir. Azure AD'de birincil etki alanı contoso.com ise, örneğin, contoso.local birincil etki alanında ise şirket içi AD ancak internet'in doğrulanabilir bir etki alanı değil ve kullanıcının yalnızca Contoso içinde kullanılan ağ.

Aşağıdaki tabloda Ayrıntılar desteği bu şirket için AD UPN, Windows 10 hibrit Azure AD'ye katılma sağlar

| Tür şirket içi AD UPN | Etki alanı türü | Windows 10 sürümü | Açıklama |
| ----- | ----- | ----- | ----- |
| Yönlendirilebilir | Federasyon | 1703 sürümünden | Genel kullanıma sunuldu |
| Yönlendirilebilir olmayan | Federasyon | 1803 sürümü | Genel kullanıma sunuldu |
| Yönlendirilebilir | Yönetilen | Desteklenmiyor | |
| Yönlendirilebilir olmayan | Yönetilen | Desteklenmiyor | |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Federasyon ortam için yapılandırma hibrit Azure Active Directory join](hybrid-azuread-join-federated-domains.md)
> [yönetilen ortam için yapılandırma hibrit Azure Active Directory join](hybrid-azuread-join-managed-domains.md)

<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
