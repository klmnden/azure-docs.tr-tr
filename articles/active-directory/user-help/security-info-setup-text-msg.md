---
title: Kısa mesaj - kullanılacak güvenlik bilgisi (Önizleme) Azure Active Directory ayarlama | Microsoft Docs
description: Güvenlik bilgilerinizi kısa mesaj ve mobil Cihazınızı kullanarak kimliğinizi doğrulamak için nasıl kurulur.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea9e4ae21ecc6538b33aed1566c10ddcd22b86c7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60482273"
---
# <a name="set-up-security-info-preview-to-use-text-messaging"></a>Güvenlik bilgileri (Önizleme), kısa mesaj kullanacak şekilde
İki aşamalı doğrulamayı eklemek için aşağıdaki adımları uygulayabilirsiniz ve yöntemleri sıfırlama. Bu ilk kez ayarladıktan sonra dönebilirsiniz **güvenlik bilgisi** sayfasına ekleme, güncelleştirme veya güvenlik bilgilerinizi silin.

Hemen iş veya Okul hesabınızda oturum açtıktan sonra bunu ayarlamak için istenirse, ayrıntılı adımlara bakın [güvenlik bilgilerinizi ayarlayın oturum açma sayfası isteminden](security-info-setup-signin.md) makalesi.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>Bir telefon seçeneği görmüyorsanız, kuruluşunuzun, bir telefon numarası doğrulama işlemlerinde sesten izin vermez mümkündür. Bu durumda, daha fazla yardım için yöneticinize başvurun veya başka bir yöntem seçmeniz gerekir.

## <a name="set-up-text-messages-from-the-security-info-page"></a>Güvenlik bilgileri sayfasından metin mesajlarını ayarla
Kuruluşunuzun ayarlara bağlı olarak, metin, güvenlik bilgisi yöntemlerinden biri olarak iletileri kullanmanız mümkün olabilir. Telefon numaranızı her şeyi yaptığınız şekilde ayarlayacağız metin mesajı seçeneğini telefon seçeneğine bir parçası olduğundan, ancak Microsoft çağırmanızı sahip olmak yerine, bir kısa mesaj kullanılacak seçeneğini belirleyin.

>[!Note]
>Bir telefon araması, SMS mesajı yerine almak istiyorsanız, adımları [güvenlik bilgilerini, telefon aramaları kullanacak şekilde](security-info-setup-phone-number.md) makalesi.

### <a name="to-set-up-text-messages"></a>Metin iletileri ayarlamak için

1. İş veya Okul hesabınızda oturum açın ve ardından Git kullanarak https://myprofile.microsoft.com/ sayfası.

    ![Vurgulanan güvenlik bilgisi bağlantıları gösteren profili sayfam](media/security-info/securityinfo-myprofile-page.png)

