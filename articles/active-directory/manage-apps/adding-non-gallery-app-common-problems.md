---
title: Galeri dışı uygulama eklenirken sorun | Microsoft Docs
description: Özel Galeri dışı uygulamalar eklerken ortak sorunları insanların yüz anlama
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 38a9ef04389318d3588649117c930ff6efa3fe4e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65784489"
---
# <a name="problem-adding-a-non-gallery-application"></a>Galeri dışı uygulama eklenirken sorun oluştu

Bu makale ortak sorunları insanların yüz eklerken anlamanıza yardımcı olur **Özel Galeri dışı uygulamalar** ve bunları çözmek için yapabilirsiniz. 

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>"Ekle" düğmesi tıkladım ve uygulamamın görünür uzun sürdü

Bazı durumlarda, 1-2 dakika sürebilir (ve bazı durumlarda daha uzun) bir uygulama dizininize eklendikten sonra görünür. Bu normal beklenen performans olmamasına karşın, uygulama eklenmesi devam ederken tıklayarak görebilirsiniz **bildirimleri** simgesine (zil) sağ üst kısmındaki [Azure portalında](https://portal.azure.com/) ve aranıyor için bir **sürüyor** veya **tamamlandı** etiketli bildirim **uygulama oluşturma**.

Uygulamanızı hiçbir zaman eklenir veya tıklandığında hatayla karşılaşan **Ekle** düğmesi, gördüğünüz bir **bildirim** içinde bir **hata** durumu. Bir destek mühendisiyle paylaşın veya daha fazla bilgi için hata hakkındaki ayrıntılar istiyorsanız, içindeki adımları izleyerek hata hakkında daha fazla bilgi görebilirsiniz [portal bildirimi ayrıntılarını görmek nasıl](#how-to-see-the-details-of-a-portal-notification) bölümü.

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>"Ekle" düğmesi tıkladım ve uygulamamın görünmedi

Bazı durumlarda, geçici bir sorun nedeniyle ağ sorunlarını veya bir hata, bir uygulama başarısız ekleniyor. ' A tıkladığınızda böyle söyleyebilirsiniz **bildirimleri** simgesine (zil) sağ üst köşesinde Azure portalı ve kırmızı (!) simgesi yanındaki bakın, **uygulama oluşturma** bildirim. Bu, uygulama oluştururken bir hata oluştu gösterir.

Tıklandığında bir hatayla karşılaşırsanız **Ekle** düğmesi, gördüğünüz bir **bildirim** içinde bir **hata** durumu. Bir destek mühendisiyle paylaşın veya daha fazla bilgi için hata hakkındaki ayrıntılar istiyorsanız, içindeki adımları izleyerek hata hakkında daha fazla bilgi görebilirsiniz [portal bildirimi ayrıntılarını görmek nasıl](#how-to-see-the-details-of-a-portal-notification) bölümü.

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>Bunu ekledikten sonra Uygulamam ayarlama bilmiyorum

Özel uygulamalar hakkında öğrenme yardıma ihtiyacınız olursa [Azure AD uygulamaları belge kitaplığı](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) Azure AD ile çoklu oturum açma ve nasıl çalıştığı hakkında daha fazla bilgi için Yardım.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Portal bildirimi ayrıntılarını görme

Aşağıdaki adımları izleyerek, herhangi bir portal bildirim ayrıntılarını görebilirsiniz:

1. tıklayın **bildirimleri** Azure portalının sağ üst kısımdaki simgesine (zil)

2. Herhangi bir bildirim seçin bir **hata** durumu (yanında bir kırmızı (!) sahip olanlar).

   >[!NOTE]
   >Bildirimlerde tıklayamazsınız bir **başarılı** veya **sürüyor** durumu.
   >
   >

4. Sağlanan bilgileri kullanmak **bildirim ayrıntılarını** sorun hakkında daha fazla ayrıntı anlamak için.

5. Hala yardıma ihtiyacınız varsa, bu bilgiler sorununuzu Yardım almak için bir destek mühendisi veya ürün grubu ile paylaşabilirsiniz.

6. Tıklayın **Kopyala simgesine** sağındaki **kopyalama hatası** desteği veya ürün grubu mühendisiyle paylaşın için tüm bildirim ayrıntılarını kopyalamak için metin kutusu.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Bir destek mühendisiyle bildirim ayrıntılarını göndererek Yardım alma

Paylaştığınız çok önemli olduğu **aşağıda listelenen tüm ayrıntıları** böylece bunlar hızlıca Yardım yardıma ihtiyacınız varsa, bir destek mühendisi ile. Bu bir kolayca ile yapabileceğiniz **bir ekran görüntüsü almayı** veya tıklayarak **kopyalama hata simgesi**, sağında bulunan **kopyalama hatası** metin.

## <a name="notification-details-explained"></a>Bildirim ayrıntıları açıklaması

Aşağıdaki açıklamalar bildirimleri hakkında daha fazla ayrıntı için bkz.

### <a name="essential-notification-items"></a>Önemli bildirim öğeleri

- **Başlık** – bildirimin açıklayıcı bir başlık
  *  Örneğin, **uygulama proxy'si ayarları**

- **Açıklama** – ne işlemi nedeniyle oluştu açıklaması

  *  Örneğin, **girilen iç url başka bir uygulama tarafından zaten kullanılıyor**

- **Bildirim kimliği** – bildirim benzersiz kimliği

  *  Örneğin, **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

- **İstemci istek kimliği** – tarayıcınız tarafından yapılan belirli bir istek kimliği

  *  Örneğin, **302fd775-3329-4670-a9f3-bea37004f0bc**

- **Zaman damgası UTC** – sırasında bildirim gerçekleştiği, UTC zaman damgası

  *  Örneğin, **2017-03-23T19:50:43.7583681Z**

- **İç işlem kimliği** – iç kimliği kullanabiliriz sistemlerimizde hata aramak için

  *  Örneğin, **71a2f329-ca29-402f-aa72-bc00a7aca603**

- **UPN** – işlemi gerçekleştiren kullanıcının

  *  Örneğin, **tperkins\@f128.info**

- **Kiracı kimliği** – işlemi gerçekleştiren kullanıcının üyesi olduğu kiracının benzersiz kimliği

  *  Örneğin, **7918d4b5-0442-4a97-be2d-36f9f9962ece**

- **Kullanıcı nesne kimliği** – işlemi gerçekleştiren kullanıcının benzersiz kimliği

  *  Örneğin, **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Ayrıntılı bildirim öğeleri

- **Görünen ad** – **(boş olabilir)** hatanın ayrıntılı bir görünen ad

  *  Örneğin, **uygulama proxy'si ayarları**

- **Durum** – bildirim özel durumu

  *  Örneğin, **başarısız oldu**

- **Nesne Kimliği** – **(boş olabilir)** karşı işlemi gerçekleştirildi nesne kimliği

  *  Örneğin, **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

- **Ayrıntılar** – ayrıntılı açıklaması ne işlemi nedeniyle oluştu

  *  Örneğin, **iç url `https://bing.com/` zaten kullanımda olduğundan geçerli değil.**

- **Kopyalama hatası** – tıklayın **Kopyala simgesine** sağındaki **kopyalama hatası** desteği veya ürün grupla paylaşmak için tüm bildirim ayrıntılarını kopyalamak için metin kutusu 
- engineer (mühendis)

  *  Örnek ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```




