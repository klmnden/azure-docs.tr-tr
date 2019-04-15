---
title: Portalda Azure uygulama kimliği oluşturma | Microsoft Docs
description: Yeni Azure Active Directory uygulaması ve Azure Resource Manager rol tabanlı erişim denetimi ile kaynaklara erişimi yönetmek için kullanılan hizmet sorumlusu oluşturmayı açıklar.
services: active-directory
documentationcenter: na
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/08/2019
ms.author: celested
ms.reviewer: tomfitz
ms.custom: seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9affec9ccc1b87f36d6f30aff4795d85532be8c1
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59565929"
---
# <a name="how-to-use-the-portal-to-create-an-azure-ad-application-and-service-principal-that-can-access-resources"></a>Nasıl yapılır: Bir Azure AD uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturmak için portalı kullanma

Bu makalede yeni bir Azure Active Directory (Azure AD) uygulama ve hizmet sorumlusu, rol tabanlı erişim denetimi ile kullanılabilen oluşturulacağını gösterir. Erişim ya da kaynakları değiştirmek için gereken kodu varsa, uygulama için bir kimlik oluşturabilirsiniz. Bu kimlik, hizmet sorumlusu olarak bilinir. Ardından, hizmet sorumlusuna gerekli izinleri atayabilirsiniz. Bu makalede hizmet sorumlusu oluşturmak için portalı kullanmayı gösterir. Tek kiracılı bir uygulama yalnızca bir kuruluş içinde çalıştırmak için uygulamayı nerede yöneliktir odaklanır. Genellikle tek kiracılı uygulamalar kuruluşunuzda çalışan satır iş kolu uygulamaları için kullanırsınız.

> [!IMPORTANT]
> Bir hizmet sorumlusu oluşturmak yerine, uygulama kimliğiniz için Azure kaynakları için yönetilen kimliklerle göz önünde bulundurun. Kodunuzu Yönetilen kimlikleri ve Azure AD kimlik doğrulamasını destekleyen erişimleri kaynak destekleyen bir hizmeti çalıştıran yönetilen kimlikleri sizin için daha iyi bir seçenek vardır. Hangi şu anda, Destek Hizmetleri dahil olmak üzere, Azure kaynakları için yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikleri nedir?](../managed-identities-azure-resources/overview.md).

## <a name="create-an-azure-active-directory-application"></a>Bir Azure Active Directory uygulaması oluşturma

