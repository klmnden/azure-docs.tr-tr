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
ms.openlocfilehash: 39c016e7b4b70368eb1ca2bd517ed7f48d223e24
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140592"
---
## <a name="generate-the-certificate-signing-request-file"></a>Sertifika imzalama isteği dosyası oluşturma

Apple Anında İletilen Bildirim Servisi (APNS), anında iletme bildirimlerinizi doğrulamak için sertifikaları kullanır. Bildirim gönderip almak için gereken bildirim sertifikasını oluşturacak bu talimatları uygulayın. Bu kavramlarla ilgili daha fazla bilgi için resmi [Apple Anında İletilen Bildirim Servisi](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html) belgelerine bakın.

İmzalı bir bildirim sertifikası oluşturmak için Apple tarafından kullanılan Sertifika İmzalama İsteği (CSR) dosyasını oluşturun.

1. Mac’inizde **Anahtarlık Erişim** aracını çalıştırın. **Yardımcı Programlar** klasöründen veya başlatma panelindeki **Diğer** klasöründen açılabilir.
2. **Anahtar Zinciri Erişimi**’ne tıklayın, **Sertifika Yardımcısı**’nı genişletin ve **Sertifika Yetkilisinden Sertifika İste...** öğesine tıklayın.

    ![Anahtarlık Erişimi kullanarak yeni sertifika isteğinde bulunma](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. **Kullanıcı E-posta Adresi**’nizi ve **Ortak Ad**’ı seçin, **Diske kaydedilmiş**’in seçili olduğundan emin olun ve ardından **Devam**’a tıklayın. Bırakın **CA e-posta adresi** gerekli değil olarak boş alan.

    ![Gerekli sertifika bilgileri](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Sertifika İmzalama İsteği (CSR) dosyası için **Farklı Kaydet**’e bir ad yazın, **Nerede**’de konum seçin ve ardından **Kaydet**’e tıklayın.

    ![Sertifika için bir dosya adı seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Bu eylem, CSR dosyasını seçili konuma kaydeder. Varsayılan konum Masaüstü’dür. Bu dosya için seçilen konumu unutmayın.

Ardından, uygulamanızı Apple’a kaydedeceksiniz; anında iletme bildirimlerini etkinleştirip anında iletme sertifikası oluşturmak için dışarı aktarılan CSR dosyasını karşıya yükleyeceksiniz.

## <a name="register-your-app-for-push-notifications"></a>Anında iletme bildirimleri için uygulamanızı kaydetme

Bir iOS uygulamasına anında iletme bildirimleri için uygulamanızı Apple'a kaydedeceksiniz ve ayrıca anında iletme bildirimleri için kaydedin.  

1. Uygulamanızı henüz kaydolmadıysanız, gidin [iOS sağlama portalı](https://go.microsoft.com/fwlink/p/?LinkId=272456) 'a tıklayın, Apple Kimliğiyle Apple Geliştirici Merkezi'nde oturum **tanımlayıcıları**, ardından **uygulama kimlikleri** ve son olarak tıklayarak **+** oturum yeni uygulamayı kaydedin.

    ![iOS Hazırlama Portalı Uygulama Kimlikleri sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Yeni uygulamanız için aşağıdaki üç alanı güncelleştirin ve ardından **Devam**’a tıklayın:

   * **Ad**: Uygulamanız için açıklayıcı bir ad yazın **adı** alanındaki **uygulama kimliği açıklaması** bölümü.
   * **Paket grubu tanımlayıcısı**: Altında **açık uygulama kimliği** bölümünde, girin bir **paket grubu tanımlayıcısı** biçiminde `<Organization Identifier>.<Product Name>` belirtildiği gibi [uygulama Dağıtım Kılavuzu'na](https://help.apple.com/xcode/mac/current/#/dev91fe7130a). *Kuruluş Tanımlayıcı* ve kullandığınız *Ürün Adı*, XCode projenizi oluşturduğunuzda kullandığınız kuruluş tanımlayıcısı ve ürün adıyla eşleşmelidir. Aşağıdaki ekran görüntüsünde *NotificationHubs* değeri kuruluş tanımlayıcı olarak kullanılır ve *GetStarted* ürün adı olarak kullanılır. Bu değerin, XCode projenizde kullandığınız değerle eşleştiğinden emin olunması XCode ile doğru yayımlama profili kullanmanızı sağlar.
   * **Anında iletme bildirimleri**: Denetleme **anında iletme bildirimleri** seçeneğini **uygulama hizmetleri** bölümü.

     ![Yeni Uygulama Kimliği kaydetme formu](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

     Bu eylem, Uygulama Kimliğinizi oluşturur ve bilgiyi onaylamanız ister. Yeni Uygulama Kimliğini onaylamak için **Kaydet**’e tıklayın.

     **Kaydet**’e tıkladıktan sonra **Kayıt tamamlandı** ekranını aşağıdaki resimde gösterildiği gibi göreceksiniz. **Bitti**’ye tıklayın.

     ![Yetkilendirmeleri gösteren tamamlanmış Uygulama Kimliği kaydı](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-registration-complete.png)

3. Geliştirici Merkezi’nde, Uygulama Kimlikleri altında oluşturduğunuz uygulama kimliğini bulup satırına tıklayın.

    ![Uygulama Kimliği listesi](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids2.png)

    Uygulama kimliğine tıklandığında uygulama ayrıntıları görüntülenir. Alttaki **Düzenle** düğmesine tıklayın.

    ![Uygulama Kimliğini Düzenle sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

4. Ekranın altına gidin ve **Geliştirme bildirimi SSL Sertifikası** altındaki **sertifika Oluştur...** düğmesine tıklayın.

    ![Uygulama Kimliği için sertifika oluştur düğmesi](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    **iOS Sertifikası Ekle** yardımcısını görürsünüz.

    > [!NOTE]
    > Bu öğretici geliştirme sertifikası kullanır. Aynı işlem üretim sertifika kaydedildiğinde de kullanılır. Bildirimleri gönderirken aynı sertifika türünü kullandığınızdan kesinlikle emin olun.

5. **Dosya Seç**’e tıklayın, ilk görevde oluşturduğunuz CSR dosyasını kaydettiğiniz konuma göz atın ve **Oluştur**’a tıklayın.

    ![Oluşturulan sertifika CSR karşıya yükleme sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

6. Portal tarafından sertifika oluşturulduktan sonra **İndir** düğmesine ve **Bitti**’ye tıklayın.

    ![Oluşturulan sertifika indirme sayfası](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Böylece sertifika indirilir ve bilgisayarınızın **İndirilenler** klasörüne kaydedilir.

    ![İndirilenler klasöründe sertifika dosyasını bulma](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > Varsayılan olarak, indirilen geliştirme sertifikası dosyası **aps_development.cer** olarak adlandırılır.

7. İndirilen **aps_development.cer** bildirim sertifikasına çift tıklayın.

    Bu eylem yeni sertifikayı Anahtar Zinciri’ne aşağıdaki resimde gösterildiği gibi yüklenir:

    ![Yeni sertifikanın gösterildiği anahtarlık erişim sertifikaları listesi](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > Sertifikanızdaki ad farklı olabilir, ancak **Apple Geliştirme iOS Bildirim Hizmetleri:** ile önek alacaktır.

8. Anahtar Zinciri Erişimi’nde **Sertifikalar** kategorisinde oluşturduğunuz yeni bildirim sertifikasına sağ tıklayın. **Dışarı Aktar**’a tıklayın, dosyaya ad verin, **.p12** biçimini seçin ve **Kaydet**’e tıklayın.

    ![Sertifikayı p12 biçiminde dışarı aktarma](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Dışarı aktarılan .p12 sertifikanın dosya adını ve konumunu not edin. APNS kimlik doğrulamasını etkinleştirmek için kullanılır.

    > [!NOTE]
    > Bu öğretici bir QuickStart.p12 dosyası oluşturur. Dosyanızın adı ve konumu farklı olabilir.

## <a name="create-a-provisioning-profile-for-the-app"></a>Uygulama için bir sağlama profili oluşturun

1. [iOS Sağlama Portalı](https://go.microsoft.com/fwlink/p/?LinkId=272456)’na geri dönün, **Sağlama Profilleri**’ni ve **Tümü**’nü seçin, ardından da yeni profil oluşturmak için **+** (artı) düğmesine tıklayın. **iOS Sağlama Profili Ekleme** sihirbazını göreceksiniz:

    ![Sağlama profili listesi](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. **iOS Uygulaması Geliştirme**’yi, **Geliştirme** altında sağlama profili türü olarak seçin ve **Devam**’a tıklayın.

3. Daha sonra, **Uygulama Kimliği** açılan listesinden oluşturduğunuz uygulama kimliğini seçin ve **Devam**’a tıklayın

    ![Uygulama Kimliği'ni seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

4. **Sertifikaları Seç** ekranında, kod imzalama için kullanılan normal geliştirme sertifikanızı seçin ve **Devam**’a tıklayın. Bu sertifika, oluşturduğunuz bildirim sertifikası değildir.

    ![Sertifikayı seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)

5. Sonra da, testte kullanmak üzere **Cihazlar**’a ve **Devam**’a tıklayın

    ![Cihazları seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)

6. Son olarak, profil için **Profil Adı**’nda bir ad seçin, **Oluştur**’a tıklayın.

    ![Sağlama profili adını seçin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

7. Yeni sağlama profili oluşturulduğunda indirmek için buna tıklayın ve Xcode geliştirme makinesine yükleyin. Sonra da **Bitti**’ye tıklayın.

    ![Sağlama profilini indirin](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-profile-ready.png)

## <a name="create-a-notification-hub"></a>Bildirim hub’ı oluşturma
Bu bölümde, bir bildirim hub’ı oluşturur ve önceden oluşturduğunuz **.p12** anında iletme sertifikasını kullanarak APNS ile kimlik doğrulamasını yapılandırırsınız. Önceden oluşturduğunuz bir bildirim hub'ını kullanmak istiyorsanız 5. adıma geçebilirsiniz.

[!INCLUDE [notification-hubs-portal-create-new-hub](notification-hubs-portal-create-new-hub.md)]

## <a name="configure-your-notification-hub-with-apns-information"></a>APNS bilgileriyle bildirim hub’ınızı yapılandırma

1. **Bildirim Hizmetleri** bölümünde **Apple (APNS)** seçeneğini belirleyin.
2. **Sertifika**’yı seçin.
3. **Dosya simgesini** seçin.
4. Daha önce dışarı aktardığınız **.p12** dosyasını seçin.
5. Doğru **parolayı** belirtin.
6. **Korumalı alan** modunu seçin. Yalnızca uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek istiyorsanız **Üretim** modunu kullanın.

    ! [Azure portalında APNS sertifikası yapılandırma] [7]

Bildirim hub'ınızı APNS ile birlikte çalışacak şekilde yapılandırdınız. Ayrıca uygulamanızı kaydetmenizi ve anlık iletme bildirimleri göndermenizi sağlayan bağlantı dizelerine sahipsiniz.