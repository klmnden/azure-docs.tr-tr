---
title: Galeri dışı bir uygulama için parola SSO yapılandırmayla ilgili sorunlar | Microsoft Docs
description: Parola çoklu oturum açma (SSO) yapılandırdığınızda oluşan özel Azure AD uygulama galerisinde bulunmayan uygulamalar için genel sorunlar.
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
ms.openlocfilehash: 24330dc874173ba1c6f15abb7b4caf9f23e2e00c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67440344"
---
# <a name="problems-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Parola çoklu oturum açma galeri dışı bir uygulama yapılandırma sorunları

Bu makalede, yapılandırdığınızda oluşabilecek yaygın sorunları açıklar *parola çoklu oturum açma* (SSO) için bir galeri dışı uygulama.

## <a name="capture-sign-in-fields-for-an-app"></a>Bir uygulama için oturum açma alanlarını Yakala

Oturum açma alanı yakalama, yalnızca HTML etkin oturum açma sayfaları için desteklenir. Bu standart oturum açma sayfaları için olanlar gibi Adobe Flash kullanan veya HTML etkin olmayan diğer teknolojileri desteklenmez.

Özel uygulamalarınız için oturum açma alanlarını Yakala iki yolu vardır:

- **Otomatik oturum açma alanı yakalama** çalışır de çoğu HTML etkin oturum açma sayfaları ile *iyi bilinen DIV kimlikleri kullanıyorlarsa* için kullanıcı adı ve parola alanları. Belirli ölçütlerle eşleşen DIV kimliklerini bulmak için sayfanın HTML scraped. Böylece, daha sonra uygulamaya çalınabilir bu meta veri kaydedildi.

- **El ile oturum açma alanı yakalama** kullanılır uygulama satıcısıyla *giriş alanlarını oturum açma etiketi olmayan*. El ile yakalama de kullanılır, satıcı *otomatik olarak algılanan birden çok alan işler*. Azure Active Directory (Azure AD) olduğundan oturum açma sayfasında, bu alanların sayfada olduğu geldiğini sayıda alanlar için veri depolayabilir.

Genel olarak, otomatik oturum açma alanı yakalama olmayan çalışma, el ile seçeneğini deneyin.

### <a name="automatically-capture-sign-in-fields-for-an-app"></a>Otomatik olarak bir uygulama için oturum açma alanlarını Yakala

Parola tabanlı SSO otomatik oturum açma alanı yakalama kullanarak yapılandırmak için aşağıdaki adımları izleyin:

