---
title: "Mobile Engagement REST API'leri ile kimlik doğrulaması: el ile Kurulum"
description: "El ile kimlik doğrulaması için Mobile Engagement REST API'leri Kurulum açıklar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0b4a999c6778040e71f862d3a010b6635e84b26e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis-manual-setup"></a>Mobile Engagement REST API'leri ile kimlik doğrulaması: el ile Kurulum
Bir ek bu belgesidir [Mobile Engagement REST API'leri ile kimlik doğrulama](mobile-engagement-api-authentication.md). İlk bağlamı anlamak için bu makaleyi okuyun emin olun. Ayrıca, Azure portalını kullanarak Mobile Engagement REST API'leri için tek seferlik kimlik doğrulaması kurulumu yapmak için alternatif bir yolu anlatır.

> [!NOTE]
> Aşağıdaki yönergelere dayalı [Bu Active Directory Kılavuzu](../azure-resource-manager/resource-group-create-service-principal-portal.md). Mobile Engagement API'leri için kimlik doğrulama gereksinimleri için özelleştirilir. Aşağıdaki adımları ayrıntılı anlamak istiyorsanız, bunları bakın.

1. Oturum Azure hesabınıza [Azure portal](https://portal.azure.com/).
2. Seçin **Active Directory** sol bölmeden.

   ![Active Directory'yi seçin][1]

3. Dizininizde uygulamaları görüntülemek için seçin **uygulama kayıtlar**.

   ![uygulamaları görüntüle][3]

4. Seçin **yeni uygulama kaydı**.

   ![Uygulama ekleme][4]

5. Uygulama adına doldurun. Tür bir uygulama bırakın **Web app/API**ve ardından **sonraki** düğmesi. Herhangi bir kukla URL için sağladığınız **oturum açma URL**. Bu senaryo için kullanılmaz ve URL'leri kendilerini doğrulanmamış.

   Bitirdikten sonra belirttiğiniz ada sahip bir Azure Active Directory (Azure AD) uygulama vardır. Bu, **AD\_uygulama\_adı**, bu nedenle, Not emin olun.

   ![Uygulama adı][8]

7. Uygulama adı seçin.

8. Bul **uygulama kimliği** ve onu not edin. Olarak kullanılacak istemci kimliği: **istemci\_kimliği** API'nize çağırır.

   ![Uygulama Kimliği bulunamıyor][10]

9. Bul **anahtarları** sağdaki bölüm.

   ![Anahtarları bölümüne][11]

10. Yeni bir anahtar oluşturun ve ardından hemen kopyalayın. Yeniden gösterilen değil.

    ![Anahtar ayrıntılarını anahtarları bölmesi][12]

    > [!IMPORTANT]
    > Bu anahtar, belirttiğiniz süre sonunda süresi dolar. Zamanı geldiğinde yenilemek emin olun, aksi takdirde artık API kimlik doğrulaması çalışmaz. Bu anahtar tehlikede olduğunu düşünüyorsanız, silin ve yeniden oluşturun.
    >
    
11. Seçin **uç noktaları** sayfanın üstündeki düğmesi. Ardından kopyalama **OAUTH 2.0 BELİRTEÇ uç noktası**.

    ![Uç noktasını kopyalayın][14]

16. Bu uç noktaya URL'de GUID olduğu aşağıdaki biçiminde olduğundan, **TENANT_ID**:`https://login.microsoftonline.com/<GUID>/oauth2/token`

17. Ardından, bu uygulama üzerinde izinlerini yapılandırın. İşlemi başlatmak için şu adrese gidin [Azure portal](https://portal.azure.com).

18. Seçin **kaynak grupları**ve ardından bulmak **MobileEngagement** kaynak grubu.

    ![MobileEngagement Bul][15]

19. Seçin **MobileEngagement** kaynak grubunu ve ardından **tüm ayarları**.

    ![Ayarları MobileEngagement için Gözat][16]

20. Seçin **kullanıcılar** içinde **ayarları** bölümü. Ardından, bir kullanıcı eklemek için seçin **Ekle**.

    ![Kullanıcı ekleme][17]

21. Tıklatın **bir rol seçin**.

    ![Rol seçin][18]

22. Seçin **sahibi**.

    ![Sahip rolünü seçin][19]

23. Arama, uygulamanızın adı için **AD\_uygulama\_adı**, arama kutusuna. Bu ad, burada varsayılan olarak değil. Bunu bulduktan sonra onu seçin. Ardından **seçin** bölümünün altındaki.

    ![Adı seçin][20]

24. İçinde **erişim Ekle** bölümünde görünür olarak **1 kullanıcının, 0 gruplarını**. Değişikliği onaylamak için seçin **Tamam**.

    ![Eklenen kullanıcı onaylayın][21]

Artık gerekli tamamladınız Azure AD yapılandırma ve olan tüm API'leri çağırmak için ayarlayın.

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client-secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



