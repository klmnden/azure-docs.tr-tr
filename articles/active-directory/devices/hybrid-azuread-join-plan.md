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
ms.openlocfilehash: 8827a51a23b2ea274d8096a154e630c9cecbba7c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59489527"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Nasıl Yapılır: Hibrit Azure Active Directory join uygulamanızı planlama

Kullanıcıya benzer şekilde bir cihaz, korumak istediğiniz ve her yerde ve zamanda kaynaklarınızı korumak için kullandığınız başka bir kimlik alır. Aşağıdaki yöntemlerden biri ile cihazlarınızın kimliklerini Azure AD'ye getirerek bu hedefi gerçekleştirebilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Cihazlarınızı Azure AD'ye taşıyarak, çoklu oturum açma (SSO) özelliği sayesinde bulut ve şirket içi kaynaklarınız genelinde kullanıcılarınızın üretkenliğini en üst düzeye çıkarırsınız. Ayrıca, [koşullu erişim](../active-directory-conditional-access-azure-portal.md) ile bulut ve şirket içi kaynaklarınıza erişimin güvenliği sağlayabilirsiniz.

Şirket içi Active Directory ortamınız varsa ve etki alanınıza katılmış cihazları Azure AD'ye katmak istiyorsanız hibrit Azure AD'ye katılmış cihazları yapılandırarak bunu gerçekleştirebilirsiniz. Bu makalede, ortamınızda katılın, hibrit Azure AD'ye uygulamak için ilgili adımları sağlar. 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, aşina olduğunuzu varsayar [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md).

> [!NOTE]
> Windows 10 hibrit Azure AD'ye katılma Windows Server 2008 R2 için gereken en düşük etki alanı işlevsel orman işlev düzeyleri. Daha düşük sürümlerde, kullanıcı bir birincil yenileme belirteci LSA sorunları nedeniyle Windows oturum açma sırasında alamayabilir.

## <a name="plan-your-implementation"></a>Uygulamanızı planlama

Karma Azure AD uygulamanız planlamak için ile kendinizi alıştırın:

|   |   |
| --- | --- |
| ![İşaretli][1] | Cihazları gözden geçir desteklenir |
| ![İşaretli][1] | Gözden geçirme bilmeniz gerekenler |
| ![İşaretli][1] | Cihazlarınızı hibrit Azure AD'ye katılma denetlemek nasıl gözden geçirin |
| ![İşaretli][1] | Senaryonuzu seçin |

## <a name="review-supported-devices"></a>Cihazları gözden geçir desteklenir

Hibrit Azure AD'ye katılım geniş Windows cihazları destekler. Yapılandırmanın daha eski Windows sürümleri çalıştıran cihazlar için ek veya bunlardan farklı adımlar gerektiğinden, desteklenen cihazları iki kategorilere ayrılır:

### <a name="windows-current-devices"></a>Windows cihazları

- Windows 10
- Windows Server 2016
- Windows Server 2019

Windows masaüstü işletim sistemini çalıştıran cihazlar için desteklenen sürüm Windows 10 Yıldönümü Güncelleştirmesi (sürüm 1607) olan veya üzeri. En iyi uygulama, Windows 10 'un en son sürüme yükseltin.

### <a name="windows-down-level-devices"></a>Windows alt düzey cihazları

- Windows 8.1
- Windows 7
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

İlk planlama adım, ortamınızı gözden geçirin ve Windows alt düzey cihazları desteklemek gerekli olup olmadığını belirleyin.

## <a name="review-things-you-should-know"></a>Gözden geçirme bilmeniz gerekenler

Ortamınızı kimlik verilerini birden fazla Azure AD kiracınıza eşitlenmiş tek bir ormanda bulunuyorsa, hibrit Azure AD'ye katılım kullanamazsınız.

Sistem hazırlığı Aracı (Sysprep) bağlı bir Windows 10, 1803 yüklemesinden oluşturulan emin görüntüler olun veya hibrit Azure AD'ye katılım için daha önce yapılandırılmamış.

