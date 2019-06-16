---
title: Azure haritalar kimlik doğrulamasını yönetme | Microsoft Docs
description: Azure haritalar içinde kimlik doğrulamasını yönetmek için Azure portalını kullanabilirsiniz.
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 617adbcda70799aa07248945bbc27f9d95aa77a3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65952559"
---
# <a name="manage-authentication-in-azure-maps"></a>Azure haritalar kimlik doğrulamasını Yönet

Bir Azure haritalar hesabı oluşturduktan sonra Azure Active Directory (Azure AD) veya paylaşılan anahtar kimlik doğrulamasını desteklemek için bir istemci kimliği ve anahtarları oluşturulur.

## <a name="view-authentication-details"></a>Kimlik doğrulaması ayrıntılarını görüntüle

Azure portalında, kimlik doğrulama ayrıntıları görüntüleyebilirsiniz. Hesabınızı ve select Git **kimlik doğrulaması** üzerinde **ayarları** menüsü.

![Kimlik doğrulama ayrıntıları](./media/how-to-manage-authentication/how-to-view-auth.png)

 Daha fazla bilgi için bkz. [Azure Haritalar ile kimlik doğrulaması](https://aka.ms/amauth).


## <a name="set-up-azure-ad-app-registration"></a>Azure AD uygulama kaydı oluşturma

Bir Azure haritalar hesabı oluşturduktan sonra Azure AD kiracınızı Azure haritalar kaynak arasında bir bağlantı kurmanız gerekir.

1. Azure AD dikey penceresine gidin ve bir uygulama kaydı oluşturun. Kayıt için bir ad belirtin. İçinde **oturum açma URL'si** kutusunda, web uygulamasının giriş sayfası sağlayın / API (örneğin, https:\//localhost/). Kayıtlı bir uygulama zaten varsa 2. adıma gidin.

    ![Uygulama kaydı](./media/how-to-manage-authentication/app-registration.png)

    ![Uygulama kayıt ayrıntıları](./media/how-to-manage-authentication/app-create.png)

2. Azure haritalar için temsilci API izinleri atamak için altındaki uygulamaya Git **uygulama kayıtları**ve ardından **ayarları**.  Seçin **gerekli izinler**ve ardından **Ekle**. Arayın ve seçin **Azure haritalar** altında **bir API seçin**ve ardından **seçin** düğmesi.

    ![API uygulama izinleri](./media/how-to-manage-authentication/app-permissions.png)

3. Altında **izinleri seçin**seçin **erişim Azure haritalar**ve ardından **seçin** düğmesi.

    ![API uygulama izinleri seçin](./media/how-to-manage-authentication/select-app-permissions.png)

4. Tamamlandı adımında bir ya da b, kimlik doğrulama yönteminizi bağlı olarak.

    1. Uygulamanız Azure haritalar Web SDK'sıyla kullanıcı belirteci kimlik doğrulaması kullanıyorsa, etkinleştirme `oauthEnableImplicitFlow` uygulaması kayıt ayrıntı sayfanızın bildirim bölümünde true olarak ayarlayarak.
    
       ![Uygulama bildirimi](./media/how-to-manage-authentication/app-manifest.png)

    2. Uygulamanız sunucu/uygulama kimlik doğrulaması kullanıyorsa, Git **anahtarları** dikey penceresinde uygulama kaydı ve parola oluşturma veya uygulama kaydı için bir ortak anahtar sertifikasını karşıya yükleyin. Bir parola seçtikten sonra oluşturursanız **Kaydet**, parolayı daha sonra kullanmak üzere kopyalayın ve güvenli bir şekilde depolayın. Azure ad belirteçlerini almak için bu parolayı kullanırsınız.

       ![Uygulama anahtarları](./media/how-to-manage-authentication/app-keys.png)


## <a name="grant-rbac-to-azure-maps"></a>Azure haritalar için RBAC izni

Azure haritalar hesabı Azure AD kiracınız ile ilişkilendirdikten sonra kullanıcı veya uygulama için bir veya daha fazla Azure haritalar erişim denetimi rolleri atayarak erişim denetimi izni verebilirsiniz.

1. Git **erişim denetimi (IAM)** seçin **rol atamaları**ve ardından **rol ataması Ekle**.

    ![Grant RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. İçinde **rol ataması Ekle** penceresinin altında **rol**seçin **Azure haritalar tarih Okuyucu (Önizleme)** . Altında **erişim Ata**seçin **Azure AD kullanıcı, Grup veya hizmet sorumlusu**. Altında **seçin**, kullanıcı veya uygulama seçin. **Kaydet**’i seçin.

    ![Rol ataması Ekle](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Kullanılabilir Azure haritalar RBAC rollerini görüntüleme

Azure haritalar için kullanılabilir olan rol tabanlı erişim denetimi (RBAC) rolleri görüntülemek için Git **erişim denetimi (IAM)** seçin **rolleri**, ve ardından arama rolleri için başlayarak **Azureharitalar**. Bu erişimi verebilir rolleridir.

![Kullanılabilir roller görüntüle](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-rbac"></a>Azure haritalar RBAC görüntüleyin

RBAC, ayrıntılı erişim denetimi sağlar.

Kullanıcılar ve Azure haritalar için RBAC verilmiş olan uygulamaları görüntülemek için Git **erişim denetimi (IAM)** seçin **rol atamaları**ve ardından filtre **Azure haritalar**.

![Görünüm kullanıcılarda ve uygulamalarda RBAC izni](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Azure haritalar için belirteç isteme

Uygulamanızı kaydetmenizi ve Azure Haritalar ile ilişkili sonra erişim belirteci isteğinde bulunabilirsiniz.

* Uygulamanızı Azure haritalar Web SDK'sıyla kullanıcı belirteci kimlik doğrulaması kullanıyorsa, HTML sayfası Azure haritalar istemci kimliği ve Azure AD uygulama kimliği ile yapılandırmanıza gerek

* Uygulamanız sunucu/uygulama kimlik doğrulaması kullanıyorsa, Azure AD oturum açma uç noktasından bir belirteç istemek gereken `https://login.microsoftonline.com` Azure AD'ye kaynak kimliği ile `https://atlas.microsoft.com/`, Azure haritalar istemci kimliği, Azure AD uygulama kimliği ve Azure AD uygulama kaydı parola veya sertifika.

Kullanıcılar ve hizmet sorumluları için Azure AD'den erişim belirteci isteği hakkında daha fazla bilgi için bkz. [Azure AD için kimlik doğrulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik doğrulaması ve Azure haritalar Web SDK'sı hakkında daha fazla bilgi için bkz: [Azure AD ve Azure haritalar Web SDK'sı](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).

Azure haritalar hesabınız için API kullanım ölçümleri hakkında bilgi edinin:
> [!div class="nextstepaction"] 
> [Kullanım ölçümlerini görüntüle](how-to-view-api-usage.md)