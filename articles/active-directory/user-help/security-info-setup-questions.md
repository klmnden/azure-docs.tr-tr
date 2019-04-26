---
title: Güvenlik bilgileri (Önizleme) - Azure Active Directory güvenlik sorunuzu kullanacak şekilde | Microsoft Docs
description: Güvenlik bilgilerinizi kullanarak kimlik doğrulamak için ayarlama güvenlik sorularını önceden tanımlanmış.
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
ms.openlocfilehash: 3e5d1546c658631911f25c43e94275f00c7a5140
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60474638"
---
# <a name="set-up-security-info-preview-to-use-security-questions"></a>Güvenlik sorularını kullanmak için güvenlik bilgileri (Önizleme) ayarlama
Yöntemi, parolanızı sıfırlayamıyoruz eklemek için aşağıdaki adımları izleyebilirsiniz. Bu ilk kez ayarladıktan sonra dönebilirsiniz **güvenlik bilgisi** sayfasına ekleme, güncelleştirme veya güvenlik bilgilerinizi silin.

Ayarladıktan sonra parolanızı sıfırlayamıyoruz yöntemi, ayrıca, iki Faktörlü doğrulama yöntemi ayarladığını ayarlamanız gerekir kullanarak bir [authenticator uygulamasını](security-info-setup-auth-app.md), [metin Mesajlaşma](security-info-setup-text-msg.md), veya bir [telefon araması](security-info-setup-phone-number.md).

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

## <a name="set-up-your-security-questions-from-the-security-info-page"></a>Güvenlik sorularınızı güvenlik bilgileri sayfasından ayarlama
Kuruluşunuzun ayarlara bağlı olarak seçin ve, güvenlik bilgisi yöntemlerinden biri olarak birkaç güvenlik soruları yanıtlamak mümkün olabilir. Yöneticiniz seçin ve yanıtlamak için gereken güvenlik sorusu sayısını ayarlar.

Güvenlik sorularını kullanıyorsanız, bunları başka bir yöntem ile birlikte kullanmanızı öneririz. Bazı kişiler sorularınızın yanıtlarını biliyor olabilirsiniz güvenlik sorularını diğer yöntemlerinden daha az güvenli olabilir.

> [!Note]
> Güvenlik sorularını dizindeki kullanıcı nesnesinin üzerinde özel ve güvenli bir şekilde depolanır ve yalnızca sizin tarafınızdan kayıt sırasında yanıtlanması. Okuma veya soru veya yanıt değiştirmek için yöneticinize bir yolu yoktur.
> 
> Güvenlik soruları seçeneğini görmüyorsanız, kuruluşunuzun, doğrulama için güvenlik sorularını kullan izin vermez mümkündür. Bu durumda, daha fazla yardım için yöneticinize başvurun veya başka bir yöntem seçmeniz gerekir.
> 
> Yönetici hesabı parola sıfırlama yöntemi olarak güvenlik sorularını kullanmak için izin verilmez. Bir yönetici düzeyi hesabı oturum açtıysanız bu seçeneği görmezsiniz.

### <a name="to-set-up-your-security-questions"></a>Güvenlik sorularınızı ayarlamak için

1. İş veya Okul hesabınızda oturum açın ve ardından Git kullanarak https://myprofile.microsoft.com/ sayfası.

    ![Vurgulanan güvenlik bilgisi bağlantıları gösteren profili sayfam](media/security-info/securityinfo-myprofile-page.png)

