---
title: "Parola tek oturum açma için Galeri olmayan uygulama yapılandırma sorunu | Microsoft Docs"
description: "Azure AD uygulama galerisinde listelenmeyen Özel Galeri olmayan uygulamalar için parola çoklu oturum açmayı yapılandırırken ortak sorunları kişiler yüz anlama"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 265d58ce4098ea924318dfe2959397d60a0721d6
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Parola tek oturum açma için Galeri olmayan uygulama yapılandırma sorunu

Bu makale ortak sorunları kişiler yüz yapılandırırken öğrenmenize yardımcı **parola çoklu oturum açma** bir galeri olmayan uygulama ile.

## <a name="how-to-capture-sign-in-fields-for-an-application"></a>Oturum açma alanları bir uygulama için yakalama

Oturum açma alan yakalama HTML etkin oturum açma sayfaları için yalnızca desteklenir ve olduğu **için standart olmayan oturum açma sayfaları desteklenmeyen**, Flash, veya diğer HTML etkin olmayan teknolojileri kullananlar ister.

Özel uygulamalarınız için oturum açma alanları yakalamak için iki yolu vardır:

-   Otomatik oturum açma alan yakalama

-   Oturum açma el ile alan yakalama

**Otomatik oturum açma alan yakalama** kullanıyorlarsa iyi çoğu HTML etkin oturum açma sayfaları ile çalışır **kullanıcı adı ve parola girişi için iyi bilinen DIV kimlikleri** alan. Bu çalışır belirli ölçütlere uyan DIV kimliklerini bulmak için sayfada HTML değiştirilene ve biz daha sonra parolaları ona oynatabilirsiniz şekilde bu uygulama için bu meta veri kaydetme tarafından yoludur.

**Oturum açma el ile alan yakalama** durumda kullanılabilmesi için uygulama **satıcı etiket değil** oturum açmak için kullanılan giriş alanları. Oturum açma el ile alan yakalama durumda de kullanılabilir olduğunda **satıcı işler birden çok alan** , otomatik olarak algılanır olamaz. Bu alanların sayfada olduğu bize sürece azure AD oturum açma sayfasında, olduğu gibi sayıda alanlar için veri depolayabilirsiniz.

Genel olarak, **otomatik oturum açma alan yakalama çalışmazsa, her zaman el ile seçeneği çalışırken öneririz.**

### <a name="how-to-automatically-capture-sign-in-fields-for-an-application"></a>Oturum açma alanları bir uygulama için otomatik olarak yakalama

Yapılandırmak için **parola tabanlı çoklu oturum açma** kullanarak bir uygulama için **otomatik oturum açma alan yakalama**, aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modunu seçin **parola tabanlı oturum açma.**

9.  Girin **oturum açma URL'si**. Bu kullanıcılar, kullanıcı adını ve oturum açmak için parola girdiğiniz yere URL'dir. **Oturum açma alanları sağladığınız URL'de görünür olduğundan emin olun**.

10. **Kaydet** düğmesine tıklayın.

11. Bunu gerçekleştirdikten sonra biz otomatik olarak bir kullanıcı adı için bu URL'yi scrape ve parola giriş kutusu ve Azure AD erişim paneli tarayıcı uzantısı kullanarak bu uygulamaya parolaları güvenli bir şekilde iletmek için kullanmanızı sağlar.

## <a name="how-to-manually-capture-sign-in-fields-for-an-application"></a>El ile oturum açma alanları bir uygulama için yakalama

