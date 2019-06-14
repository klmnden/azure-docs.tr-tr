---
title: Parola çoklu oturum açma galeri dışı bir uygulama yapılandırma sorunu | Microsoft Docs
description: Azure AD uygulama Galerisi'nde listelenmeyen Özel Galeri dışı uygulamalar için parola çoklu oturum açmayı yapılandırırken ortak sorunları insanların yüz anlama
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
ms.date: 07/11/2017
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8787008b396c2dd8ce1c006a40fee1e32e8100d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60442073"
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Parola çoklu oturum açma galeri dışı bir uygulama yapılandırma sorunu

Bu makale ortak sorunları insanların yüz yapılandırırken anlamanıza yardımcı **parola çoklu oturum açma** galeri dışı bir uygulama ile.

## <a name="how-to-capture-sign-in-fields-for-an-application"></a>Nasıl bir uygulama için oturum açma alanlarını Yakala

Oturum açma alanı yakalama yalnızca HTML etkin oturum açma sayfaları için desteklendiği ve **için standart olmayan oturum açma sayfalarını desteklenmeyen**, Flash veya diğer HTML etkin teknolojileri kullananlar ister.

Özel uygulamalarınız için oturum açma alanlarını yakalamak için iki yolu vardır:

-   Otomatik oturum açma alanı yakalama

-   El ile oturum açma alanı yakalama

**Otomatik oturum açma alanı yakalama** kullanıyorlarsa de çoğu HTML etkin oturum açma sayfaları ile çalışır **iyi bilinen DIV kimliklerini kullanıcı adı ve parola** alan. Bu işlemin çalıştığı belirli ölçütlerle eşleşen DIV kimliklerini bulmak için sayfanın HTML değiştirilene göre ve ardından bu uygulama parolaları, daha sonra yeniden yürütme için meta verilerin kaydederek yoludur.

**El ile oturum açma alanı yakalama** durumda kullanılabilmesi için uygulama **satıcı etiketi olmayan** oturum açmak için kullanılan giriş alanları. El ile oturum açma alanı yakalama durumunda da kullanılabilir olduğunda **satıcı işler birden çok alan** otomatik olarak algılanan olamaz. Bu alanların sayfada olduğu bize sürece azure AD oturum açma sayfasında olduğu gibi çok alanlar için veri depolayabilir.

Genel olarak, **otomatik oturum açma alanı yakalama çalışmazsa elle seçeneğini deneyin.**

### <a name="how-to-automatically-capture-sign-in-fields-for-an-application"></a>Nasıl otomatik olarak bir uygulama için oturum açma alanlarını Yakala

Yapılandırmak için **parola tabanlı çoklu oturum açma** kullanarak uygulama için **otomatik oturum açma alanı yakalama**, aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Modu **parola tabanlı oturum açma.**

9. Girin **oturum açma URL'si**, kullanıcılara nereden girin, kullanıcı adını ve oturum açmak için parola URL'si. **Oturum açma alanlarını sağladığınız URL'SİNDE görünür olduğundan emin olun**.

10. **Kaydet** düğmesine tıklayın.

11. Bir kez parola giriş kutusu ve Azure AD erişim paneli tarayıcı uzantısını kullanırken, uygulama parolaları güvenli bir şekilde aktarmaya kullanmanıza olanak sağlayan URL'sini otomatik olarak bir kullanıcı adı için scraped ve bunu.

## <a name="how-to-manually-capture-sign-in-fields-for-an-application"></a>Nasıl el ile bir uygulama için oturum açma alanlarını Yakala

