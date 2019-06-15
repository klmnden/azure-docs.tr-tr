---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 08/28/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: c664e73b39ad48a860661cfd9141ee74df203f3e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67116681"
---
## <a name="generate-the-certificate-signing-request-file"></a>Sertifika imzalama isteği dosyası oluşturma

Apple anında iletilen bildirim servisi (APNs) anında iletme bildirimlerinizi doğrulamak için sertifikaları kullanır. Bildirim gönderip almak için gereken bildirim sertifikasını oluşturacak bu talimatları uygulayın. Bu kavramlarla ilgili daha fazla bilgi için resmi [Apple Anında İletilen Bildirim Servisi](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) belgelerine bakın.

İmzalı bir bildirim sertifikası oluşturmak için Apple kullandığı sertifika imzalama isteği (CSR) dosyası oluşturun.

1. Mac’inizde Anahtar Zinciri Erişimi aracını çalıştırın. Gelen açılabilir **yardımcı programları** klasör veya **diğer** Launchpad klasöründedir.

1. Seçin **Anahtarlık erişimi**, genişletme **sertifika Yardımcısı**ve ardından **bir sertifika yetkilisinden bir sertifika isteği**.

    ![Anahtarlık Erişimi kullanarak yeni sertifika isteğinde bulunma](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

1. Seçin, **kullanıcı e-posta adresi**, girin, **ortak ad** değerini, belirttiğiniz emin **diske kaydedilmiş**ve ardından **devam**. Bırakın **CA e-posta adresi** boş olarak gerekli değildir.

    ![Gerekli sertifika bilgileri](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

1. CSR dosyasında için bir ad girin **Kaydet**, konum seçin **burada**ve ardından **Kaydet**.

    ![Sertifika için bir dosya adı seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Bu eylem CSR dosyasını seçili konuma kaydeder. Varsayılan konum **Masaüstü**. Bu dosya için seçilen konumu unutmayın.

Ardından, uygulamanızı Apple'a kaydedeceksiniz, anında iletme bildirimlerini etkinleştirin ve anında iletme sertifikası oluşturmak için dışa aktarılan CSR dosyasını karşıya yükleyin.

## <a name="register-your-app-for-push-notifications"></a>Anında iletme bildirimleri için uygulamanızı kaydetme

Bir iOS uygulamasına anında iletme bildirimleri için uygulamanızı Apple'a kaydedeceksiniz ve ayrıca anında iletme bildirimleri için kaydedin.  

1. Uygulamanızı henüz kaydolmadıysanız, Gözat [iOS sağlama portalı](https://go.microsoft.com/fwlink/p/?LinkId=272456) Apple Developer Center'da. Bundan sonra seçin, Apple Kimliğiyle oturum **tanımlayıcıları**seçin **uygulama kimlikleri**ve son olarak **+** yeni uygulamayı kaydedin.

    ![iOS Hazırlama Portalı Uygulama Kimlikleri sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)

1. Yeni uygulamanız için aşağıdaki üç değerlerini güncelleştirin ve ardından **devam**:

   * **Ad**: Uygulamanız için açıklayıcı bir ad yazın **adı** kutusunda **uygulama kimliği açıklaması** bölümü.

   * **Paket grubu tanımlayıcısı**: İçinde **açık uygulama kimliği** bölümünde, girin bir **paket grubu tanımlayıcısı** formun `<Organization Identifier>.<Product Name>` belirtildiği gibi [uygulama Dağıtım Kılavuzu'na](https://help.apple.com/xcode/mac/current/#/dev91fe7130a). *Kuruluş tanımlayıcısı* ve *ürün adı* değerleri, Xcode projenizi oluşturduğunuzda kullandığınız kuruluş tanımlayıcısı ve ürün adıyla eşleşmelidir. Aşağıdaki ekran görüntüsünde *NotificationHubs* değeri kuruluş tanımlayıcı olarak kullanılır ve *GetStarted* değeri, ürün adı olarak kullanılır. Emin **paket grubu tanımlayıcısı** değerle eşleşen değeri Xcode projenizde, böylece doğru yayımlama profili Xcode kullanacaktır.

   * **Anında iletme bildirimleri**: Denetleme **anında iletme bildirimleri** seçeneğini **uygulama hizmetleri** bölümü.

     ![Yeni Uygulama Kimliği kaydetme formu](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

     Bu eylem, uygulama Kimliğiniz ve bilgiyi onaylamanız istenir istekleri oluşturur. Seçin **kaydetme** yeni uygulama kimliğini onaylamak için

     Seçtikten sonra **kaydetme**, gördüğünüz **kaydı tamamlamak** ekran aşağıdaki görüntüde gösterildiği gibi. **Done** (Bitti) öğesini seçin.

     ![Yetkilendirmeleri gösteren tamamlanmış Uygulama Kimliği kaydı](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)

1. Geliştirici Merkezi'nde altında **uygulama kimlikleri**oluşturduğunuz uygulama Kimliğini bulup satırına seçin.

    ![Uygulama Kimliği listesi](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)

    Uygulama ayrıntılarını görüntülemek ve ardından uygulama Kimliğini seçin **Düzenle** altındaki düğmesini.

    ![Uygulama Kimliğini Düzenle sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

1. Seçin ve ekranı en alta kadar kaydır **oluşturduğunuz sertifika** düğmesini **geliştirme bildirimi SSL sertifikası** bölümü.

    ![Uygulama Kimliği için sertifika oluştur düğmesi](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Şimdi **iOS sertifikası Ekle** Yardımcısı.

    > [!NOTE]
    > Bu öğretici geliştirme sertifikası kullanır. Aynı işlem üretim sertifika kaydedildiğinde de kullanılır. Bildirimleri gönderirken aynı sertifika türünü kullandığınızdan kesinlikle emin olun.

1. Seçin **Dosya Seç**CSR dosyasını kaydettiğiniz ilk görevden konuma göz atın ve ardından **Oluştur**.

    ![Oluşturulan sertifika CSR karşıya yükleme sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

1. Portal sertifikayı oluşturduktan sonra seçin **indirme** düğmesine ve ardından **Bitti**.

    ![Oluşturulan sertifika indirme sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Sertifika indirilir ve bilgisayarınızın kaydedildi, **indirir** klasör.

    ![İndirilenler klasöründe sertifika dosyasını bulma](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > Varsayılan olarak, indirilen geliştirme sertifikası adlı **aps_development.cer**.

1. İndirilen anında iletme sertifikası seçin **aps_development.cer**.

    Bu eylem yeni sertifikayı Anahtar Zinciri’ne aşağıdaki resimde gösterildiği gibi yüklenir:

    ![Yeni sertifikanın gösterildiği anahtarlık erişim sertifikaları listesi](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > Sertifikanızdaki ad farklı olabilir, ancak adı ile önek alacaktır **Apple geliştirme iOS Bildirim Hizmetleri**.

1. Anahtar Zinciri Erişimi’nde **Sertifikalar** kategorisinde oluşturduğunuz yeni bildirim sertifikasına sağ tıklayın. Seçin **dışarı**, dosya adı, seçin **.p12** biçimlendirmek ve ardından **Kaydet**.

    ![Sertifikayı p12 biçiminde dışarı aktarma](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Dışarı aktarılan .p12 sertifikanın dosya adını ve konumunu not edin. APNs kimlik doğrulamasını etkinleştirmek için kullanılırlar.

    > [!NOTE]
    > Bu öğreticide adlı bir dosya oluşturur **QuickStart.p12**. Dosyanızın adı ve konumu farklı olabilir.

## <a name="create-a-provisioning-profile-for-the-app"></a>Uygulama için bir sağlama profili oluşturun

1. İçinde [iOS sağlama portalı](https://go.microsoft.com/fwlink/p/?LinkId=272456)seçin **sağlama profilleri**seçin **tüm**ve ardından **+** oluşturmak için bir Yeni profili. Gördüğünüz **iOS sağlama profili ekleme** Sihirbazı.

    ![Sağlama profili listesi](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

1. Seçin **iOS uygulama geliştirmeyi** altında **geliştirme** sağlama olarak profil türü seçin ve **devam**.

1. Ardından, oluşturduğunuz uygulama Kimliğini seçin **uygulama kimliği** seçin ve açılır listede **devam**.

    ![Uygulama Kimliği'ni seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

1. İçinde **sertifikaları Seç** penceresinde, kod imzalama için kullanılan normal geliştirme sertifikanızı seçin ve seçin **devam**. Bu sertifika, oluşturduğunuz bildirim sertifikası değildir.

    ![Sertifikayı seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)

1. Ardından, test etmek için kullanılacak aygıtlar seçip **devam**.

    ![Cihazları seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)

1. Son olarak, profili için bir ad seçin **profil adı**seçip **Oluştur**.

    ![Sağlama profili adını seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

1. Yeni sağlama profili oluşturulduğunda indirmek ve Xcode geliştirme makinenizde yüklemek seçin. Ardından **Bitti**'yi seçin.

    ![Sağlama profilini indirin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)

## <a name="create-a-notification-hub"></a>Bildirim hub’ı oluşturma

Bu bölümde, bildirim hub'ı oluşturma ve önceden oluşturduğunuz .p12 anında iletme sertifikasını kullanarak APNs ile kimlik doğrulamasını yapılandırma. Önceden oluşturduğunuz bir bildirim hub'ı kullanmak istiyorsanız, 5. adıma atlayabilirsiniz.

[!INCLUDE [notification-hubs-portal-create-new-hub](notification-hubs-portal-create-new-hub.md)]

## <a name="configure-your-notification-hub-with-apns-information"></a>Bildirim hub'ınızı APNs bilgilerle yapılandırın.

1. **Bildirim Hizmetleri** bölümünde **Apple (APNS)** seçeneğini belirleyin.

1. **Sertifika**’yı seçin.

1. Dosya simgesini seçin.

1. Daha önce dışarı aktarılan .p12 dosyası seçin.

1. Doğru parolayı belirtin.

1. **Korumalı alan** modunu seçin. Yalnızca uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek istiyorsanız **Üretim** modunu kullanın.

    ![Azure portal'da APNs sertifikasını yapılandırma](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-apple-config-cert.png)

Şimdi, bildirim hub'ınızı APNs ile birlikte yapılandırdınız. Ayrıca uygulamanızı kaydetmenizi ve anında iletme bildirimleri göndermek için bağlantı dizelerini de vardır.