1. [Azure portalı](https://portal.azure.com/) açın. Bir genel yönetici veya ortak yönetici olarak oturum açın

2. Sol taraftaki gezinti bölmesinde seçin **tüm hizmetleri** Azure AD uzantısı'nı açın.

3. Tür **Azure Active Directory** filtre arama kutusuna ve ardından **Azure Active Directory**.

4. Seçin **kurumsal uygulamalar** Azure AD Gezinti bölmesinde.

5. Seçin **tüm uygulamaları** , uygulamaların bir listesini görüntülemek için.

   > [!NOTE]
   > İstediğiniz uygulamayı göremiyorsanız, kullanın **filtre** üst kısmındaki denetim **tüm uygulamaları** listesi. Ayarlama **Göster** seçeneği "Tüm uygulamalara."

6. SSO için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra seçin **çoklu oturum açma** sol taraftaki gezinti bölmesinde.

8. Seçin **parola tabanlı oturum açma** modu.

9. Girin **oturum açma URL'si**, kullanıcılar girdiğiniz yere kullanıcı adını ve oturum açmak için parola sayfasının URL'sini olduğu. *Oturum açma alanlarını sağladığınız URL için sayfada görünür olduğundan emin olun*.

10. **Kaydet**’i seçin.

    Sayfa, kullanıcı adı ve parola girdi kutularının için otomatik olarak scraped. Artık Azure AD erişim paneli tarayıcı uzantısını kullanarak bu uygulama parolaları güvenli bir şekilde iletmek için de kullanabilirsiniz.

### <a name="manually-capture-sign-in-fields-for-an-app"></a>El ile bir uygulama için oturum açma alanlarını Yakala

El ile oturum açma alanlarını Yakala için erişim paneli tarayıcı uzantısının yüklü olmalıdır. Ayrıca, tarayıcınız çalıştırılamaz. *InPrivate*, *ıncognito*, veya *özel* modu.

Uzantıyı yüklemek için bkz: [erişim paneli tarayıcı uzantısını yükleme](#install-the-access-panel-browser-extension) bu makalenin.

Bir uygulama için parola tabanlı SSO el ile oturum açma alanı yakalama kullanarak yapılandırmak için aşağıdaki adımları izleyin:

1. [Azure portalı](https://portal.azure.com/) açın. Bir genel yönetici veya ortak yönetici olarak oturum açın

2. Sol taraftaki gezinti bölmesinde seçin **tüm hizmetleri** Azure AD uzantısı'nı açın.

3. Tür **Azure Active Directory** filtre arama kutusuna ve ardından **Azure Active Directory**.

4. Seçin **kurumsal uygulamalar** Azure AD Gezinti bölmesinde.

5. Seçin **tüm uygulamaları** , uygulamaların bir listesini görüntülemek için.

   > [!NOTE] 
   > İstediğiniz uygulamayı göremiyorsanız, kullanın **filtre** üst kısmındaki denetim **tüm uygulamaları** listesi. Ayarlama **Göster** seçeneği "Tüm uygulamalara."

6. SSO için yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra seçin **çoklu oturum açma** sol taraftaki gezinti bölmesinde.

8. Seçin **parola tabanlı oturum açma** modu.

9. Girin **oturum açma URL'si**, sayfanın kullanıcılar girdiğiniz yere kullanıcı adını ve oturum açmak için parola olduğu. *Oturum açma alanlarını sağladığınız URL için sayfada görünür olduğundan emin olun*.

10. Seçin **yapılandırma *&lt;appname&gt;* parola çoklu oturum açma ayarları**.

11. Seçin **oturum açma alanlarını el ile algılama**.

14. **Tamam**’ı seçin.

15. **Kaydet**’i seçin.

16. Erişim paneli kullanmak için yönergeleri izleyin.

## <a name="troubleshoot-problems"></a>Sorunlarını giderme

### <a name="i-get-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>Bir "Bu URL'de oturum açma alanları bulamadık" hatası alıyorum

Oturum açma alanlarını otomatik olarak algılanmasını başarısız olduğunda bu hata iletisini alırsınız. Sorunu çözmek için el ile oturum açma alanı algılaması deneyin. Bkz: [el ile bir uygulama için oturum açma alanlarını Yakala](#manually-capture-sign-in-fields-for-an-app) bu makalenin.

### <a name="i-get-an-unable-to-save-single-sign-on-configuration-error"></a>Alabilir miyim "çoklu oturum açma yapılandırması kaydetmek için yapılandırılamıyor" hatası

Nadiren de olsa, SSO yapılandırmasını güncelleştirme başarısız olur. Bu sorunu çözmek için yapılandırmayı tekrar kaydetmeyi deneyin.

Hata almaya devam ederseniz destek talebinde bulunun. Açıklanan bilgileri içerir [portal bildirimi ayrıntıları görüntüle](#view-portal-notification-details) ve [bildirim ayrıntılarını Yardım almak için bir destek mühendisine göndermek](#send-notification-details-to-a-support-engineer-to-get-help) bu makalenin bölümler.

### <a name="i-cant-manually-detect-sign-in-fields-for-my-app"></a>Uygulamam için el ile oturum açma alanlarını algılanamıyor

El ile algılama çalışmayan olduğunda aşağıdaki davranışları mümkün görebilirsiniz:

- El ile yakalama işleminin çalışması için görünen, ancak yakalanan alanları doğru değil.

- Yakalama işlemi çalıştırdığında, doğru alanların vurgulanan yok.

- Yakalama işlemi, uygulamanın oturum açma sayfasına beklendiği gibi sürer, ancak hiçbir şey olmaz.

- El ile yakalama çalışıyor gibi görünür, ancak kullanıcılar uygulamaya erişim panelinden gittiğinizde SSO gerçekleşmez.

Bu sorunları yaşarsanız, şunları yapın:

- Erişim paneli tarayıcı uzantısını en son sürümüne sahip olduğunuzdan emin olun *yüklü ve etkin*. Bkz: [erişim paneli tarayıcı uzantısını yükleme](#install-the-access-panel-browser-extension) bu makalenin.

- Tarayıcınız içinde olmadığından emin olun *ıncognito*, *InPrivate*, veya *özel* yakalama işlemi sırasında modu. Erişim paneli uzantısı bu modda desteklenmez.

- Kullanıcılarınız uygulamaya erişim Paneli'nde oturum çalışılırken olmayan emin *ıncognito*, *InPrivate*, veya *özel modda*.

- El ile yakalama işlemi yeniden deneyin. Kırmızı işaretçileri doğru alanları üzerinde olduğundan emin olun.

- El ile yakalama işlemi yanıt vermiyor gibi görünüyor veya oturum açma sayfası yanıt vermezse, el ile yakalama işlemi yeniden deneyin. Ancak bu kez, işlemini tamamladıktan sonra tarayıcınızın Geliştirici konsolunu açmak için F12 tuşuna basın. Seçin **konsol** sekmesi. Tür **window.location= " *&lt;Uygulamayı yapılandırırken belirttiğiniz oturum açma URL&gt;* "** , ve ardından Enter tuşuna basın. Bu, yakalama işlemi sonlandırır ve yakalanan alanları depolayan bir sayfayı yeniden yönlendirme zorlar.

### <a name="contact-support"></a>Desteğe başvurun

Hala sorunlarla karşılaşırsanız, Microsoft Support bir servis talebi açın. Çalıştığınız açıklanmaktadır. Açıklanan ayrıntıları dahil [portal bildirimi ayrıntıları görüntüle](#view-portal-notification-details) ve [bildirim ayrıntılarını Yardım almak için bir destek mühendisine göndermek](#send-notification-details-to-a-support-engineer-to-get-help) (varsa), bu makalenin bölümler.

## <a name="install-the-access-panel-browser-extension"></a>Erişim paneli tarayıcı uzantısını yükleme

Şu adımları uygulayın:

1. Açık [erişim paneli](https://myapps.microsoft.com) desteklenen bir tarayıcı içinde. Azure AD'ye oturum bir *kullanıcı*.

2. Seçin **parola SSO uygulama** erişim Paneli'nde.

3. Yazılımı yüklemek için istendiğinde seçin **Şimdi Yükle**.

4. Sizin için tarayıcınızın indirme sayfasına yönlendirilirsiniz. Tercih **Ekle** uzantısı.

5. İstenirse, seçin **etkinleştirme** veya **izin**.

6. Yükleme işleminden sonra tarayıcınızı yeniden başlatın.

7. Erişim paneli oturum açın. Parola SSO etkin uygulamalarınızı açarsanız bakın.

Ayrıca doğrudan tarayıcı uzantısı Chrome ve Firefox için bu bağlantıları indirebilirsiniz:

-   [Chrome erişim paneli uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Firefox erişim paneli uzantısı](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="view-portal-notification-details"></a>Portal bildirimi ayrıntılarını görüntüle

Herhangi bir portal bildirim ayrıntılarını görmek için bu adımları izleyin:

1. Seçin **bildirimleri** Azure portalının sağ üst köşedeki simgesine (zil).

2. Gösteren herhangi bir bildirim seçin bir *hata* durumu. (Kırmızı sahip oldukları "!".)

   > [!NOTE]
   > İçinde bulunduğunuz bildirimleri seçemezsiniz *başarılı* veya *sürüyor* durumu.

3. **Bildirim ayrıntılarını** bölmesi açılır. Sorun hakkında bilgi edinmek için bilgileri okuyun.

5. Hala yardıma ihtiyacınız varsa, bilgileri destek mühendisine veya ürün grubu ile paylaşın. Seçin **kopyalama** simgesinin sağındaki **kopyalama hatası** kutusunu paylaşmak için bildirim ayrıntılarını kopyalayın.

## <a name="send-notification-details-to-a-support-engineer-to-get-help"></a>Bildirim ayrıntıları konusunda yardım almak için bir destek mühendisiyle Gönder

Paylaştığınız önemlidir *tüm* desteğiyle bu bölümünde listelenir ve böylece bunlar hızlıca Yardım ayrıntıları. Bunu kaydetmek için bir ekran görüntüsü alabilir ya da seçin **kopyalama hatası**.

Aşağıdaki bilgiler ne her bildirim öğesi anlamına gelir ve örnekler açıklanmaktadır.

### <a name="essential-notification-items"></a>Önemli bildirim öğeleri

- **Başlık**: bildirim açıklayıcı bir başlık.

   Örnek: *Uygulama proxy'si ayarları*

- **Açıklama**: ne işlemi nedeniyle oluştu.

   Örnek: *Girilen İç URL, başka bir uygulama tarafından zaten kullanılıyor.*

- **Bildirim kimliği**: bildirim benzersiz kimliği.

    Örnek: *clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068*

- **İstemci istek kimliği**: tarayıcınız yapılan belirli bir istek kimliği.

    Örnek: *302fd775-3329-4670-a9f3-bea37004f0bc*

- **Zaman damgası UTC**: bildirim, UTC gerçekleştiği, zaman damgası.

    Örnek: *2017-03-23T19:50:43.7583681Z*

- **İç işlem kimliği**: hata sistemlerimizde aramak için kullanılan iç kimliği.

    Örnek: **71a2f329-ca29-402f-aa72-bc00a7aca603**

- **UPN**: Kullanıcı işlemi çalıştırıldı.

    Örnek: *tperkins\@f128.info*

- **Kiracı kimliği**: işlemi çalıştıran kullanıcının üyesi olduğu kiracının benzersiz kimliği.

    Örnek: *7918d4b5-0442-4a97-be2d-36f9f9962ece*

- **Kullanıcı nesne kimliği**: İşlemi çalıştırıldı kullanıcının benzersiz kimliği.

    Örnek: *17f84be4-51f8-483a-b533-383791227a99*

### <a name="detailed-notification-items"></a>Ayrıntılı bildirim öğeleri

- **Görünen ad**: (boş olabilir) hata daha ayrıntılı görünen adı.

    Örnek: *Uygulama proxy'si ayarları*

- **Durum**: bildirim özel durumu.

    Örnek: *Başarısız*

- **Nesne Kimliği**: (boş olabilir) karşı işlemi çalıştırıldı nesne kimliği.

   Örnek: *8e08161d-f2fd-40ad-a34a-a9632d6bb599*

- **Ayrıntılar**: hangi işlemin sonucunda oluştu ayrıntılı açıklaması.

    Örnek: *İç url '<https://bing.com/>' zaten kullanımda olduğundan geçerli değil.*

- **Kopyalama hatası**: Seçmenize olanak tanır **Kopyala simgesine** sağındaki **kopyalama hatası** desteğiyle yardımcı olmak için bildirim ayrıntılarını kopyalamak için metin kutusu.

    Örnek:   ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
