---
title: Bir Azure AD galeri uygulama eklenirken hata | Microsoft Docs
description: "Azure AD galeri uygulamaları ve bunları gidermek için neler yapabileceğinizi eklerken, ortak sorunları kişiler yüz anlama"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: b3ae472d52208d3c76424d29192c1eb982639572
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a>Bir Azure AD galeri uygulama eklenirken hata

Bu makalede Azure AD galeri uygulamaları ve bunları gidermek için neler yapabileceğinizi eklerken, ortak sorunları kişiler yüzünü anlamanıza yardımcı olur.

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>"Ekle" düğmesine tıklandığında ve uygulamamın görünür uzun sürdü

Bazı durumlarda, 1-2 dakika alabilir (ve bazı durumlarda daha uzun) uygulamanın dizininize eklendikten sonra görünür. Bu normal beklenen performans olmamasına karşın, uygulama eklenmesi ediyor tıklayarak görebilirsiniz **bildirimleri** (zil) simgesine sağ üst tarafındaki [Azure Portal](https://portal.azure.com/) ve aranıyor için bir **sürüyor** veya **tamamlandı** etiketli bildirim **uygulaması oluşturun.**

Uygulamanızı hiçbir zaman eklenir veya tıklatıldığında hatayla karşılaşırsanız **Ekle** düğmesi, göreceğiniz bir **bildirim** içinde bir **hata** durumu. Daha fazla bilgi edinmek veya destek engingeer ile paylaşmak için hata hakkında daha fazla ayrıntı isterseniz içindeki adımları izleyerek hata hakkında daha fazla bilgi görebilirsiniz [portal bildirim ayrıntılarını görmek nasıl](#how-to-see-the-details-of-a-portal-notification) bölümü.

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>"Ekle" düğmesine tıklandığında ve uygulamamın görünmedi

Bazı durumlarda, geçici sorunlar nedeniyle ağ sorunları veya bir hata, bir uygulama başarısız ekleniyor. Tıkladığınızda böyle anlayabilirsiniz **bildirimleri** (zil) simgesine sağ üst tarafındaki Azure Portalı'nı ve bir kırmızı (!) simgesi yanına bakın, **uygulaması oluşturma** bildirim. Bu, uygulama oluştururken bir hata oluştu gösterir.

Tıklatıldığında bir hatayla karşılaşırsanız **Ekle** düğmesi, göreceğiniz bir **bildirim** içinde bir **hata** durumu. Daha fazla bilgi edinmek veya destek engingeer ile paylaşmak için hata hakkında daha fazla ayrıntı isterseniz içindeki adımları izleyerek hata hakkında daha fazla bilgi görebilirsiniz [portal bildirim ayrıntılarını görmek nasıl](#how-to-see-the-details-of-a-portal-notification) bölümü.

 ## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>Bunu ekledikten sonra my uygulamayı kurmak nasıl çıkılacağını bilmiyoruz

Uygulamaları hakkında öğrenme yardıma gereksinim duyarsanız [SaaS uygulamaları Azure Active Directory ile tümleştirme için nasıl öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) makale başlatmak için iyi bir yerdir.

Bu, ek olarak [Azure AD uygulamaları belge kitaplığı](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) Azure AD ile çoklu oturum açma ve nasıl çalıştığı hakkında daha fazla bilgi için Yardım.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Bir portal bildirim ayrıntılarını görmek nasıl

Aşağıdaki adımları izleyerek herhangi bir portal bildirim ayrıntılarını görebilirsiniz:

1.  tıklatın **bildirimleri** simgesi (zil) Azure portalının sağ üst

2.  Herhangi bir bildirim seçin bir **hata** durumu (bunları yanında kırmızı (!) sahip olanlar).

    >[!NOTE]
    >Bildirimlerde'ı bir **başarılı** veya **sürüyor** durumu.
    >
    >

3.  Bu açık **bildirim ayrıntıları** dikey.

4.  Bu bilgileri kullanın kendinize sorun hakkında daha fazla ayrıntı anlayın.

5.  Hala yardıma ihtiyacınız varsa, bu bilgiler sorununuzu Yardım almak için bir destek mühendisine veya ürün grubu ile paylaşabilirsiniz.

6.  Tıklatın **kopya** **simgesi** sağındaki **kopyalama hatası** desteği veya ürün grubu mühendislik ile paylaşmak için tüm bildirim ayrıntıları kopyalamak için metin kutusu

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Bir destek mühendisine bildirim ayrıntıları göndererek Yardım alma

Paylaştığınız çok önemlidir **aşağıda listelenen tüm ayrıntıları** size hızla yardımcı böylece yardıma gereksiniminiz varsa bir destek mühendisine ile. Bu yana kolayca yapabilirsiniz **bir ekran görüntüsü alma** veya tıklatarak **kopyalama hata simgesi**, sağ tarafında bulunan **kopyalama hatası** metin kutusu.

## <a name="notification-details-explained"></a>Açıklanan bildirim ayrıntıları

Daha fazla her bildirimin öğelerini anlamına gelir ve bunların her birini örnekleri verir aşağıda açıklanmaktadır.

### <a name="essential-notification-items"></a>Temel bildirim öğeleri

-   **Başlık** – bildirimin açıklayıcı bir başlık

  * Örnek – **uygulama proxy ayarları**

-   **Açıklama** – ne işlem sonucunda oluşan açıklaması

    -   Örnek – **girilen iç url başka bir uygulama tarafından zaten kullanılıyor**

-   **Bildirim kimliği** – bildirim benzersiz kimliği

    -   Örnek – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **İstemci istek kimliği** – tarayıcınız tarafından yapılan belirli bir istek kimliği

    -   Örnek – **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Zaman damgası UTC** – sırasında bildirim meydana geldiği UTC zaman damgası

    -   Örnek – **2017-03-23T19:50:43.7583681Z**

-   **İç işlem kimliği** – iç kimlik kullanabileceğiniz bizim sistemlerinde hata aramak için

    -   Örnek – **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **UPN** – işlemi gerçekleştiren kullanıcı

    -   Örnek –**tperkins@f128.info**

-   **Kiracı kimliği** – işlemi gerçekleştiren kullanıcının üyesi olduğu Kiracı benzersiz kimliği

    -   Örnek – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Kullanıcı nesnesi kimliği** – işlemi gerçekleştiren kullanıcının benzersiz kimliği

    -   Örnek – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Ayrıntılı bildirim öğeleri

-   **Görünen ad** – **(boş olabilir)** hatası için ayrıntılı bir görünen ad

    -   Örnek – **uygulama proxy ayarları**

-   **Durum** – bildirim özel durumu

    -   Örnek – **başarısız oldu**

-   **Nesne Kimliği** – **(boş olabilir)** göre işlemi gerçekleştirildiği nesne kimliği

    -   Örnek – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Ayrıntılar** – ayrıntılı ne işlem sonucunda oluşan açıklaması

    -   Örnek – **iç url 'http://bing.com/', zaten kullanımda olduğundan geçersiz**

-   **Kopyalama hatası** – tıklatın **Kopyala simgesi** sağındaki **kopyalama hatası** desteği veya ürün grubu mühendislik ile paylaşmak için tüm bildirim ayrıntıları kopyalamak için metin kutusu

    -   Örnek```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile uygulamaları yönetme](active-directory-enable-sso-scenario.md)
