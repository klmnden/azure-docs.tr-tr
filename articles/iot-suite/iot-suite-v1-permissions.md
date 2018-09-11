---
title: Azure IOT paketi ve Azure Active Directory | Microsoft Docs
description: Azure IoT Paketinin izinleri yönetmek için Azure Active Directory’i nasıl kullandığını açıklar.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: a56d535ee06a097c7c18bcd507c25708f6a33f91
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296933"
---
# <a name="permissions-on-the-azureiotsuitecom-site"></a>azureiotsuite.com sitesindeki izinler

## <a name="what-happens-when-you-sign-in"></a>Oturum açtığınızda ne olur

İlk kez oturum açtığınızda, [azureiotsuite.com][lnk-azureiotsuite], bağlı Azure aboneliği ve şu anda seçili olan Azure Active Directory (AAD) Kiracı üzerinde izin düzeyleri site belirler.

1. İlk olarak, kullanıcı adınızın yanında görülen kiracılar listesini doldurmak için site Azure'dan ait hangi AAD kiracılarına. Şu anda site aynı anda yalnızca tek bir kiracı için kullanıcı belirteci edinebilirsiniz. Sağ üst köşede açılır menüyü kullanarak kiracılar arasında geçiş yaptığınızda, bu nedenle, site size ilgili kiracının belirteçlerini almak için bu kiracıda kaydeder.

2. Site daha sonra, seçilen kiracı ile hangi aboneliğin ilişkilendirildiğini Azure’dan öğrenir. Önceden yapılandırılmış yeni bir çözüm oluşturduğunuzda kullanılabilir abonelikleri görürsünüz.

3. Site son olarak önceden yapılandırılmış çözümler olarak etiketlenmiş abonelikler ve kaynak gruplarındaki tüm kaynakları alır ve giriş sayfasındaki kutucukları doldurur.

Aşağıdaki bölümlerde önceden yapılandırılmış çözümlere erişimi denetleyen roller açıklanmıştır.

## <a name="aad-roles"></a>AAD rolleri

AAD rolleri önceden yapılandırılmış çözümleri sağlama özelliğini denetler ve önceden yapılandırılmış bir çözümdeki kullanıcıları yönetir.

AAD'de yönetici rolleri hakkında daha fazla bilgi bulabilirsiniz [Azure AD'de yönetici rolleri atama][lnk-aad-admin]. Bu makalede odaklanır **genel yönetici** ve **kullanıcı** olarak önceden yapılandırılmış çözümlerde kullanılan dizin rolleri.

### <a name="global-administrator"></a>Genel yönetici

AAD kiracısı başına çok sayıda genel yönetici olabilir:

* Bir AAD kiracısı oluşturduğunuzda varsayılan olarak bu kiracının genel yöneticisi olursunuz.
* Genel yönetici önceden yapılandırılmış bir çözüm sağlayabilir ve atanmış bir **yönetici** rolü, AAD kiracılarının içindeki uygulama için.
* Aynı AAD kiracısındaki başka bir kullanıcı bir uygulama oluşturur genel yöneticinin varsayılan rolü varsa, **salt okunur**.
* Genel yönetici kullanıcıları kullanarak uygulamalar için roller atayabilirsiniz [Azure portalında][lnk-portal].

### <a name="domain-user"></a>Etki alanı kullanıcısı

AAD kiracısı başına çok sayıda etki alanı kullanıcıları olabilir:

