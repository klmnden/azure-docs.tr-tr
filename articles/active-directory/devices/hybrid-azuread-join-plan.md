---
title: Hibrit Azure Active Directory'ye katılmış cihazları yapılandırma | Microsoft Docs
description: Hibrit Azure Active Directory'ye katılmış cihazları elle nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 28344ac7c50b48b472ba6f907b116b3b202de454
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238806"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Hibrit Azure Active Directory join uygulamanızı planlama

Kullanıcıya benzer şekilde bir cihaz, korumak istediğiniz ve her yerde ve zamanda kaynaklarınızı korumak için kullandığınız başka bir kimlik alır. Aşağıdaki yöntemlerden biri ile cihazlarınızın kimliklerini Azure AD'ye getirerek bu hedefi gerçekleştirebilirsiniz:

- Azure AD'ye katılım
- Hibrit Azure AD'ye katılım
- Azure AD kaydı

Cihazlarınızı Azure AD'ye taşıyarak, çoklu oturum açma (SSO) özelliği sayesinde bulut ve şirket içi kaynaklarınız genelinde kullanıcılarınızın üretkenliğini en üst düzeye çıkarırsınız. Ayrıca, [koşullu erişim](../active-directory-conditional-access-azure-portal.md) ile bulut ve şirket içi kaynaklarınıza erişimin güvenliği sağlayabilirsiniz.

Şirket içi Active Directory ortamınız varsa ve etki alanınıza katılmış cihazları Azure AD'ye katmak istiyorsanız hibrit Azure AD'ye katılmış cihazları yapılandırarak bunu gerçekleştirebilirsiniz. Bu makalede, ortamınızda katılın, hibrit Azure AD'ye uygulamak için ilgili adımları sağlar. 


## <a name="prerequisites"></a>Önkoşullar

Bu makalede, aşina olduğunuzu varsayar [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md).


## <a name="plan-your-implementation"></a>Uygulamanızı planlama

Karma Azure AD uygulamanız planlamak için ile kendinizi alıştırın:

|   |   |
|---|---|
|![İşaretli][1]|Cihazları gözden geçir desteklenir|
|![İşaretli][1]|Gözden geçirme bilmeniz gerekenler|
|![İşaretli][1]|Senaryonuzu seçin|


 


## <a name="review-supported-devices"></a>Cihazları gözden geçir desteklenir 

Hibrit Azure AD'ye katılım geniş Windows cihazları destekler. Yapılandırmanın daha eski Windows sürümleri çalıştıran cihazlar için ek veya bunlardan farklı adımlar gerektiğinden, desteklenen cihazları iki kategorilere ayrılır:

**Windows cihazları**

- Windows 10
    
- Windows Server 2016


Windows masaüstü işletim sistemini çalıştıran cihazlar için desteklenen sürüm Windows 10 Yıldönümü Güncelleştirmesi (sürüm 1607) olan veya üzeri. En iyi uygulama, Windows 10 'un en son sürüme yükseltin.



 **Windows alt düzey cihazları**

- Windows 8.1
 
- Windows 7

- Windows Server 2012 R2
 
- Windows Server 2012 
 
- Windows Server 2008 R2 


İlk planlama adım, ortamınızı gözden geçirin ve Windows alt düzey cihazları desteklemek gerekli olup olmadığını belirleyin.



## <a name="review-things-you-should-know"></a>Gözden geçirme bilmeniz gerekenler

Ortamınızı kimlik verilerini birden fazla Azure AD kiracınıza eşitlenmiş tek bir ormanda bulunuyorsa, hibrit Azure AD'ye katılım kullanamazsınız.

Sistem hazırlığı Aracı (Sysprep) FQDN'yi kullanıyorsanız, bir hibrit Azure AD'ye katılım için yapılandırılmamış bir Windows yüklemesinden görüntüleri'ı oluşturduğunuzdan emin olun.

Ek sanal makineler oluşturmak için bir sanal makine (VM) üzerinde anlık görüntü FQDN'yi kullanıyorsanız, hibrit Azure AD'ye katılım için yapılandırılmamış bir VM anlık görüntüsü kullandığınızdan emin olun.

Windows alt düzey cihazların hibrit Azure AD'ye katılma:

- **Olan** Federasyon olmayan ortamlarda desteklenen [Azure Active Directory sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start). 

- **Değil** sorunsuz çoklu oturum açma olmadan Azure AD geçişli kimlik doğrulaması kullanılırken desteklenir.

- **Değil** dolaşımı veya gezici kullanıcı profili kullanırken veya Sanal Masaüstü Altyapısı (VDI) kullanılırken desteklenmez.


Windows etki alanı denetleyicisi (DC) rolünü çalıştıran sunucu kaydını desteklenmiyor.

Kuruluşunuz, kimliği doğrulanmış bir giden bağlantı proxy'si aracılığıyla İnternete erişimi gerektiriyorsa Windows 10 bilgisayarlarınızın giden bağlantı proxy'sine başarıyla kimlik doğrulayabildiğinden emin olmanız gerekir. Windows 10 bilgisayarlar makine bağlamını kullanarak cihaz kaydını çalıştırdığından makine bağlamı ile giden bağlantı proxy'sinin yapılandırılması gerekir.


Hibrit Azure AD'ye katılma, Azure AD ile şirket içi etki alanına katılmış cihazlarınızı otomatik olarak kaydedilecek bir işlemdir. Otomatik olarak kaydetmek için tüm cihazlar burada istemediğiniz durumlar vardır. Bu sizin için doğru olup olmadığını [cihazlarınızı hibrit Azure AD'ye katılma denetlemek nasıl](hybrid-azuread-join-control.md).



## <a name="select-your-scenario"></a>Senaryonuzu seçin

Hibrit Azure AD'ye katılım'ı aşağıdaki senaryolar için yapılandırabilirsiniz:

- Yönetilen etki alanları
- Federasyon etki alanları  



Ortamınızı etki alanları yönettiği, hibrit Azure AD'ye katılım'ı destekler:

- Kimlik doğrulaması (PTA) ile sorunsuz çoklu oturum açma (SSO) geçirin. 

- Sorunsuz çoklu oturum açma (SSO) ile parola karma eşitlemesi (PHS) 

1.1.819.0 sürümünden itibaren Azure AD Connect hibrit Azure AD'ye katılımı yapılandırmak için bir sihirbaz sağlar. Sihirbaz, yapılandırma işlemini önemli ölçüde basitleştirebilmenizi sağlar. Daha fazla bilgi için bkz.

- [Federasyon etki alanları için hibrit Azure Active Directory'ye katılmayı yapılandırma](hybrid-azuread-join-federated-domains.md)


- [Yönetilen etki alanları için hibrit Azure Active Directory'ye katılmayı yapılandırma](hybrid-azuread-join-managed-domains.md)


 Azure AD Connect gerekli sürümünü yüklemek sizin için bir seçenek değilse, bkz. [el ile cihaz kaydını yapılandırmak nasıl](../device-management-hybrid-azuread-joined-devices-setup.md). 






## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Federasyon etki alanları için yapılandırma hibrit Azure Active Directory join](hybrid-azuread-join-federated-domains.md)
> [yapılandırma hibrit Azure Active Directory join yönetilen etki alanları için](hybrid-azuread-join-managed-domains.md)




<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
