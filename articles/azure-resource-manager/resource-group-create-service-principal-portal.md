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
ms.openlocfilehash: 264befc6c60b87d41658b4da763e477fbb7e3f8c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Bir Azure Active Directory Uygulama ve kaynaklarına erişebilir hizmet sorumlusu oluşturmak için Portal kullanın

Erişmek veya kaynak değiştirmek için gereken kodu varsa, Azure Active Directory (AD) uygulama ayarlamanız gerekir. AD uygulaması için gerekli izinleri atayın. Bu yaklaşım kendi izinlerinizi farklı uygulama kimliği için izinler atayabilirsiniz çünkü uygulama kendi kimlik bilgileri altında çalışan için tercih edilir. Genellikle, bu izinleri tam olarak hangi uygulama yapması gereken için kısıtlanır.

Bu makalede, portal üzerinden bu adımların nasıl gerçekleştirileceğini gösterir. Uygulama yalnızca bir kuruluş içinde çalıştırmak için burada hedeflenen bir tek kiracılı uygulama odaklanır. Genellikle tek Kiracı uygulamalar, kuruluşunuzun içinde çalışan satır iş kolu uygulamaları için kullanılır.

> [!IMPORTANT]
> Bir hizmet sorumlusu oluşturmak yerine, uygulama kimliğiniz için Azure AD yönetilen hizmet kimliği kullanmayı düşünün. Azure AD MSI kod için bir kimlik oluşturma basitleştirir Azure Active Directory genel Önizleme özelliğidir. Kodunuzu Azure AD MSI destekleyen ve Azure Active Directory kimlik doğrulamasını destekleyen kaynaklarına erişen bir hizmette çalıştırıyorsa, Azure AD MSI sizin için daha iyi bir seçenektir. Hangi Hizmetleri şu anda, destek dahil olmak üzere Azure AD MSI hakkında daha fazla bilgi için bkz: [Azure kaynakları için Yönetilen hizmet kimliği](../active-directory/managed-service-identity/overview.md).

## <a name="required-permissions"></a>Gerekli izinler

Bu makalede tamamlamak için bir uygulamayı Azure AD kiracınıza ile kaydetmek için yeterli izinlere sahip ve Azure aboneliğinizde bir rolü uygulamaya atamanız gerekir. Şimdi bu adımları gerçekleştirmek için doğru izinlere sahip olduğunuzdan emin olun.

### <a name="check-azure-active-directory-permissions"></a>Azure Active Directory izinlerini denetleyin

