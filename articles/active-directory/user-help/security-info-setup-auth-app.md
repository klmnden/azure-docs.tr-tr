---
title: Güvenlik bilgileri (Önizleme) bir doğrulayıcı uygulama - Azure Active Directory kullanacak şekilde | Microsoft Docs
description: Güvenlik bilgilerinizi Microsoft Authenticator uygulamasını kullanarak kimliğinizi doğrulamak için nasıl kurulur.
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
ms.openlocfilehash: a4757be00a3633f56aed52dd7af22923e49b0b62
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60475722"
---
# <a name="set-up-security-info-preview-to-use-an-authenticator-app"></a>Authenticator uygulamasını kullanmak için güvenlik bilgileri (Önizleme) ayarlama
İki aşamalı doğrulamayı eklemek için aşağıdaki adımları uygulayabilirsiniz ve yöntemleri sıfırlama. Bu ilk kez ayarladıktan sonra dönebilirsiniz **güvenlik bilgisi** sayfasına ekleme, güncelleştirme veya güvenlik bilgilerinizi silin.

Hemen iş veya Okul hesabınızda oturum açtıktan sonra bunu ayarlamak için istenirse, ayrıntılı adımlara bakın [güvenlik bilgilerinizi ayarlayın oturum açma sayfası isteminden](security-info-setup-signin.md) makalesi.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>Bir doğrulayıcı uygulama seçeneğini görmüyorsanız, kuruluşunuzun doğrulama için bir authentication uygulamasını kullanmak için izin vermeyen mümkündür. Bu durumda, daha fazla yardım için yöneticinize başvurun veya başka bir yöntem seçmeniz gerekir.

## <a name="set-up-the-microsoft-authenticator-app-from-the-security-info-page"></a>Güvenlik bilgileri sayfasından Microsoft Authenticator uygulamasını ayarlama
Kuruluşunuzun ayarlara bağlı olarak, bir kimlik doğrulama uygulaması, güvenlik bilgisi yöntemlerinden biri olarak kullanmanız mümkün olabilir. Microsoft Authenticator uygulamasını kullanmak için gerekli değildir ve Kurulum işlemi sırasında farklı bir uygulama seçebilirsiniz. Ancak, bu makalede, Microsoft Authenticator uygulamasını kullanır. 

### <a name="to-set-up-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasını ayarlama

1. İş veya Okul hesabınızda oturum açın ve ardından Git kullanarak https://myprofile.microsoft.com/ sayfası.

    ![Vurgulanan güvenlik bilgisi bağlantıları gösteren profili sayfam](media/security-info/securityinfo-myprofile-page.png)

