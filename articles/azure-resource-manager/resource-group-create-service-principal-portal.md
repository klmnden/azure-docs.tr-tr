---
title: Portalda Azure uygulama kimliği oluşturma | Microsoft Docs
description: Yeni Azure Active Directory uygulaması ve Azure Resource Manager rol tabanlı erişim denetimi ile kaynaklara erişimi yönetmek için kullanılan hizmet sorumlusu oluşturmayı açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2018
ms.author: tomfitz
ms.openlocfilehash: 2f053f6dd98b9f4e97d69e51bce933a003633277
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2018
ms.locfileid: "46497952"
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Bir Azure Active Directory uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturmak için portalı kullanma

Erişim ya da kaynakları değiştirmek için gereken kodu varsa, Azure Active Directory (AD) uygulama ayarlamanız gerekir. Ardından, AD uygulamasına gerekli izinleri atayabilirsiniz. Bu yaklaşım, kendi izinlerinizi farklı uygulama kimliğine izin atayabilirsiniz olmadığından, uygulama kendi kimlik bilgileriniz altında çalışan için tercih edilir. Normalde, bu izinler tam olarak uygulamaya gereken izinlerle sınırlı olur.

Bu makalede, portal üzerinden bu adımların nasıl gerçekleştirileceğini gösterir. Tek kiracılı bir uygulama yalnızca bir kuruluş içinde çalıştırmak için uygulamayı nerede yöneliktir odaklanır. Genellikle tek kiracılı uygulamalar kuruluşunuzda çalışan satır iş kolu uygulamaları için kullanırsınız.

