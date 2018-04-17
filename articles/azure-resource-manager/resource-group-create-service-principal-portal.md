---
title: Portalda Azure uygulama kimliği oluşturma | Microsoft Docs
description: Yeni Azure Active Directory uygulaması ve Azure Kaynak Yöneticisi'nde rol tabanlı erişim denetimi ile kaynaklara erişimi yönetmek için kullanılabilir hizmet sorumlusu nasıl oluşturulacağını açıklar.
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
ms.date: 03/21/2018
ms.author: tomfitz
ms.openlocfilehash: bbda406633f97d9a6c90bc49374268df28b68f2a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Bir Azure Active Directory Uygulama ve kaynaklarına erişebilir hizmet sorumlusu oluşturmak için Portal kullanın

Erişmek veya kaynak değiştirmek için gereken kodu varsa, Azure Active Directory (AD) uygulama ayarlamanız gerekir. AD uygulaması için gerekli izinleri atayın. Bu yaklaşım kendi izinlerinizi farklı uygulama kimliği için izinler atayabilirsiniz çünkü uygulama kendi kimlik bilgileri altında çalışan için tercih edilir. Genellikle, bu izinleri tam olarak hangi uygulama yapması gereken için kısıtlanır.

Bu makalede, portal üzerinden bu adımların nasıl gerçekleştirileceğini gösterir. Uygulama yalnızca bir kuruluş içinde çalıştırmak için burada hedeflenen bir tek kiracılı uygulama odaklanır. Genellikle tek Kiracı uygulamalar, kuruluşunuzun içinde çalışan satır iş kolu uygulamaları için kullanılır.

> [!IMPORTANT]
> Bir hizmet sorumlusu oluşturmak yerine, uygulama kimliğiniz için Azure AD yönetilen hizmet kimliği kullanmayı düşünün. Azure AD MSI kod için bir kimlik oluşturma basitleştirir Azure Active Directory genel Önizleme özelliğidir. Kodunuzu Azure AD MSI destekleyen ve Azure Active Directory kimlik doğrulamasını destekleyen kaynaklarına erişen bir hizmette çalıştırıyorsa, Azure AD MSI sizin için daha iyi bir seçenektir. Hangi Hizmetleri şu anda, destek dahil olmak üzere Azure AD MSI hakkında daha fazla bilgi için bkz: [Azure kaynakları için Yönetilen hizmet kimliği](../active-directory/managed-service-identity/overview.md).

## <a name="required-permissions"></a>Gerekli izinler

Bu makalede tamamlamak için bir uygulamayı Azure AD kiracınıza ile kaydetmek için yeterli izinlere sahip ve Azure aboneliğinizde bir rolü uygulamaya atamanız gerekir. Şimdi bu adımları gerçekleştirmek için doğru izinlere sahip olduğunuzdan emin olun.

### <a name="check-azure-active-directory-permissions"></a>Azure Active Directory izinlerini denetleyin

