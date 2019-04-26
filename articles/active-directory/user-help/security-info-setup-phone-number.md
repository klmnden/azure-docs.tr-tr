---
title: Güvenlik bilgileri (Önizleme), telefon aramaları - Azure Active Directory kullanacak şekilde | Microsoft Docs
description: Telefon aramaları kullanarak kimliğinizi doğrulamak için güvenlik bilgilerinizi ayarlamak nasıl.
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
ms.openlocfilehash: 9c1620be30d8cdf3a592ab0fc118938783579689
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60475002"
---
# <a name="set-up-security-info-preview-to-use-phone-calls"></a>Telefon aramaları kullanmak için güvenlik bilgileri (Önizleme) ayarlama
İki aşamalı doğrulamayı eklemek için aşağıdaki adımları uygulayabilirsiniz ve yöntemleri sıfırlama. Bu ilk kez ayarladıktan sonra dönebilirsiniz **güvenlik bilgisi** sayfasına ekleme, güncelleştirme veya güvenlik bilgilerinizi silin.

Hemen iş veya Okul hesabınızda oturum açtıktan sonra bunu ayarlamak için istenirse, ayrıntılı adımlara bakın [güvenlik bilgilerinizi ayarlayın oturum açma sayfası isteminden](security-info-setup-signin.md) makalesi.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

> [!Note]
> Güvenlik bilgileri, telefon uzantılarıyla desteklemiyor. Doğru biçimde ekleseniz bile 12345, uzantıları X + 1 4255551234 kurulmadan önce kaldırılır.
> 
> Bir telefon seçeneği görmüyorsanız, kuruluşunuzun, bir telefon numarası doğrulama işlemlerinde sesten izin vermez mümkündür. Bu durumda, daha fazla yardım için yöneticinize başvurun veya başka bir yöntem seçmeniz gerekir.

## <a name="set-up-phone-calls-from-the-security-info-page"></a>Telefon aramaları güvenlik bilgileri sayfasından ayarlayın
Kuruluşunuzun ayarlara bağlı olarak, telefon görüşmeleri, güvenlik bilgisi yöntemlerinden biri olarak kullanmanız mümkün olabilir.

>[!Note]
>Bir metin iletisi yerine bir telefon araması almak istiyorsanız, adımları [güvenlik bilgilerini, kısa mesaj kullanacak şekilde](security-info-setup-text-msg.md) makale.

### <a name="to-set-up-phone-calls"></a>Telefon aramaları ayarlamak için

1. İş veya Okul hesabınızda oturum açın ve ardından Git kullanarak https://myprofile.microsoft.com/ sayfası.

    ![Vurgulanan güvenlik bilgisi bağlantıları gösteren profili sayfam](media/security-info/securityinfo-myprofile-page.png)