Oturum açma alanlarını el ile yakalamak için öncelikle erişim paneli tarayıcı uzantısının yüklü olması gerekir ve **InPrivate, gizli veya özel modunda çalışmıyor.** Tarayıcı uzantısını yüklemek için adımları [erişim paneli tarayıcı uzantısını nasıl yükleyeceğiniz](#i-cannot-manually-detect-sign-in-fields-for-my-application) bölümü.

Yapılandırmak için **parola tabanlı çoklu oturum açma** kullanarak uygulama için **el ile oturum açma alanı yakalama**, aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8. Modu **parola tabanlı oturum açma.**

9. Girin **oturum açma URL'si**, kullanıcılara nereden girin, kullanıcı adını ve oturum açmak için parola URL'si. **Oturum açma alanlarını sağladığınız URL'SİNDE görünür olduğundan emin olun**.

10. **Kaydet** düğmesine tıklayın.

11. Bir kez parola giriş kutusu ve Azure AD erişim paneli tarayıcı uzantısını kullanırken, uygulama parolaları güvenli bir şekilde aktarmaya kullanmanıza olanak sağlayan URL'sini otomatik olarak bir kullanıcı adı için scraped ve bunu. Hata olması durumunda, aşağıdakileri yapabilirsiniz **el ile oturum açma alanı yakalama kullanmak için oturum açma modunu değiştirme** 12. adıma devam etmeden tarafından.

12. tıklayın **yapılandırma &lt;appname&gt; parola çoklu oturum açma ayarları**.

13. Seçin **oturum açma alanlarını el ile algılama** yapılandırma seçeneği.

14. **Tamam**’a tıklayın.

15. **Kaydet**’e tıklayın.

16. Erişim paneli kullanmak için açık ekran yönergeleri izleyin.

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>Bir "Bu URL'de oturum açma alanları bulamadık" hatası görüyorum

Oturum açma alanlarını otomatik olarak algılanmasını başarısız olduğunda bu hatayı görürsünüz. Sorunu çözmek için el ile oturum açma alanı algılaması içindeki adımları izleyerek deneyin [nasıl el ile bir uygulama için oturum açma alanlarını Yakala](#how-to-manually-capture-sign-in-fields-for-an-application) bölümü.

## <a name="i-see-an-unable-to-save-single-sign-on-configuration-error"></a>Görüyorum "çoklu oturum açmayı yapılandırmayı kaydetmek için yapılandırılamıyor" hatası

Bazı nadir durumlarda, çoklu oturum açma yapılandırmasını güncelleştirme başarısız olabilir. Çözmek için çoklu oturum açma yapılandırması yeniden kaydetmeyi deneyin.

Tutarlı bir şekilde başarısız olmaya devam ederse, destek talebinde bulunun ve toplanan bilgiler sağlayan [portal bildirimi ayrıntılarını görmek nasıl](#i-cannot-manually-detect-sign-in-fields-for-my-application) ve [destek bildirim ayrıntılarını göndererek Yardım alma mühendislik](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) bölümler.

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a>Uygulamam için el ile oturum açma alanlarını algılayamıyor

El ile algılama değil çalışırken görebileceğiniz davranışları bazıları şunlardır:

-   El ile yakalama işleminin çalışması için görünen, ancak yakalanan alanları doğru değildi

-   Sağ alanları yakalama işlemi gerçekleştirirken vurgulanmış yok

-   Yakalama işlemi bana uygulamanın oturum açma sayfasına beklendiği gibi sürer, ancak hiçbir şey olmaz

-   El ile yakalama çalışıyor gibi görünür, ancak kullanıcılarımın uygulamaya erişim panelinden gittiğinizde SSO gerçekleşmez.

Bu sorunları yaşarsanız aşağıdakileri denetleyin:

-   Erişim paneli tarayıcı uzantısını en son sürümüne sahip olduğunuzdan emin olun **yüklü** ve **etkin** adımları izleyerek [erişimpanelitarayıcıuzantısınıyükleme](#how-to-install-the-access-panel-browser-extension) bölümü.

-   Yakalama işlemi sırasında tarayıcınızda çalıştığınız değil emin olmak **gizli, InPrivate veya özel modu**. Erişim paneli uzantısını bu modda desteklenmez.

-   Kullanıcılarınız uygulamaya erişim panelinden oturum açmaya çalışırken değil olun **gizli, InPrivate veya özel modu**. Erişim paneli uzantısını bu modda desteklenmez.

-   Doğru alanları üzerinde kırmızı işaretlerinin sağlayarak el ile yakalama işlemi yeniden deneyin.

-   El ile yakalama işlemi yanıt vermiyor gibi görünüyor veya oturum açma sayfası yapmaz (durum 3 yukarıda), hiçbir şey el ile yakalama işlemi yeniden deneyin. Ancak, işlemi tamamladıktan sonra bu kez basın **F12** tarayıcınızın Geliştirici konsolu açmak için düğmeyi. Burada, bir kez açana **konsol** ve türü **window.location= "&lt;Uygulamayı yapılandırırken belirttiğiniz oturum açma URL'sini girin&gt;"** ve tuşuna **Enter** . Bu, yakalama işlemi sonlandırır ve yakalanan alanları depolayan bir sayfayı yeniden yönlendirme zorlar.

Bu yaklaşımların hiçbiri işinize yaramazsa destek yardımcı olabilir. Hangi, toplanan bilgilerinin yanı sıra çalıştığınız ayrıntılarını ile destek talebinde bulunun [portal bildirimi ayrıntılarını görmek nasıl](#i-cannot-manually-detect-sign-in-fields-for-my-application) ve [bir destek mühendisiyle bildirim ayrıntılarını göndererek Yardım alma ](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) (varsa) bölümler.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükleme

Erişim paneli tarayıcı uzantısını yüklemek için aşağıdaki adımları izleyin:

1.  Açık [erişim paneli](https://myapps.microsoft.com) olarak oturum açın ve desteklenen tarayıcılar birinde bir **kullanıcı** Azure ad.

2.  ' a tıklayın bir **parola SSO uygulama** erişim panelinde.

3.  Yazılımı yüklemek soran istem içinde seçin **Şimdi Yükle**.

4.  Tarayıcınıza bağlı olarak indirme bağlantısını yönlendirilir. **Ekleme** tarayıcınızı uzantısı.

5.  Tarayıcınız isterse, ya da seçin **etkinleştirme** veya **izin** uzantısı.

6.  Yüklendiğinde, **yeniden** , tarayıcı oturumu.

7.  Erişim paneline oturum açın ve, varsa görebilirsiniz **başlatma** parola SSO uygulamalarınızı.

Ayrıca uzantısı Chrome ve Firefox için aşağıdaki doğrudan bağlantılardan indirebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Portal bildirimi ayrıntılarını görme

Aşağıdaki adımları izleyerek, herhangi bir portal bildirim ayrıntılarını görebilirsiniz:

1. tıklayın **bildirimleri** Azure portalının sağ üst kısımdaki simgesine (zil)

2. Herhangi bir bildirim seçin bir **hata** durumu (yanında bir kırmızı (!) sahip olanlar).

   >! Bildirimlerde dikkat] tıklayamazsınız bir **başarılı** veya **sürüyor** durumu.
   >
   >

3. **Bildirim ayrıntılarını** bölmesi açılır.

4. Bilgileri kendiniz sorun hakkında daha fazla ayrıntı anlamak için.

5. Hala yardıma ihtiyacınız varsa, sorununuzu Yardım almak için bir destek mühendisi veya ürün grubu bilgileri de paylaşabilirsiniz.

6. Tıklayın **kopyalama** **simgesi** sağındaki **kopyalama hatası** desteği veya ürün grubu mühendisiyle paylaşın için tüm bildirim ayrıntılarını kopyalamak için metin kutusu.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Bir destek mühendisiyle bildirim ayrıntılarını göndererek Yardım alma

Paylaştığınız çok önemli olduğu **aşağıda listelenen tüm ayrıntıları** böylece bunlar hızlıca Yardım yardıma ihtiyacınız varsa, bir destek mühendisi ile. Yapabilecekleriniz **bir ekran görüntüsünü** veya **kopyalama hata simgesi**, sağında bulunan **kopyalama hatası** metin.

## <a name="notification-details-explained"></a>Bildirim ayrıntıları açıklaması

Daha her bildirimin öğelerini anlamına gelir ve bunların her birini örnekleri verir aşağıda açıklanmaktadır.

### <a name="essential-notification-items"></a>Önemli bildirim öğeleri

-   **Başlık** – bildirimin açıklayıcı bir başlık

    -   Örneğin, **uygulama proxy'si ayarları**

-   **Açıklama** – ne işlemi nedeniyle oluştu açıklaması

    -   Örneğin, **girilen iç url başka bir uygulama tarafından zaten kullanılıyor**

-   **Bildirim kimliği** – bildirim benzersiz kimliği

    -   Örneğin, **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **İstemci istek kimliği** – tarayıcınız tarafından yapılan belirli bir istek kimliği

    -   Örneğin, **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Zaman damgası UTC** – sırasında bildirim gerçekleştiği, UTC zaman damgası

    -   Örneğin, **2017-03-23T19:50:43.7583681Z**

-   **İç işlem kimliği** – iç sistemlerimizde hata aramak için kullanılan

    -   Örneğin, **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **UPN** – işlemi gerçekleştiren kullanıcının

    -   Örneğin, **tperkins\@f128.info**

-   **Kiracı kimliği** – işlemi gerçekleştiren kullanıcının üyesi olduğu kiracının benzersiz kimliği

    -   Örneğin, **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Kullanıcı nesne kimliği** – işlemi gerçekleştiren kullanıcının benzersiz kimliği

    -   Örneğin, **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Ayrıntılı bildirim öğeleri

-   **Görünen ad** – **(boş olabilir)** hatanın ayrıntılı bir görünen ad

    -   Örneğin, **uygulama proxy'si ayarları**

-   **Durum** – bildirim özel durumu

    -   Örneğin, **başarısız oldu**

-   **Nesne Kimliği** – **(boş olabilir)** karşı işlemi gerçekleştirildi nesne kimliği

    -   Örneğin, **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Ayrıntılar** – ayrıntılı açıklaması ne işlemi nedeniyle oluştu

    -   Örneğin, **iç url '<https://bing.com/>' zaten kullanımda olduğundan geçerli değil.**

-   **Kopyalama hatası** – tıklayın **Kopyala simgesine** sağındaki **kopyalama hatası** desteği veya ürün grubu mühendisiyle paylaşın için tüm bildirim ayrıntılarını kopyalamak için metin kutusu

    -   Örnek: ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)

