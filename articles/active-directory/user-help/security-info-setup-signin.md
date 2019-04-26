---
title: Güvenlik bilgileri (Önizleme), oturum açma istemi'nden - Azure Active Directory ayarlama | Microsoft Docs
description: Kuruluşunuzun oturum açma sayfasından istenirse iş veya Okul hesabınızın güvenlik bilgilerini nasıl oluşturulacağı.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: overview
ms.date: 02/13/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: f9c6faff10f68d720bc3c86a191e4cd1b1f9abdc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60475308"
---
# <a name="set-up-your-security-info-preview-from-the-sign-in-page-prompt"></a>Güvenlik bilgilerinizi (Önizleme) oturum açma sayfası isteminden ayarlayın
Hemen iş veya Okul hesabınızda oturum sonra güvenlik bilgilerinizi ayarlamak için istenirse, bu adımları takip edebilirsiniz.

Kuruluşunuz için gerekli güvenlik bilgilerini ayarlamadıysanız, yalnızca bu istemi görürsünüz. Güvenlik bilgilerinizi daha önce ayarladığınız ancak değişiklik yapmak istiyorsanız, çeşitli yöntemi tabanlı nasıl yapılır makaleleri adımları takip edebilirsiniz. Daha fazla bilgi için [Ekle veya güvenlik bilgisi genel bakış güncelleştirme](security-info-add-update-methods-overview.md).

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

## <a name="sign-in-to-your-work-or-school-account"></a>İş veya Okul hesabınızda oturum açın
İş veya Okul hesabınızda oturum açtıktan sonra hesabınıza erişmenize olanak tanır önce daha fazla bilgi sağlamak isteyen bir istem göreceksiniz.

![Daha fazla bilgi için soran istem](media/security-info/securityinfo-prompt.png)

## <a name="set-up-your-security-info-using-the-wizard"></a>Güvenlik bilgilerini Sihirbazı'nı kullanarak ayarlayın
İş veya Okul hesabınız için güvenlik bilgilerinizi isteminden ayarlamak için aşağıdaki adımları izleyin.

>[!Important]
>Bu işlem yalnızca bir örneği var. Kuruluşunuzun gereksinimlerine bağlı olarak, yöneticiniz bu işlem sırasında ayarlamak için ihtiyacınız olacak farklı bir kimlik doğrulama yöntemleri ayarlamış olabilir. Bu örnekte, biz Microsoft Authenticator uygulamasını ve cep telefonu numarası doğrulama aramalarını veya mesajları için iki yöntem gerektiren.

1. Seçtikten sonra **sonraki** isteminde bir **hesabı güvenli Sihirbazı'nı tutmak** , yönetici ve kuruluş ayarlamak gerekli ilk yöntem gösteren görünür. Bu örnek için Microsoft Authenticator uygulamasıdır.

   > [!Note]
   > Microsoft Authenticator uygulaması dışında bir authenticator uygulamasını kullanmak isteyip istemediğinizi seçin **farklı authenticator uygulamasını kullanmak istiyorum** bağlantı.
   > 
   > Kuruluşunuz kimlik doğrulayıcı uygulamasını yanı sıra farklı bir yöntemi seçmenize izin veriyorsa, seçebileceğiniz **farklı yöntem bağlantıyı oluşturan ayarlamak istediğiniz**.

    ![Kimlik doğrulama uygulaması indirme sayfasını gösteren, hesap güvenli Sihirbazı tutun](media/security-info/securityinfo-prompt-get-auth-app.png)

2. Seçin **hemen indirin** indirin ve mobil Cihazınızda Microsoft Authenticator uygulamasını yükleyin ve ardından **sonraki**. Uygulamayı karşıdan yüklenip kurulacak hakkında daha fazla bilgi için bkz. [indirme ve yükleme için Microsoft Authenticator uygulamasını](user-help-auth-app-download-install.md).

    ![' % S'Doğrulayıcısı ayarlama, hesap sayfasını gösteren, hesap güvenli Sihirbazı tutun](media/security-info/securityinfo-prompt-auth-app-setup-acct.png)

3. Kalır **hesabınızı ayarlarken** mobil Cihazınızda Microsoft Authenticator uygulamasını ayarlama sırasında sayfa.

4. Microsoft Authenticator uygulamasını açın, bildirimleri (istenirse) izin vermek için seçin, **Hesap Ekle** gelen **özelleştirme ve Denetim** sağ üst köşede, simgesine ve ardından **iş veya Okul hesabı**.

5. Geri dönüp **hesabınızı ayarlarken** sayfasında bilgisayarınızda ve ardından **sonraki**.

    **QR kodunu tarayın** sayfası görüntülenir.

    ![Kimlik Doğrulayıcı uygulamasını kullanarak bir QR kodu tarama](media/security-info/securityinfo-prompt-auth-app-qrcode.png)

