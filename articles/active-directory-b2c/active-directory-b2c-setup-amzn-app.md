---
title: "Azure Active Directory B2C: Amazon yapılandırma | Microsoft Docs"
description: "Uygulamalarınızda Azure Active Directory B2C tarafından güvenliği sağlanan Amazon hesaplarıyla tüketiciye kaydolma ve oturum açma sağlar."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 77c099bb-a005-4d75-87f9-f61e3de48725
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: dcc97e1b7f6287bd7692c52bf068950065a26572
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory B2C: Kaydolma ve oturum açma Amazon hesaplarıyla tüketicileri sağlayın
## <a name="create-an-amazon-application"></a>Amazon uygulama oluşturma
Amazon Azure Active Directory (Azure AD) B2C bir kimlik sağlayıcısı olarak kullanmak için Amazon uygulama oluşturma ve doğru parametrelerle sağlamanız gerekir. Bunu yapmak için bir Amazon hesabınızın olması gerekir. Bir sahip değilseniz, yerinde edinebilirsiniz [http://www.amazon.com/](http://www.amazon.com/).

1. Git [Amazon Geliştirici Merkezi](https://login.amazon.com/) ve Amazon hesabı kimlik bilgilerinizle oturum açın.
2. Zaten yapmadıysanız, tıklatın **kaydolun**, geliştirici kayıt adımları izleyin ve ilke kabul edin.
3. Tıklatın **kaydetmek yeni uygulama**.
   
    ![Amazon Web sitesinde yeni bir uygulama kaydetme](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. Uygulama bilgilerini sağlayın (**adı**, **açıklama**, ve **gizlilik bildirimi URL'si**) tıklatıp **kaydetmek**.
   
    ![Amazon en yeni bir uygulama kaydetmeye yönelik uygulama bilgileri sağlama](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. İçinde **Web ayarları** bölümünde, değerlerini kopyalamayı **istemci kimliği** ve **gizli**. (Tıklamanız gerekir **Göster gizli** bu düğmeyi.) Her ikisi de kiracınızda kimlik sağlayıcısı Amazon yapılandırmak için gerekir. Tıklatın **Düzenle** bölümünün altındaki. **İstemci parolası** önemli güvenlik kimlik bilgileri.
   
    ![Yeni uygulamanızı Amazon için istemci kimliği ve istemci parolası sağlama](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. Girin `https://login.microsoftonline.com` içinde **izin JavaScript çıkış** alan ve `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` içinde **dönüş URL'leri izin** alan. Değiştir **{tenant}** , kiracının adlı (örneğin, contoso.onmicrosoft.com). **Kaydet** düğmesine tıklayın. **{Tenant}** duyarlıdır.
   
    ![Yeni uygulamanızı Amazon için JavaScript kaynakları ve dönüş URL'leri sağlama](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Kimlik sağlayıcısı kiracınızda Amazon yapılandırın
1. Aşağıdaki adımları izleyin [B2C özellikleri dikey penceresine gidin](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) Azure portalındaki.
2. B2C özellikleri dikey penceresinde **kimlik sağlayıcıları**.
3. Dikey pencerenin en üstündeki **+Add (+Ekle)** seçeneğine tıklayın.
4. Kolay bir sağlamak **adı** kimlik sağlayıcısı yapılandırması için. Örneğin, "Amzn" girin.
5. Tıklatın **kimlik sağlayıcısı türü**seçin **Amazon**, tıklatıp **Tamam**.
6. Tıklatın **bu kimlik sağlayıcısı'nı ayarlama** istemci kimliği ve istemci parolası Amazon uygulamasının daha önce oluşturduğunuz girin.
7. Tıklatın **Tamam** ve ardından **oluşturma** Amazon yapılandırmanızı kaydetmek için.

