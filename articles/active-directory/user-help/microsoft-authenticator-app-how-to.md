---
title: Microsoft Authenticator uygulaması - Azure Active Directory ile çalışmaya başlama | Microsoft Docs
description: Microsoft Authenticator'ın en son sürümüne yükseltmeyi öğrenin.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: active-directory
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.reviewer: librown
ms.openlocfilehash: 590f9e2a9cf531a1124f9cb132791f2956227d9c
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048275"
---
# <a name="get-started-with-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması ile çalışmaya başlama

Microsoft Authenticator uygulamasını hesaplarına ve sahte işlemleri çalışmanız için ek bir güvenlik düzeyi vererek durdurmak için yetkisiz erişimi önlemeye yardımcı olmak ya da Okul hesabı (örneğin, alain@contoso.com) veya kişisel Microsoft hesabınızı (için Örneğin, alain@outlook.com).

Uygulama için iki aşamalı doğrulamayı kullanırken, iki yoldan biriyle çalışabilirsiniz:

- **Bildirim.** Uygulama, cihazınıza bir bildirim gönderir. Bildirim, sağ ve ardından olduğundan emin olun **doğrulama**. Bildirim tanımıyorsanız seçin **Reddet**.

- **Doğrulama kodu.** Kullanıcı kimliğiniz ve parolanızı girdikten sonra uygulamayı açıp sağlanan doğrulama kodu kopyalayın **hesapları** ekran oturum açma ekranı açın. Doğrulama kodu, ikinci bir kimlik doğrulama biçimi görev yapar.

## <a name="opt-in-for-two-step-verification"></a>İki aşamalı doğrulama için kabul et

Kuruluşunuz iş veya Okul hesabınızla iki aşamalı doğrulama kullanıp kullanmadığını belirler. Yöneticiniz size ayarlama ve kullanılan gereken hangi doğrulama yöntemleri sağlar. Daha fazla bilgi için [Azure multi-Factor Authentication benim için ne demektir](multi-factor-authentication-end-user.md).