6. Adım 5'te iş veya Okul hesabınızı oluşturduktan sonra mobil Cihazınızda görünen Microsoft Authenticator uygulaması QR kodu okuyucu ile sağlanan kodunu tarayın.

    Authenticator uygulaması başarıyla iş veya Okul hesabı Ekle'ye sizden herhangi bir ek bilgi gerek kalmadan. Ancak, QR kodu okuyucu kod okunamıyor, seçebileceğiniz **QR kodu bağlantısı tarayamıyor** ve el ile Microsoft Authenticator uygulamasına kodu ve URL'yi girin. El ile bir kod ekleme hakkında daha fazla bilgi için bkz. [hesap uygulamaya el ile Ekle](user-help-auth-app-add-account-manual.md).

7. Seçin **sonraki** üzerinde **QR kodunu tarayın** bilgisayarınızda sayfası.

    Bir bildirim hesabınızı test etmek için mobil Cihazınızda Microsoft Authenticator uygulamasını gönderilir.

    ![Hesabınızla kimlik doğrulayıcı uygulamasını deneyin](media/security-info/securityinfo-prompt-test-app.png)

8. Microsoft Authenticator uygulamasını bildirimi onaylayın ve ardından **sonraki**.

    ![Başarı bildirimini, uygulama ve hesabınıza bağlanma](media/security-info/securityinfo-prompt-auth-app-success.png).

    Güvenlik bilgilerinizi Microsoft Authenticator uygulamasını kullanmak için iki aşamalı doğrulama veya parola sıfırlaması kullanırken, kimliğinizi doğrulamak için varsayılan olarak güncelleştirilir.

9. Üzerinde **telefon** ayarlama sayfası, SMS mesajı ya da telefon aramasına alabilir ve ardından seçiminizi **sonraki**. Metin iletileri kabul edebilen bir cihaz için bir telefon numarası kullanın, böylece bu örneğin amacı doğrultusunda, kısa mesaj kullanıyoruz.

    ![Telefonunuza kısa mesaj için ayarlama başlayın](media/security-info/securityinfo-prompt-text-msg.png)

    Telefonunuza SMS mesajı gönderilir. Bir telefon araması almak tercih ettiğiniz, işlem aynıdır. Ancak, bir telefon araması, SMS mesajı yerine yönergelerle alırsınız.

10. Mobil cihazınıza gönderilen SMS mesajı tarafından sağlanan kodu girin ve ardından **sonraki**.

    ![SMS mesajı hesabınızla deneyin](media/security-info/securityinfo-prompt-text-msg-enter-code.png)

11. Başarı bildirimini gözden geçirin ve ardından **Bitti**.

    ![Başarı bildirimi](media/security-info/securityinfo-prompt-call-answered-success.png)

    İki aşamalı doğrulama veya parola sıfırlaması kullanırken, kimliğinizi doğrulamak için yedek yöntemi Mesajlaşma metin kullanmak için güvenlik bilgileriniz güncelleştirildi.

12. Gözden geçirme **başarı** başarıyla Microsoft Authenticator uygulamasını ve telefon (SMS mesajı ya da telefon aramasına) yöntemi için güvenlik bilgilerinizi ayarladığınız olduğunu doğrulamak için sayfa ve ardından **Bitti** .

    ![Sihirbazı başarıyla tamamlandı](media/security-info/securityinfo-prompt-setup-success.png)

## <a name="next-steps"></a>Sonraki adımlar

- Değiştirmek için delete veya update varsayılan güvenlik bilgisi yöntemleri, bkz:

    - [Güvenlik bilgilerini bir kimlik doğrulayıcı uygulama ayarlama](security-info-setup-auth-app.md).

    - [Metin iletileri için güvenlik bilgileri ayarlama](security-info-setup-text-msg.md).

    - [Güvenlik bilgilerini, telefon aramaları kullanacak şekilde](security-info-setup-phone-number.md).

    - [Güvenlik bilgileri e-posta kullanacak şekilde](security-info-setup-email.md).

    - [Güvenlik bilgilerini, önceden tanımlı güvenlik soruları kullanacak şekilde](security-info-setup-questions.md).

- Belirtilen yönteminizi kullanarak imzalama hakkında daha fazla bilgi için bkz: [oturum açma](user-help-sign-in.md).

- Kaybolur veya, gelen unutulabilir, parolanızı sıfırlayamıyoruz [parola sıfırlama portalı](https://passwordreset.microsoftonline.com/) veya adımları [iş veya Okul parolanızı sıfırlama](user-help-reset-password.md) makalesi.

- İpuçları ve Yardım almak için oturum açma sorunlarını giderme alma [Microsoft hesabınızda oturum açamazsınız](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.