2. Seçin **güvenlik bilgisi** sol gezinti bölmesinden ya da bağlantıyı **güvenlik bilgisi** engelleyebilir ve ardından **yöntemi ekleyin** gelen **güvenlik bilgisi**  sayfası.

    ![Güvenlik bilgileri sayfasını vurgulanan Ekle yöntemi seçeneği](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Üzerinde **bir yöntem ekleyin** sayfasında **güvenlik sorularını** seçin ve açılır listede **Ekle**.

    ![Yöntemi kutusunda, seçili güvenlik soruları Ekle](media/security-info/securityinfo-myprofile-addquestions.png)

4. Üzerinde **güvenlik sorularını** sayfasında, seçin ve güvenlik sorularınızı yanıtlayın ve ardından **Kaydet**.

    ![Telefon numarası ekleyin ve telefon aramaları seçin](media/security-info/securityinfo-myprofile-securityquestions.png)

    Güvenlik bilgileriniz güncelleştirildi ve güvenlik sorularınızı parola sıfırlaması kullanırken, kimliğinizi doğrulamak için kullanabilirsiniz.

## <a name="delete-security-questions-from-your-security-info-methods"></a>Güvenlik sorularını güvenlik bilgisi yöntemlerinizi Sil
Artık bir güvenlik bilgisi yöntemi olarak güvenlik sorularını kullanmak istiyorsanız, bunları kaldırabilirsiniz **güvenlik bilgisi** sayfası.

>[!Important]
>Güvenlik sorularınızı yanlışlıkla silerseniz, geri almak için hiçbir yolu yoktur. Yöntem yeniden eklemek aşağıdaki adımları gerekir [, güvenlik sorularını](#set-up-your-security-questions-from-the-security-info-page) bu makalenin.

### <a name="to-delete-your-security-questions"></a>Güvenlik sorularınızı silmek için

1. Üzerinde **güvenlik bilgisi** sayfasında **Sil** yanındaki bağlantı **güvenlik sorularını** seçeneği.

    ![Güvenlik bilgisi telefon yöntemi silmek için bağlantı](media/security-info/securityinfo-myprofile-questionsdelete.png)

2. Seçin **Evet** silmek için onay kutusundan, **güvenlik sorularını**. Güvenlik sorularınızı silinir, yöntem güvenlik bilgilerinizden kaldırılır ve bu kaybolur sonra **güvenlik bilgisi** sayfası.

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemi
Nasıl kuruluş kişilerinizi, olduğuna göre kimliğinizi doğrulamak için yapmak çalıştığınız için ek bir seçeneğiniz vardır. Seçeneklere şunlar dahildir:

- **Authenticator uygulaması.** İndirin ve iki aşamalı doğrulama veya parola sıfırlama için bir onay bildirimi ya da bir rastgele oluşturulmuş bir onay kodu almak için bir authenticator uygulamasını kullanın. Ayarlama ve Microsoft Authenticator uygulamasını kullanma hakkında adım adım yönergeler için bkz. [authenticator uygulamasını kullanmak için güvenlik bilgileri ' ayarlamak](security-info-setup-auth-app.md).

- **Mobil cihaz metin.** Mobil cihaz numaranızı girin ve kısa mesaj, iki aşamalı doğrulama veya parola için kullanacağınız bir kod alın sıfırlayın. Kısa mesaj (SMS) ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz: [güvenlik bilgilerini, metin iletileri (SMS) kullanacak şekilde](security-info-setup-text-msg.md).

- **Mobil cihaz veya iş telefon çağrısı.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için bir telefon araması alın. Bir telefon numarası ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz: [güvenlik bilgilerini, telefon aramaları kullanacak şekilde](security-info-setup-phone-number.md).

- **E-posta adresi.** Girin, iş veya Okul e-posta adresinizi, parola sıfırlama için bir e-posta almak için. Bu seçenek, iki aşamalı doğrulama için kullanılamaz. E-posta kurma hakkında adım adım yönergeler için bkz. [güvenlik bilgileri e-posta kullanacak şekilde](security-info-setup-email.md).
   
    >[!Note]
    >Bu seçeneklerden bazısı eksikse, kuruluşunuz bu yöntemleri izin vermeyen büyük olasılıkla olmasıdır. Bu durumda, kullanılabilir bir yöntem seçin veya daha fazla yardım için yöneticinize başvurun gerekecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Kaybolur veya, gelen unutulabilir, parolanızı sıfırlayamıyoruz [parola sıfırlama portalı](https://passwordreset.microsoftonline.com/) veya adımları [iş veya Okul parolanızı sıfırlama](user-help-reset-password.md) makalesi.

- İpuçları ve Yardım almak için oturum açma sorunlarını giderme alma [Microsoft hesabınızda oturum açamazsınız](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.
