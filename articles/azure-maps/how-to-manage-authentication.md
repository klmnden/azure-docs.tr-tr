---
title: Azure haritalar kimlik doğrulamasını yönetme | Microsoft Docs
description: Azure haritalar içinde kimlik doğrulamasını yönetmek için Azure portalını kullanabilirsiniz.
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 68c3c8ac39f5803e01ee1038ec85ddb96ac80b30
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56242694"
---
# <a name="manage-authentication-in-azure-maps"></a>Azure haritalar kimlik doğrulamasını Yönet

Bir Azure haritalar hesabı oluşturduktan sonra istemci kimliği ve anahtarlar veya Azure Active Directory (Azure AD), hem de paylaşılan anahtar kimlik doğrulamasını desteklemek için oluşturulur.

Kimlik doğrulama ayrıntılarınızı giderek bulabilirsiniz **kimlik doğrulaması** altındaki **ayarları** Azure portalında. Hesabınıza gidin. Ardından **kimlik doğrulaması** menüsünde.


## <a name="view-authentication-details"></a>Kimlik doğrulaması ayrıntılarını görüntüle

Kimlik doğrulama ayrıntılarınızı görüntülemek için gidin **kimlik doğrulaması** Ayarlar menüsünden seçeneği.

![Görünüm kimlik doğrulaması](./media/how-to-manage-authentication/how-to-view-auth.png)

 Kimlik doğrulaması ve Azure haritalar hakkında bilgi edinmek için [Azure Haritalar ile kimlik doğrulaması](https://aka.ms/amauth).


## <a name="configure-azure-ad-app-registration"></a>Azure AD uygulaması kaydını yapılandırma

Azure harita hesabınız oluşturulduktan sonra Azure AD kiracınız ile Azure haritalar Azure kaynak arasında bir bağlantı gereklidir. 

1. Azure AD dikey penceresine gidin ve web uygulamasının giriş sayfası olarak bir ad ve oturum açma URL'si ile bir uygulama kaydı oluşturmak / API gibi "https://localhost/". Kayıtlı bir uygulama zaten varsa 2. adıma geçin.

    ![Uygulama kaydı](./media/how-to-manage-authentication/app-registration.png)

    ![Uygulama kaydı](./media/how-to-manage-authentication/app-create.png)

2. Uygulama kayıtları altındaki uygulama giderek Azure haritalar için temsilci API izinleri atayın ve tıklayarak **ayarları**.  Seçin **gerekli izinler** seçip **Ekle**. Arayın ve ardından Azure haritalar altında seçin **bir API seçin** tıklatıp **seçin**.

    ![Uygulama API izinleri](./media/how-to-manage-authentication/app-permissions.png)

3. Son olarak, Select izinleri altında erişim Azure haritalar'ı seçin ve Seç'e tıklayın.

    ![Uygulama API izinleri seçin](./media/how-to-manage-authentication/select-app-permissions.png)

4. Adımı bir ya da b, kimlik doğrulama uygulaması bağlı olarak.

    1. Azure haritalar Web SDK ile kullanıcı belirteci kimlik doğrulamasını kullanmak uygulamayı amaçlanıyorsa etkinleştirmelisiniz `oauthEnableImplicitFlow` uygulaması kayıt ayrıntı sayfanızın bildirim bölümünde true olarak ayarlayarak.
    
       ![Uygulama bildirimi](./media/how-to-manage-authentication/app-manifest.png)

    2. Uygulama kullanan sunucu/uygulama kimlik doğrulamasını giderseniz **anahtarları** dikey penceresinde uygulama kaydı ve ya da bir parola oluşturmanız veya uygulama kaydı için bir ortak anahtar sertifikasını karşıya yükleyin. Sonra parola oluşturursanız **Kaydet**, parolayı daha sonra kullanmak üzere, kopyalama ve güvenli bir şekilde depolayın, bu Azure ad belirteçlerini almak için kullanır.

       ![Uygulama anahtarları](./media/how-to-manage-authentication/app-keys.png)


## <a name="grant-rbac-to-azure-maps"></a>Azure haritalar için RBAC izni

Azure haritalar hesabı Azure AD kiracınız ile ilişkilendirildikten sonra kullanıcı veya uygulama için kullanılabilir Azure haritalar erişim denetimi rolleri atayarak erişim denetimi verilebilir.

1. Gidin **erişim denetimi** seçeneğinde, tıklayın **rol atamaları**, ve **rol ataması Ekle**.

    ![Grant RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. Azure haritalar tarih Okuyucu (Önizleme) penceresinde rol atamasını büyütme üzerinde seçin **rol**, **erişim Ata**: Azure AD kullanıcı, Grup veya hizmet sorumlusu **seçin** kullanıcı veya açılır listesinde, uygulamadan ve **Kaydet**.

    ![Rol ataması ekle](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Kullanılabilir Azure haritalar RBAC rollerini görüntüleme

Erişim izni verilebileceğini Azure haritalar için kullanılabilir rol tabanlı erişim denetimi rolleri görüntülemek için gidin **erişim denetimi (IAM)** seçeneğinde, tıklayın **rolleri**, ve roller için arama ilebaşlayan**Azure haritalar**.

![Kullanılabilir roller görüntüle](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-role-based-access-control-rbac"></a>Azure haritalar rol tabanlı erişim denetimi (RBAC) görüntüleme

Azure AD rol tabanlı erişim denetimi (RBAC) ayrıntılı erişim denetimi sağlar. 

Kullanıcılar veya Azure haritalar için RBAC verilmiş olan uygulamaları görüntülemek için gidin **erişim denetimi (IAM)** seçeneği için **rol atamaları**ve filtre **Azure haritalar**. Kullanılabilir tüm roller altında görünür.

![RBAC görüntüle](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Azure haritalar için belirteç isteme

Uygulamanızı kayıtlı ve Azure Haritalar ile ilişkili sonra artık erişim belirteci isteğinde bulunabilirsiniz.

* Azure haritalar Web SDK (Web) ile kullanıcı belirteci kimlik doğrulamasını kullanmak uygulamayı amaçlanıyorsa, html sayfası Azure haritalar istemci kimliği ve Azure AD uygulama kimliği ile yapılandırmanız gerekiyor

* Azure AD oturum açma uç noktasından bir belirteç isteği sunucu/uygulama kimlik doğrulaması kullanan uygulamalar için `https://login.microsoftonline.com` Azure AD'ye kaynak kimliği ile `https://atlas.microsoft.com/`, Azure haritalar istemci kimliği, Azure AD uygulama kimliği ve Azure AD uygulaması kayıt parola veya sertifika.

Kullanıcılar ve hizmet sorumluları için Azure AD'den erişim belirteci isteme hakkında daha fazla bilgi için bkz. [Azure AD için kimlik doğrulama senaryoları](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Sonraki adımlar

* Azure AD kimlik doğrulaması ve Azure haritalar Web SDK'sı hakkında daha fazla bilgi için bkz: [Azure AD ve Azure haritalar Web SDK'sı](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).