Şimdi, doğrudan kimlik oluşturmaya geçin. Bir sorunla karşılaştıysanız denetleyin [gerekli izinler](#required-permissions) hesabınızın kimliğini oluşturabilirsiniz emin olmak için.

1. Üzerinden Azure hesabınızla oturum açın [Azure portalında](https://portal.azure.com).
1. **Azure Active Directory**'yi seçin.
1. **Uygulama kayıtları**'nı seçin.

   ![Uygulama kayıtlarını seçme](./media/howto-create-service-principal-portal/select-app-registrations.png)

1. **Yeni uygulama kaydı**’nı seçin.

   ![Uygulama ekle](./media/howto-create-service-principal-portal/select-add-app.png)

1. Uygulama için bir ad ve URL sağlayın. Oluşturmak istediğiniz uygulama türü olarak **Web uygulaması / API**'yi seçin. Kimlik bilgileri oluşturulamıyor bir [yerel uygulama](../manage-apps/application-proxy-configure-native-client-application.md). Bu tür için otomatik uygulama kullanamazsınız. Değerleri ayarladıktan sonra seçin **Oluştur**.

   ![uygulamayı adlandırma](./media/howto-create-service-principal-portal/create-app.png)

Azure AD uygulaması ve hizmet sorumlusu oluşturmuş oldunuz.

## <a name="assign-the-application-to-a-role"></a>Bir role uygulama atama

Aboneliğinizdeki kaynaklara erişmek için uygulamaya bir rol atamanız gerekir. Uygulama için doğru izinlere hangi rolü sunar karar verin. Kullanılabilir roller hakkında bilgi edinmek için [RBAC: Yerleşik roller](../../role-based-access-control/built-in-roles.md).

Abonelik, kaynak grubu veya kaynak düzeyinde kapsamı ayarlayabilirsiniz. Daha düşük düzeyde kapsam için izinler devralınmıştır. Örneğin, bir kaynak grubu için okuyucu rolüne uygulamaya ekleme kaynak grubunu ve içerdiği tüm kaynakları okuyun anlamına gelir.

1. Uygulamayı atamak istediğiniz kapsam düzeyine gidin. Örneğin abonelik kapsamında bir rol atamak için seçin **tüm hizmetleri** ve **abonelikleri**.

   ![Abonelik seçme](./media/howto-create-service-principal-portal/select-subscription.png)

1. Uygulamaya atamak için belirli bir abonelik seçin.

   ![Abonelik atama için seçin](./media/howto-create-service-principal-portal/select-one-subscription.png)

   Aradığınız bir abonelik görmüyorsanız seçin **genel abonelik filtresi**. Seçili için portalda istediğiniz aboneliği olduğundan emin olun. 

1. Seçin **erişim denetimi (IAM)**.
1. Seçin **rol ataması Ekle**.

   ![Rol ataması Ekle'yi seçin](./media/howto-create-service-principal-portal/select-add.png)

1. Uygulamayı atamak istediğiniz rolü seçin. Gibi eylemleri yürütmek uygulama izin vermek için **yeniden**, **Başlat** ve **Durdur** örnekleri, select **katkıda bulunan** rol. Varsayılan olarak, Azure AD uygulamaları kullanılabilir seçenekleri görüntülenmiyor. Uygulamanızı bulmak için adını arayın ve seçin.

   ![Rol seç](./media/howto-create-service-principal-portal/select-role.png)

1. Seçin **Kaydet** rol atama tamamlanması. Bu kapsam için bir role atanmış kullanıcı listesinde uygulamanızı görürsünüz.

Hizmet sorumlunuzu ayarlayın. Betikleri veya uygulamalarını çalıştırmak için kullanmaya başlayabilirsiniz. Sonraki bölümde programlamayla oturum açılırken gerekli değerleri alma işlemi gösterilmektedir.

## <a name="get-values-for-signing-in"></a>Oturum açmak için değerlerini alma

### <a name="get-tenant-id"></a>Kiracı kimliğini alma

Programlamayla oturum açılırken, kimlik doğrulama isteğinizle birlikte Kiracı Kimliğini geçirmeniz gerekir.

1. **Azure Active Directory**'yi seçin.
1. Seçin **özellikleri**.

   ![Azure AD özelliklerini seçme](./media/howto-create-service-principal-portal/select-ad-properties.png)

1. Kopyalama **dizin kimliği** Kiracı kimliğinizi almak için

   ![Kiracı Kimliği](./media/howto-create-service-principal-portal/copy-directory-id.png)

### <a name="get-application-id-and-authentication-key"></a>Uygulama kimliği ve kimlik doğrulama anahtarını alma

Ayrıca, uygulamanızın ve kimlik doğrulama anahtarı için kimliği gerekir. Bu değerleri almak için aşağıdaki adımları kullanın:

1. Gelen **uygulama kayıtları** Azure AD'de uygulamanızı seçin.

   ![Uygulama seçme](./media/howto-create-service-principal-portal/select-app.png)

1. **Uygulama kimliği**'ni kopyalayın ve bunu uygulama kodunuzda depolayın.

   ![İstemci Kimliği](./media/howto-create-service-principal-portal/copy-app-id.png)

1. Seçin **ayarları**.

   ![ayarları seçin](./media/howto-create-service-principal-portal/select-settings.png)

1. **Anahtarlar**’ı seçin.
1. Anahtar için bir açıklama ve süre sağlayın. İşiniz bittiğinde **Kaydet**’i seçin.

   ![anahtarı kaydetme](./media/howto-create-service-principal-portal/save-key.png)

   Anahtar kaydedildikten sonra, anahtarın değeri görüntülenir. Daha sonra anahtarı alamazsınız mümkün olmadığından, bu değeri kopyalayın. Uygulama kimliği ile bir uygulama olarak oturum açmak için anahtar değerini sağlayın. Anahtarı, uygulamanızın alabileceği bir konumda depolayın.

   ![kaydedilen anahtar](./media/howto-create-service-principal-portal/copy-key.png)

## <a name="required-permissions"></a>Gerekli izinler

Bir uygulamayı Azure AD kiracınızı kaydetmek için yeterli izinlere sahip ve Azure aboneliğinizde uygulama rolü atamanız gerekir.

### <a name="check-azure-ad-permissions"></a>Azure AD izinleri denetleyin

1. **Azure Active Directory**'yi seçin.
1. Rolünüz unutmayın. Varsa **kullanıcı** rolü, yönetici olmayan kullanıcılar uygulamaları kaydedebilir emin olmalısınız.

   ![Kullanıcı Bul](./media/howto-create-service-principal-portal/view-user-info.png)

1. Seçin **kullanıcı ayarları**.

   ![kullanıcı ayarlarını seçin](./media/howto-create-service-principal-portal/select-user-settings.png)

1. Denetleme **uygulama kayıtları** ayarı. Bu değer yalnızca bir yönetici tarafından ayarlanabilir. Varsa kümesine **Evet**, Azure AD kiracısında herhangi bir kullanıcı uygulama kaydedebilir.

   ![Uygulama kayıtları görüntüle](./media/howto-create-service-principal-portal/view-app-registrations.png)

Uygulama kayıtları ayarı ayarlanırsa **Hayır**yalnızca Yönetici rolüne sahip kullanıcılar bu tür uygulamaları kaydedebilir. Bkz: [kullanılabilir roller](../users-groups-roles/directory-assign-admin-roles.md#available-roles) ve [rol izinleri](../users-groups-roles/directory-assign-admin-roles.md#role-permissions) kullanılabilir yönetici rolleri ve Azure AD'de her role verilen belirli izinleri hakkında bilgi edinmek için. Hesabınız için kullanıcı rolü atanmış, ancak uygulama kayıt ayarının yönetici kullanıcıyla sınırlıdır, yöneticinize ya da atama, oluşturma ve uygulama kayıtları veya kullanıcılara etkinleştirmek için tüm özelliklerini yönetebilir yönetici rollerinden isteyin uygulamaları kaydedin.

### <a name="check-azure-subscription-permissions"></a>Azure aboneliği izinlerini denetleyin

Azure aboneliğinizde, hesabınızın olması gerekir `Microsoft.Authorization/*/Write` bir AD uygulamasını bir role atamak için erişim. Bu eylemin izni, [Sahip](../../role-based-access-control/built-in-roles.md#owner) rolüyle veya [Kullanıcı Erişimi Yöneticisi](../../role-based-access-control/built-in-roles.md#user-access-administrator) rolüyle verilir. Hesabınızı atanırsa **katkıda bulunan** rolü, yeterli izniniz yok. Hizmet sorumlusu, rol atama çalışılırken bir hata alırsınız.

Abonelik izinlerinizi denetlemek için:

1. Sağ üst köşedeki hesabınızı seçin ve seçin **İzinlerim**.

   ![Kullanıcı izinleri seçin](./media/howto-create-service-principal-portal/select-my-permissions.png)

1. Aşağı açılan listeden, hizmet sorumlusu oluşturmak istediğiniz aboneliği seçin. Ardından, **tam erişim görüntülemek için burayı tıklatın, bu abonelik için ayrıntılı**.

   ![Kullanıcı Bul](./media/howto-create-service-principal-portal/view-details.png)

1. Atanmış olan rolleri görüntülemek ve bir AD uygulamasını bir role atamak için yeterli izinlere sahip olup olmadığını belirler. Aksi durumda, kullanıcı erişimi yöneticisi rolü eklemek için abonelik yöneticinize başvurun. Aşağıdaki görüntüde, kullanıcının kullanıcı yeterli izne sahip olduğu anlamına gelir sahip rolü atanır.

   ![izinleri göster](./media/howto-create-service-principal-portal/view-user-role.png)

## <a name="next-steps"></a>Sonraki adımlar

* Bir çok kiracılı uygulamayı kurmak için bkz: [Azure Resource Manager API ile yetkilendirme için Geliştirici Kılavuzu](../../azure-resource-manager/resource-manager-api-authentication.md).
* Güvenlik ilkeleri belirtme hakkında bilgi edinmek için [Azure rol tabanlı erişim denetimi](../../role-based-access-control/role-assignments-portal.md).  
* Verilen veya engellenen kullanıcılara kullanılabilir eylemleri listesi için bkz. [Azure Resource Manager kaynak sağlayıcısı işlemleri](../../role-based-access-control/resource-provider-operations.md).
