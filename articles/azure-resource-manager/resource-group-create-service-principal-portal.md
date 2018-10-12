---
title: Portalda Azure uygulama kimliği oluşturma | Microsoft Docs
description: Yeni Azure Active Directory uygulaması ve Azure Resource Manager rol tabanlı erişim denetimi ile kaynaklara erişimi yönetmek için kullanılan hizmet sorumlusu oluşturmayı açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/11/2018
ms.author: tomfitz
ms.openlocfilehash: 5233cde48d52e34960e87d3362b8cfaf3a0722c0
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49113742"
---
# <a name="use-portal-to-create-an-azure-active-directory-application-and-service-principal-that-can-access-resources"></a>Bir Azure Active Directory uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturmak için portalı kullanma

Erişim ya da kaynakları değiştirmek için gereken kodu varsa, uygulama için bir kimlik oluşturabilirsiniz. Bu kimlik, hizmet sorumlusu olarak bilinir. Ardından, hizmet sorumlusuna gerekli izinleri atayabilirsiniz. Bu makalede hizmet sorumlusu oluşturmak için portalı kullanmayı gösterir. Tek kiracılı bir uygulama yalnızca bir kuruluş içinde çalıştırmak için uygulamayı nerede yöneliktir odaklanır. Genellikle tek kiracılı uygulamalar kuruluşunuzda çalışan satır iş kolu uygulamaları için kullanırsınız.