Kişisel Microsoft hesabınız için iki aşamalı doğrulama için kendiniz ayarlayabilirsiniz. Daha ayrıntılı bilgi ve yönergeler için bkz. [iki basamaklı doğrulama hakkında](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

Bu gibi durumlarda, Microsoft Authenticator uygulamasını da Microsoft olmayan hesapların ile kullanabilirsiniz. Söz konusu hesaplar özellik iki aşamalı doğrulamanın dışında bir çağrı ancak güvenlik veya oturum açma ayarları içinde bulabilirsiniz olmalıdır. Bu Microsoft dışı hesaplarını ayarlama hakkında daha fazla bilgi için bkz. [Microsoft müşteri desteği videoları](https://www.youtube.com/playlist?list=PLyhj1WZ29G65QdD9NxTOAm8HwOS-OBUrX).

## <a name="install-the-app"></a>Uygulama yükleme

Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) ve [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)'da kullanılabilir. En iyi deneyimi elde etmek için istendiğinde bildirim alma uygulama izin vermelisiniz. 

## <a name="add-accounts-to-the-app"></a>Uygulamaya hesapları ekleme

Microsoft Authenticator uygulaması için iş veya Okul hesapları veya kişisel hesaplar ekleyebilirsiniz. 

### <a name="add-a-personal-microsoft-account"></a>Kişisel Microsoft hesabı ekleme

Kişisel Microsoft hesabı için (bir Outlook.com, Xbox, Skype, vb. için oturum açmak için kullandığınız), tüm yapmanız gereken olan Microsoft Authenticator uygulaması hesabınızda oturum açın.

### <a name="add-a-work-or-school-account"></a>Bir iş veya Okul hesabı Ekle

1. Mümkünse, Git [ek güvenlik doğrulaması](http://aka.ms/mfasetup) ekranda başka bir bilgisayar veya cihaz. Bu ekrana alma hakkında daha fazla bilgi için bkz: [güvenlik ayarlarınızı değiştirme](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page) veya yöneticinize başvurun.

    >[!Note]
    >Yöneticiniz güvenlik bilgisi Önizleme deneyimi kapattıysa yönergeleri izleyebilirsiniz [authenticator uygulamasını kullanmak için güvenlik bilgileri ' ayarlamak](security-info-setup-auth-app.md) bölümü.

2. Yanındaki kutuyu işaretleyin **Authenticator uygulamasını**ve ardından **yapılandırma**.

    ![Güvenlik doğrulama ayarları ekranında Yapılandır düğmesi](./media/microsoft-authenticator-app-how-to/auth-app-configure.png)

    **Mobil uygulama yapılandırma** ekran, kimlik doğrulayıcı uygulamayla taramak QR kodu görüntülenir.

    ![QR kodunu sağlayan ekranı](./media/microsoft-authenticator-app-how-to/auth-app-barcode.png)

3. Microsoft Authenticator uygulamasını açın. Üzerinde **hesapları** ekranındayken **Hesap Ekle**ve ardından **iş veya Okul hesabı**.

4. QR kodunu tarayın ve ardından cihazınızın kamerasının kullanın **Bitti** QR kodu ekranı kapatmak için.

    >[!Note]
    >Kameranız düzgün çalışmayan varsa [QR kodu ve URL'yi el ile girmek](#add-an-account-to-the-app-manually).

    **Hesapları** ekranında gösterir, hesabınızın adını ve altı basamaklı doğrulama kodu. Ek güvenlik için her 30 saniyede, aynı kod iki kez tüketmesini doğrulama kodunu değiştirir.  

    ![Hesaplar ekranı](./media/microsoft-authenticator-app-how-to/auth-app-accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Hesap için uygulamayı el ile Ekle

1. Git **ek güvenlik doğrulaması** ekran. Bu ekrana alma hakkında daha fazla bilgi için bkz: [güvenlik ayarlarınızı değiştirme](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).

2. Yanındaki kutuyu işaretleyin **Authenticator uygulamasını**ve ardından **yapılandırma**.

    **Mobil uygulama yapılandırma** ekranı görüntülenir.

3. Kodu ve URL bilgileri Kopyala **mobil uygulama yapılandırma** bunları elle QR tarayıcısını girebilmeniz için ekran.

4. Microsoft Authenticator uygulamasını açın. Üzerinde **hesapları** ekranındayken **Hesap Ekle**ve ardından **iş veya Okul hesabı**.

5. QR tarayıcı penceresinde seçtiğinizden **kodu el ile girmeniz**.

    ![Ekran için QR kodu tarama](./media/microsoft-authenticator-app-how-to/auth-app-manual-code.png)
   
6. QR kodu ile ekranından kodu ve URL'yi yazın **Hesap Ekle** ekran ve ardından **son**.

    ![Kodu ve URL'yi girme ekran](./media/microsoft-authenticator-app-how-to/auth-app-code-url.png)

    **Hesapları** ekranında gösterir, hesabınızın adını ve altı basamaklı doğrulama kodu. Ek güvenlik için her 30 saniyede, aynı kod iki kez tüketmesini doğrulama kodunu değiştirir.

### <a name="using-your-devices-fingerprint-or-facial-recognition-capabilities"></a>Cihazınızın parmak izi veya yüz tanıma özelliklerini kullanma

Kuruluşunuz Kimlik doğrulamanızı tamamlamak bir PIN gerektirebilir. PIN yerine cihazınızın parmak izi veya yüz tanıma özelliklerini kullanmak için Microsoft Authenticator uygulamasını ayarlayabilirsiniz. Bu ilk defa ayarlayabilirsiniz authenticator uygulamasını cihaz biyometrik yeteneklerinizi PIN'iniz yerine kimlik olarak kullanmak için seçeneği belirleyerek hesabınızı doğrulamak için kullanın.

## <a name="use-the-app-when-you-sign-in"></a>Oturum açtığınızda, uygulama kullanma

Hesaplarınız için uygulamayı ekledikten sonra hesaplarınıza oturum açmak için uygulamayı kullanabilirsiniz.

Uygulama doğrulama kodları kullanmayı seçerseniz, bunları görmek başlayacaksınız **hesapları** sayfası. Böylece, ihtiyacınız olduğunda yeni bir kod her zaman sahip kodları her 30 saniyede değiştirin. Ancak oturum açın ve bir doğrulama kodu girmeniz istenir kadar bunları ile herhangi bir şey yapmanız gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

- Uygulama hakkında daha fazla genel sorularınız varsa bkz [Microsoft Authenticator ile ilgili SSS](microsoft-authenticator-app-faq.md)

- İki aşamalı doğrulama hakkında daha fazla bilgi istiyorsanız bkz [hesabım için iki aşamalı doğrulama ayarlayın](multi-factor-authentication-end-user-first-time.md)

- Güvenlik bilgileri hakkında daha fazla bilgi istiyorsanız bkz [güvenlik bilgilerinizi yönetin](security-info-manage-settings.md)
