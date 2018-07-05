---
title: Azure IOT çözüm hızlandırıcıları ve Azure Active Directory | Microsoft Docs
description: Azure IOT Çözüm Hızlandırıcıları kullanma Azure Active Directory izinlerini yönetmek açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 676d5e553e2929ae09d447141ca315fd1cc448e3
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450270"
---
# <a name="permissions-on-the-azureiotsolutionscom-site"></a>Azureiotsolutions.com sitesindeki izinler

## <a name="what-happens-when-you-sign-in"></a>Oturum açtığınızda ne olur

İlk kez oturum açtığınızda, [azureiotsuite.com][lnk-azureiotsolutions], bağlı Azure aboneliği ve şu anda seçili olan Azure Active Directory (AAD) Kiracı üzerinde izin düzeyleri site belirler.

1. İlk olarak, kullanıcı adınızın yanında görülen kiracılar listesini doldurmak için site Azure'dan ait hangi AAD kiracılarına. Şu anda site aynı anda yalnızca tek bir kiracı için kullanıcı belirteci edinebilirsiniz. Sağ üst köşede açılır menüyü kullanarak kiracılar arasında geçiş yaptığınızda, bu nedenle, site size ilgili kiracının belirteçlerini almak için bu kiracıda kaydeder.

2. Site daha sonra, seçilen kiracı ile hangi aboneliğin ilişkilendirildiğini Azure’dan öğrenir. Yeni bir çözüm Hızlandırıcı oluşturduğunuzda kullanılabilir abonelikleri görürsünüz.

3. Son olarak, site tüm kaynakları abonelikler ve kaynak gruplarını Çözüm Hızlandırıcıları etiketlenmiş ve giriş sayfasındaki kutucukları doldurur alır.

Aşağıdaki bölümlerde erişimi denetleyen roller açıklanmıştır Çözüm Hızlandırıcıları için.

## <a name="aad-roles"></a>AAD rolleri

AAD rolleri, kullanıcılara ve Çözüm Hızlandırıcısı, bazı Azure Hizmetleri yönetme olanağı sağlama Çözüm Hızlandırıcıları için denetler.

AAD'de yönetici rolleri hakkında daha fazla bilgi bulabilirsiniz [Azure AD'de yönetici rolleri atama][lnk-aad-admin]. Bu makalede odaklanır **genel yönetici** ve **kullanıcı** Çözüm Hızlandırıcıları tarafından kullanılan dizin rolleri.

### <a name="global-administrator"></a>Genel yönetici

AAD kiracısı başına çok sayıda genel yönetici olabilir:

* Bir AAD kiracısı oluşturduğunuzda varsayılan olarak bu kiracının genel yöneticisi olursunuz.
* Genel yönetici (temel dağıtımı tek bir Azure sanal makine kullanır) bir temel ve standart Çözüm Hızlandırıcıları sağlayabilirsiniz.

### <a name="domain-user"></a>Etki alanı kullanıcısı

AAD kiracısı başına çok sayıda etki alanı kullanıcıları olabilir:

* Bir etki alanı kullanıcısı aracılığıyla temel çözüm Hızlandırıcısını sağlayabilirsiniz [azureiotsolutions.com] [ lnk-azureiotsolutions] site.
* Bir etki alanı kullanıcısı, CLI kullanarak temel bir çözüm Hızlandırıcısını oluşturabilirsiniz.

### <a name="guest-user"></a>Konuk kullanıcı

AAD kiracısı başına çok sayıda Konuk kullanıcı olabilir. Konuk kullanıcılar AAD kiracısında sınırlı bir haklar kümesine sahiptir. Sonuç olarak, Konuk kullanıcılar AAD kiracısında çözüm Hızlandırıcısını sağlayamazsınız.

Kullanıcılar ve AAD rolleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure AD'de kullanıcı oluşturma][lnk-create-edit-users]
* [Uygulamalar için kullanıcı atama][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure abonelik yöneticisi rolleri

Azure yöneticisi rolleri bir Azure aboneliği bir AAD kiracısıyla eşleme özelliğini denetler.

Bu makalede Azure yönetici rolleri hakkında daha fazla bilgi edinin [ekleme veya Azure ortak yönetici, Hizmet Yöneticisi ve Hesap Yöneticisi değiştirme][lnk-admin-roles].

## <a name="faq"></a>SSS

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Hizmet yöneticisiyim ve aboneliğim ile belirli bir AAD kiracısı arasında dizin eşlemesini değiştirmek istiyorum. Bu görevi nasıl tamamlamak?

Bkz: [Azure AD dizininize mevcut bir aboneliğe eklemek için](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organizational-account"></a>Bir Hizmet Yöneticisi veya bir kuruluş hesabıyla oturum açıldığında ortak yöneticiyi değiştirmek istiyorum

Destek makalesine bakın [Hizmet Yöneticisi değiştiriliyor ve bir kuruluş hesabıyla oturum açıldığında ortak yönetici][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Bu hatayı neden görüyorum? "Hesabınız bir çözüm oluşturmak için uygun izinlere sahip değil. Lütfen hesap yöneticinize başvurun veya farklı bir hesap ile deneyin."

Yönergeler için aşağıdaki çizime bakın:

![][img-flowchart]

> [!NOTE]
> Doğruladıktan sonra hatayı görmeye devam ederseniz olan AAD kiracısının genel Yöneticisi ve aboneliğin ortak yönetici, hesap yöneticinizden kullanıcıyı kaldırmasını ve gerekli izinleri şu sırayla yeniden atama vardır. İlk olarak, kullanıcının genel yönetici olarak ekleyin ve ardından kullanıcı Azure aboneliğinin ortak yönetici olarak ekleyin. Sorunlar devam ederse, başvurun [Yardım ve Destek][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Bir Azure aboneliğim varken bu hatayı neden görüyorum? "Azure aboneliğinin önceden yapılandırılmış çözümler oluşturmak için gereklidir. Ücretsiz bir deneme hesabı yalnızca birkaç dakika içinde oluşturabilirsiniz."

Bir Azure aboneliğinizin olduğundan eminseniz aboneliğinizin kiracı eşlemesini doğrulayın ve açılır menüden doğru kiracının seçildiğinden emin olun. İstenilen kiracının doğru olduğunu onayladıysanız yukarıdaki diyagramı izleyin ve aboneliğiniz ile bu AAD kiracısının eşlemesini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
IOT Çözüm Hızlandırıcıları hakkında daha fazla bilgi için bkz nasıl [çözüm Hızlandırıcısını özelleştirme][lnk-customize].

[img-flowchart]: media/iot-accelerators-permissions/flowchart.png

[lnk-azureiotsolutions]: https://www.azureiotsolutions.com
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]:../active-directory/users-groups-roles/directory-assign-admin-roles.md
[lnk-portal]: https://portal.azure.com
[lnk-create-edit-users]:../active-directory/fundamentals/active-directory-users-profile-azure-portal.md
[lnk-assign-app-roles]:../active-directory/manage-apps/assign-user-or-group-access-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-accelerators-remote-monitoring-customize.md