> [!IMPORTANT]
> Bir hizmet sorumlusu oluşturmak yerine, uygulama kimliğiniz için Azure kaynakları için yönetilen kimliklerle göz önünde bulundurun. Kodunuzu Yönetilen kimlikleri ve Azure Active Directory kimlik doğrulamasını destekleyen erişimleri kaynak destekleyen bir hizmeti çalıştıran yönetilen kimlikleri sizin için daha iyi bir seçenek vardır. Hangi şu anda, Destek Hizmetleri dahil olmak üzere, Azure kaynakları için yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikleri nedir?](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="create-an-azure-active-directory-application"></a>Bir Azure Active Directory uygulaması oluşturma

Şimdi, doğrudan kimlik oluşturmaya geçin. Bir sorunla karşılaştıysanız denetleyin [gerekli izinler](#required-permissions) hesabınızın kimliğini oluşturabilirsiniz emin olmak için.

1. Üzerinden Azure hesabınızla oturum açın [Azure portalında](https://portal.azure.com).
1. **Azure Active Directory**'yi seçin.

   ![Azure active Directory'yi seçin](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. **Uygulama kayıtları**'nı seçin.

   ![Uygulama kayıtlarını seçme](./media/resource-group-create-service-principal-portal/select-app-registrations.png)

1. Seçin **+ yeni uygulama kaydı**.

   ![Uygulama ekleme](./media/resource-group-create-service-principal-portal/select-add-app.png)

1. Uygulama için bir ad ve URL sağlayın. Oluşturmak istediğiniz uygulama türü olarak **Web uygulaması / API**'yi seçin. Kimlik bilgileri oluşturulamıyor bir [yerel uygulama](../active-directory/manage-apps/application-proxy-configure-native-client-application.md). Bu tür için otomatik uygulama kullanamazsınız. Değerleri ayarladıktan sonra seçin **Oluştur**.

   ![uygulamayı adlandırma](./media/resource-group-create-service-principal-portal/create-app.png)

Azure Active Directory uygulaması ve hizmet sorumlusu oluşturmuş oldunuz.

## <a name="assign-application-to-role"></a>Uygulama rolü atama

Aboneliğinizdeki kaynaklara erişmek için uygulamaya bir rol atamanız gerekir. Uygulama için doğru izinlere hangi rolü sunar karar verin. Kullanılabilir roller hakkında bilgi edinmek için [RBAC: yerleşik roller](../role-based-access-control/built-in-roles.md).

Abonelik, kaynak grubu veya kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam için izinler devralınmıştır. Örneğin, bir kaynak grubu için okuyucu rolüne uygulamaya ekleme kaynak grubunu ve içerdiği tüm kaynakları okuyun anlamına gelir.

1. Uygulamayı atamak istediğiniz kapsam düzeyine gidin. Örneğin abonelik kapsamında bir rol atamak için seçin **tüm hizmetleri** ve **abonelikleri**.

   ![Abonelik seçme](./media/resource-group-create-service-principal-portal/select-subscription.png)

1. Uygulamaya atamak için belirli bir abonelik seçin.

   ![Abonelik atama için seçin](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

   Aradığınız bir abonelik görmüyorsanız seçin **genel abonelik filtresi**. Seçili için portalda istediğiniz aboneliği olduğundan emin olun. 

1. Seçin **erişim denetimi (IAM)**.

   ![erişim seçin](./media/resource-group-create-service-principal-portal/select-access-control.png)

1. **+ Ekle** öğesini seçin.

   ![Ekle'yi seçin](./media/resource-group-create-service-principal-portal/select-add.png)

1. Uygulamayı atamak istediğiniz rolü seçin. Uygulamayı izin vermek gibi eylemleri yürütme **yeniden**, **Başlat** ve **Durdur** örnekleri, select **katkıda bulunan** rol. Varsayılan olarak, Azure Active Directory uygulamaları kullanılabilir seçenekleri görüntülenmiyor. Uygulamanızı bulmak için adını arayın ve seçin.

   ![Rol seç](./media/resource-group-create-service-principal-portal/select-role.png)

1. Seçin **Kaydet** rol atama tamamlanması. Bu kapsam için bir role atanmış kullanıcı listesinde uygulamanızı görürsünüz.

Hizmet sorumlunuzu ayarlayın. Betikleri veya uygulamalarını çalıştırmak için kullanmaya başlayabilirsiniz. Sonraki bölümde programlamayla oturum açılırken gerekli değerleri alma işlemi gösterilmektedir.

## <a name="get-values-for-signing-in"></a>Oturum açmak için değerlerini alma

### <a name="get-tenant-id"></a>Kiracı kimliğini alma

Programlamayla oturum açılırken, kimlik doğrulama isteğinizle birlikte Kiracı Kimliğini geçirmeniz gerekir.

1. **Azure Active Directory**'yi seçin.

   ![Azure active Directory'yi seçin](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Seçin **özellikleri**.

   ![Azure AD özelliklerini seçme](./media/resource-group-create-service-principal-portal/select-ad-properties.png)

1. Kopyalama **dizin kimliği** Kiracı kimliğinizi almak için

   ![Kiracı Kimliği](./media/resource-group-create-service-principal-portal/copy-directory-id.png)

### <a name="get-application-id-and-authentication-key"></a>Uygulama kimliği ve kimlik doğrulama anahtarını alma

Ayrıca, uygulamanızın ve kimlik doğrulama anahtarı için kimliği gerekir. Bu değerleri almak için aşağıdaki adımları kullanın:

1. Azure Active Directory'deki **Uygulama kayıtları**'nda uygulamanızı seçin.

   ![Uygulama seçme](./media/resource-group-create-service-principal-portal/select-app.png)

1. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın.

   ![İstemci Kimliği](./media/resource-group-create-service-principal-portal/copy-app-id.png)

1. Seçin **ayarları**.

   ![ayarları seçin](./media/resource-group-create-service-principal-portal/select-settings.png)

1. **Anahtarlar**’ı seçin.

   ![anahtarları seçin](./media/resource-group-create-service-principal-portal/select-keys.png)

1. Anahtar için bir açıklama ve süre sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.

   ![anahtarı kaydetme](./media/resource-group-create-service-principal-portal/save-key.png)

   Anahtar kaydedildikten sonra, anahtarın değeri görüntülenir. Daha sonra anahtarı alamazsınız mümkün olmadığından, bu değeri kopyalayın. Uygulama kimliği ile bir uygulama olarak oturum açmak için anahtar değerini sağlayın. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

   ![kaydedilen anahtar](./media/resource-group-create-service-principal-portal/copy-key.png)

## <a name="required-permissions"></a>Gerekli izinler

Bir uygulamayı Azure AD kiracınızı kaydetmek için yeterli izinlere sahip ve Azure aboneliğinizde uygulama rolü atamanız gerekir.

### <a name="check-azure-active-directory-permissions"></a>Azure Active Directory izinlerini denetleme

1. **Azure Active Directory**'yi seçin.

   ![Azure Active Directory'yi seçin](./media/resource-group-create-service-principal-portal/select-active-directory.png)

1. Rolünüz dikkat edin. Varsa **kullanıcı** rolü, yönetici olmayan kullanıcılar uygulamaları kaydedebilir emin olmalısınız.

   ![Kullanıcı Bul](./media/resource-group-create-service-principal-portal/view-user-info.png)

1. Seçin **kullanıcı ayarları**.

   ![kullanıcı ayarlarını seçin](./media/resource-group-create-service-principal-portal/select-user-settings.png)

1. Denetleme **uygulama kayıtları** ayarı. Bu değer yalnızca bir yönetici tarafından ayarlanabilir. Varsa kümesine **Evet**, Azure AD kiracısında herhangi bir kullanıcı uygulama kaydedebilir.

   ![Uygulama kayıtları görüntüle](./media/resource-group-create-service-principal-portal/view-app-registrations.png)

Uygulama kayıtları ayarı ayarlanırsa **Hayır**, yalnızca [genel Yöneticiler](../active-directory/users-groups-roles/directory-assign-admin-roles.md) uygulamalar kaydolabilir. Hesabınız için kullanıcı rolü atanmış, ancak uygulama kayıt ayarının yönetici kullanıcıyla sınırlıdır, ya da yöneticinizi atamak, size, genel Yönetici rolüne ya da uygulamaları kaydetme olanağı isteyin.

### <a name="check-azure-subscription-permissions"></a>Azure aboneliği izinlerini denetleyin

Azure aboneliğinizde, hesabınızın olması gerekir `Microsoft.Authorization/*/Write` bir AD uygulamasını bir role atamak için erişim. Bu eylemin izni, [Sahip](../role-based-access-control/built-in-roles.md#owner) rolüyle veya [Kullanıcı Erişimi Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) rolüyle verilir. Hesabınızı atanırsa **katkıda bulunan** rolü, yeterli izniniz yok. Hizmet sorumlusu, rol atama çalışılırken bir hata alırsınız.

Abonelik izinlerinizi denetlemek için:

1. Sağ üst köşedeki hesabınızı seçin ve seçin **İzinlerim**.

   ![Kullanıcı izinleri seçin](./media/resource-group-create-service-principal-portal/select-my-permissions.png)

1. Aşağı açılan listeden, hizmet sorumlusu oluşturmak istediğiniz aboneliği seçin. Ardından, **tam erişim görüntülemek için burayı tıklatın, bu abonelik için ayrıntılı**.

   ![Kullanıcı Bul](./media/resource-group-create-service-principal-portal/view-details.png)

1. Atanmış olan rolleri görüntülemek ve bir AD uygulamasını bir role atamak için yeterli izinlere sahip olup olmadığını belirler. Aksi durumda, kullanıcı erişimi yöneticisi rolü eklemek için abonelik yöneticinize başvurun. Aşağıdaki görüntüde, kullanıcının kullanıcı yeterli izne sahip olduğu anlamına gelir sahip rolü atanır.

   ![izinleri göster](./media/resource-group-create-service-principal-portal/view-user-role.png)


## <a name="next-steps"></a>Sonraki adımlar
* Bir çok kiracılı uygulamayı kurmak için bkz: [Azure Resource Manager API ile yetkilendirme için Geliştirici Kılavuzu](resource-manager-api-authentication.md).
* Güvenlik ilkeleri belirtme hakkında bilgi edinmek için [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).  
* Verilen veya engellenen kullanıcılara kullanılabilir eylemleri listesi için bkz. [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md).
