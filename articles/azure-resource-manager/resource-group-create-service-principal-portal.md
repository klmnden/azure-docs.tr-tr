---
title: "Portalda Azure uygulama kimliği oluşturma | Microsoft Docs"
description: "Yeni Azure Active Directory uygulaması ve Azure Kaynak Yöneticisi'nde rol tabanlı erişim denetimi ile kaynaklara erişimi yönetmek için kullanılabilir hizmet sorumlusu nasıl oluşturulacağını açıklar."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2017
ms.author: tomfitz
ms.openlocfilehash: 9b5b33f61021bf4b0ae238e88c2926c0d17b4929
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Bir Azure Active Directory Uygulama ve kaynaklarına erişebilir hizmet sorumlusu oluşturmak için Portal kullanın

Erişmek veya kaynak değiştirmek için gereken bir uygulamanız varsa, Azure Active Directory (AD) uygulama ayarlama ve gerekli izinler atayın. Bu yaklaşım, çünkü uygulama kendi kimlik bilgileri altında çalışırken için tercih edilir:

* Kendi izinlerinizi farklı uygulama kimliği için izinleri atayabilirsiniz. Genellikle, bu izinleri tam olarak hangi uygulama yapması gereken için kısıtlanır.
* Sizin Sorumluluklarınız uygulamanın kimlik bilgilerini değiştirirseniz gerekmez. 
* Katılımsız betik yürütülürken kimlik doğrulaması otomatikleştirmek için bir sertifika kullanabilirsiniz.

Bu makalede, portal üzerinden bu adımların nasıl gerçekleştirileceğini gösterir. Uygulama yalnızca bir kuruluş içinde çalıştırmak için burada hedeflenen bir tek kiracılı uygulama odaklanır. Genellikle tek Kiracı uygulamalar, kuruluşunuzun içinde çalışan satır iş kolu uygulamaları için kullanılır.

## <a name="required-permissions"></a>Gerekli izinler

Bu makalede tamamlamak için bir uygulamayı Azure AD kiracınıza ile kaydetmek için yeterli izinlere sahip ve Azure aboneliğinizde bir rolü uygulamaya atamanız gerekir. Şimdi bu adımları gerçekleştirmek için doğru izinlere sahip olduğunuzdan emin olun.

### <a name="check-azure-active-directory-permissions"></a>Azure Active Directory izinlerini denetleyin

