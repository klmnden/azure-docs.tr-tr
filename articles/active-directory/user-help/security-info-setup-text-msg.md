---
title: Kısa mesaj - kullanılacak güvenlik bilgileri Azure Active Directory ayarlama | Microsoft Docs
description: Metin (SMS) mesajı kullanarak kimliğinizi doğrulamak için güvenlik bilgilerinizi ayarlayın.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: 64b3ba5d6f3c257fdbd462c0297ff6b0944163db
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54814115"
---
# <a name="set-up-security-info-to-use-text-messaging-preview"></a>Metin iletileri (Önizleme) kullanmak için güvenlik bilgileri ' ayarlayın

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Güvenlik bilgilerinizi ayarlamak için iş veya Okul hesabınızda oturum açın ve ardından kayıt işlemini tamamlamak gerekir. Güvenlik bilgilerinizi daha önce oluşturmadıysanız, artık yapmanız istenir.

## <a name="set-up-text-messaging"></a>SMS mesajlaşmasını ayarlama

Kuruluşunuzun ayarlara bağlı olarak, metin, oturum açtığında güvenlik bilgilerinizi iletileri eklemek için istenebilir. Aksi takdirde, güvenlik bilgilerini ileti metni ayarlama başlamak için adımları izleyin. [güvenlik bilgilerinizi yönetmek](security-info-manage-settings.md).

Telefon numaranızı her şeyi yaptığınız şekilde ayarlayacağız metin mesajı seçeneğini telefon seçeneğine bir parçası olduğundan, ancak Microsoft çağırmanızı sahip olmak yerine, bir kısa mesaj kullanılacak seçeneğini belirleyin. Telefon seçeneğine görmüyorsanız, kuruluşunuzun, bir telefon numarası doğrulama işlemlerinde sesten izin vermez mümkündür. Bu durumda, daha fazla yardım için yöneticinize başvurun veya başka bir yöntem seçmeniz gerekir.

### <a name="to-use-a-text-message"></a>Kısa mesaj kullanmak için

1. Seçin **telefon** seçeneği.

    **, Telefon numarasını ayarla** Sihirbazı görünür.

    ![Ülke veya bölge kodu ve telefon numaranızı ayarlayın](media/security-info/security-info-keep-secure-setup-text.png)

2. Çekme, **ülke veya bölge** telefon numaranızı (eğer varsa, alan kodunu dahil) yazın açılan kutusundan **telefon numarası** kutusunda **metin bana bir kod** seçeneğini ve ardından **sonraki**.

    Doğrulama sayfasına girmek ihtiyacınız olacak, bir kod içeren bir kısa mesaj alacaksınız.

    ![Doğrulama sayfasına metin iletisi kodunu girin](media/security-info/security-info-keep-secure-verify-text-msg.png)

    İki aşamalı doğrulama veya Self Servis parola sıfırlaması kullanırken, kimliğinizi doğrulamak için bir kısa mesaj göndermek için güvenlik bilgilerinizi güncelleştirir.

    >[!Note]
    >Bir telefon araması, SMS mesajı yerine almak istiyorsanız, adımları [güvenlik bilgilerini, telefon aramaları kullanacak şekilde](security-info-setup-phone-number.md) makalesi.

## <a name="additional-security-info-options"></a>Ek güvenlik bilgisi seçenekleri

Nasıl kuruluş kişilerinizi, olduğuna göre kimliğinizi doğrulamak için yapmak çalıştığınız için seçeneğiniz vardır. Seçeneklere şunlar dahildir:

- **Authenticator uygulaması.** İndirin ve iki aşamalı doğrulama veya parola sıfırlama için bir onay bildirimi ya da bir rastgele oluşturulmuş bir onay kodu almak için bir authenticator uygulamasını kullanın. Ayarlama ve Microsoft Authenticator uygulamasını kullanma hakkında adım adım yönergeler için bkz. [authenticator uygulamasını kullanmak için güvenlik bilgileri ' ayarlamak](security-info-setup-auth-app.md).

- **Mobil cihaz veya iş telefon çağrısı.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için bir telefon araması alın. Bir telefon numarası ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz: [güvenlik bilgilerini, telefon aramaları kullanacak şekilde](security-info-setup-phone-number.md).

- **E-posta adresi.** Girin, iş veya Okul e-posta adresinizi, parola sıfırlama için bir e-posta almak için. Bu seçenek, iki aşamalı doğrulama için kullanılamaz. E-posta kurma hakkında adım adım yönergeler için bkz. [güvenlik bilgileri e-posta kullanacak şekilde](security-info-setup-email.md).
   
    >[!Note]
    >Bu seçeneklerden bazısı eksikse, kuruluşunuz bu yöntemleri izin vermeyen büyük olasılıkla olmasıdır. Bu durumda, kullanılabilir bir yöntem seçin veya daha fazla yardım için yöneticinize başvurun gerekecektir.

- **Güvenlik sorusu.** Yöneticiniz kuruluşunuz için oluşturduğu bazı güvenlik soruları yanıtlayın. Bu seçenek, yalnızca parola sıfırlama için ve iki aşamalı doğrulama için kullanılabilir. Güvenlik sorularınızı ayarlama konusunda adım adım yönergeler için bkz: [güvenlik bilgisi güvenlik sorularını kullanacak şekilde](security-info-setup-questions.md) makalesi.

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik bilgilerinizi güncelleştirmeniz gerekiyorsa, yönergeleri izleyin [güvenlik bilgilerinizi yönetmek](security-info-manage-settings.md) makalesi.

- Kaybolur veya, gelen unutulabilir, parolanızı sıfırlayamıyoruz [parola sıfırlama portalı](https://passwordreset.microsoftonline.com/) veya adımları [iş veya Okul parolanızı sıfırlama](user-help-reset-password.md) makalesi.

- İpuçları ve Yardım almak için oturum açma sorunlarını giderme alma [Microsoft hesabınızda oturum açamazsınız](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.