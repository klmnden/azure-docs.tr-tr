---
title: Microsoft Authenticator uygulamasına - Azure Active Directory, Microsoft olmayan hesapların ekleyin | Microsoft Docs
description: Microsoft olmayan hesapların gibi Google, Facebook veya GitHub için iki aşamalı doğrulama için Microsoft Authenticator uygulamasını ekleme.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: lizross
ms.reviewer: olhaun
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6f94ba30c06fc6975ab212c895cecefe5d383fa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60473915"
---
# <a name="add-your-non-microsoft-accounts"></a>Microsoft olmayan hesapların Ekle
Microsoft olmayan hesapların gibi Google, Facebook veya GitHub için iki aşamalı doğrulama için Microsoft Authenticator uygulaması ekleyin. Microsoft Authenticator uygulamasını iki aşamalı doğrulama kullanan tüm uygulamalarda ve zamana bağlı bir kerelik parola (TOTP) standartlarını destekleyen herhangi bir hesabı ile çalışır.

>[!Important]
>Hesabınızı ekleyebilmeniz için önce indirin ve Microsoft Authenticator uygulamasını yüklemeniz gerekir. Henüz yapmadıysanız adımları [uygulamayı yükleyip](user-help-auth-app-download-install.md) makalesi.

## <a name="add-personal-accounts"></a>Kişisel hesapları ekleme
Genel olarak, tüm kişisel hesaplarınız için aşağıdakileri yapmalısınız:

1. Hesabınızda oturum açın ve ardından, cihaz veya bilgisayarınıza kullanarak iki aşamalı doğrulamayı açmak.

2. Hesabı, Microsoft Authenticator uygulamasına ekleyin. Bu işlemin bir parçası olarak bir QR kodu tarama istenebilir.

Facebook, Google, GitHub ve Amazon hesaplarınız için işlemi burada sağlayarak, ancak bu işlem Instagram, Netflix veya Adobe gibi diğer tüm uygulamalar için aynıdır.

## <a name="add-your-google-account"></a>Google hesabınızı ekleyin
İki aşamalı doğrulamayı açmak ve ardından hesap uygulamaya ekleyerek Google hesabınızı ekleyin.

### <a name="turn-on-two-factor-verification"></a>İki aşamalı doğrulamayı açmak

1. Bilgisayarınızda Git https://myaccount.google.com/signinoptions/two-step-verification/enroll-welcomeseçin **Başlarken**ve ardından kimlik bilgilerinizi doğrulayın.

2. Kişisel Google hesabınız için iki aşamalı doğrulamayı açmak için sayfadaki adımları izleyin.

### <a name="add-your-google-account-to-the-app"></a>Google hesabınız uygulamaya ekleme

1. Google sayfasında bilgisayarınızda Git **alternatif ikinci adım kümesi** bölümünde, seçin **ayarlamak** gelen **Authenticator uygulamasını** bölümü.

2. Üzerinde **Authenticator uygulamasından kodu alma** sayfasında **Android** veya **iPhone** telefon türüne göre ve ardından **sonraki**.

    Size hesabınızı Microsoft Authenticator uygulamasını ile otomatik olarak ilişkilendirmek için kullanabileceğiniz bir QR kodu verilir. Bu pencereyi kapatmayın.

3. Microsoft Authenticator uygulamasını açın, **Hesap Ekle** gelen **özelleştirme ve Denetim** simgesini sağ tıklatıp seçin **başka bir hesap (Google, Facebook, vb.)** .

4. QR kodu taraması için cihazınızın kamerasının kullanın **Authenticator kümesi** bilgisayarınızda sayfası.

    >[!Note]
    >Kameranız düzgün çalışmıyorsa, QR kodu ve URL'yi elle girebilirsiniz.

5. Gözden geçirme **hesapları** hesap bilgilerinizi emin olmak için Cihazınızda Microsoft Authenticator uygulamasının sayfa, sağ ve ilişkili altı basamaklı doğrulama kodu yok.

    Ek güvenlik için her 30 saniyede birisi bir kod birden çok kez tüketmesini doğrulama kodunu değiştirir.

6. Seçin **sonraki** üzerinde **Authenticator kümesi** sayfasında bilgisayarınızda uygulamada Google hesabınız için sağlanan altı basamaklı doğrulama kodunu yazın ve ardından **doğrulama**.

