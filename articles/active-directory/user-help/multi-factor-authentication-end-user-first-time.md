---
title: İki aşamalı doğrulama - Azure Active Directory'yi ayarlama | Microsoft Docs
description: Şirket, Azure multi-Factor Authentication yapılandırdığında, iki aşamalı doğrulama için kaydolun istenir. Bu örneği ayarlamanız öğrenin.
services: active-directory
keywords: Azure dizini, active directory bulutta, active directory öğreticisini kullanma
author: eross-msft
manager: daveba
ms.reviewer: richagi
ms.assetid: 46f83a6a-dbdd-4375-8dc4-e7ea77c16357
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2df72d03bae8987de4998276a0be0f3ce1ec0333
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230051"
---
# <a name="set-up-my-account-for-two-step-verification"></a>Hesabım için iki aşamalı doğrulama ayarlayın
İki aşamalı doğrulama, kesmek diğer kişiler için daha zor hale getirerek hesabınızın korunmasına yardımcı olan bir ek güvenlik adımdır. Bu makalede okuyorsanız, büyük olasılıkla bir e-posta iş veya Okul yöneticinizin multi-Factor Authentication hakkında aldığınız. Veya belki de oturum açmaya ve ek güvenlik doğrulama ayarlayın isteyen bir ileti alındı. Bu durum söz konusuysa **otomatik kayıt işlemini tamamlayıncaya kadar oturum açamazsınız**.

