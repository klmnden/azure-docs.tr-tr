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
ms.openlocfilehash: 9d6132e1a01be489b8e8e28a0219cf8a0b50b318
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Mobile Engagement REST API'leri ile - el ile kuruluma kimlik doğrulaması
Bir ek belgelerine budur [Mobile Engagement REST API'leri ile kimlik doğrulama](mobile-engagement-api-authentication.md). İlk içerik Al okuma emin olun. Bu, kimlik doğrulaması için Azure Portalı'nı kullanarak Mobile Engagement REST API'leri ayarlamak için tek seferlik kurulumu yapmak için alternatif bir yolu açıklar. 

> [!NOTE]
> Aşağıdaki yönergeleri bunu temel alarak [Active Directory Kılavuzu](../azure-resource-manager/resource-group-create-service-principal-portal.md) ve Mobile Engagement API'leri için kimlik doğrulaması için gerekli olan için özelleştirilebilir. Ayrıntılı adımları anlamak istiyorsanız, bu nedenle başvurduğu. 
> 
> 

1. Azure hesabınız ile oturum açma [Klasik portal](https://manage.windowsazure.com/).
2. Seçin **Active Directory** sol bölmeden.
   
     ![Active Directory'yi seçin][1]
3. Seçin **varsayılan Active Directory** , Azure portalında. 
   
     ![dizin seçin][2]
   
   > [!IMPORTANT]
   > Bu yaklaşım, yalnızca varsayılan Active Directory hesabınızın çalışıyorsanız ve hesabınızı oluşturduğunuz bir Active Directory bu yapıyorlarsa, işe yaramaz olduğunda çalışır. 
   > 
   > 
4. Dizininizde uygulamaları görüntülemek için tıklatın **uygulamaları**.
   
     ![uygulamaları görüntüle][3]
5. Tıklayın **eklemek**. 
   
     ![Uygulama ekleme][4]
6. Tıklayın **kuruluşumun geliştirmekte olduğu bir uygulama Ekle**
   
     ![Yeni uygulama][5]
7. Uygulamanın adını girin ve uygulama olarak türünü seçin **WEB uygulaması ve/veya WEB API** ve İleri düğmesine tıklayın.
   
     ![Uygulama adı][6]
8. Herhangi bir kukla URL için sağladığınız **oturum açma URL** ve **uygulama kimliği URI'si**. Senaryomuz için kullanılmaz ve URL'leri kendilerini doğrulanmamış.  
   
     ![Uygulama özellikleri][7]
9. Sonunda bu, aşağıdaki gibi daha önce verilen ada sahip bir AAD uygulaması gerekir. Bu, **AD\_uygulama\_adı** ve onu not edin.  
   
     ![Uygulama adı][8]
10. Uygulama adına tıklayın ve tıklayın **yapılandırma**.
    
      ![uygulamayı yapılandırma][9]
11. Olarak kullanılacak istemci Kimliğini Not **istemci\_kimliği** API'nize çağırır. 
    
     ![uygulamayı yapılandırma][10]
12. Ekranı aşağı kaydırarak **anahtarları** bölüm ve tercihen 2 yıl (Bitiş) süresi ile bir anahtar ekleyin ve tıklatın **kaydetmek**. 
    
     ![uygulamayı yapılandırma][11]
13. Hemen herhangi bir zamanda yeniden görüntülenmeyecek depolanır, böylece değildir ve yalnızca şimdi gösterilen gibi anahtar için gösterilen değeri kopyalayın. Ardından, kaybetmeniz durumunda yeni bir anahtar oluşturmak gerekir. Bu **CLIENT_SECRET** API'nize çağırır. 
    
     ![uygulamayı yapılandırma][12]
    
    > [!IMPORTANT]
    > Bu anahtar zamanı Aksi takdirde, API kimlik geldiğinde yenilemek emin olun artık çalışmayacak şekilde, belirttiğiniz süre sonunda süresi dolar. Ayrıca silin ve tehlikeye düşünüyorsanız, bu anahtarı oluşturun.
    > 
    > 
14. Tıklayın **uç noktalarını GÖRÜNTÜLE** düğmesini şimdi açılacağı **uygulama uç noktaları** iletişim kutusu. 
    
    ![][13]
15. Uygulama uç noktaları iletişim kutusundan kopyalama **OAUTH 2.0 BELİRTEÇ uç noktası**. 
    
    ![][14]
16. Bu uç noktaya URL'de GUID olduğu şu biçimde olacaktır, **TENANT_ID** bu nedenle, not edin: 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. Şimdi Biz bu uygulamasını izinlerini yapılandırmak için devam edin. Bunun için açmanız gerekecek [Azure portal](https://portal.azure.com). 
18. Tıklayın **kaynak grupları** ve Bul **Mobile Engagement** kaynak grubu.  
    
    ![][15]
19. Tıklatın **Mobile Engagement** kaynak grubu ve gidin **ayarları** dikey burada. 
    
    ![][16]
20. Tıklayın **kullanıcılar** ayarlar dikey ve ardından üzerinde **Ekle** bir kullanıcı eklemek için. 
    
    ![][17]
21. Tıklayın **bir rol seçin**
    
    ![][18]
22. Tıklayın **sahibi**
    
    ![][19]
23. Uygulamanızın adını arayın **AD\_uygulama\_adı** arama kutusuna. Bunu burada varsayılan olarak görmez. Bulduğunuzda, onu seçin ve tıklayın **seçin** dikey pencerenin altındaki. 
    
    ![][20]
24. Üzerinde **eklemek erişim** dikey penceresinde gösterilir olarak **1 kullanıcının, 0 gruplarını**. Tıklatın **Tamam** Değişikliği onaylamak için bu dikey penceredeki. 
    
    ![][21]

Gerekli bir AAD yapılandırmasına tamamladınız ve tüm API'leri çağırmak için ayarlanır. 

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
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
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