2. Seçin **güvenlik bilgisi** sol gezinti bölmesinden ya da bağlantıyı **güvenlik bilgisi** engelleyebilir ve ardından **yöntemi ekleyin** gelen **güvenlik bilgisi**  sayfası.

    ![Güvenlik bilgileri sayfasını vurgulanan Ekle yöntemi seçeneği](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Üzerinde **bir yöntem ekleyin** sayfasında **Authenticator uygulamasını** seçin ve açılır listede **Ekle**.

    ![Authenticator uygulama seçildi yöntemi Kutusu Ekle](media/security-info/securityinfo-myprofile-addauthapp.png)

4. Üzerinde **Başlat uygulamanın alarak** sayfasında **hemen indirin** indirin ve mobil Cihazınızda Microsoft Authenticator uygulamasını yükleyin ve ardından **sonraki**.

    Uygulamayı karşıdan yüklenip kurulacak hakkında daha fazla bilgi için bkz. [indirme ve yükleme için Microsoft Authenticator uygulamasını](user-help-auth-app-download-install.md).

    ![Uygulama sayfası alarak başlayın](media/security-info/securityinfo-myprofile-getauthapp.png)

   > [!Note]
   > Microsoft Authenticator uygulaması dışında bir authenticator uygulamasını kullanmak isteyip istemediğinizi seçin **farklı authenticator uygulamasını kullanmak istiyorum** bağlantı.
   > 
   > Kuruluşunuz kimlik doğrulayıcı uygulamasını yanı sıra farklı bir yöntemi seçmenize izin veriyorsa, seçebileceğiniz **farklı yöntem bağlantıyı oluşturan ayarlamak istediğiniz**.

5. Kalır **hesabınızı ayarlarken** mobil Cihazınızda Microsoft Authenticator uygulamasını ayarlama sırasında sayfa.

    ![Ayarlama authenticator uygulama sayfası](media/security-info/securityinfo-myprofile-setupauthapp.png)

6. Microsoft Authenticator uygulamasını açın, bildirimleri (istenirse) izin vermek için seçin, **Hesap Ekle** gelen **özelleştirme ve Denetim** sağ üst köşede, simgesine ve ardından **iş veya Okul hesabı**.

7. Geri dönüp **hesabınızı ayarlarken** sayfasında ve ardından **sonraki**.

    **QR kodunu tarayın** sayfası görüntülenir.

    ![Kimlik Doğrulayıcı uygulamasını kullanarak bir QR kodu tarama](media/security-info/securityinfo-myprofile-qrcodeauthapp.png)

6. Adım 6'da iş veya Okul hesabınızı oluşturduktan sonra mobil Cihazınızda görünen Microsoft Authenticator uygulaması QR kodu okuyucu ile sağlanan kodunu tarayın.

    Authenticator uygulaması başarıyla iş veya Okul hesabı Ekle'ye sizden herhangi bir ek bilgi gerek kalmadan. Ancak, QR kodu okuyucu kod okunamıyor, seçebileceğiniz **QR kodu bağlantısı tarayamıyor** ve el ile Microsoft Authenticator uygulamasına kodu ve URL'yi girin. El ile bir kod ekleme hakkında daha fazla bilgi için bkz. [hesap uygulamaya el ile Ekle](user-help-auth-app-add-account-manual.md).

7. Seçin **sonraki** üzerinde **QR kodunu tarayın** sayfasında.

    Bir bildirim hesabınızı test etmek için mobil Cihazınızda Microsoft Authenticator uygulamasını gönderilir.

    ![Hesabınızla kimlik doğrulayıcı uygulamasını deneyin](media/security-info/securityinfo-myprofile-tryitauthapp.png)

8. Microsoft Authenticator uygulamasını bildirimi onaylayın ve ardından **sonraki**.

     ![Başarı bildirimini, uygulama ve hesabınıza bağlanma](media/security-info/securityinfo-myprofile-successauthapp.png)

     Güvenlik bilgilerinizi Microsoft Authenticator uygulamasını kullanmak için iki aşamalı doğrulama veya parola sıfırlaması kullanırken, kimliğinizi doğrulamak için varsayılan olarak güncelleştirilir.

## <a name="delete-your-authenticator-app-from-your-security-info-methods"></a>Güvenlik bilgileri yöntemlerinizi cihazınızdaki kimlik doğrulayıcı uygulamasında Sil
Artık bir güvenlik bilgisi yöntemi olarak cihazınızdaki kimlik doğrulayıcı uygulamasında kullanmak istiyorsanız, buradan kaldırabilirsiniz **güvenlik bilgisi** sayfası. Bu, yalnızca Microsoft Authenticator uygulamasını tüm authenticator uygulamaları için çalışır. Uygulama sildikten sonra mobil Cihazınızda kimlik doğrulayıcı uygulamasına gidin ve hesap silme gerekecektir.

>[!Important]
>Kimlik Doğrulayıcı uygulamasını yanlışlıkla silerseniz, geri almak için hiçbir yolu yoktur. Kimlik Doğrulayıcı uygulamasını yeniden eklemek aşağıdaki adımları gerekir [authenticator uygulamasını ayarlama](#set-up-the-microsoft-authenticator-app-from-the-security-info-page) bu makalenin.

### <a name="to-delete-the-authenticator-app"></a>Kimlik Doğrulayıcı uygulamasını silmek için

1. Üzerinde **güvenlik bilgisi** sayfasında **Sil** kimlik doğrulayıcı uygulamasını yanındaki bağlantı.

    ![Güvenlik bilgisi kimlik doğrulayıcı uygulamasını silmek için bağlantı](media/security-info/securityinfo-myprofile-deleteauthapp.png)

2. Seçin **Evet** kimlik doğrulayıcı uygulamasını silmek için onay kutusu. Kimlik Doğrulayıcı uygulamasını silinir, güvenlik bilgilerinizden kaldırılır ve bu kaybolur sonra **güvenlik bilgisi** sayfası. Kimlik Doğrulayıcı uygulamasını varsayılan yönteminiz varsa, varsayılan kullanılabilir başka bir yönteme değişecektir.

3. Mobil Cihazınızda select authenticator uygulamasını açın **hesapları Düzenle**ve ardından iş veya Okul hesabınızla kimlik doğrulayıcı uygulamasında silin.

    Hesabınız iki adımlı doğrulama için authenticator uygulamasından tamamen kaldırılır ve parola sıfırlama isteği.

## <a name="change-your-default-security-info-method"></a>Varsayılan güvenlik bilgisi yönteminizi değiştirmek
Kimlik Doğrulayıcı uygulamasını iki aşamalı doğrulama kullanarak iş veya Okul hesabınıza oturum açtığınızda ya da parola sıfırlama istekleri için kullanılan varsayılan yöntemini olmasını istiyorsanız, güvenlikten ayarlayabilirsiniz **bilgisi** sayfası.

### <a name="to-change-your-default-security-info-method"></a>Varsayılan güvenlik bilgisi yönteminizi değiştirmek için

1. Üzerinde **güvenlik bilgisi** sayfasında **değişiklik** yanındaki bağlantı **varsayılan oturum açma yöntemi** bilgileri.

    ![Bağlantı için varsayılan oturum açma yöntemini değiştirme](media/security-info/securityinfo-myprofile-changedefaultauthapp.png)

2. Seçin **Microsoft Authenticator - bildirim** aşağı açılan listeden kullanılabilir yöntemleri. Microsoft Authenticator uygulamasını kullanmıyorsanız seçin **Authenticator uygulaması veya donanım belirteci** seçeneği.

    ![İçin varsayılan oturum açma yöntemini seçin](media/security-info/securityinfo-myprofile-defaultauthapp.png)

3. Seçin **onaylayın**.

    Microsoft Authenticator uygulamasını yapılan oturum açma için kullanılan varsayılan yöntemi.

## <a name="additional-security-info-methods"></a>Ek güvenlik bilgileri yöntemi
Nasıl kuruluş kişilerinizi, olduğuna göre kimliğinizi doğrulamak için yapmak çalıştığınız için ek bir seçeneğiniz vardır. Seçeneklere şunlar dahildir:

- **Mobil cihaz metin.** Mobil cihaz numaranızı girin ve kısa mesaj, iki aşamalı doğrulama veya parola için kullanacağınız bir kod alın sıfırlayın. Kısa mesaj (SMS) ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz: [güvenlik bilgilerini, metin iletileri (SMS) kullanacak şekilde](security-info-setup-text-msg.md).

- **Mobil cihaz veya iş telefon çağrısı.** Mobil cihaz numaranızı girin ve iki aşamalı doğrulama veya parola sıfırlama için bir telefon araması alın. Bir telefon numarası ile kimliğinizi doğrulama hakkında adım adım yönergeler için bkz: [güvenlik bilgilerini, telefon aramaları kullanacak şekilde](security-info-setup-phone-number.md).

- **E-posta adresi.** Girin, iş veya Okul e-posta adresinizi, parola sıfırlama için bir e-posta almak için. Bu seçenek, iki aşamalı doğrulama için kullanılamaz. E-posta kurma hakkında adım adım yönergeler için bkz. [güvenlik bilgileri e-posta kullanacak şekilde](security-info-setup-email.md).

- **Güvenlik sorusu.** Yöneticiniz kuruluşunuz için oluşturduğu bazı güvenlik soruları yanıtlayın. Bu seçenek, yalnızca parola sıfırlama için ve iki aşamalı doğrulama için kullanılabilir. Güvenlik sorularınızı ayarlama konusunda adım adım yönergeler için bkz: [güvenlik bilgisi güvenlik sorularını kullanacak şekilde](security-info-setup-questions.md) makalesi.
    
    >[!Note]
    >Bu seçeneklerden bazısı eksikse, kuruluşunuz bu yöntemleri izin vermeyen büyük olasılıkla olmasıdır. Bu durumda, kullanılabilir bir yöntem seçin veya daha fazla yardım için yöneticinize başvurun gerekecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Kaybolur veya, gelen unutulabilir, parolanızı sıfırlayamıyoruz [parola sıfırlama portalı](https://passwordreset.microsoftonline.com/) veya adımları [iş veya Okul parolanızı sıfırlama](user-help-reset-password.md) makalesi.

- İpuçları ve Yardım almak için oturum açma sorunlarını giderme alma [Microsoft hesabınızda oturum açamazsınız](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) makalesi.