Bu makale, ayarladığınız yardımcı olur, **iş veya Okul hesabı**. Kendi kişisel Microsoft hesabı için iki aşamalı doğrulamayı etkinleştirmek istiyorsanız, bkz. [iki basamaklı doğrulama hakkında](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="set-up-your-account"></a>Hesabınızı ayarlayın

Şirketinizin destek birimi, iki aşamalı doğrulamayı kullanmaya başlamak gerektirdiğinde bildiren bir ekran görürsünüz **yöneticiniz bu hesabı ek güvenlik doğrulaması için ayarlamanızı zorunlu kıldı**:

![Kurulum](./media/multi-factor-authentication-end-user-first-time/first.png)

Başlamak için seçin **şimdi ayarlayın.**

Oturum açtığınızda böyle bir ekran görmüyorsanız bölümündeki yönergeleri izleyin [iki adımlı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page) doğrulama seçeneklerinizi yönetebileceğiniz ayarları sayfasını bulun.

## <a name="decide-how-you-want-to-verify-your-sign-ins"></a>Nasıl oturum açma işlemlerinin doğrulamak istediğinize karar verin

İlk soru kayıt işleminin nasıl sizinle iletişim kurmamızı istiyorsanız ' dir. Tablodaki seçenekleri göz atın ve her yöntem için kurulum adımları gitmek için bağlantıları kullanın.

| İletişim yöntemi | Açıklama |
| --- | --- |
| [Mobil uygulama](#use-a-mobile-app-as-the-contact-method) |- **Doğrulama bildirimleri Al.** Bu seçenek, akıllı telefonundaki veya tabletindeki authenticator uygulamasına bir bildirim iter. Bildirimi görüntülemeniz ve işlem meşru ise seçin **doğrulaması** uygulama. İş yeriniz veya okulunuz, kimliğinizi doğrulamak için önce bir PIN girmesini gerektirebilir.<br>- **Doğrulama kodu kullanın.** Bu modda, kimlik doğrulayıcı uygulamasını güncelleştiren her 30 saniyede bir doğrulama kodu oluşturur. Oturum açma arabirimine en geçerli doğrulama kodunu girin.<br>Microsoft Authenticator uygulamasını kullanılabilir [Android](https://go.microsoft.com/fwlink/?linkid=866594) ve [iOS](https://go.microsoft.com/fwlink/?linkid=866594).|
| [Cep telefonu çağrı veya kısa mesaj](#use-your-mobile-phone-as-the-contact-method) |- **Telefon araması** otomatik bir sesli çağrıyla belirttiğiniz telefon numarasına yerleştirir. Aramayı yanıtlamalı ve telefon tuş kimliğini doğrulamak için # tuşuna basın.<br>- **SMS mesajı** bir doğrulama kodu içeren bir kısa mesaj sona erer. Metin satırında, aşağıdaki mesaj Yanıtla veya oturum açma arabirimine sağlanan doğrulama kodunu girin. |
| [Ofis telefonu çağrısı](#use-your-office-phone-as-the-contact-method) |Otomatik bir sesli çağrıyla belirttiğiniz telefon numarasına yerleştirir. Aramayı yanıtlamalı ve telefon tuş kimliğini doğrulamak için # tuşuna basar. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>İletişim yönteminiz olarak mobil uygulama kullan
Bu yöntemi kullanarak, telefon veya tablette bir doğrulayıcı uygulaması yüklemeniz gerekir. Bu makaledeki adımları için kullanılabilir olan Microsoft Authenticator uygulamasını dayalı [Android](https://go.microsoft.com/fwlink/?Linkid=825072) ve [iOS](https://go.microsoft.com/fwlink/?Linkid=825073).

>[!NOTE]
>Microsoft Authenticator uygulamasını kullanmak zorunda değilsiniz. Başka bir kimlik doğrulayıcı uygulamasını kullanıyorsanız, bunu kullanmaya devam edebilirsiniz.

1. Seçin **mobil uygulama** aşağı açılan listeden.
2. Şunlardan birini seçin **doğrulama bildirimleri Al** veya **doğrulama kodunu kullan**, ardından **ayarlanan**.

   ![Ek güvenlik doğrulama ekranı](./media/multi-factor-authentication-end-user-first-time/mobileapp.png)

3. Telefon veya tablet, uygulamayı açıp seçin **+** hesap eklemek için. (Android cihazlarda, üç noktayı seçip **Hesap Ekle**.)
4. Bir iş veya Okul hesabı eklemek istediğinizi belirtin. QR kodu tarayıcısını telefonunuzda açılır. Kameranız düzgün çalışmıyorsa, şirketinizin bilgilerini el ile girmek için seçebilirsiniz. Daha fazla bilgi için [hesap el ile Ekle](#add-an-account-manually).  
5. Mobil uygulama yapılandırma ekran ile görünen QR kodu resmini tarayın.  Seçin **Bitti** QR kodu ekranı kapatmak için.  

   ![QR kodu ekranı](./media/multi-factor-authentication-end-user-first-time/scan2.png)

6. Telefonda etkinleştirme sona erdiğinde seçin **benimle iletişim kurun**.  Bu adım, telefonunuza bir bildirim ya da bir doğrulama kodu gönderir. Seçin **doğrulayın**.  
7. Şirketiniz, oturum açma doğrulama onaylamak için bir PIN gerektiriyorsa, bunu girin.

   ![PIN girmek için kutusu](./media/multi-factor-authentication-end-user-first-time/scan3.png)

8. PIN girişi tamamlandıktan sonra seçin **Kapat**. Bu noktada Doğrulamanızın başarılı olması.
9. Mobil uygulamanıza erişimi kaybetmeniz durumunda cep telefonu numaranızı girmeniz önerilir. Aşağı açılan listeden ülkenizi/bölgenizi belirtin ve ülke/bölge adının yanındaki kutuya cep telefonu numaranızı girin. **İleri**’yi seçin.
10. Bu noktada, Outlook 2010 gibi eski veya tarayıcı olmayan uygulamalar için uygulama parolaları veya Apple cihazlarda yerel e-posta uygulaması ayarlamanız istenir. Bazı uygulamalar, iki aşamalı doğrulamayı desteklemeyen olmasıdır. Bu uygulamaları kullanmıyorsanız tıklayın **Bitti** ve kalan adımları atlayın.
11. Bu uygulamaları kullanıyorsanız, kopya uygulama parolasını sağlanan ve uygulamanızın normal parolanız yerine yapıştırın. Aynı uygulama parolasını birden fazla uygulama için kullanabilirsiniz. Daha fazla bilgi için [yardımcı uygulama parolalarıyla].
12. **Bitti**’ye tıklayın.

### <a name="add-an-account-manually"></a>Hesap el ile Ekle
QR reader'ı kullanarak yerine el ile hesap mobil uygulamaya eklemek istiyorsanız aşağıdaki adımları izleyin.

1. Seçin **hesabını el ile girin** düğmesi.  
2. Kod ve barkod gösteren aynı sayfada sağlanan URL'yi girin. Bu bilgiler gelecek **kod** ve **URL** mobil uygulamayı kutular.

    ![Kurulum](./media/multi-factor-authentication-end-user-first-time/barcode2.png)
3. Etkinleştirme sona erdiğinde seçin **benimle iletişim kurun**. Bu adım, telefonunuza bir bildirim ya da bir doğrulama kodu gönderir. Seçin **doğrulayın**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>İletişim yönteminiz olarak cep telefonunuzu kullanın
1. Seçin **kimlik doğrulama telefonu** aşağı açılan listeden.  

    ![Kurulum](./media/multi-factor-authentication-end-user-first-time/phone.png)  
2. Aşağı açılan listeden ülkenizi/bölgenizi seçin ve cep telefonu numaranızı girin.
3. Cep telefonunuz ile - metin veya çağrı kullanmayı tercih ettiğiniz yöntemi seçin.
4. Seçin **benimle iletişim kurun** telefon numaranızı doğrulamak için. Seçtiğiniz moda bağlı olarak bir metin iletisi veya çağırırsınız. Ekrandaki yönergeleri izleyin ve ardından **doğrulama**.
5. Bu noktada, Outlook 2010 gibi eski veya tarayıcı olmayan uygulamalar için uygulama parolaları veya Apple cihazlarda yerel e-posta uygulaması ayarlamanız istenir. Bazı uygulamalar, iki aşamalı doğrulamayı desteklemeyen olmasıdır. Bu uygulamaları kullanmıyorsanız tıklayın **Bitti** ve kalan adımları atlayın.
6. Bu uygulamaları kullanıyorsanız, kopya uygulama parolasını sağlanan ve uygulamanızın normal parolanız yerine yapıştırın. Aynı uygulama parolasını birden fazla uygulama için kullanabilirsiniz. Daha fazla bilgi için [yardımcı uygulama parolalarıyla].
7. **Bitti**’ye tıklayın.

## <a name="use-your-office-phone-as-the-contact-method"></a>İletişim yönteminiz olarak ofis telefonunuzu kullanın
1. Seçin **ofis telefonu** açılır listeden  

    ![Kurulum](./media/multi-factor-authentication-end-user-first-time/office.png)  
2. Telefon numarası kutusu ile şirket iletişim bilgilerini otomatik olarak doldurulur. Numarası eksik veya yanlış ise, değişiklik yapmak için yöneticinize başvurun.
3. Seçin **benimle iletişim kurun** telefonunuzu doğrulayın ve sayı numaranızı çağırır. Ekrandaki yönergeleri izleyin ve ardından **doğrulama**.
4. Bu noktada, Outlook 2010 gibi eski veya tarayıcı olmayan uygulamalar için uygulama parolaları veya Apple cihazlarda yerel e-posta uygulaması ayarlamanız istenir. Bazı uygulamalar, iki aşamalı doğrulamayı desteklemeyen olmasıdır. Bu uygulamaları kullanmıyorsanız tıklayın **Bitti** ve kalan adımları atlayın.
5. Bu uygulamaları kullanıyorsanız, kopya uygulama parolasını sağlanan ve uygulamanızın normal parolanız yerine yapıştırın. Aynı uygulama parolasını birden fazla uygulama için kullanabilirsiniz. Daha fazla bilgi için bkz. [uygulama parolaları nedir](multi-factor-authentication-end-user-app-passwords.md).
6. **Bitti**’ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* Tercih edilen seçeneklerinizi değiştirin ve [iki adımlı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)
* Ayarlanan [uygulama parolaları](multi-factor-authentication-end-user-app-passwords.md) iki aşamalı doğrulamayı desteklemeyen yerel cihaz uygulamaları için.
* Kullanıma [Microsoft Authenticator uygulamasını](user-help-auth-app-download-install.md) bile hücre hizmeti olmadığında hızlı ve güvenli kimlik doğrulaması.
