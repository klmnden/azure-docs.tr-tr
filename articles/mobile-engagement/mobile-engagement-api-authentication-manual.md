---
title: "Mobile Engagement REST API'leri ile - el ile kuruluma kimlik doğrulaması"
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
ms.openlocfilehash: 3b678acbae225c28223a2ee76e5be2462a529362
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Mobile Engagement REST API'leri ile - el ile kuruluma kimlik doğrulaması
Bu belge için bir ek belgesidir [Mobile Engagement REST API'leri ile kimlik doğrulama](mobile-engagement-api-authentication.md). İlk içerik Al okuma emin olun.
Azure portalını kullanarak mobil katılım REST API'leri için kimlik doğrulamasını ayarlamak için tek seferlik kurulumu yapmak için alternatif bir yolu açıklar.

> [!NOTE]
> Aşağıdaki yönergeler bunu temel alarak [Active Directory Kılavuzu](../azure-resource-manager/resource-group-create-service-principal-portal.md) ve Mobile Engagement API'leri için kimlik doğrulaması için gerekli olan için özelleştirilebilir. Ayrıntılı adımları anlamak istiyorsanız, bu nedenle başvurduğu.

1. Oturum açtığınızda Azure hesabınız üzerinden [Azure portal](https://portal.azure.com/).
2. Seçin **Active Directory** sol bölmeden.

     ![Active Directory'yi seçin][1]

3. Dizininizde uygulamaları görüntülemek için tıklatın **uygulama kayıtlar**.

     ![uygulamaları görüntüle][3]

4. Tıklayın **yeni uygulama kaydı**.

     ![Uygulama ekleme][4]

5. Uygulamanın adını girin ve tür bir uygulama bırakın **Web app/API** ve İleri düğmesine tıklayın. Herhangi bir kukla URL için sağladığınız **oturum açma URL**: Bu senaryo için kullanılmaz ve URL'leri kendilerini doğrulanmamış.
6. Bunu yaptıktan sonra sağladığınız ada sahip bir Azure AD uygulaması sahip. Bu, **AD\_uygulama\_adı**, lütfen bunu not edin.

     ![uygulama adı][8]

7. Uygulama adına tıklayın.
8. Bul **uygulama kimliği**, Not, olarak kullanılacak istemci kimliği olan **istemci\_kimliği** API'nize çağırır.

     ![uygulamayı yapılandırma][10]

9. Bul **anahtarları** sağdaki bölüm.

     ![uygulamayı yapılandırma][11]

10. Yeni bir anahtar oluşturun, hemen kopyalayın ve kullanmak üzere kaydetmek. Bu asla yeniden gösterilir.

     ![uygulamayı yapılandırma][12]

    > [!IMPORTANT]
    > Bu anahtar zamanı Aksi takdirde, API kimlik geldiğinde yenilemek emin olun artık çalışmayacak şekilde, belirttiğiniz süre sonunda süresi dolar. Ayrıca silin ve tehlikeye düşünüyorsanız, bu anahtarı oluşturun.
    >
    >
11. Tıklayın **uç noktaları** üst sayfasının ve Kopyala düğmesini **OAUTH 2.0 BELİRTEÇ uç noktası**.

    ![][14]

16. Bu uç noktaya URL'de GUID olduğu şu biçimde olacaktır, **TENANT_ID** bu nedenle, not edin:`https://login.microsoftonline.com/<GUID>/oauth2/token`
17. Şimdi Biz bu uygulamasını izinlerini yapılandırmak için devam edin. Bunun için açmanız gerekecek [Azure portal](https://portal.azure.com). 
18. Tıklayın **kaynak grupları** ve Bul **Mobile Engagement** kaynak grubu.

    ![][15]

19. Tıklatın **Mobile Engagement** kaynak grubu ve gidin **ayarları** burada bölüm.

    ![][16]

20. Tıklayın **kullanıcılar** Ayarlar bölümünde ve tıklayın **Ekle** bir kullanıcı eklemek için.

    ![][17]

21. Tıklayın **bir rol seçin**.

    ![][18]

22. Tıklayın **sahibi**.

    ![][19]

23. Uygulamanızın adını arayın **AD\_uygulama\_adı** arama kutusuna. Bunu burada varsayılan olarak görmez. Bulduğunuzda, onu seçin ve tıklayın **seçin** bölümünün altındaki.

    ![][20]

24. Üzerinde **eklemek erişim** bölümünde gösterilir olarak **1 kullanıcının, 0 gruplarını**. Tıklatın **Tamam** Değişikliği onaylamak için bu bölümü üzerinde.

    ![][21]

Artık gerekli tamamladınız ve Azure AD yapılandırma olduğunuzda API'leri çağırmak için tüm kümesi.

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