Ek sanal makineler oluşturmak için bir sanal makine (VM) üzerinde anlık görüntü FQDN'yi kullanıyorsanız, hibrit Azure AD'ye katılım için yapılandırılmamış bir VM anlık görüntüsü kullandığınızdan emin olun.

Windows alt düzey cihazların hibrit Azure AD'ye katılma:

- **Olan** Federasyon olmayan ortamlarda desteklenen [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start). 
- **Değil** sorunsuz çoklu oturum açma olmadan Azure AD geçişli kimlik doğrulaması kullanılırken desteklenir.
- **Değil** dolaşımı veya gezici kullanıcı profili kullanırken veya Sanal Masaüstü Altyapısı (VDI) kullanılırken desteklenmez.

Windows etki alanı denetleyicisi (DC) rolünü çalıştıran sunucu kaydını desteklenmiyor.

Kuruluşunuz, kimliği doğrulanmış bir giden bağlantı proxy'si aracılığıyla İnternete erişimi gerektiriyorsa Windows 10 bilgisayarlarınızın giden bağlantı proxy'sine başarıyla kimlik doğrulayabildiğinden emin olmanız gerekir. Windows 10 bilgisayarlar makine bağlamını kullanarak cihaz kaydını çalıştırdığından makine bağlamı ile giden bağlantı proxy'sinin yapılandırılması gerekir.

Hibrit Azure AD'ye katılma, Azure AD ile şirket içi etki alanına katılmış cihazlarınızı otomatik olarak kaydedilecek bir işlemdir. Otomatik olarak kaydetmek için tüm cihazlar burada istemediğiniz durumlar vardır. Bu sizin için doğru olup olmadığını [cihazlarınızı hibrit Azure AD'ye katılma denetlemek nasıl](hybrid-azuread-join-control.md).