* Bir etki alanı kullanıcısı ile önceden yapılandırılmış bir çözüm sağlayabilir [azureiotsuite.com] [ lnk-azureiotsuite] site. Varsayılan olarak, etki alanı kullanıcısı verilir **yönetici** sağlanan uygulama rolü.
* Bir etki alanı kullanıcısı build.cmd komut dosyasını kullanarak bir uygulama oluşturabilirsiniz [azure-IOT-remote-monitoring][lnk-rm-github-repo], [azure-IOT-Tahmine dayalı Bakım] [ lnk-pm-github-repo], veya [azure-iot-connected-factory] [ lnk-cf-github-repo] depo. Ancak, etki alanı kullanıcısı olarak verilen varsayılan rol olduğu **salt okunur**, bir etki alanı kullanıcı rolleri atamak için izni yok.
* AAD kiracısında başka bir kullanıcı bir uygulama oluşturursa, etki alanı kullanıcısı atanır **salt okunur** rolü bu uygulama için varsayılan.
* Bir etki alanı kullanıcısı, uygulamalar için rol atama yapılamaz; Bu nedenle bunlar sağlanmış olsa bile bir etki alanı kullanıcısı kullanıcı veya uygulamanın kullanıcılar için rolleri eklenemiyor.

### <a name="guest-user"></a>Konuk kullanıcı

AAD kiracısı başına çok sayıda Konuk kullanıcı olabilir. Konuk kullanıcılar AAD kiracısında sınırlı bir haklar kümesine sahiptir. Sonuç olarak, konuk kullanıcılar AAD kiracısında önceden yapılandırılmış bir çözüm sağlayamaz.