2. Seçin **güvenlik bilgisi** sol gezinti bölmesinden ya da bağlantıyı **güvenlik bilgisi** engelleyebilir ve ardından **yöntemi ekleyin** gelen **güvenlik bilgisi**  sayfası.

    ![Güvenlik bilgileri sayfasını vurgulanan Ekle yöntemi seçeneği](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Üzerinde **bir yöntem ekleyin** sayfasında **telefon** seçin ve açılır listede **Ekle**.

    ![Seçili olan telefon yöntemi Kutusu Ekle](media/security-info/securityinfo-myprofile-addphonetext.png)

4. Üzerinde **telefon** sayfasında mobil cihazınız için telefon numarasını yazın, **beni**ve ardından **sonraki**.

    ![Telefon numarası ekleyin ve telefon aramaları seçin](media/security-info/securityinfo-myprofile-phonecall-addnumber.png)

5. Girdiğiniz telefon numarasına gönderilen doğrulama telefon görüşmesi, yanıt ve yönergeleri izleyin.

    Başarınızı göstermek için sayfa değişiklikleri.

    ![Başarı bildirimi telefon bağlanma, sayı, telefon görüşmeleri ve hesabınızı alma seçeneği](media/security-info/securityinfo-myprofile-phonetext-success.png)

    Güvenlik bilgileriniz güncelleştirildi ve iki aşamalı doğrulama veya parola sıfırlaması kullanırken, kimliğinizi doğrulamak için telefon aramaları kullanabilirsiniz. Varsayılan yönteminizi telefon aramaları yapmak istiyorsanız, bkz. [varsayılan güvenlik bilgisi yönteminizi değiştirmek](#change-your-default-security-info-method) bu makalenin.

## <a name="delete-phone-calls-from-your-security-info-methods"></a>Güvenlik bilgileri yöntemlerinizi telefon aramaları silin
Artık telefon aramaları güvenlik bilgisi yöntemi olarak kullanmak istiyorsanız, buradan kaldırabilirsiniz **güvenlik bilgisi** sayfası.

>[!Important]
>Telefon aramaları yanlışlıkla silerseniz, geri almak için hiçbir yolu yoktur. Yöntem yeniden eklemek aşağıdaki adımları gerekir [telefon aramaları ayarlayın](#set-up-phone-calls-from-the-security-info-page) bu makalenin.

### <a name="to-delete-phone-calls"></a>Telefon görüşmeleri silmek için

1. Üzerinde **güvenlik bilgisi** sayfasında **Sil** yanındaki bağlantı **telefon** seçeneği.

    ![Güvenlik bilgisi telefon yöntemi silmek için bağlantı](media/security-info/securityinfo-myprofile-phonetext-delete.png)

2. Seçin **Evet** silmek için onay kutusundan **telefon** sayı. Telefon numaranızı silinir, güvenlik bilgilerinizden kaldırılır ve bu kaybolur sonra **güvenlik bilgisi** sayfası. Varsa **telefon** , varsayılan yöntemdir varsayılan kullanılabilir başka bir yönteme değişir.
    
## <a name="change-your-default-security-info-method"></a>Varsayılan güvenlik bilgisi yönteminizi değiştirmek
Telefon görüşmeleri, iki aşamalı doğrulama kullanarak iş veya Okul hesabınıza oturum açtığınızda ya da parola sıfırlama istekleri için kullanılan varsayılan yöntemi olmasını istiyorsanız, buradan ayarlayabilirsiniz **güvenlik bilgisi** sayfası.

### <a name="to-change-your-default-security-info-method"></a>Varsayılan güvenlik bilgisi yönteminizi değiştirmek için

1. Üzerinde **güvenlik bilgisi** sayfasında **değişiklik** yanındaki bağlantı **varsayılan oturum açma yöntemi** bilgileri.

    ![Bağlantı için varsayılan oturum açma yöntemini değiştirme](media/security-info/securityinfo-myprofile-phonetext-defaultchange.png)

2. Seçin **telefon - çağrı (*_your_phone_number_*)** metotları seçin ve açılan listeden **Onayla**.

    ![İçin varsayılan oturum açma yöntemini seçin](media/security-info/securityinfo-myprofile-phonecall-changeddefault.png)

    Oturum açma, değişiklikler için kullanılan varsayılan yöntemi **telefon - çağrı (*_your_phone_number_*)**.

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemi
Nasıl kuruluş kişilerinizi, olduğuna göre kimliğinizi doğrulamak için yapmak çalıştığınız için ek bir seçeneğiniz vardır. Seçeneklere şunlar dahildir:

- **Authenticator uygulaması.** İndirin ve iki aşamalı doğrulama veya parola sıfırlama için bir onay bildirimi ya da bir rastgele oluşturulmuş bir onay kodu almak için bir authenticator uygulamasını kullanın. Ayarlama ve Microsoft Authenticator uygulamasını kullanma hakkında adım adım yönergeler için bkz. [authenticator uygulamasını kullanmak için güvenlik bilgileri ' ayarlamak](security-info-setup-auth-app.md).

- **Mobil cihaz metin.** Mobil cihaz numaranızı girin ve kısa mesaj, iki aşamalı doğrulama veya parola için kullanacağınız bir kod alın sıfırlayın. Kısa mesaj (SMS) ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz: [güvenlik bilgilerini, metin iletileri (SMS) kullanacak şekilde](security-info-setup-text-msg.md).

- **E-posta adresi.** Girin, iş veya Okul e-posta adresinizi, parola sıfırlama için bir e-posta almak için. Bu seçenek, iki aşamalı doğrulama için kullanılamaz. E-posta kurma hakkında adım adım yönergeler için bkz. [güvenlik bilgileri e-posta kullanacak şekilde](security-info-setup-email.md).

- **Güvenlik sorusu.** Yöneticiniz kuruluşunuz için oluşturduğu bazı güvenlik soruları yanıtlayın. Bu seçenek, yalnızca parola sıfırlama için ve iki aşamalı doğrulama için kullanılabilir. Güvenlik sorularınızı ayarlama konusunda adım adım yönergeler için bkz: [güvenlik bilgisi güvenlik sorularını kullanacak şekilde](security-info-setup-questions.md) makalesi.
    
    >[!Note]
    >Bu seçeneklerden bazısı eksikse, kuruluşunuz bu yöntemleri izin vermeyen büyük olasılıkla olmasıdır. Bu durumda, kullanılabilir bir yöntem seçin veya daha fazla yardım için yöneticinize başvurun gerekecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Kaybolur veya, gelen unutulabilir, parolanızı sıfırlayamıyoruz [parola sıfırlama portalı](https://passwordreset.microsoftonline.com/) veya adımları [iş veya Okul parolanızı sıfırlama](user-help-reset-password.md) makalesi.

- İpuçları ve Yardım almak için oturum açma sorunlarını giderme alma [Microsoft hesabınızda oturum açamazsınız](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.