Windows 10 etki alanına katılmış ise zaten cihazlardır [kayıtlı Azure AD](https://docs.microsoft.com/azure/active-directory/devices/overview#azure-ad-registered-devices) kiracınız için hibrit Azure AD'ye katılma etkinleştirmeden önce bu duruma kaldırılması önerilir. Windows 10 1809 yayından çift bu durumu önlemek için aşağıdaki değişiklikler yapılmıştır:

- Hibrit Azure AD'ye katılmış cihaz olduktan sonra herhangi bir mevcut Azure AD kayıtlı durumu otomatik olarak kaldırılması.
- Etki alanına katılmış cihaz Azure AD'ye bu kayıt defteri anahtarı - HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin, "BlockAADWorkplaceJoin" ekleyerek kayıtlı olmasını engelleyebilir = DWORD: 00000001.
- Bu değişiklik, artık Windows 10, 1803 KB4489894 sürümüyle kullanılabilir.

FIPS uyumlu TPM'ler için hibrit Azure AD'ye katılma desteklenmez. FIPS uyumlu TPM'ler cihazlarınız varsa, bunları hibrit Azure AD'ye katılma devam etmeden önce devre dışı bırakmanız gerekir. Microsoft, TPM üreticisine bağımlı olduğu FIPS modundayken TPM'ler için devre dışı bırakmak için herhangi bir aracı sağlamaz. Lütfen OEM donanımınız için desteğe başvurun.

## <a name="review-how-to-control-the-hybrid-azure-ad-join-of-your-devices"></a>Cihazlarınızı hibrit Azure AD'ye katılma denetlemek nasıl gözden geçirin

Hibrit Azure AD'ye katılma, Azure AD ile şirket içi etki alanına katılmış cihazlarınızı otomatik olarak kaydedilecek bir işlemdir. Otomatik olarak kaydetmek için tüm cihazlar burada istemediğiniz durumlar vardır. Bu örnek için her şeyin beklendiği gibi çalıştığını doğrulamak için ilk dağıtım sırasında true'dur.

Daha fazla bilgi için [cihazlarınızı hibrit Azure AD'ye katılma denetleme](hybrid-azuread-join-control.md)

## <a name="select-your-scenario"></a>Senaryonuzu seçin

Hibrit Azure AD'ye katılım'ı aşağıdaki senaryolar için yapılandırabilirsiniz:

- Yönetilen etki alanları
- Federasyon etki alanları  

Ortamınızı etki alanları yönettiği, hibrit Azure AD'ye katılım'ı destekler:

- Kimlik doğrulaması (PTA) geçirin
- Parola karma eşitlemesi (PHS)

1.1.819.0 sürümünden itibaren Azure AD Connect hibrit Azure AD'ye katılımı yapılandırmak için bir sihirbaz sağlar. Sihirbaz, yapılandırma işlemini önemli ölçüde basitleştirebilmenizi sağlar. Daha fazla bilgi için bkz.

- [Federasyon etki alanları için hibrit Azure Active Directory'ye katılmayı yapılandırma](hybrid-azuread-join-federated-domains.md)
- [Yönetilen etki alanları için hibrit Azure Active Directory'ye katılmayı yapılandırma](hybrid-azuread-join-managed-domains.md)

 Azure AD Connect gerekli sürümünü yüklemek sizin için bir seçenek değilse, bkz. [el ile cihaz kaydını yapılandırmak nasıl](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual). 

## <a name="on-premises-ad-upn-support-in-hybrid-azure-ad-join"></a>Şirket içi AD UPN'sini desteği hibrit Azure AD'ye katılma

Bazı durumlarda, şirket içi AD UPN, Azure AD UPN farklı olabilir. Bu gibi durumlarda, Windows 10 hibrit Azure AD'ye katılma sınırlı destek için şirket içi AD UPN göre sağlar [kimlik doğrulama yöntemi](https://docs.microsoft.com/azure/security/azure-ad-choose-authn), etki alanı türü ve Windows 10 sürümü. Ortamınızda bulunabilir AD UPN şirket içi iki tür vardır:

- UPN yönlendirilebilir: UPN yönlendirilebilir bir etki alanı kayıt şirketi ile kayıtlı bir geçerli doğrulanmış etki sahiptir. Örneğin, Azure AD'de birincil etki alanı contoso.com ise şirket içi birincil etki alanı contoso.org olan Contoso tarafından sahip olunan bir AD ve [Azure AD'de doğrulanmış](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)
- Yönlendirilemeyen UPN: Yönlendirilemeyen bir UPN doğrulanmış bir etki alanı yok. Yalnızca kuruluşunuzun özel ağına içinde geçerlidir. Azure AD'de birincil etki alanı contoso.com ise, örneğin, contoso.local birincil etki alanında ise şirket içi AD ancak internet'in doğrulanabilir bir etki alanı değil ve kullanıcının yalnızca Contoso içinde kullanılan ağ.

Aşağıdaki tabloda Ayrıntılar desteği bu şirket için AD UPN, Windows 10 hibrit Azure AD'ye katılma sağlar

| Tür şirket içi AD UPN | Etki alanı türü | Windows 10 sürümü | Açıklama |
| ----- | ----- | ----- | ----- |
| Yönlendirilebilir | Federasyon | 1703 sürümünden | Genel kullanıma sunuldu |
| Yönlendirilebilir | Yönetilen | 1709 sürümü | Şu anda özel Önizleme aşamasındadır. Azure AD SSPR desteklenmiyor |
| Yönlendirilebilir olmayan | Federasyon | 1803 sürümü | Genel kullanıma sunuldu |
| Yönlendirilebilir olmayan | Yönetilen | Desteklenmiyor | |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Federasyon etki alanları için yapılandırma hibrit Azure Active Directory join](hybrid-azuread-join-federated-domains.md)
> [yapılandırma hibrit Azure Active Directory join yönetilen etki alanları için](hybrid-azuread-join-managed-domains.md)

<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