> [!IMPORTANT]
> Bir hizmet sorumlusu oluşturmak yerine, uygulama kimliğiniz için Azure kaynakları için yönetilen kimliklerle göz önünde bulundurun. Kodunuzu Yönetilen kimlikleri ve Azure Active Directory kimlik doğrulamasını destekleyen erişimleri kaynak destekleyen bir hizmeti çalıştıran yönetilen kimlikleri sizin için daha iyi bir seçenek vardır. Hangi şu anda, Destek Hizmetleri dahil olmak üzere, Azure kaynakları için yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikleri nedir?](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="required-permissions"></a>Gerekli izinler

Bu makaleyi tamamlamak için bir uygulama ile Azure AD kiracınızı kaydetmek için yeterli izinlere sahip ve Azure aboneliğinizde uygulama rolü atayın. Şimdi bu adımları gerçekleştirmek için doğru izinlere sahip olduğunuzdan emin olun.

### <a name="check-azure-active-directory-permissions"></a>Azure Active Directory izinlerini denetleme

1. **Azure Active Directory**'yi seçin.

   ![azure active directory'yi seçme](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Azure Active Directory’de **Kullanıcı ayarları**’nı seçin.

   ![kullanıcı ayarlarını seçin](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Denetleme **uygulama kayıtları** ayarı. Varsa kümesine **Evet**, yönetici olmayan kullanıcılar, AD uygulamalarını kaydedebilir. Bu ayar, Azure AD kiracısı içindeki herhangi bir kullanıcının bir uygulamayı kaydedebileceği anlamına gelir. Geçebilirsiniz [denetleyin Azure abonelik izinleri](#check-azure-subscription-permissions).

   ![Uygulama kayıtları görüntüle](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

1. Uygulama kayıtları ayarı ayarlanırsa **Hayır**, yalnızca [genel Yöneticiler](../active-directory/users-groups-roles/directory-assign-admin-roles.md) uygulamalar kaydolabilir. Hesabınız Azure AD kiracısı için yönetici olup olmadığını denetleyin. Seçin **genel bakış** ve kullanıcı bilgilerinizi bakın. Hesabınız için kullanıcı rolü atanmış, ancak uygulama kayıt ayarının (önceki adımdaki) yönetici kullanıcıyla sınırlıdır, ya da yöneticinizi atamak, size, genel Yönetici rolüne ya da uygulamaları kaydetme olanağı isteyin.

   ![Kullanıcı Bul](./media/resource-group-create-service-principal-portal/view-user-info.png)

### <a name="check-azure-subscription-permissions"></a>Azure aboneliği izinlerini denetleyin

Azure aboneliğinizde, hesabınızın olması gerekir `Microsoft.Authorization/*/Write` bir AD uygulamasını bir role atamak için erişim. Bu eylemin izni, [Sahip](../role-based-access-control/built-in-roles.md#owner) rolüyle veya [Kullanıcı Erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) rolüyle verilir. Hesabınızı atanırsa **katkıda bulunan** rolü, yeterli izniniz yok. Hizmet sorumlusu, rol atama çalışılırken bir hata alırsınız.

Abonelik izinlerinizi denetlemek için:

1. Sağ üst köşedeki hesabınızı seçin ve seçin **İzinlerim**.

   ![Kullanıcı izinleri seçin](./media/resource-group-create-service-principal-portal/select-my-permissions.png)

1. Aşağı açılan listeden aboneliği seçin. Seçin **tam erişim görüntülemek için burayı tıklatın, bu abonelik için ayrıntılı**.

   ![Kullanıcı Bul](./media/resource-group-create-service-principal-portal/view-details.png)

1. Atanmış olan rolleri görüntülemek ve bir AD uygulamasını bir role atamak için yeterli izinlere sahip olup olmadığını belirler. Aksi durumda, kullanıcı erişimi yöneticisi rolü eklemek için abonelik yöneticinize başvurun. Aşağıdaki görüntüde, kullanıcının kullanıcı yeterli izne sahip olduğu anlamına gelir sahip rolü atanır.

   ![izinleri göster](./media/resource-group-create-service-principal-portal/view-user-role.png)

## <a name="create-an-azure-active-directory-application"></a>Bir Azure Active Directory uygulaması oluşturma

1. Üzerinden Azure hesabınızla oturum açın [Azure portalında](https://portal.azure.com).
1. **Azure Active Directory**'yi seçin.

   ![azure active directory'yi seçme](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. **Uygulama kayıtları**'nı seçin.

   ![uygulama kayıtlarını seçme](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. **Yeni uygulama kaydı**’nı seçin.

   ![uygulama ekleme](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Uygulama için bir ad ve URL sağlayın. Oluşturmak istediğiniz uygulama türü olarak **Web uygulaması / API**'yi seçin. Kimlik bilgileri oluşturulamıyor bir [yerel uygulama](../active-directory/manage-apps/application-proxy-configure-native-client-application.md); bu nedenle, türü için otomatik uygulama çalışmaz. Değerleri ayarladıktan sonra seçin **Oluştur**.

   ![uygulamayı adlandırma](./media/resource-group-create-service-principal-portal/create-app.png)

Uygulamanızı oluşturdunuz.

## <a name="get-application-id-and-authentication-key"></a>Uygulama kimliği ve kimlik doğrulama anahtarını alma

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

   Anahtar kaydedildikten sonra, anahtarın değeri görüntülenir. Daha sonra anahtarı alamazsınız mümkün olmadığından, bu değeri kopyalayın. Uygulama kimliği ile bir uygulama olarak oturum açmak için anahtar değerini sağlayın. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

   ![kaydedilen anahtar](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="get-tenant-id"></a>Kiracı kimliğini alma

Programlamayla oturum açılırken, kimlik doğrulama isteğinizle birlikte kiracı kimliğini geçirmeniz gerekir.

1. **Azure Active Directory**'yi seçin.

   ![azure active directory'yi seçme](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Kiracı kimliğini almak için Azure AD kiracınızda **Özellikler**'i seçin.

   ![Azure AD özelliklerini seçme](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

1. **Dizin kimliği**'ni kopyalayın. Bu değer kiracı kimliğinizdir.

   ![kiracı kimliği](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

## <a name="assign-application-to-role"></a>Uygulama rolü atama

Aboneliğinizdeki kaynaklara erişmek için uygulamaya bir rol atamanız gerekir. Uygulama için doğru izinlere rolünü karar verin. Kullanılabilir roller hakkında bilgi edinmek için [RBAC: yerleşik roller](../role-based-access-control/built-in-roles.md).

Abonelik, kaynak grubu veya kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam için izinler devralınmıştır. Örneğin, bir kaynak grubu için okuyucu rolüne uygulamaya ekleme kaynak grubunu ve içerdiği tüm kaynakları okuyun anlamına gelir.

1. Uygulamayı atamak istediğiniz kapsam düzeyine gidin. Örneğin abonelik kapsamında bir rol atamak için seçin **abonelikleri**. Bunun yerine, bir kaynak grubu veya kaynak seçebilirsiniz.

   ![Abonelik seçin](./media/resource-group-create-service-principal-portal/select-subscription.png)

1. Belirli bir abonelikte (kaynak grubu veya kaynak) uygulama atamak için seçin.

   ![Abonelik atama için seçin](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

1. Seçin **erişim denetimi (IAM)**.

   ![erişim seçin](./media/resource-group-create-service-principal-portal/select-access-control.png)

1. **Add (Ekle)** seçeneğini belirleyin.

   ![Ekle'yi seçin](./media/resource-group-create-service-principal-portal/select-add.png)

1. Uygulamayı atamak istediğiniz rolü seçin. Uygulamayı yürütme gibi eylemleri izin vermek üzere **yeniden**, **Başlat** ve **Durdur** örnekleri olmalıdır rolü **katkıdabulunan**. Aşağıdaki görüntüde **okuyucu** rol.

   ![rol seçin](./media/resource-group-create-service-principal-portal/select-role.png)

1. Varsayılan olarak, Azure Active Directory uygulamaları kullanılabilir seçenekleri görüntülenmiyor. Uygulamanızı bulmak için arama alanına bu adı sağlamanız gerekir. Bunu seçin.

   ![Uygulama arayın](./media/resource-group-create-service-principal-portal/search-app.png)

1. Seçin **Kaydet** rol atama tamamlanması. Bu kapsam için bir role atanmış kullanıcı listesinde uygulamanızı görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar
* Bir çok kiracılı uygulamayı kurmak için bkz: [Azure Resource Manager API ile yetkilendirme için Geliştirici Kılavuzu](resource-manager-api-authentication.md).
* Güvenlik ilkeleri belirtme hakkında bilgi edinmek için [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).  
* Verilen veya engellenen kullanıcılara kullanılabilir eylemleri listesi için bkz. [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md).