Kullanıcılar ve AAD rolleri hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure AD'de kullanıcı oluşturma][lnk-create-edit-users]
* [Uygulamalar için kullanıcı atama][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure abonelik yöneticisi rolleri

Azure yöneticisi rolleri bir Azure aboneliğini bir AD kiracısıyla eşleme özelliğini denetler.

Bu makalede Azure yönetici rolleri hakkında daha fazla bilgi edinin [ekleme veya değiştirme Azure aboneliği yöneticileri][lnk-admin-roles].

## <a name="application-roles"></a>Uygulama rolleri

Uygulama rolleri önceden yapılandırılmış çözümünüzdeki cihazlara erişimi denetler.

İki tanımlanmış rol ve sağlanan uygulamasında bir örtük rol mevcuttur:

* **Yönetici:** ekleme, yönetme, cihazları kaldırın ve ayarlarını değiştirmek için tam denetime sahiptir.
* **Salt okunur:** cihazlar, kuralları, Eylemler, işleri ve telemetri görüntüleyebilirsiniz.

Her rolüne atanan izinleri bulabilirsiniz [RolePermissions.cs] [ lnk-resource-cs] kaynak dosyası.

### <a name="changing-application-roles-for-a-user"></a>Bir kullanıcının uygulama rollerini değiştirme

Active Directory’de bir kullanıcıyı önceden yapılandırılmış çözümünüzün yöneticisi yapmak için aşağıdaki yordamı kullanabilirsiniz.

Bir kullanıcının rollerini değiştirmek için AAD genel yöneticisi olmanız gerekir:

1. [Azure Portalı][lnk-portal]’na gidin.
2. **Azure Active Directory**'yi seçin.
3. Çözümünüzü sağlarken azureiotsuite.com üzerinde seçtiğiniz dizini kullandığınızdan emin olun. Aboneliğinizle ilişkili birden fazla dizine sahipseniz, hesap adınız portalın üst sağ tıklayın, bunlar arasında geçiş yapabilirsiniz.
4. Tıklayın **kurumsal uygulamalar**, ardından **tüm uygulamaları**.
4. Göster **tüm uygulamaları** ile **herhangi** durumu. Ardından bir uygulama adı ile önceden yapılandırılmış çözümünüzün arayın.
5. Önceden yapılandırılmış çözümünüzün adıyla eşleşen uygulamanın adına tıklayın.
6. **Kullanıcılar ve gruplar**’a tıklayın.
7. Rollerini değiştirmek istediğiniz kullanıcıyı seçin.
8. **Ata**’ya tıklayın ve kullanıcıya atamak istediğiniz rolü (**Yönetici** gibi) seçip onay işaretine tıklayın.

## <a name="faq"></a>SSS

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Hizmet yöneticisiyim ve aboneliğim ile belirli bir AAD kiracısı arasında dizin eşlemesini değiştirmek istiyorum. Bu görevi nasıl tamamlamak?

Bkz: [Azure AD dizininize mevcut bir aboneliğe eklemek için](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory)

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>AAD kiracısı üzerinde etki alanı kullanıcısı/üyesiyim ve önceden yapılandırılmış bir çözüm oluşturdum. Uygulamam için bana nasıl rol atanır?

AAD kiracısı üzerinde genel Yöneticisi olun ve ardından roller kullanıcılara kendinize atamak için genel yönetici isteyin. Alternatif olarak, doğrudan bir rol atamak için genel yönetici isteyin. Önceden yapılandırılmış çözümünüzün dağıtıldığı AAD kiracısını değiştirmek isterseniz sonraki soruya bakın.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Uzaktan izlediğim önceden yapılandırılmış çözümümün ve uygulamamın atandığı AAD kiracısına nasıl geçiş yapabilirim?

Bir bulut dağıtımını çalıştırabileceğiniz <https://github.com/Azure/azure-iot-remote-monitoring> ve yeni oluşturulan bir AAD kiracısı ile yeniden dağıtın. Olduğundan bir genel yönetici, bir AAD kiracısı oluşturduğunuzda varsayılan olarak kullanıcılar eklemek ve bu kullanıcılara roller atama için izinlere sahip.

1. Bir AAD dizini oluşturun [Azure portalında][lnk-portal].
2. <https://github.com/Azure/azure-iot-remote-monitoring> kısmına gidin.
3. `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` komutunu çalıştırın (Örneğin, `build.cmd cloud debug myRMSolution`)
4. Sorulduğunda **tenantid** değerini önceki kiracınız yerine yeni oluşturduğunuz kiracı olarak ayarlayın.

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Şirket hesabıyla oturum açtığımda bir Hizmet Yöneticisini veya Ortak Yöneticiyi değiştirmek istiyorum

Destek makalesine bakın [Hizmet Yöneticisi değiştiriliyor ve bir kuruluş hesabıyla oturum açıldığında ortak yönetici][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Bu hatayı neden görüyorum? "Hesabınız bir çözüm oluşturmak için uygun izinlere sahip değil. Lütfen hesap yöneticinize başvurun veya farklı bir hesap ile deneyin."

Yönergeler için aşağıdaki çizime bakın:

![][img-flowchart]

> [!NOTE]
> Doğruladıktan sonra hatayı görmeye devam ederseniz olan AAD kiracısında genel yönetici ve abonelikte ortak yönetici, hesap yöneticinizden kullanıcıyı kaldırmasını ve gerekli izinleri şu sırayla yeniden atama vardır. İlk olarak, kullanıcı genel yönetici olarak ekleyin ve sonra da Azure aboneliğinin ortak yönetici olarak kullanıcı ekleyin. Sorunlar devam ederse, başvurun [Yardım ve Destek][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>Bir Azure aboneliğim varken bu hatayı neden görüyorum? "Azure aboneliğinin önceden yapılandırılmış çözümler oluşturmak için gereklidir. Ücretsiz bir deneme hesabı yalnızca birkaç dakika içinde oluşturabilirsiniz."

Bir Azure aboneliğinizin olduğundan eminseniz aboneliğinizin kiracı eşlemesini doğrulayın ve açılır menüden doğru kiracının seçildiğinden emin olun. İstenilen kiracının doğru olduğunu onayladıysanız yukarıdaki diyagramı izleyin ve aboneliğiniz ile bu AAD kiracısının eşlemesini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
IOT paketi hakkında daha fazla bilgi için bkz nasıl [önceden yapılandırılmış çözümü özelleştirme][lnk-customize].

[img-flowchart]: media/iot-suite-v1-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]:../active-directory/manage-apps/assign-user-or-group-access-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