El ile oturum açma alanları yakalamak için öncelikle erişim paneli tarayıcı uzantısı yüklü olması gerekir ve **InPrivate, incognito ya da özel modunda çalışmıyor.** Tarayıcı uzantısı yüklemek için adımları [erişim paneli tarayıcı uzantısı yükleme](#i-cannot-manually-detect-sign-in-fields-for-my-application) bölümü.

Yapılandırmak için **parola tabanlı çoklu oturum açma** kullanarak bir uygulama için **oturum açma el ile alan yakalama**, aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Çoklu oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **çoklu oturum açma** uygulamanın sol taraftaki gezinti menüsünde.

8.  Modunu seçin **parola tabanlı oturum açma.**

9.  Girin **oturum açma URL'si**. Bu kullanıcılar, kullanıcı adını ve oturum açmak için parola girdiğiniz yere URL'dir. **Oturum açma alanları sağladığınız URL'de görünür olduğundan emin olun**.

10. **Kaydet** düğmesine tıklayın.

11. Bunu gerçekleştirdikten sonra biz otomatik olarak bir kullanıcı adı için bu URL'yi scrape ve parola giriş kutusu ve Azure AD erişim paneli tarayıcı uzantısı kullanarak bu uygulamaya parolaları güvenli bir şekilde iletmek için kullanmanızı sağlar. Bu başarısız durumda, cihazı **oturum açma el ile alan yakalama kullanmak için oturum açma modunu değiştirme** 12 adıma geçmeden tarafından.

12. tıklatın **yapılandırma &lt;appname&gt; parola çoklu oturum açma ayarları**.

13. Seçin **el ile oturum açma alanları algılama** yapılandırma seçeneği.

14. **Tamam**’a tıklayın.

15. **Kaydet** düğmesine tıklayın.

16. Erişim paneli kullanmak için üzerinde ekran yönergelerini izleyin.

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>"Bu URL'de oturum açma alanları bulamadık" hata bakın

Oturum açma alanlarının otomatik algılama başarısız olduğunda, bu hatayı görürsünüz. Bu sorunu gidermek için adımları izleyerek oturum açma el ile alan algılama deneyin [el ile oturum açma alanları bir uygulama için yakalama](#how-to-manually-capture-sign-in-fields-for-an-application) bölümü.

## <a name="i-see-an-unable-to-save-single-sign-on-configuration-error"></a>"Çoklu oturum açma yapılandırmasını kaydetmek için kurtarılamadı" bakın hatası

Belirli nadir durumlarda, tek oturum açma yapılandırmasını güncelleştirme başarısız olabilir. Bu çözümlemeye tek oturum açma yapılandırması yeniden kaydediliyor.

Bu sürekli başarısız olmaya devam ederse, destek olayı açın ve toplanan bilgi sağlamak [portal bildirim ayrıntılarını görmek nasıl](#i-cannot-manually-detect-sign-in-fields-for-my-application) ve [bir destek mühendisine bildirim ayrıntıları göndererek Yardım alma](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) bölümler.

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a>I el ile oturum alanlarında Uygulamam için algılayamıyor

El ile algılama çalışmıyor açtığınızda görebilirsiniz davranışları bazıları şunlardır:

-   Çalışması için el ile yakalama işlemi görünen ancak yakalanan alanları doğru değildi

-   Sağ alanları yakalama işlemi gerçekleştirirken vurgulanmış yok

-   Yakalama işlemi bana uygulama oturum açma sayfasına beklendiği gibi alır, ancak hiçbir şey olmaz

-   Çalışması için el ile yakalama görünür, ancak Kullanıcılarım uygulamaya erişim panelinden gittiğinizde SSO gerçekleşmez.

Bu sorunlarla karşılaşırsanız aşağıdakileri denetleyin:

-   Erişim paneli tarayıcı uzantısı en son sürümüne sahip olduğunuzdan emin olun **yüklü** ve **etkin** içindeki adımları izleyerek [erişim paneli tarayıcı uzantısıYükleme](#how-to-install-the-access-panel-browser-extension) bölümü.

-   Yakalama işlemi sırasında tarayıcınızda denediğiniz değil emin olun **incognito, InPrivate ya da özel mod**. Erişim paneli uzantısı bu modda desteklenmiyor.

-   Kullanıcılarınız uygulamaya erişim panelinde oturum açmak ayarlamadığınızdan emin olun **incognito, InPrivate ya da özel mod**. Erişim paneli uzantısı bu modda desteklenmiyor.

-   Doğru alanları kırmızı işaretçileri sağlayarak el ile yakalama işlemi yeniden deneyin.

-   El ile yakalama işlemi askıda görünüyor veya oturum açma sayfası yapmaz her şeyi (durumda yukarıdaki 3), el ile yakalama işlemi yeniden deneyin. Ancak, işlem tamamlandıktan sonra bu kez basın **F12** düğmesi tarayıcınızın Geliştirici konsolunu açın. Bir kez, açık **konsol** ve türü **window.location= "&lt;uygulama yapılandırırken belirttiğiniz URL'de oturum girin&gt;"** ve tuşuna basın **Enter**. Bu zorla bir sayfa, yakalama işlemi sona yeniden yönlendirilmesi için ve yakalanan alanlar depolar.

Bu yaklaşım hiçbiri sizin için çalışıyorsanız, size yardımcı olabilir. Ne, toplanan bilgilerinin yanı sıra çalıştığınız ayrıntılarını içeren bir destek servis talebi açma [portal bildirim ayrıntılarını görmek nasıl](#i-cannot-manually-detect-sign-in-fields-for-my-application) ve [destek mühendisine bildirim ayrıntıları göndererek Yardım alma ](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) (varsa) bölümler.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısı yükleme

Erişim paneli tarayıcı uzantısı yüklemek için aşağıdaki adımları izleyin:

1.  Açık [erişim paneli](https://myapps.microsoft.com) olarak oturum açın ve desteklenen tarayıcılar birinde bir **kullanıcı** Azure ad.

2.  tıklatın bir **parola SSO uygulaması** erişim panelinde.

3.  Yazılımı yüklemek soran istem içinde seçin **Şimdi Yükle**.

4.  Tarayıcınıza bağlı için karşıdan yükleme bağlantısı yönlendirilmiş. **Ekleme** tarayıcınız uzantısı.

5.  Tarayıcınız isterse, ya da seçin **etkinleştirmek** veya **izin** uzantısı.

6.  Bir kez yüklenir, **yeniden** tarayıcı oturumunda.

7.  Erişim paneline oturum açın ve, varsa görebilirsiniz **başlatma** parola SSO uygulamalarınızı.

Ayrıca uzantısı Chrome ve Firefox için aşağıya doğrudan bağlantılarından yükleyebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Bir portal bildirim ayrıntılarını görmek nasıl

Aşağıdaki adımları izleyerek herhangi bir portal bildirim ayrıntılarını görebilirsiniz:

1.  tıklatın **bildirimleri** simgesi (zil) Azure portalının sağ üst

2.  Herhangi bir bildirim seçin bir **hata** durumu (bunları yanında kırmızı (!) sahip olanlar).

  >! DİKKAT] bildirimleri tıklatın olamaz bir **başarılı** veya **sürüyor** durumu.
  >
  >

3.  Bu açık **bildirim ayrıntıları** dikey.

4.  Bu bilgileri kullanın kendinize sorun hakkında daha fazla ayrıntı anlayın.

5.  Hala yardıma ihtiyacınız varsa, bu bilgiler sorununuzu Yardım almak için bir destek mühendisine veya ürün grubu ile paylaşabilirsiniz.

6.  Tıklatın **kopya** **simgesi** sağındaki **kopyalama hatası** desteği veya ürün grubu mühendislik ile paylaşmak için tüm bildirim ayrıntıları kopyalamak için metin kutusu.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Bir destek mühendisine bildirim ayrıntıları göndererek Yardım alma

Paylaştığınız çok önemlidir **aşağıda listelenen tüm ayrıntıları** size hızla yardımcı böylece yardıma gereksiniminiz varsa bir destek mühendisine ile. Bu yana kolayca yapabilirsiniz **bir ekran görüntüsü alma** veya tıklatarak **kopyalama hata simgesi**, sağ tarafında bulunan **kopyalama hatası** metin kutusu.

## <a name="notification-details-explained"></a>Açıklanan bildirim ayrıntıları

Daha fazla her bildirimin öğelerini anlamına gelir ve bunların her birini örnekleri verir aşağıda açıklanmaktadır.

### <a name="essential-notification-items"></a>Temel bildirim öğeleri

-   **Başlık** – bildirimin açıklayıcı bir başlık

    -   Örnek – **uygulama proxy ayarları**

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

    -   Örnek * – **uygulama proxy ayarları**

-   **Durum** – bildirim özel durumu

    -   Örnek * – **başarısız oldu**

-   **Nesne Kimliği** – **(boş olabilir)** göre işlemi gerçekleştirildiği nesne kimliği

    -   Örnek – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Ayrıntılar** – ayrıntılı ne işlem sonucunda oluşan açıklaması

    -   Örnek – **iç url 'http://bing.com/', zaten kullanımda olduğundan geçersiz**

-   **Kopyalama hatası** – tıklatın **Kopyala simgesi** sağındaki **kopyalama hatası** desteği veya ürün grubu mühendislik ile paylaşmak için tüm bildirim ayrıntıları kopyalamak için metin kutusu

    -   Örnek –```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Sonraki adımlar
[Çoklu oturum açma uygulamalarınızı uygulama proxy'si ile sağlayın.](active-directory-application-proxy-sso-using-kcd.md)

