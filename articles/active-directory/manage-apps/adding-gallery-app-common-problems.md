---
title: Azure AD galeri uygulaması eklenirken sorun | Microsoft Docs
description: Azure AD galeri uygulamaları ve bunların çözülmesine yönelik yapabilecekleriniz eklerken ortak sorunları insanların yüz anlama
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: a898b5b235099109fcfeaaa4d647493e54caf57e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60438804"
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a>Azure AD galeri uygulaması eklenirken sorun oluştu

Bu makalede Azure AD galeri uygulamaları ve bunların çözülmesine yönelik yapabilecekleriniz eklerken ortak sorunları insanların yüz anlamanıza yardımcı olur.

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>"Ekle" düğmesi tıkladım ve uygulamamın görünür uzun sürdü

Bazı durumlarda, 1-2 dakika sürebilir (ve bazı durumlarda daha uzun) bir uygulama dizininize eklendikten sonra görünür. Bu normal beklenen performans olmamasına karşın, uygulama eklenmesi devam ederken tıklayarak görebilirsiniz **bildirimleri** simgesine (zil) sağ üst kısmındaki [Azure portalında](https://portal.azure.com/) ve aranıyor için bir **sürüyor** veya **tamamlandı** etiketli bildirim **uygulama ekleniyor.**

Uygulamanızı hiçbir zaman eklenir veya tıklandığında hatayla karşılaşan **Ekle** düğmesi, gördüğünüz bir **bildirim** içinde bir **hata** durumu. Bir destek mühendisiyle paylaşın veya daha fazla bilgi için hata hakkındaki ayrıntılar istiyorsanız, içindeki adımları izleyerek hata hakkında daha fazla bilgi görebilirsiniz [portal bildirimi ayrıntılarını görmek nasıl](#how-to-see-the-details-of-a-portal-notification) bölümü.

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>"Ekle" düğmesi tıkladım ve uygulamamın görünmedi

Bazı durumlarda, geçici bir sorun, ağ sorunlarını veya bir hata nedeniyle, bir uygulama ekleme başarısız. ' A tıkladığınızda böyle söyleyebilirsiniz **bildirimleri** simgesine (zil) sağ üst köşesinde Azure portalı ve kırmızı (!) simgesi yanındaki bakın, **uygulamasını ekleme** bildirim. Bu, uygulama oluştururken bir hata oluştu gösterir.

Tıklandığında bir hatayla karşılaşırsanız **Ekle** düğmesi, gördüğünüz bir **bildirim** içinde bir **hata** durumu. Bir destek mühendisiyle paylaşın veya daha fazla bilgi için hata hakkındaki ayrıntılar istiyorsanız, içindeki adımları izleyerek hata hakkında daha fazla bilgi görebilirsiniz [portal bildirimi ayrıntılarını görmek nasıl](#how-to-see-the-details-of-a-portal-notification) bölümü.

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>Bunu ekledikten sonra Uygulamam ayarlama bilmiyorum

Öğrenimi uygulamaları hakkında yardıma ihtiyacınız olursa [nasıl Azure Active Directory ile SaaS uygulamalarını tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) makale başlatmak için iyi bir yerdir.

Bu, ek olarak [Azure AD uygulamaları belge kitaplığı](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) , Azure AD ile çoklu oturum açma ve nasıl çalıştığı hakkında daha fazla bilgi edinmek için yardımcı olur.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Portal bildirimi ayrıntılarını görme

Aşağıdaki adımları izleyerek, herhangi bir portal bildirim ayrıntılarını görebilirsiniz:

1.  Seçin **bildirimleri** simgesine (zil) Azure portalının sağ üst

2.  Herhangi bir bildirim seçin bir **hata** durumu (yanında bir kırmızı (!) sahip olanlar).

    >[!NOTE]
    >Bildirimlerde tıklayamazsınız bir **başarılı** veya **sürüyor** durumu.
    >
    >

4.  Sağlanan bilgileri kullanmak **bildirim ayrıntılarını** sorun hakkında daha fazla ayrıntı anlamak için.

5.  Hala yardıma ihtiyacınız varsa, bu bilgiler sorununuzu Yardım almak için bir destek mühendisi veya ürün grubu ile paylaşabilirsiniz.

6.  Tıklayın **kopyalama** **simgesi** sağındaki **kopyalama hatası** desteği veya ürün grubu mühendisiyle paylaşın için tüm bildirim ayrıntılarını kopyalamak için metin kutusu.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Bir destek mühendisiyle bildirim ayrıntılarını göndererek Yardım alma

Paylaştığınız çok önemli olduğu **aşağıda listelenen tüm ayrıntıları** böylece bunlar hızlıca Yardım yardıma ihtiyacınız varsa, bir destek mühendisi ile. Bu bir kolayca ile yapabileceğiniz **bir ekran görüntüsü almayı** veya tıklayarak **kopyalama hata simgesi**, sağında bulunan **kopyalama hatası** metin.

## <a name="notification-details-explained"></a>Bildirim ayrıntıları açıklaması

Aşağıdaki açıklamalar bildirimleri hakkında daha fazla ayrıntı için bkz.

### <a name="essential-notification-items"></a>Önemli bildirim öğeleri

- **Başlık** – bildirimin açıklayıcı bir başlık

  * Örneğin, **uygulama proxy'si ayarları**

- **Açıklama** – ne işlemi nedeniyle oluştu açıklaması

  -   Örneğin, **girilen iç url başka bir uygulama tarafından zaten kullanılıyor**

- **Bildirim kimliği** – bildirim benzersiz kimliği

  -   Örneğin, **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

- **İstemci istek kimliği** – tarayıcınız tarafından yapılan belirli bir istek kimliği

  -   Örneğin, **302fd775-3329-4670-a9f3-bea37004f0bc**

- **Zaman damgası UTC** – sırasında bildirim gerçekleştiği, UTC zaman damgası

  -   Örneğin, **2017-03-23T19:50:43.7583681Z**

- **İç işlem kimliği** – iç kimliği kullanabiliriz sistemlerimizde hata aramak için

  -   Örneğin, **71a2f329-ca29-402f-aa72-bc00a7aca603**

- **UPN** – işlemi gerçekleştiren kullanıcının

  -   Örneğin, **tperkins\@f128.info**

- **Kiracı kimliği** – işlemi gerçekleştiren kullanıcının üyesi olduğu kiracının benzersiz kimliği

  -   Örneğin, **7918d4b5-0442-4a97-be2d-36f9f9962ece**

- **Kullanıcı nesne kimliği** – işlemi gerçekleştiren kullanıcının benzersiz kimliği

  -   Örneğin, **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Ayrıntılı bildirim öğeleri

-   **Görünen ad** – **(boş olabilir)** hatanın ayrıntılı bir görünen ad

    -   Örneğin, **uygulama proxy'si ayarları**

-   **Durum** – bildirim özel durumu

    -   Örneğin, **başarısız oldu**

-   **Nesne Kimliği** – **(boş olabilir)** karşı işlemi gerçekleştirildi nesne kimliği

    -   Örneğin, **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Ayrıntılar** – ayrıntılı açıklaması ne işlemi nedeniyle oluştu

    -   Örneğin, **iç url `https://bing.com/` zaten kullanımda olduğundan geçerli değil.**

-   **Kopyalama hatası** – tıklayın **Kopyala simgesine** sağındaki **kopyalama hatası** desteği veya ürün grupla paylaşmak için tüm bildirim ayrıntılarını kopyalamak için metin kutusu 
-   engineer (mühendis)

    -   Örnek ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