2. Seçin **güvenlik bilgisi** sol gezinti bölmesinden ya da bağlantıyı **güvenlik bilgisi** engelleyebilir ve ardından **yöntemi ekleyin** gelen **güvenlik bilgisi**  sayfası.

    ![Güvenlik bilgileri sayfasını vurgulanan Ekle yöntemi seçeneği](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Üzerinde **bir yöntem ekleyin** sayfasında **telefon** seçin ve açılır listede **Ekle**.

    ![Seçili olan telefon yöntemi Kutusu Ekle](media/security-info/securityinfo-myprofile-addphonetext.png)

4. Üzerinde **telefon** sayfasında mobil cihazınız için telefon numarasını yazın, **metin bana bir kod**ve ardından **sonraki**.

    ![Telefon numarası ekleyin ve metin iletisi seçin](media/security-info/securityinfo-myprofile-phonetext-addnumber.png)

5. Mobil cihazınıza bir kısa mesaj aracılığıyla size gönderilen kodu girin ve ardından **sonraki**.

    ![Telefon numarası ekleyin ve metin iletisi seçin](media/security-info/securityinfo-myprofile-phonetext-entercode.png)

    Başarınızı göstermek için sayfa değişiklikleri.

    ![Başarı bildirimi telefon bağlanma, sayı, metin iletileri ve hesabınızı alma seçeneği](media/security-info/securityinfo-myprofile-phonetext-success.png)

    Güvenlik bilgileriniz güncelleştirildi ve iki aşamalı doğrulama veya parola sıfırlaması kullanırken, kimliğinizi doğrulamak için ileti metni kullanabilirsiniz. Varsayılan yönteminizi ileti metni yapmak istiyorsanız bkz [varsayılan güvenlik bilgisi yönteminizi değiştirmek](#change-your-default-security-info-method) bu makalenin.

## <a name="delete-text-messaging-from-your-security-info-methods"></a>Güvenlik bilgileri yöntemlerinizi ileti metni silin
Artık metin iletileri güvenlik bilgisi yöntemi olarak kullanmak istiyorsanız, buradan kaldırabilirsiniz **güvenlik bilgisi** sayfası.

>[!Important]
>Metin iletileri yanlışlıkla silerseniz, geri almak için hiçbir yolu yoktur. Yöntem yeniden eklemek aşağıdaki adımları gerekir [metin mesajlarını ayarla](#set-up-text-messages-from-the-security-info-page) bu makalenin.

### <a name="to-delete-text-messaging"></a>İleti metni silmek için

1. Üzerinde **güvenlik bilgisi** sayfasında **Sil** yanındaki bağlantı **telefon** seçeneği.

    ![Telefon ve güvenlik bilgileri yönteminden ileti metni silmek için bağlantı](media/security-info/securityinfo-myprofile-phonetext-delete.png)

2. Seçin **Evet** silmek için onay kutusundan **telefon** sayı. Telefon numaranızı silinir, güvenlik bilgilerinizden kaldırılır ve bu kaybolur sonra **güvenlik bilgisi** sayfası. Varsa **telefon** , varsayılan yöntemdir varsayılan kullanılabilir başka bir yönteme değişir.

## <a name="change-your-default-security-info-method"></a>Varsayılan güvenlik bilgisi yönteminizi değiştirmek
Metin iletileri iki aşamalı doğrulama kullanarak iş veya Okul hesabınıza oturum açtığınızda ya da parola sıfırlama istekleri için kullanılan varsayılan yöntemi olmasını istiyorsanız, buradan ayarlayabilirsiniz **güvenlik bilgisi** sayfası.

### <a name="to-change-your-default-security-info-method"></a>Varsayılan güvenlik bilgisi yönteminizi değiştirmek için

1. Üzerinde **güvenlik bilgisi** sayfasında **değişiklik** yanındaki bağlantı **varsayılan oturum açma yöntemi** bilgileri.

    ![Bağlantı için varsayılan oturum açma yöntemini değiştirme](media/security-info/securityinfo-myprofile-phonetext-defaultchange.png)

2. Seçin **telefon - metin ( *_your_phone_number_* )** metotları seçin ve açılan listeden **Onayla**.

    ![İçin varsayılan oturum açma yöntemini seçin](media/security-info/securityinfo-myprofile-phonetext-changeddefault.png)

    Oturum açma, değişiklikler için kullanılan varsayılan yöntemi **telefon - metin ( *_your_phone_number_* )** .

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemi
Nasıl kuruluş kişilerinizi, olduğuna göre kimliğinizi doğrulamak için yapmak çalıştığınız için ek bir seçeneğiniz vardır. Seçeneklere şunlar dahildir:

- **Authenticator uygulaması.** İndirin ve iki aşamalı doğrulama veya parola sıfırlama için bir onay bildirimi ya da bir rastgele oluşturulmuş bir onay kodu almak için bir authenticator uygulamasını kullanın. Ayarlama ve Microsoft Authenticator uygulamasını kullanma hakkında adım adım yönergeler için bkz. [authenticator uygulamasını kullanmak için güvenlik bilgileri ' ayarlamak](security-info-setup-auth-app.md).

- **Mobil cihaz veya iş telefon çağrısı.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için bir telefon araması alın. Bir telefon numarası ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz: [güvenlik bilgilerini, telefon aramaları kullanacak şekilde](security-info-setup-phone-number.md).

- **E-posta adresi.** Girin, iş veya Okul e-posta adresinizi, parola sıfırlama için bir e-posta almak için. Bu seçenek, iki aşamalı doğrulama için kullanılamaz. E-posta kurma hakkında adım adım yönergeler için bkz. [güvenlik bilgileri e-posta kullanacak şekilde](security-info-setup-email.md).

- **Güvenlik sorusu.** Yöneticiniz kuruluşunuz için oluşturduğu bazı güvenlik soruları yanıtlayın. Bu seçenek, yalnızca parola sıfırlama için ve iki aşamalı doğrulama için kullanılabilir. Güvenlik sorularınızı ayarlama konusunda adım adım yönergeler için bkz: [güvenlik bilgisi güvenlik sorularını kullanacak şekilde](security-info-setup-questions.md) makalesi.
    
    >[!Note]
    >Bu seçeneklerden bazısı eksikse, kuruluşunuz bu yöntemleri izin vermeyen büyük olasılıkla olmasıdır. Bu durumda, kullanılabilir bir yöntem seçin veya daha fazla yardım için yöneticinize başvurun gerekecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Kaybolur veya, gelen unutulabilir, parolanızı sıfırlayamıyoruz [parola sıfırlama portalı](https://passwordreset.microsoftonline.com/) veya adımları [iş veya Okul parolanızı sıfırlama](user-help-reset-password.md) makalesi.

- İpuçları ve Yardım almak için oturum açma sorunlarını giderme alma [Microsoft hesabınızda oturum açamazsınız](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.