7. Hesabınız doğrulandı ve seçebileceğiniz **Bitti** kapatmak için **Authenticator kümesi** sayfası.

    >[!NOTE]
    >Hakkında daha fazla bilgi için bkz: iki aşamalı doğrulama ve Google hesabınızı [2 adımlı doğrulamayı açmak](https://support.google.com/accounts/answer/185839) ve [2 adımlı doğrulama hakkında daha fazla bilgi](https://www.google.com/landing/2step/help.html).

## <a name="add-your-facebook-account"></a>Facebook hesabınızı ekleyin
İki aşamalı doğrulamayı açmak ve ardından hesap uygulamaya ekleyerek Facebook hesabınızı ekleyin.

### <a name="turn-on-two-factor-verification"></a>İki aşamalı doğrulamayı açmak

1. Bilgisayarınızda, Facebook açın, sağ üst köşedeki aşağı açılan menüyü seçin ve ardından Git **ayarları** > **güvenlik ve oturum açma**.

    **Güvenlik ve oturum açma** sayfası görüntülenir.

2. Gidin **iki öğeli kimlik doğrulamasını kullan** seçeneğini **iki öğeli kimlik doğrulama** bölümüne ve ardından **Düzenle**.

    **İki öğeli kimlik doğrulama** sayfası görüntülenir.

3. Seçin **açma**.

### <a name="add-your-facebook-account-to-the-app"></a>Uygulamaya Facebook hesabınızı ekleyin

1. Facebook sayfasında bilgisayarınızda Git **bir yedekleme eklemek** bölümüne ve ardından **Kurulum** gelen **kimlik doğrulama uygulaması** alan.

    Size hesabınızı Microsoft Authenticator uygulamasını ile otomatik olarak ilişkilendirmek için kullanabileceğiniz bir QR kodu verilir. Bu pencereyi kapatmayın.

2. Microsoft Authenticator uygulamasını açın, **Hesap Ekle** gelen **özelleştirme ve Denetim** simgesini sağ tıklatıp seçin **başka bir hesap (Google, Facebook, vb.)** .

3. QR kodu taraması için cihazınızın kamerasının kullanın **iki öğeli kimlik doğrulamasını** bilgisayarınızda sayfası.

    >[!Note]
    >Kameranız düzgün çalışmıyorsa, QR kodu ve URL'yi elle girebilirsiniz.

4. Gözden geçirme **hesapları** hesap bilgilerinizi emin olmak için Cihazınızda Microsoft Authenticator uygulamasının sayfa, sağ ve ilişkili altı basamaklı doğrulama kodu yok.

    Ek güvenlik için her 30 saniyede birisi bir kod birden çok kez tüketmesini doğrulama kodunu değiştirir.

5. Seçin **sonraki** üzerinde **iki öğeli kimlik doğrulamasını** sayfasında bilgisayarınızda ve Facebook hesabınız için uygulamada sağlanan altı basamaklı doğrulama kodunu yazın.

    Hesabınız doğrulandı ve hesabınızı doğrulamak için uygulamayı artık kullanabilirsiniz.

    >[!NOTE]
    >Hakkında daha fazla bilgi için bkz: iki aşamalı doğrulama ve Facebook hesabınıza [iki öğeli kimlik doğrulama nedir ve nasıl onu çalışır?](https://www.facebook.com/help/148233965247823).

## <a name="add-your-github-account"></a>GitHub hesabınızı ekleyin
İki aşamalı doğrulamayı açmak ve ardından hesap uygulamaya ekleyerek GitHub hesabınızı ekleyin.

### <a name="turn-on-two-factor-verification"></a>İki aşamalı doğrulamayı açmak

1. Bilgisayarınızda, GitHub açın, sağ üst köşeden görüntünüzü seçin ve ardından **ayarları**.

    **İki öğeli kimlik doğrulama** sayfası görüntülenir.

2. Seçin **güvenlik** gelen **kişisel ayarlarını** kenar tıklayın ve ardından **iki öğeli kimlik doğrulamasını etkinleştirme** gelen **iki öğeli kimlik doğrulama**  alan.

### <a name="add-your-github-account-to-the-app"></a>GitHub hesabınızı uygulamaya ekleme

1. Üzerinde **iki öğeli kimlik doğrulama** bilgisayarı seçin sayfasında **bir uygulama kullanarak**.

2. Erişimlerini ve ardından, hesabınızı geri alabilmeniz için kurtarma kodlarınızı kaydetmek **sonraki**. 

    Kodlarınızı, cihazınıza indirerek basılı kopya yazdırma veya bir parola Yöneticisi aracına kopyalayarak kaydedebilirsiniz.

3. Üzerinde **iki öğeli kimlik doğrulama** sayfasında **bir uygulama kullanarak**.

    QR kodu göstermek için sayfa değişiklikleri. Bu sayfayı kapatmayın.

4. Microsoft Authenticator uygulamasını açın, **Hesap Ekle** gelen **özelleştirme ve Denetim** seçme simgesine sağ üst kısımdaki **başka bir hesap (Google, Facebook, vb.)** ve ardından **bu metin kod girin** sayfanın üstündeki metin.

    Microsoft Authenticator uygulamasını kodu el ile girmeniz gerekir böylece QR kodunu tarama silemiyor.

5. Girin bir **hesap adı** (örneğin GitHub) yazın **gizli anahtar** 4. adım ve ardından **son**.

4. Üzerinde **iki öğeli kimlik doğrulayıcı** sayfasında bilgisayarınızda GitHub hesabınız için uygulamada sağlanan altı basamaklı doğrulama kodunu yazın ve ardından **etkinleştirme**.

    **Hesapları** uygulamasının sayfası gösterilir, hesabınızın adını ve altı basamaklı doğrulama kodu. Ek güvenlik için her 30 saniyede birisi bir kod birden çok kez tüketmesini doğrulama kodunu değiştirir.

    >[!NOTE]
    >Hakkında daha fazla bilgi için bkz: iki aşamalı doğrulama ve GitHub hesabınızı [yaklaşık iki öğeli kimlik doğrulama](https://help.github.com/articles/about-two-factor-authentication/).

## <a name="add-your-amazon-account"></a>Amazon hesabınızı ekleyin
İki aşamalı doğrulamayı açmak ve ardından hesap uygulamaya ekleyerek Amazon hesabınızı ekleyin.

### <a name="turn-on-two-factor-verification"></a>İki aşamalı doğrulamayı açmak

1. Bilgisayarınızda açık Amazon seçin **hesabı & listeleri** öğelerine tıklayın ve ardından **hesabınızı**.

2. Seçin **oturum açma ve güvenlik**Amazon hesabınızda oturum açın ve ardından **Düzenle** içinde **Gelişmiş güvenlik ayarları** alan.

    **Gelişmiş güvenlik ayarları** sayfası görüntülenir.

3. Seçin **başlama**.

4. Seçin **Authenticator uygulamasını** gelen **kodları nasıl alırsınız seçin** sayfası.

    QR kodu göstermek için sayfa değişiklikleri. Bu sayfayı kapatmayın.

5. Microsoft Authenticator uygulamasını açın, **Hesap Ekle** gelen **özelleştirme ve Denetim** simgesini sağ tıklatıp seçin **başka bir hesap (Google, Facebook, vb.)** .

6. QR kodu taraması için cihazınızın kamerasının kullanın **kodları nasıl alırsınız seçin** bilgisayarınızda sayfası.

    >[!Note]
    >Kameranız düzgün çalışmıyorsa, QR kodu ve URL'yi elle girebilirsiniz.

5. Gözden geçirme **hesapları** hesap bilgilerinizi emin olmak için Cihazınızda Microsoft Authenticator uygulamasının sayfa, sağ ve ilişkili altı basamaklı doğrulama kodu yok.

    Ek güvenlik için her 30 saniyede birisi bir kod birden çok kez tüketmesini doğrulama kodunu değiştirir.

6. Üzerinde **kodları nasıl alırsınız seçin** sayfasında bilgisayarınızda Amazon hesabınız için uygulamada sağlanan altı basamaklı doğrulama kodunu yazın ve ardından **kodu doğrulamak ve devam et**.

7. Bir yedekleme doğrulama yöntemi gibi bir kısa mesaj ekleme dahil olmak üzere kayıt işlemini tamamlayın ve ardından **kod Gönder**.

8. Üzerinde **yedekleme doğrulama yöntemini eklemek** sayfasında bilgisayarınızda Amazon hesabınız için yedek doğrulama yönteminizi tarafından sağlanan altı basamaklı doğrulama kodunu yazın ve ardından **kodu doğrulamak vedevamet**.

9. Üzerinde Almost sayfasında, güvenilir bir cihaz bilgisayarınızı getirin ve ardından karar **anladım. İki basamaklı doğrulamayı açmak**.

    **Gelişmiş güvenlik ayarları** sayfası görüntülenirse, güncelleştirilmiş iki Faktörlü doğrulama ayrıntılarınızı gösteriliyor.

    >[!NOTE]
    >Hakkında daha fazla bilgi için iki aşamalı doğrulama ve Amazon hesabınızı [hakkında iki aşamalı doğrulama](https://www.amazon.com/gp/help/customer/display.html?nodeId=201596330) ve [iki aşamalı doğrulaması ile oturum açma](https://www.amazon.com/gp/help/customer/display.html?nodeId=201962440).


## <a name="next-steps"></a>Sonraki adımlar

- Hesaplarınız için uygulamayı ekledikten sonra kimlik doğrulayıcı uygulamasını Cihazınızda kullanarak oturum açabilirsiniz. Daha fazla bilgi için [uygulamayı kullanarak oturum](user-help-auth-app-sign-in.md).

- İOS çalıştıran cihazlar için hesap kimlik bilgilerinizi da yedekleyebilirsiniz ve hesaplarınızı sırası gibi uygulama ayarlarını bulutla ilgili. Daha fazla bilgi için [yedekleme ve Kurtarma Microsoft Authenticator uygulaması ile](user-help-auth-app-backup-recovery.md).
