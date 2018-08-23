---
title: Güvenlik bilgileri bir doğrulayıcı uygulama - Azure Active Directory kullanacak şekilde | Microsoft Docs
description: Microsoft Authenticator uygulamasını kullanarak kimliğinizi doğrulamak için güvenlik bilgilerinizi ayarlayın.
services: active-directory
author: eross-msft
manager: mtillman
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: e7b07ba892f8f904b1b2127fa8e76649eb004388
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42059758"
---
# <a name="set-up-security-info-to-use-an-authenticator-app-preview"></a>Güvenlik bilgileri (Önizleme) bir authenticator uygulamasını kullanmak için ayarlayın

[!INCLUDE[preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Güvenlik bilgilerinizi ayarlamak için iş veya Okul hesabınızda oturum açın ve ardından kayıt işlemini tamamlamak gerekir. Güvenlik bilgilerinizi daha önce oluşturmadıysanız, artık yapmanız istenir.

## <a name="set-up-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasını ayarlama

Kuruluşunuzun ayarlara bağlı olarak, oturum açtığınızda Microsoft Authenticator uygulamasını ayarlama istenebilir. Aksi takdirde, güvenlik bilgileri Microsoft Authenticator uygulamasını ayarlama başlamak için adımları izleyin. [güvenlik bilgilerinizi yönetmek](security-info-manage-settings.md).

İndirin ve Microsoft Authenticator uygulaması hakkında daha fazla bilgi için bkz: [Microsoft Authenticator uygulaması ile çalışmaya başlama](microsoft-authenticator-app-how-to.md).

>[!Note]
>Microsoft Authenticator uygulamasını kullanmak istemiyorsanız, farklı bir uygulama sırasında ayarlanan seçebilirsiniz. Bu makalede, Microsoft Authenticator uygulamasını kullanır. Authenticator uygulaması seçeneğinin görmüyorsanız, kuruluşunuzun doğrulama için bir authentication uygulamasını kullanmak için izin vermeyen mümkündür. Bu durumda, daha fazla yardım için yöneticinize başvurun veya başka bir yöntem seçmeniz gerekir.

### <a name="to-use-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasını kullanmak için

1. Seçin **Authenticator uygulamasını** seçeneği.

    **Uygulamayı Al** Sihirbazı görünür.

    ![Uygulama Sihirbazı, ilk ekran Al](media/security-info/security-info-auth-app-wizard.png)

    Microsoft Authenticator uygulamasını kullanmak istemiyorsanız, tıklayabilirsiniz **farklı authenticator uygulamasını kullanmak istiyorum** bağlantı **uygulamayı Al** ekran.

2. Microsoft Authenticator uygulamasını yükledikten sonra seçin **sonraki**.

    İstenirse, bildirimlerine izin ver, yeni bir hesap ekleyin ve ardından **iş veya Okul hesabı**.

3. **İleri**’yi seçin.

    **QR kodunu tarayın** ekranı görüntülenir.

    ![Kimlik Doğrulayıcı uygulamasını kullanarak bir QR kodu tarama](media/security-info/security-info-scan-qr.png)

4. Microsoft Authenticator uygulamasını açın, **Hesap Ekle** gelen **özelleştirme ve Denetim** simgesini sağ üst köşede ve ardından **iş veya Okul hesabı**. 

5. Bir QR kodu reader uygulaması varsa, sağlanan kodunu tarayın. Bir kod okuyucu uygulaması yoksa, seçebileceğiniz **QR kodu bağlantısı tarayamıyor** ve el ile Microsoft Authenticator uygulamasına kodu ve URL'yi girin.

6. Uygulamasını etkinleştirmek için bildirimi onaylamanız için Microsoft Authenticator uygulamasını kullanın.

    İki aşamalı doğrulama veya Self Servis parola sıfırlaması kullanırken, kimliğinizi doğrulamak için Microsoft Authenticator uygulamasını kullanmak için güvenlik bilgileriniz güncelleştirildi.

    >[!Note]
    >Kuruluşunuz izin veriyorsa, ayrıca Microsoft Authenticator uygulama bildirimi ile birlikte bir doğrulama kodu alırsınız. Varsayılan yönteminizi kod yapmak istiyorsanız,'ndaki yönergeleri izleyin [güvenlik bilgilerinizi yönetmek](security-info-setup-auth-app.md).

## <a name="additional-security-info-options"></a>Ek güvenlik bilgisi seçenekleri

Nasıl kuruluş kişilerinizi, olduğuna göre kimliğinizi doğrulamak için yapmak çalıştığınız için ek bir seçeneğiniz vardır. Seçeneklere şunlar dahildir:

- **Mobil cihaz metin.** Mobil cihaz numaranızı girin ve kısa mesaj, iki aşamalı doğrulama veya parola için kullanacağınız bir kod alın sıfırlayın. Kısa mesaj (SMS) ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz: [güvenlik bilgilerini, metin iletileri (SMS) kullanacak şekilde](security-info-setup-text-msg.md).

- **Mobil cihaz veya iş telefon çağrısı.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için bir telefon araması alın. Bir telefon numarası ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz: [güvenlik bilgilerini, telefon aramaları kullanacak şekilde](security-info-setup-phone-number.md).

- **E-posta adresi.** Girin, iş veya Okul e-posta adresinizi, parola sıfırlama için bir e-posta almak için. Bu seçenek, iki aşamalı doğrulama için kullanılamaz. E-posta kurma hakkında adım adım yönergeler için bkz. [güvenlik bilgileri e-posta kullanacak şekilde](security-info-setup-email.md).

- **Güvenlik sorusu.** Yöneticiniz kuruluşunuz için oluşturduğu bazı güvenlik soruları yanıtlayın. Bu seçenek, yalnızca parola sıfırlama için ve iki aşamalı doğrulama için kullanılabilir. Güvenlik sorularınızı ayarlama konusunda adım adım yönergeler için bkz: [güvenlik bilgisi güvenlik sorularını kullanacak şekilde](security-info-setup-questions.md) makalesi.
    
    >[!Note]
    >Bu seçeneklerden bazısı eksikse, kuruluşunuz bu yöntemleri izin vermeyen büyük olasılıkla olmasıdır. Bu durumda, kullanılabilir bir yöntem seçin veya daha fazla yardım için yöneticinize başvurun gerekecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik bilgilerinizi güncelleştirmeniz gerekiyorsa, yönergeleri izleyin [güvenlik bilgilerinizi yönetmek](security-info-manage-settings.md) makalesi.

- Kaybolur veya, gelen unutulabilir, parolanızı sıfırlayamıyoruz [parola sıfırlama portalı](https://passwordreset.microsoftonline.com/) veya adımları [iş veya Okul parolanızı sıfırlama](user-help-reset-password.md) makalesi.

- İpuçları ve Yardım almak için oturum açma sorunlarını giderme alma [Microsoft hesabınızda oturum açamazsınız](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.