1. Seçin **Azure Active Directory**.

   ![Azure active Directory'yi seçin](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Azure Active Directory'de seçin **kullanıcı ayarlarını**.

   ![kullanıcı ayarlarını seçin](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Denetleme **uygulama kayıtlar** ayarı. Varsa kümesine **Evet**, yönetici olmayan kullanıcıların AD uygulamaları kaydolabilir. Bu ayar, Azure AD kiracısı içinde herhangi bir kullanıcı bir uygulama kaydedebilirsiniz anlamına gelir. Geçebilirsiniz [denetleyin Azure abonelik izinleri](#check-azure-subscription-permissions).

   ![Uygulama kayıtları görüntüle](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Ayarlama uygulama kayıtlar ayarlanmış ise **Hayır**, yalnızca yönetici kullanıcılar uygulamaların kaydedebilirsiniz. Hesabınıza bir Azure AD Kiracı yönetici olup olmadığını denetleyin. Seçin **genel bakış** ve kullanıcı bilgilerinizi bakın. Hesabınız için kullanıcı rolü atanmış, ancak uygulama kayıt ayarı (önceki adımdaki) yönetici kullanıcılara sınırlıdır, yöneticinize ya da atama, bir yönetici rolü için kullanıcıların uygulamaları kaydetmek isteyin.

   ![Kullanıcı Bul](./media/resource-group-create-service-principal-portal/view-user-info.png)

### <a name="check-azure-subscription-permissions"></a>Azure abonelik izinlerinizi denetleyin

Azure aboneliğinizde, hesabınızın olması gerekir `Microsoft.Authorization/*/Write` bir AD uygulamasını bir role atamak için erişim. Bu eylem aracılığıyla verilen [sahibi](../active-directory/role-based-access-built-in-roles.md#owner) rol veya [kullanıcı erişimi Yöneticisi](../active-directory/role-based-access-built-in-roles.md#user-access-administrator) rol. Hesabınızı atanırsa **katkıda bulunan** rolü, yeterli izniniz yok. Hizmet sorumlusu rol atama çalışılırken bir hata alırsınız.

Abonelik izinlerinizi denetlemek için:

1. Sağ üst köşedeki hesabınızı seçin ve Seç **izinlerimi**.

   ![Kullanıcı izinleri seçin](./media/resource-group-create-service-principal-portal/select-my-permissions.png)

1. Aşağı açılan listeden aboneliği seçin. Seçin **ayrıntılarını Bu abonelik için tam erişim görüntülemek için buraya tıklayın**.

   ![Kullanıcı Bul](./media/resource-group-create-service-principal-portal/view-details.png)

1. Atanan rollerinizi görüntülemek ve bir AD uygulamasını bir role atamak için yeterli izinlere sahip olup belirleyin. Aksi durumda, kullanıcı erişimi Yöneticisi rolüne eklemek için abonelik yöneticinize başvurun. Aşağıdaki görüntüde, kullanıcıya kullanıcının yeterli izinlere sahip anlamına gelir sahibi rolüne atanır.

   ![izinleri göster](./media/resource-group-create-service-principal-portal/view-user-role.png)

## <a name="create-an-azure-active-directory-application"></a>Azure Active Directory Uygulama oluşturma

1. Oturum açtığınızda Azure hesabınız üzerinden [Azure portal](https://portal.azure.com).
1. Seçin **Azure Active Directory**.

   ![Azure active Directory'yi seçin](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Seçin **uygulama kayıtlar**.

   ![Uygulama kayıtlar seçin](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. Seçin **yeni uygulama kaydı**.

   ![Uygulama Ekle](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Uygulama için bir ad ve URL belirtin. Seçin **Web uygulaması / API** oluşturmak istediğiniz uygulama türü için. Kimlik bilgilerini oluşturulamıyor bir [yerel uygulama](../active-directory/active-directory-application-proxy-native-client.md); bu nedenle, türü için otomatik uygulama çalışmaz. Değerleri ayarladıktan sonra Seç **oluşturma**.

   ![Uygulama adı](./media/resource-group-create-service-principal-portal/create-app.png)

Uygulamanızı oluşturdunuz.

## <a name="get-application-id-and-authentication-key"></a>Uygulama kimliği ile kimlik doğrulama anahtarı alma

Program aracılığıyla oturum açarken uygulamanız ve bir kimlik doğrulama anahtarı kimliği gerekir. Bu değerleri almak için aşağıdaki adımları kullanın:

1. Gelen **uygulama kayıtlar** uygulamanızı Azure Active Directory'de seçin.

   ![Uygulama seçin](./media/resource-group-create-service-principal-portal/select-app.png)

1. Kopya **uygulama kimliği** ve uygulama kodunuzda saklayın. Bazı [örnek uygulamaları](#log-in-as-the-application) istemci kimliği olarak bu değer bakın

   ![İstemci kimliği](./media/resource-group-create-service-principal-portal/copy-app-id.png)

1. Bir kimlik doğrulama anahtarı oluşturmak için seçin **ayarları**.

   ![ayarlarını seçin](./media/resource-group-create-service-principal-portal/select-settings.png)

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

1. Varsayılan olarak, Azure Active Directory uygulamaları kullanılabilir seçenekleri görüntülenmiyor. Uygulamanızı bulmak için bunu arama alanındaki adı sağlamanız gerekir. Bunu seçin.

   ![uygulamayı arayın](./media/resource-group-create-service-principal-portal/search-app.png)

1. Seçin **kaydetmek** rol atama tamamlamak için. Listenin uygulamanızda bu kapsam için bir rolüne atanan kullanıcıların bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Çok kiracılı uygulamayı kurmak için bkz: [Geliştirici Kılavuzu'na yetkilendirme Azure Kaynak Yöneticisi API'si ile](resource-manager-api-authentication.md).
* Güvenlik ilkeleri belirtme hakkında bilgi edinmek için [Azure rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).  
* Verilen veya kullanıcılar için reddedilen kullanılabilir eylemler listesi için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../active-directory/role-based-access-control-resource-provider-operations.md).