1. Oturum açtığınızda Azure hesabınız üzerinden [Azure portal](https://portal.azure.com).

1. Seçin **Azure Active Directory**.

   ![Azure active Directory'yi seçin](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Azure Active Directory'de seçin **kullanıcı ayarlarını**.

   ![kullanıcı ayarlarını seçin](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Denetleme **uygulama kayıtlar** ayarı. Varsa kümesine **Evet**, yönetici olmayan kullanıcıların AD uygulamaları kaydolabilir. Bu ayar, Azure AD kiracısı içinde herhangi bir kullanıcı bir uygulama kaydedebilirsiniz anlamına gelir. Geçebilirsiniz [denetleyin Azure abonelik izinleri](#check-azure-subscription-permissions).

   ![Uygulama kayıtları görüntüle](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Ayarlama uygulama kayıtlar ayarlanmış ise **Hayır**, yalnızca yönetici kullanıcılar uygulamaların kaydedebilirsiniz. Hesabınıza bir Azure AD Kiracı yönetici olup olmadığını denetleyin. Seçin **genel bakış** ve **kullanıcı Bul** hızlı görevleri.

   ![Kullanıcı Bul](./media/resource-group-create-service-principal-portal/find-user.png)

1. Hesabınız için arama yapın ve bunu bulduğunuzda seçin.

   ![arama kullanıcı](./media/resource-group-create-service-principal-portal/show-user.png)

1. Hesabınız için seçin **dizin rolünü**.

   ![dizin rolü](./media/resource-group-create-service-principal-portal/select-directory-role.png)

1. Atanmış dizini rolünüze Azure AD'de görüntüleyin. Hesabınız için kullanıcı rolü atanmış, ancak uygulama kayıt ayarı (önceki adımlardan) yönetici kullanıcılara sınırlıdır, yöneticinize ya da atama, bir yönetici rolü için kullanıcıların uygulamaları kaydetmek isteyin.

   ![Görünüm rolü](./media/resource-group-create-service-principal-portal/view-role.png)

### <a name="check-azure-subscription-permissions"></a>Azure abonelik izinlerinizi denetleyin

Azure aboneliğinizde, hesabınızın olması gerekir `Microsoft.Authorization/*/Write` bir AD uygulamasını bir role atamak için erişim. Bu eylem aracılığıyla verilen [sahibi](../active-directory/role-based-access-built-in-roles.md#owner) rol veya [kullanıcı erişimi Yöneticisi](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) rol. Hesabınızı atanırsa **katkıda bulunan** rolü, yeterli izniniz yok. Hizmet sorumlusu rol atama çalışılırken bir hata alırsınız.

Abonelik izinlerinizi denetlemek için:

1. Zaten Azure AD hesabınızla önceki adımlardan aradığınız değil, seçin **Azure Active Directory** sol bölmeden.

1. Azure AD hesabınıza bulun. Seçin **genel bakış** ve **kullanıcı Bul** hızlı görevleri.

   ![Kullanıcı Bul](./media/resource-group-create-service-principal-portal/find-user.png)

1. Hesabınız için arama yapın ve bunu bulduğunuzda seçin.

   ![arama kullanıcı](./media/resource-group-create-service-principal-portal/show-user.png)

1. Seçin **Azure kaynaklarını**.

   ![kaynakları seçin](./media/resource-group-create-service-principal-portal/select-azure-resources.png)

1. Atanan rollerinizi görüntülemek ve bir AD uygulamasını bir role atamak için yeterli izinlere sahip olup belirleyin. Aksi durumda, kullanıcı erişimi Yöneticisi rolüne eklemek için abonelik yöneticinize başvurun. Aşağıdaki görüntüde, kullanıcının yeterli izinlere sahip anlamı sahibi rolüne iki abonelikler için kullanıcıya atanır.

   ![izinleri göster](./media/resource-group-create-service-principal-portal/view-assigned-roles.png)

## <a name="create-an-azure-active-directory-application"></a>Azure Active Directory Uygulama oluşturma

1. Oturum açtığınızda Azure hesabınız üzerinden [Azure portal](https://portal.azure.com).
1. Seçin **Azure Active Directory**.

   ![Azure active Directory'yi seçin](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Seçin **uygulama kayıtlar**.

   ![Uygulama kayıtlar seçin](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. Seçin **yeni uygulama kaydı**.

   ![Uygulama Ekle](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Uygulama için bir ad ve URL belirtin. Seçin **Web uygulaması / API** oluşturmak istediğiniz uygulama türü için. Kimlik bilgilerini oluşturulamıyor bir **yerel** uygulama; bu nedenle, bu tür için otomatik uygulama çalışmaz. Değerleri ayarladıktan sonra Seç **oluşturma**.

   ![Uygulama adı](./media/resource-group-create-service-principal-portal/create-app.png)

Uygulamanızı oluşturdunuz.

## <a name="get-application-id-and-authentication-key"></a>Uygulama kimliği ile kimlik doğrulama anahtarı alma

Program aracılığıyla oturum açarken uygulamanız ve bir kimlik doğrulama anahtarı kimliği gerekir. Bu değerleri almak için aşağıdaki adımları kullanın:

1. Gelen **uygulama kayıtlar** uygulamanızı Azure Active Directory'de seçin.

   ![Uygulama seçin](./media/resource-group-create-service-principal-portal/select-app.png)

1. Kopya **uygulama kimliği** ve uygulama kodunuzda saklayın. Bazı [örnek uygulamaları](#log-in-as-the-application) istemci kimliği olarak bu değer bakın

   ![İstemci kimliği](./media/resource-group-create-service-principal-portal/copy-app-id.png)

1. Bir kimlik doğrulama anahtarı oluşturmak için seçin **anahtarları**.

   ![anahtarları seçin](./media/resource-group-create-service-principal-portal/select-keys.png)

1. Anahtar ve bir süre anahtarı için bir açıklama belirtin. İşiniz bittiğinde, seçin **kaydetmek**.

   ![anahtarı Kaydet](./media/resource-group-create-service-principal-portal/save-key.png)

   Anahtar kaydedildikten sonra anahtar değeri görüntülenir. Daha sonra anahtar almak mümkün olmadığı için bu değeri kopyalayın. Anahtar değeri olarak uygulamadan oturum açmak için uygulama kimliği sağlayın. Burada, uygulamanızın alabildiği anahtar değer deposu.

   ![anahtar kaydedildi](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Kiracı Kimliğinizi alma

Program aracılığıyla oturum açarken, kimlik doğrulama isteği Kiracı Kimliğiyle geçmesi gerekir.

1. Seçin **Azure Active Directory**.

   ![Azure active Directory'yi seçin](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Kiracı Kimliği almak için seçin **özellikleri** Azure AD kiracınız için.

   ![Azure AD Özellikler'i seçin](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

1. Kopya **dizin kimliği**. Bu değer, Kiracı kimliğidir.

   ![Kiracı kimliği](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a>Uygulama rolü atayın

Aboneliğinizde kaynaklara erişmek için bir rol uygulamaya atamanız gerekir. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir rolleri hakkında bilgi edinmek için [RBAC: yerleşik roller](../active-directory/role-based-access-built-in-roles.md).

Abonelik, kaynak grubu ya da kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam devralınan izinleri. Örneğin, bir kaynak grubu için okuyucu rolüne uygulamaya ekleme kaynak grubu ve içerdiği tüm kaynaklar okuyabilir anlamına gelir.

1. Uygulamaya atamak istediğiniz kapsam düzeyine gidin. Örneğin, abonelik kapsamında bir rol atamak için seçin **abonelikleri**. Bunun yerine, bir kaynak grubu veya kaynak seçebilirsiniz.

   ![Abonelik seç](./media/resource-group-create-service-principal-portal/select-subscription.png)

1. Belirli bir abonelik (kaynak grubu veya kaynak) uygulama atamak için seçin.

   ![atama için abonelik seçin](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

1. Seçin **erişim denetimi (IAM)**.

   ![erişim seçin](./media/resource-group-create-service-principal-portal/select-access-control.png)

1. **Add (Ekle)** seçeneğini belirleyin.

   ![Select ekleme](./media/resource-group-create-service-principal-portal/select-add.png)

1. Uygulamaya atamak istediğiniz rolü seçin. Aşağıdaki resimde gösterildiği **okuyucu** rol.

   ![Rol Seç](./media/resource-group-create-service-principal-portal/select-role.png)

1. Uygulamanız için arayın ve seçin.

   ![uygulamayı arayın](./media/resource-group-create-service-principal-portal/search-app.png)

1. Seçin **kaydetmek** rol atama tamamlamak için. Listenin uygulamanızda bu kapsam için bir rolüne atanan kullanıcıların bakın.

## <a name="log-in-as-the-application"></a>Uygulamayı iş olarak oturum açın

Uygulamanız artık Azure Active Directory'de ayarlanır. Bir kimliği ve anahtarı olarak uygulamadan oturum açmak için kullanılacak var. Uygulama, belirli eylemleri sağlayan bir rol atanır. Farklı platformlarda üzerinden uygulama olarak oturum açma hakkında daha fazla bilgi için bkz:

* [PowerShell](resource-group-authenticate-service-principal.md#provide-credentials-through-powershell)
* [Azure CLI](resource-group-authenticate-service-principal-cli.md)
* [REST](/rest/api/#create-the-request)
* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Sonraki adımlar
* Çok kiracılı uygulamayı kurmak için bkz: [Geliştirici Kılavuzu'na yetkilendirme Azure Kaynak Yöneticisi API'si ile](resource-manager-api-authentication.md).
* Güvenlik ilkeleri belirtme hakkında bilgi edinmek için [Azure rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).  
* Verilen veya kullanıcılar için reddedilen kullanılabilir eylemler listesi için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../active-directory/role-based-access-control-resource-provider-operations.md).
