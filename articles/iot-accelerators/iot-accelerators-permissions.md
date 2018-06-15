---
title: Azure IOT çözüm hızlandırıcıları ve Azure Active Directory | Microsoft Docs
description: Azure IOT Çözüm Hızlandırıcıları kullanma Azure Active Directory izinleri yönetmek için açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: aa4e1d1f78549a8d1955def7a1c57e61d405e347
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35297147"
---
# <a name="permissions-on-the-azureiotsolutionscom-site"></a>Azureiotsolutions.com sitesindeki izinler

## <a name="what-happens-when-you-sign-in"></a>Oturum açtığınızda ne olur

İlk kez oturum açtığınızda, [azureiotsuite.com][lnk-azureiotsolutions], sahip şu anda seçili olan Azure Active Directory (AAD) kiracısına ve Azure aboneliğine göre izin düzeyleri site belirler.

1. İlk olarak, kullanıcı adınızın yanında görülen kiracılar listesini doldurmak için site Azure'dan hangi AAD kiracılarına ait olduğunuz öğrenir. Şu anda, site aynı anda yalnızca bir kiracı için kullanıcı belirteçleri alabilir. Sağ üst köşedeki açılır menüyü kullanarak kiracılar geçiş yaptığınızda, bu nedenle, site size ilgili kiracının belirteçlerini almak için bu kiracıda kaydeder.

2. Site daha sonra, seçilen kiracı ile hangi aboneliğin ilişkilendirildiğini Azure’dan öğrenir. Yeni bir çözüm Hızlandırıcısı oluşturduğunuzda kullanılabilir abonelikleri görürsünüz.

3. Son olarak, site tüm kaynakları abonelikler ve kaynak grupları Çözüm Hızlandırıcıları etiketlenir ve giriş sayfasındaki kutucukları doldurur alır.

Aşağıdaki bölümlerde erişimi denetleyen roller açıklanmıştır Çözüm Hızlandırıcıları için.

## <a name="aad-roles"></a>AAD rolleri

AAD rolleri kullanıcılar ve bazı Azure Çözüm Hızlandırıcısı hizmetleri yönetmek için sağlama Çözüm Hızlandırıcıları yeteneği denetler.

AAD'de yönetici rolleri hakkında daha fazla bilgi bulabilirsiniz [Azure AD'de yönetici rolleri atama][lnk-aad-admin]. Geçerli makaleyi odaklanır **genel yönetici** ve **kullanıcı** olarak Çözüm Hızlandırıcıları tarafından kullanılan directory rolleri.

### <a name="global-administrator"></a>Genel yönetici

AAD kiracısı başına çok sayıda genel yönetici olabilir:

* Bir AAD kiracısı oluşturduğunuzda varsayılan olarak bu kiracının genel yöneticisi olursunuz.
* Genel yönetici (temel dağıtımı tek bir Azure sanal makine kullanır) temel ve standart Çözüm Hızlandırıcıları sağlayabilirsiniz.

### <a name="domain-user"></a>Etki alanı kullanıcısı

AAD kiracısı başına çok sayıda etki alanı kullanıcı olabilir:

* Bir etki alanı kullanıcısı temel Çözüm Hızlandırıcısı aracılığıyla sağlayabilirsiniz [azureiotsolutions.com] [ lnk-azureiotsolutions] site.
* Bir etki alanı kullanıcısı CLI kullanarak bir temel Çözüm Hızlandırıcısı oluşturabilirsiniz.

### <a name="guest-user"></a>Konuk kullanıcı

AAD kiracısı başına çok sayıda Konuk kullanıcı olabilir. Konuk kullanıcılar AAD kiracısında sınırlı bir haklar kümesine sahiptir. Sonuç olarak, Konuk kullanıcılar AAD kiracısında Çözüm Hızlandırıcısı sağlayamazsınız.

Kullanıcılar ve roller aad'de hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure AD'de kullanıcı oluşturma][lnk-create-edit-users]
* [Kullanıcılar uygulamalara atama][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure abonelik yöneticisi rolleri

Azure yöneticisi rolleri bir Azure aboneliği için bir AAD kiracısı eşleme özelliğini denetler.

Makaleyi Azure yönetici rolleri hakkında daha fazla bilgi edinin [Azure ortak yönetici, Hizmet Yöneticisi ve Hesap Yöneticisi ekleme veya değiştirme konusunda][lnk-admin-roles].

## <a name="faq"></a>SSS

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Hizmet yöneticisiyim ve aboneliğim ile belirli bir AAD kiracısı arasında dizin eşlemesini değiştirmek istiyorum. Bu görevi nasıl tamamlamak?

Bkz: [Azure AD dizininiz için mevcut bir aboneliğe eklemek için](../active-directory/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organizational-account"></a>Bir Hizmet Yöneticisi veya kurumsal bir hesapla oturum ortak yöneticiyi değiştirmek istiyorum

Destek makalesine bakın [değiştirme Hizmet Yöneticisi ve bir kuruluş hesabıyla oturum açıldığında ortak yönetici][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Bu hatayı neden görüyorum? "Hesabınız bir çözüm oluşturmak için uygun izinlere sahip değil. Lütfen hesap yöneticinize başvurun veya farklı bir hesap ile deneyin."

Yönergeler için aşağıdaki çizime bakın:

![][img-flowchart]

> [!NOTE]
> Görmeye devam ederseniz, doğrulama sonra hata: AAD kiracısında genel Yöneticisi ve aboneliğin ortak yöneticisiyseniz, hesap yöneticinizden kullanıcıyı kaldırmasını ve gerekli izinleri şu sırayla yeniden atamanız gerekir. İlk olarak, kullanıcı genel yönetici olarak ekleyin ve sonra kullanıcı bir Azure aboneliğinin ortak Yöneticisi ekleyin. Sorun devam ederse başvurun [Yardım ve Destek][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Bir Azure aboneliğim varken bu hatayı neden görüyorum? "Azure aboneliği önceden yapılandırılmış çözümler oluşturmak için gereklidir. Ücretsiz bir deneme hesabı yalnızca birkaç dakika içinde oluşturabileceğiniz."

Bir Azure aboneliğinizin olduğundan eminseniz aboneliğinizin kiracı eşlemesini doğrulayın ve açılır menüden doğru kiracının seçildiğinden emin olun. Doğrulanacak istenilen kiracının doğru olduğunu, yukarıdaki diyagramı izleyin ve aboneliğiniz ile bu AAD kiracısının eşlemesini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
IOT Çözüm Hızlandırıcıları hakkında bilgi almaya devam etmek için bkz nasıl [bir çözüm Hızlandırıcısı özelleştirme][lnk-customize].

[img-flowchart]: media/iot-accelerators-permissions/flowchart.png

[lnk-azureiotsolutions]: https://www.azureiotsolutions.com
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles-azure-portal.md
[lnk-portal]: https://portal.azure.com
[lnk-create-edit-users]: ../active-directory/active-directory-users-profile-azure-portal.md
[lnk-assign-app-roles]:../active-directory/manage-apps/assign-user-or-group-access-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-accelerators-remote-monitoring-customize.md
