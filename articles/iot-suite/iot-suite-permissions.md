---
title: Azure IOT paketi ve Azure Active Directory | Microsoft Docs
description: "Azure IoT Paketinin izinleri yönetmek için Azure Active Directory’i nasıl kullandığını açıklar."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: ee7acd393539277a5bc23a6fed40d07961974b9a
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="permissions-on-the-azureiotsuitecom-site"></a>azureiotsuite.com sitesindeki izinler

## <a name="what-happens-when-you-sign-in"></a>Oturum açtığınızda ne olur

İlk kez oturum açtığınızda, [azureiotsuite.com][lnk-azureiotsuite], sahip şu anda seçili olan Azure Active Directory (AAD) kiracısına ve Azure aboneliğine göre izin düzeyleri site belirler.

1. İlk olarak, kullanıcı adınızın yanında görülen kiracılar listesini doldurmak için site Azure'dan hangi AAD kiracılarına ait olduğunuz öğrenir. Şu anda, site aynı anda yalnızca bir kiracı için kullanıcı belirteçleri alabilir. Sağ üst köşedeki açılır menüyü kullanarak kiracılar geçiş yaptığınızda, bu nedenle, site size ilgili kiracının belirteçlerini almak için bu kiracıda kaydeder.

2. Site daha sonra, seçilen kiracı ile hangi aboneliğin ilişkilendirildiğini Azure’dan öğrenir. Önceden yapılandırılmış yeni bir çözüm oluşturduğunuzda kullanılabilir abonelikleri görürsünüz.

3. Site son olarak önceden yapılandırılmış çözümler olarak etiketlenmiş abonelikler ve kaynak gruplarındaki tüm kaynakları alır ve giriş sayfasındaki kutucukları doldurur.

Aşağıdaki bölümlerde önceden yapılandırılmış çözümlere erişimi denetleyen roller açıklanmıştır.

## <a name="aad-roles"></a>AAD rolleri

AAD rolleri önceden yapılandırılmış çözümleri sağlama özelliğini denetler ve önceden yapılandırılmış bir çözümdeki kullanıcıları yönetir.

AAD'de yönetici rolleri hakkında daha fazla bilgi bulabilirsiniz [Azure AD'de yönetici rolleri atama][lnk-aad-admin]. Geçerli makaleyi odaklanır **genel yönetici** ve **kullanıcı** olarak önceden yapılandırılmış çözümler tarafından kullanılan directory rolleri.

### <a name="global-administrator"></a>Genel yönetici

AAD kiracısı başına çok sayıda genel yönetici olabilir:

* Bir AAD kiracısı oluşturduğunuzda varsayılan olarak bu kiracının genel yöneticisi olursunuz.
* Genel yönetici temel ve standart bir önceden yapılandırılmış çözümü sağlayabilirsiniz.

### <a name="domain-user"></a>Etki alanı kullanıcısı

AAD kiracısı başına çok sayıda etki alanı kullanıcı olabilir:

* Bir etki alanı kullanıcısı üzerinden temel önceden yapılandırılmış bir çözüm sağlayabilir [azureiotsuite.com] [ lnk-azureiotsuite] site.
* Bir etki alanı kullanıcısı CLI kullanarak temel önceden yapılandırılmış bir çözüm oluşturabilirsiniz.

### <a name="guest-user"></a>Konuk kullanıcı

AAD kiracısı başına çok sayıda Konuk kullanıcı olabilir. Konuk kullanıcılar AAD kiracısında sınırlı bir haklar kümesine sahiptir. Sonuç olarak, konuk kullanıcılar AAD kiracısında önceden yapılandırılmış bir çözüm sağlayamaz.

Kullanıcılar ve roller aad'de hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure AD'de kullanıcı oluşturma][lnk-create-edit-users]
* [Kullanıcılar uygulamalara atama][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure abonelik yöneticisi rolleri

Azure yöneticisi rolleri bir Azure aboneliğini bir AD kiracısıyla eşleme özelliğini denetler.

Makaleyi Azure yönetici rolleri hakkında daha fazla bilgi edinin [Azure ortak yönetici, Hizmet Yöneticisi ve Hesap Yöneticisi ekleme veya değiştirme konusunda][lnk-admin-roles].

## <a name="faq"></a>SSS

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Hizmet yöneticisiyim ve aboneliğim ile belirli bir AAD kiracısı arasında dizin eşlemesini değiştirmek istiyorum. Bu görevi nasıl tamamlamak?

Bkz: [Azure AD dizininiz için mevcut bir aboneliğe eklemek için](../active-directory/active-directory-how-subscriptions-associated-directory.md#to-add-an-existing-subscription-to-your-azure-ad-directory)

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Şirket hesabıyla oturum açtığımda bir Hizmet Yöneticisini veya Ortak Yöneticiyi değiştirmek istiyorum

Destek makalesine bakın [değiştirme Hizmet Yöneticisi ve bir hesabıyla oturum açtığımda ortak yöneticiyi][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Bu hatayı neden görüyorum? "Hesabınız bir çözüm oluşturmak için uygun izinlere sahip değil. Lütfen hesap yöneticinize başvurun veya farklı bir hesap ile deneyin."

Yönergeler için aşağıdaki çizime bakın:

![][img-flowchart]

> [!NOTE]
> Görmeye devam ederseniz, doğrulama sonra hata: AAD kiracısında genel yönetici ve abonelikte ortak yönetici, hesap yöneticinizden kullanıcıyı kaldırmasını ve gerekli izinleri şu sırayla yeniden atamanız gerekir. İlk olarak, kullanıcı genel yönetici olarak ekleyin ve ardından Azure aboneliğinde ortak yönetici olarak kullanıcı ekleyin. Sorun devam ederse başvurun [Yardım ve Destek][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Bir Azure aboneliğim varken bu hatayı neden görüyorum? "Azure aboneliği önceden yapılandırılmış çözümler oluşturmak için gereklidir. Ücretsiz bir deneme hesabı yalnızca birkaç dakika içinde oluşturabileceğiniz."

Bir Azure aboneliğinizin olduğundan eminseniz aboneliğinizin kiracı eşlemesini doğrulayın ve açılır menüden doğru kiracının seçildiğinden emin olun. Doğrulanacak istenilen kiracının doğru olduğunu, yukarıdaki diyagramı izleyin ve aboneliğiniz ile bu AAD kiracısının eşlemesini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
IOT paketi hakkında bilgi almaya devam etmek için işlemine bakın [önceden yapılandırılmış çözümü özelleştirme][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
