---
title: Güvenlik bilgileri (Önizleme), e-posta adresinizi - Azure Active Directory kullanacak şekilde | Microsoft Docs
description: E-posta adresinizi kullanarak kimliğinizi doğrulamak için güvenlik bilgilerinizi ayarlamak nasıl.
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
ms.openlocfilehash: 720aafac79a67f64b0974dba0dd60c6aa24a8c54
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60480630"
---
# <a name="set-up-security-info-preview-to-use-your-email-address"></a>Güvenlik bilgileri (Önizleme) e-posta adresinizi ayarlayın
Yöntemi, parolanızı sıfırlayamıyoruz eklemek için aşağıdaki adımları izleyebilirsiniz. Bu ilk kez ayarladıktan sonra dönebilirsiniz **güvenlik bilgisi** sayfasına ekleme, güncelleştirme veya güvenlik bilgilerinizi silin.

Ayarladıktan sonra parolanızı sıfırlayamıyoruz yöntemi, ayrıca, iki Faktörlü doğrulama yöntemi ayarladığını ayarlamanız gerekir kullanarak bir [authenticator uygulamasını](security-info-setup-auth-app.md), [metin Mesajlaşma](security-info-setup-text-msg.md), veya bir [telefon araması](security-info-setup-phone-number.md).

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

## <a name="set-up-your-email-address-from-the-security-info-page"></a>Güvenlik bilgileri sayfasından e-posta adresinizi ayarlayın
Kuruluşunuzun ayarlara bağlı olarak, güvenlik bilgisi yöntemlerinden biri olarak e-posta adresinizi kullanmanız mümkün olabilir.

>[!Note]
>Erişmek için Ağ parolanızı gerektirmeyen bir e-posta adresi kullanmanızı öneririz. E-posta seçeneği görmüyorsanız, kuruluşunuzun, e-posta doğrulama işlemlerinde sesten izin vermez mümkündür. Bu durumda, daha fazla yardım için yöneticinize başvurun veya başka bir yöntem seçmeniz gerekir.

### <a name="to-set-up-your-email-address"></a>E-posta adresinizi ayarlamak için

1. İş veya Okul hesabınızda oturum açın ve ardından Git kullanarak https://myprofile.microsoft.com/ sayfası.

    ![Vurgulanan güvenlik bilgisi bağlantıları gösteren profili sayfam](media/security-info/securityinfo-myprofile-page.png)

2. Seçin **güvenlik bilgisi** sol gezinti bölmesinden ya da bağlantıyı **güvenlik bilgisi** engelleyebilir ve ardından **yöntemi ekleyin** gelen **güvenlik bilgisi**  sayfası.

    ![Güvenlik bilgileri sayfasını vurgulanan Ekle yöntemi seçeneği](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Üzerinde **bir yöntem ekleyin** sayfasında **e-posta** seçin ve açılır listede **Ekle**.

    ![Eposta ile yöntemi Kutusu Ekle](media/security-info/securityinfo-myprofile-addemail.png)

4. Üzerinde **e-posta** sayfasında, e-posta adresinizi yazın (örneğin, alain@gmail.com) ve ardından **sonraki**.

    ![Telefon numarası ekleyin ve telefon aramaları seçin](media/security-info/securityinfo-myprofile-emailaddress.png)

    >[!Important]
    >Bu e-posta adresi, iş veya Okul e-posta olamaz.

5. Belirtilen e-posta adresinize gönderilen kodu yazın ve ardından **sonraki**.

    ![Telefon numarası ekleyin ve metin iletisi seçin](media/security-info/securityinfo-myprofile-emailcode.png)

    Güvenlik bilgilerinizi güncelleştirilir ve e-posta adresinizi, parola sıfırlaması kullanırken, kimliğinizi doğrulamak için kullanabilirsiniz.

## <a name="delete-your-email-address-from-your-security-info-methods"></a>E-posta adresinizi güvenlik bilgisi yöntemlerinizi Sil
Artık e-posta adresinizi güvenlik bilgisi yöntemi olarak kullanmak istiyorsanız, buradan kaldırabilirsiniz **güvenlik bilgisi** sayfası.

>[!Important]
>E-posta adresinizi yanlışlıkla silerseniz, geri almak için hiçbir yolu yoktur. Yöntem yeniden eklemek aşağıdaki adımları gerekir [e-posta adresinizi ayarlayın](#set-up-your-email-address-from-the-security-info-page) bu makalenin.

### <a name="to-delete-your-email-address"></a>E-posta adresinizi silmek için

1. Üzerinde **güvenlik bilgisi** sayfasında **Sil** yanındaki bağlantı **e-posta** seçeneği.

    ![Güvenlik bilgisi telefon yöntemi silmek için bağlantı](media/security-info/securityinfo-myprofile-emaildelete.png)

2. Seçin **Evet** silmek için onay kutusundan **e-posta** hesabı. E-posta hesabı tarafından silindiği, güvenlik bilgilerinizden kaldırılır ve bu kaybolur sonra **güvenlik bilgisi** sayfası.

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemi
Nasıl kuruluş kişilerinizi, olduğuna göre kimliğinizi doğrulamak için yapmak çalıştığınız için ek bir seçeneğiniz vardır. Seçeneklere şunlar dahildir:

- **Authenticator uygulaması.** İndirin ve iki aşamalı doğrulama veya parola sıfırlama için bir onay bildirimi ya da bir rastgele oluşturulmuş bir onay kodu almak için bir authenticator uygulamasını kullanın. Ayarlama ve Microsoft Authenticator uygulamasını kullanma hakkında adım adım yönergeler için bkz. [authenticator uygulamasını kullanmak için güvenlik bilgileri ' ayarlamak](security-info-setup-auth-app.md).

- **Mobil cihaz metin.** Mobil cihaz numaranızı girin ve kısa mesaj, iki aşamalı doğrulama veya parola için kullanacağınız bir kod alın sıfırlayın. Kısa mesaj (SMS) ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz: [güvenlik bilgilerini, metin iletileri (SMS) kullanacak şekilde](security-info-setup-text-msg.md).

- **Mobil cihaz veya iş telefon çağrısı.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için bir telefon araması alın. Bir telefon numarası ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz: [güvenlik bilgilerini, telefon aramaları kullanacak şekilde](security-info-setup-phone-number.md).

- **Güvenlik sorusu.** Yöneticiniz kuruluşunuz için oluşturduğu bazı güvenlik soruları yanıtlayın. Bu seçenek, yalnızca parola sıfırlama için ve iki aşamalı doğrulama için kullanılabilir. Güvenlik sorularınızı ayarlama konusunda adım adım yönergeler için bkz: [güvenlik bilgisi güvenlik sorularını kullanacak şekilde](security-info-setup-questions.md) makalesi.
    
    >[!Note]
    >Bu seçeneklerden bazısı eksikse, kuruluşunuz bu yöntemleri izin vermeyen büyük olasılıkla olmasıdır. Bu durumda, kullanılabilir bir yöntem seçin veya daha fazla yardım için yöneticinize başvurun gerekecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Kaybolur veya, gelen unutulabilir, parolanızı sıfırlayamıyoruz [parola sıfırlama portalı](https://passwordreset.microsoftonline.com/) veya adımları [iş veya Okul parolanızı sıfırlama](user-help-reset-password.md) makalesi.

- İpuçları ve Yardım almak için oturum açma sorunlarını giderme alma [Microsoft hesabınızda oturum açamazsınız](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.
