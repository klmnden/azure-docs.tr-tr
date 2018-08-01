---
title: Karma yapılandırmak için Azure Active Directory'ye katılmış cihazlarda nasıl | Microsoft Docs
description: Hibrit Azure Active Directory'ye katılmış cihazlarda yapılandırmayı öğrenin.
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
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 2b74a85d866bcb2f67f61792d1afe2b590ffa418
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39369047"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Hibrit Azure Active Directory join uygulamanızı planlama

Benzer şekilde bir kullanıcı, cihaz, BT yöneticisi olarak korumak istediğiniz ve istediğiniz zamanda ve konumda kaynaklarınızı korumak için kullanabileceğiniz başka bir kimlik gelmektedir. Azure AD'ye cihaz kimliklerinizi taşıyarak bu hedefe gerçekleştirebilirsiniz. Bir Azure AD'ye katılım'ı yaparak bunu yapabilirsiniz, hibrit Azure AD'ye katılım'ı veya Azure AD cihaz durumu kaydedildi. Cihazlarınızı Azure AD'ye kullanıma sunulması sayesinde aynı anda koşullu etkinleştirerek, Bulut ve şirket kaynaklarına erişimi güvenli hale getirme ve Bulut ve şirket içi kaynaklarınız arasında çoklu oturum açma (SSO) keyfini, son kullanıcı üretkenliğini en üst düzeye elde Erişim. Daha fazla bilgi için Azure AD'de koşullu erişim bakın.

Bir şirket içi Active Directory ortamınız varsa ve etki alanına katılan cihazları Azure AD'ye istiyorsanız, bunu hibrit Azure AD'ye katılmış cihazları yapılandırarak gerçekleştirebilirsiniz. Bu makalede, ortamınızda katılın, hibrit Azure AD'ye uygulamak için ilgili adımları sağlar. 


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

Windows alt düzey cihazların kaydını dolaşımı veya gezici kullanıcı profili için yapılandırılan cihazlar için desteklenmiyor. Gezici profilleri veya ayarlarını FQDN'yi kullanıyorsanız, Windows 10 kullanın.

Windows etki alanı denetleyicisi (DC) rolünü çalıştıran sunucu kaydını desteklenmiyor.

Kuruluşunuzda giden kimliği doğrulanmış bir ara sunucu üzerinden Internet erişimi gerekiyorsa, Windows 10 bilgisayarlarınızı giden proxy sunucusuna başarıyla kimlik doğrulayabildiğini emin olmanız gerekir. Windows 10 bilgisayarlar, makine bağlamını kullanarak cihaz kaydı çalıştığından, makine bağlamını kullanarak giden bağlantı proxy'si kimlik doğrulamasını yapılandırmak gereklidir.

Hibrit Azure AD'ye katılma, Azure AD ile şirket içi etki alanına katılmış cihazlarınızı otomatik olarak kaydedilecek bir işlemdir. Otomatik olarak kaydetmek için tüm cihazlar burada istemediğiniz durumlar vardır. Bu sizin için doğru ise, cihazlarınızı hibrit Azure AD'ye katılımı denetlemek nasıl bakın.


Hibrit Azure AD'ye katılma, Azure AD ile şirket içi etki alanına katılmış cihazlarınızı otomatik olarak kaydedilecek bir işlemdir. Otomatik olarak kaydetmek için tüm cihazlar burada istemediğiniz durumlar vardır. Bu sizin için doğru olup olmadığını [cihazlarınızı hibrit Azure AD'ye katılma denetlemek nasıl](hybrid-azuread-join-control.md).



## <a name="select-your-scenario"></a>Senaryonuzu seçin

Hibrit Azure AD'ye katılım'ı aşağıdaki senaryolar için yapılandırabilirsiniz:

- Yönetilen etki alanları
- Federasyon etki alanları  



Ortamınızı etki alanları yönettiği, hibrit Azure AD'ye katılım'ı destekler:

- Kimlik doğrulaması (PTA) ile sorunsuz çoklu oturum açma (SSO) geçirin. 

- Parola Eşitleme (PHS) ile sorunsuz çoklu oturum açma (SSO) sahiptir. 

1.1.819.0 sürümünden başlayarak, Azure AD Connect ile hibrit Azure AD'ye katılım'ı yapılandırmak için bir sihirbaz sağlar. Sihirbaz, önemli ölçüde yapılandırma işlemini basitleştirmenize olanak sağlar. Daha fazla bilgi için bkz.

- [Federasyon etki alanları için hibrit Azure Active Directory katılımını Yapılandır](hybrid-azuread-join-federated-domains.md)

- [Yönetilen etki alanları için hibrit Azure Active Directory katılımını Yapılandır](hybrid-azuread-join-managed-domains.md)


 Azure AD Connect gerekli sürümünü yüklemek sizin için bir seçenek değilse, bkz. [el ile cihaz kaydını yapılandırmak nasıl](../device-management-hybrid-azuread-joined-devices-setup.md). 






## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Federasyon etki alanları için yapılandırma hibrit Azure Active Directory join](hybrid-azuread-join-federated-domains.md)
> [yapılandırma hibrit Azure Active Directory join yönetilen etki alanları için](hybrid-azuread-join-managed-domains.md)




<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png
