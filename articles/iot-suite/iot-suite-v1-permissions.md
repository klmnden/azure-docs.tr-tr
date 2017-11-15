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
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 891752e2b600112b1e44cecc3c6e8e50215c597b
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
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
* Genel yönetici önceden yapılandırılmış bir çözüm sağlayabilir ve atanmış bir **yönetici** AAD kiracısının içindeki uygulama için rol.
* Aynı AAD kiracısındaki başka bir kullanıcı bir uygulama oluşturduğunda genel yöneticinin varsayılan rolü olan **salt okunur**.
* Genel yönetici kullanıcıları kullanarak uygulamalar için roller atayabilirsiniz [Azure portal][lnk-portal].

### <a name="domain-user"></a>Etki alanı kullanıcısı

AAD kiracısı başına çok sayıda etki alanı kullanıcı olabilir:

* Bir etki alanı kullanıcısı üzerinden önceden yapılandırılmış bir çözüm sağlayabilir [azureiotsuite.com] [ lnk-azureiotsuite] site. Varsayılan olarak, etki alanı kullanıcı izni **yönetici** sağlanan uygulama rolü.
* Bir etki alanı kullanıcısı build.cmd komut dosyasını kullanarak bir uygulama oluşturabilirsiniz [azure-iot-remote-monitoring][lnk-rm-github-repo], [azure-iot-predictive-maintenance] [ lnk-pm-github-repo], veya [azure-IOT-bağlı-Fabrika] [ lnk-cf-github-repo] deposu. Ancak, etki alanı kullanıcıya verilen varsayılan rol olan **ReadOnly**, bir etki alanı kullanıcı rolleri atamak için izni yok.
* AAD kiracısında başka bir kullanıcı bir uygulama oluşturduğunda, etki alanı kullanıcısı atanır **ReadOnly** rolü bu uygulama için varsayılan.
* Bir etki alanı kullanıcısı uygulamalar için roller atayamazsınız; Bu nedenle bunlar olsa bile bir etki alanı kullanıcısı kullanıcıları veya bir uygulama için kullanıcıların rollerini ekleyemezsiniz.

### <a name="guest-user"></a>Konuk kullanıcı

AAD kiracısı başına çok sayıda Konuk kullanıcı olabilir. Konuk kullanıcılar AAD kiracısında sınırlı bir haklar kümesine sahiptir. Sonuç olarak, konuk kullanıcılar AAD kiracısında önceden yapılandırılmış bir çözüm sağlayamaz.

Kullanıcılar ve roller aad'de hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure AD'de kullanıcı oluşturma][lnk-create-edit-users]
* [Kullanıcılar uygulamalara atama][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure abonelik yöneticisi rolleri

Azure yöneticisi rolleri bir Azure aboneliğini bir AD kiracısıyla eşleme özelliğini denetler.

Makaleyi Azure yönetici rolleri hakkında daha fazla bilgi edinin [Azure ortak yönetici, Hizmet Yöneticisi ve Hesap Yöneticisi ekleme veya değiştirme konusunda][lnk-admin-roles].

## <a name="application-roles"></a>Uygulama rolleri

Uygulama rolleri önceden yapılandırılmış çözümünüzdeki cihazlara erişimi denetler.

İki tanımlanmış rol ve sağlanan bir uygulamada bir örtük rol mevcuttur:

* **Yönetici:** ekleme, yönetme, cihazları kaldırın ve ayarlarını değiştirmek için tam denetime sahiptir.
* **Salt okunur:** aygıtlar, kurallar, Eylemler, işleri ve telemetri görüntüleyebilirsiniz.

Her rol için atanan izinler bulabilirsiniz [RolePermissions.cs] [ lnk-resource-cs] kaynak dosyası.

### <a name="changing-application-roles-for-a-user"></a>Bir kullanıcının uygulama rollerini değiştirme

Active Directory’de bir kullanıcıyı önceden yapılandırılmış çözümünüzün yöneticisi yapmak için aşağıdaki yordamı kullanabilirsiniz.

Bir kullanıcının rollerini değiştirmek için AAD genel yöneticisi olmanız gerekir:

1. [Azure Portalı][lnk-portal]’na gidin.
2. Seçin **Azure Active Directory**.
3. Çözümünüzü sağlarken azureiotsuite.com üzerinde seçtiğiniz dizin kullandığınızdan emin olun. Aboneliğinizle ilişkili birden fazla dizine sahipseniz, bunları hesap adınızı portalın sağ üst tıklatırsanız arasında geçiş yapabilirsiniz.
4. Tıklatın **kurumsal uygulamalar**, ardından **tüm uygulamaları**.
4. Göster **tüm uygulamaları** ile **herhangi** durumu. Ardından, önceden yapılandırılmış çözümünüz ada sahip bir uygulama arayın.
5. Önceden yapılandırılmış çözümünüzün adıyla eşleşen uygulamanın adına tıklayın.
6. tıklatın **kullanıcılar ve gruplar**.
7. Rollerini değiştirmek istediğiniz kullanıcıyı seçin.
8. **Ata**’ya tıklayın ve kullanıcıya atamak istediğiniz rolü (**Yönetici** gibi) seçip onay işaretine tıklayın.

## <a name="faq"></a>SSS

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Hizmet yöneticisiyim ve aboneliğim ile belirli bir AAD kiracısı arasında dizin eşlemesini değiştirmek istiyorum. Bu görevi nasıl tamamlamak?

1. Git [Klasik Azure portalı][lnk-classic-portal], tıklatın **ayarları** sol taraftaki Hizmet listesinde.
2. Dizin eşlemesini değiştirmek istediğiniz aboneliği seçin.
3. **Dizini Düzenle**’ye tıklayın.
4. Açılır listede kullanmak istediğiniz **Dizin**’i seçin. İleri okuna tıklayın.
5. Dizin eşlemesini ve etkilenen ortak yöneticileri onaylayın. Başka bir dizinden taşıyorsanız, özgün dizindeki tüm ortak Yöneticiler kaldırılır.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>AAD kiracısı üzerinde etki alanı kullanıcısı/üyesiyim ve önceden yapılandırılmış bir çözüm oluşturdum. Uygulamam için bana nasıl rol atanır?

AAD kiracısında genel yönetici yapın ve ardından roller kullanıcılara kendinize atamak için genel yönetici isteyin. Alternatif olarak, doğrudan bir rol atamasını isteyin. Önceden yapılandırılmış çözümünüzün dağıtıldığı AAD kiracısını değiştirmek isterseniz sonraki soruya bakın.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Uzaktan izlediğim önceden yapılandırılmış çözümümün ve uygulamamın atandığı AAD kiracısına nasıl geçiş yapabilirim?

<https://github.com/Azure/azure-iot-remote-monitoring> adresinden bir bulut dağıtımını yeniden çalıştırabilir ve yeni oluşturulan bir AAD kiracısı ile yeniden dağıtabilirsiniz. Varsayılan olarak, bir AAD kiracısı oluşturduğunuzda, bir genel yönetici olduğu için kullanıcı ekleyin ve bu kullanıcılara roller atama için izinlere sahip.

1. Bir AAD dizini oluşturun [Azure portal][lnk-portal].
2. <https://github.com/Azure/azure-iot-remote-monitoring> sayfasına gidin.
3. `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` komutunu çalıştırın (Örneğin, `build.cmd cloud debug myRMSolution`)
4. Sorulduğunda **tenantid** değerini önceki kiracınız yerine yeni oluşturduğunuz kiracı olarak ayarlayın.

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

[img-flowchart]: media/iot-suite-v1-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles-azure-portal.md
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