1. **Azure Active Directory**'yi seçin.

   ![azure active directory'yi seçme](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Azure Active Directory'de seçin **kullanıcı ayarlarını**.

   ![kullanıcı ayarlarını seçin](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Denetleme **uygulama kayıtlar** ayarı. Varsa kümesine **Evet**, yönetici olmayan kullanıcıların AD uygulamaları kaydolabilir. Bu ayar, Azure AD kiracısı içinde herhangi bir kullanıcı bir uygulama kaydedebilirsiniz anlamına gelir. Geçebilirsiniz [denetleyin Azure abonelik izinleri](#check-azure-subscription-permissions).

   ![Uygulama kayıtları görüntüle](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Ayar uygulama kayıtlar ayarlanmış ise **Hayır**, yalnızca [genel Yöneticiler](../active-directory/active-directory-assign-admin-roles-azure-portal.md) uygulamaları kaydedebilirsiniz. Hesabınıza bir Azure AD Kiracı yönetici olup olmadığını denetleyin. Seçin **genel bakış** ve kullanıcı bilgilerinizi bakın. Hesabınız için kullanıcı rolü atanmış, ancak uygulama kayıt ayarı (önceki adımdaki) yönetici kullanıcılara sınırlıdır, yöneticinize ya da atamak, size, genel Yönetici rolüne veya kullanıcıların uygulamaları kaydetmek isteyin.

   ![Kullanıcı Bul](./media/resource-group-create-service-principal-portal/view-user-info.png)

### <a name="check-azure-subscription-permissions"></a>Azure abonelik izinlerinizi denetleyin

Azure aboneliğinizde, hesabınızın olması gerekir `Microsoft.Authorization/*/Write` bir AD uygulamasını bir role atamak için erişim. Bu eylem aracılığıyla verilen [sahibi](../role-based-access-control/built-in-roles.md#owner) rol veya [kullanıcı erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) rol. Hesabınızı atanırsa **katkıda bulunan** rolü, yeterli izniniz yok. Hizmet sorumlusu rol atama çalışılırken bir hata alırsınız.

Abonelik izinlerinizi denetlemek için:

1. Sağ üst köşedeki hesabınızı seçin ve Seç **izinlerimi**.

   ![Kullanıcı izinleri seçin](./media/resource-group-create-service-principal-portal/select-my-permissions.png)

1. Aşağı açılan listeden aboneliği seçin. Seçin **ayrıntılarını Bu abonelik için tam erişim görüntülemek için buraya tıklayın**.

   ![Kullanıcı Bul](./media/resource-group-create-service-principal-portal/view-details.png)

1. Atanan rollerinizi görüntülemek ve bir AD uygulamasını bir role atamak için yeterli izinlere sahip olup belirleyin. Aksi durumda, kullanıcı erişimi Yöneticisi rolüne eklemek için abonelik yöneticinize başvurun. Aşağıdaki görüntüde, kullanıcıya kullanıcının yeterli izinlere sahip anlamına gelir sahibi rolüne atanır.

   ![izinleri göster](./media/resource-group-create-service-principal-portal/view-user-role.png)

## <a name="create-an-azure-active-directory-application"></a>Azure Active Directory Uygulama oluşturma

1. Oturum açtığınızda Azure hesabınız üzerinden [Azure portal](https://portal.azure.com).
1. **Azure Active Directory**'yi seçin.

   ![azure active directory'yi seçme](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. **Uygulama kayıtları**'nı seçin.

   ![uygulama kayıtlarını seçme](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. **Yeni uygulama kaydı**’nı seçin.

   ![uygulama ekleme](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Uygulama için bir ad ve URL sağlayın. Oluşturmak istediğiniz uygulama türü olarak **Web uygulaması / API**'yi seçin. Kimlik bilgilerini oluşturulamıyor bir [yerel uygulama](../active-directory/active-directory-application-proxy-native-client.md); bu nedenle, türü için otomatik uygulama çalışmaz. Değerleri ayarladıktan sonra Seç **oluşturma**.

   ![uygulamayı adlandırma](./media/resource-group-create-service-principal-portal/create-app.png)

Uygulamanızı oluşturdunuz.

## <a name="get-application-id-and-authentication-key"></a>Uygulama kimliği ile kimlik doğrulama anahtarı alma

Programlamayla oturum açılırken, uygulamanızın kimliği ve kimlik doğrulama anahtarı gerekir. Bu değerleri almak için aşağıdaki adımları kullanın:

1. Azure Active Directory'deki **Uygulama kayıtları**'nda uygulamanızı seçin.

   ![uygulama seçme](./media/resource-group-create-service-principal-portal/select-app.png)

1. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın. Bazı [örnek uygulamalar](#log-in-as-the-application) istemci kimliği olarak bu değere başvurur.

   ![istemci kimliği](./media/resource-group-create-service-principal-portal/copy-app-id.png)

1. Kimlik doğrulama anahtarını oluşturmak için **Ayarlar**'ı seçin.

   ![ayarları seçme](./media/resource-group-create-service-principal-portal/select-settings.png)

1. Kimlik doğrulama anahtarını oluşturmak için **Anahtarlar**'ı seçin.

   ![anahtarları seçme](./media/resource-group-create-service-principal-portal/select-keys.png)

1. Anahtar için bir açıklama ve süre sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.

   ![anahtarı kaydetme](./media/resource-group-create-service-principal-portal/save-key.png)

   Anahtar kaydedildikten sonra, anahtarın değeri görüntülenir. Bu değeri kopyalayın çünkü daha sonra anahtarı alamazsınız. Uygulama olarak uygulama kimliğiyle oturum açarken anahtar değerini sağlamanız gerekir. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

   ![kaydedilen anahtar](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Kiracı kimliğini alma

Programlamayla oturum açılırken, kimlik doğrulama isteğinizle birlikte kiracı kimliğini geçirmeniz gerekir.

1. **Azure Active Directory**'yi seçin.

   ![azure active directory'yi seçme](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Kiracı kimliğini almak için Azure AD kiracınızda **Özellikler**'i seçin.

   ![Azure AD özelliklerini seçme](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

1. **Dizin kimliği**'ni kopyalayın. Bu değer kiracı kimliğinizdir.

   ![kiracı kimliği](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a>Uygulama rolü atayın

Aboneliğinizde kaynaklara erişmek için bir rol uygulamaya atamanız gerekir. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir rolleri hakkında bilgi edinmek için [RBAC: yerleşik roller](../role-based-access-control/built-in-roles.md).

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

1. Varsayılan olarak, Azure Active Directory uygulamaları kullanılabilir seçenekleri görüntülenmiyor. Uygulamanızı bulmak için bunu arama alanındaki adı sağlamanız gerekir. Bunu seçin.

   ![uygulamayı arayın](./media/resource-group-create-service-principal-portal/search-app.png)

1. Seçin **kaydetmek** rol atama tamamlamak için. Listenin uygulamanızda bu kapsam için bir rolüne atanan kullanıcıların bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Çok kiracılı uygulamayı kurmak için bkz: [Geliştirici Kılavuzu'na yetkilendirme Azure Kaynak Yöneticisi API'si ile](resource-manager-api-authentication.md).
* Güvenlik ilkeleri belirtme hakkında bilgi edinmek için [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).  
* Verilen veya kullanıcılar için reddedilen kullanılabilir eylemler listesi için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md).
