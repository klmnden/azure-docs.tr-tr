---
title: İş veya Okul hesabım için iki aşamalı doğrulamayı ayarlama | Microsoft Docs
description: 'Şirketiniz Azure multi-Factor Authentication yapılandırdığında, iki aşamalı doğrulama için kaydolmak için istenir. Bunu nasıl ayarlanacağını öğrenin. '
services: multi-factor-authentication
keywords: Azure dizini, active directory bulutta, active directory öğretici kullanma
documentationcenter: ''
author: barlanmsft
manager: mtillman
ms.reviewer: richagi
ms.assetid: 46f83a6a-dbdd-4375-8dc4-e7ea77c16357
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: 04b8d2b8d7d84bd4c6b46507be5d597c03d9dbb0
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
ms.locfileid: "28198364"
---
# <a name="set-up-my-account-for-two-step-verification"></a>Hesabımı iki aşamalı doğrulama için ayarlama
İki aşamalı doğrulamayı geçirmesini diğer kişiler için daha zor hale getirerek hesabınızın korunmasına yardımcı olan bir ek güvenlik adımdır. Bu makaleyi okuduğunuz, büyük olasılıkla bir e-posta yöneticinizden iş veya Okul çok faktörlü kimlik doğrulaması hakkında aldığınız. Veya belki de oturum açmaya ve ek güvenlik doğrulaması ayarlamak için isteyen bir ileti aldı. Bu durumda, olursa **otomatik kayıt işlemini tamamlayıncaya kadar oturum açamazsınız**.

Bu makale, ayarladığınız yardımcı olur, **iş veya Okul hesabı**. Kendi kişisel Microsoft hesabı için iki aşamalı doğrulamayı etkinleştirmek istiyorsanız, bkz: [iki basamaklı doğrulama hakkında](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="set-up-your-account"></a>Hesabınızı ayarlama

Şirket destek iki aşamalı doğrulamayı kullanmaya başlamak gerektirdiğinde bildiren bir ekran görürsünüz **yöneticiniz bu hesabı ek güvenlik doğrulaması için ayarlamanızı zorunlu kıldı**:

![Kurulum](./media/multi-factor-authentication-end-user-first-time/first.png)

Başlamak için seçin **şimdi ayarlayın.**

Oturum açtığınızda bu gibi bir ekran görmüyorsanız'ndaki yönergeleri izleyin [iki aşamalı doğrulama için ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page) doğrulama seçeneklerinizi yönetebileceğiniz ayarları sayfasını bulun.

## <a name="decide-how-you-want-to-verify-your-sign-ins"></a>Oturum açma işlemleri doğrulamak istediğiniz nasıl karar verin

Kayıt işlemini ilk soruya sizinle iletişim kurmamızı nasıl istediğiniz sayıdır. Tablodaki seçenekleri göz atın ve her bir yöntemi için kurulum adımlarını gitmek için bağlantıları kullanın.

| İletişim yöntemi | Açıklama |
| --- | --- |
| [Mobil uygulama](#use-a-mobile-app-as-the-contact-method) |- **Doğrulama için bildirim alırsınız.** Bu seçenek, akıllı telefon veya tablet authenticator uygulaması için bir bildirim iter. Bildirimi görüntülemeniz ve işlem meşru ise seçin **kimlik doğrulama** uygulama. İş veya Okul kimlik doğrulaması yapmak için önce bir PIN girmeniz gerekebilir.<br>- **Doğrulama kodunu kullanın.** Bu modda, Doğrulayıcı uygulama güncelleştirmeleri her 30 saniyede bir doğrulama kodu oluşturur. Oturum açma arabiriminde en güncel doğrulama kodunu girin.<br>Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594), ve [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071). |
| [Cep telefonu araması veya SMS](#use-your-mobile-phone-as-the-contact-method) |- **Telefon araması** otomatik bir sesli aramayla sağladığınız telefon numarasına yerleştirir. Aramayı yanıtlayın ve kimlik doğrulaması için telefon tuş # tuşuna basın.<br>- **SMS mesajı** bir doğrulama kodu içeren bir kısa mesaj sona erer. Metin satırında, aşağıdaki SMS mesajını yanıtlayın veya oturum açma arabirimine sağlanan doğrulama kodunu girin. |
| [Ofis telefonu araması](#use-your-office-phone-as-the-contact-method) |Otomatik bir sesli aramayla sağladığınız telefon numarasına yerleştirir. Aramayı yanıtlayın ve # kimliğini doğrulamak için telefon basar. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>İletişim yönteminiz olarak bir mobil uygulama kullanma
Bu yöntemi kullanarak, telefon veya Tabletinizi Doğrulayıcı uygulama yüklemenizi gerektirir. Bu makaledeki adımları için kullanılabilir Microsoft Authenticator uygulamasını esas alan [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), ve [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

1. Seçin **mobil uygulama** aşağı açılan listeden.
2. Şunlardan birini seçin **doğrulama için bildirimlerin** veya **doğrulama kodunu kullan**seçeneğini belirleyip **ayarlanan**.

   ![Ek güvenlik doğrulama ekranı](./media/multi-factor-authentication-end-user-first-time/mobileapp.png)

3. Telefonunuzu veya Tabletinizi, uygulamayı açın ve seçin  **+**  hesap eklemek için. (Android cihazlarda, üç noktaya seçip **hesabı eklemek**.)
4. Bir iş veya Okul hesabı eklemek istediğinizi belirtin. Telefonunuzda QR kodu tarayıcı açar. Kameranızı düzgün çalışmıyorsa, şirketinizin bilgilerini el ile girmeyi seçebilirsiniz. Daha fazla bilgi için bkz: [hesap el ile eklemek](#add-an-account-manually).  
5. Mobil uygulama yapılandırma ekranlı görünen QR kodu resmini tarayın.  Seçin **Bitti** QR kodu ekranı kapatmak için.  

   ![QR kodu ekranı](./media/multi-factor-authentication-end-user-first-time/scan2.png)

6. Telefonda etkinleştirme sona erdiğinde, seçin **kişi benim**.  Bu adım, telefonunuza bir bildirim veya bir doğrulama kodu gönderir. Seçin **doğrulayın**.  
7. Şirketiniz, oturum açma doğrulama onaylamak için bir PIN gerektiriyorsa, bunu girin.

   ![Bir PIN girme kutusu](./media/multi-factor-authentication-end-user-first-time/scan3.png)

8. PIN girişi tamamlandıktan sonra Seç **Kapat**. Bu noktada Doğrulamanızın başarılı olması.
9. Mobil uygulamanıza erişimi kaybetmeniz durumunda cep telefonu numaranızı girin öneririz. Aşağı açılan listeden ülkenizi belirtin ve ülke adının yanındaki kutuya cep telefonu numaranızı girin. **İleri**’yi seçin.
10. Bu noktada, Outlook 2010 gibi eski veya tarayıcı olmayan uygulamalar için uygulama parolaları veya Apple cihazlarda yerel e-posta uygulamasını ayarlamanız istenir. Bazı uygulamalar iki aşamalı doğrulamayı desteklemeyen olmasıdır. Bu uygulamaları kullanmıyorsanız tıklatın **Bitti** ve kalan adımları atlayın.
11. Bu uygulamalar kullanıyorsanız, uygulama parolası sağlanan kopyalayıp normal parolanız yerine uygulamanıza yapıştırın. Aynı uygulama parolasını birden çok uygulamaları için kullanabilirsiniz. Daha fazla bilgi için [Yardım ile uygulama parolaları].
12. **Bitti**’ye tıklayın.

### <a name="add-an-account-manually"></a>Hesap el ile Ekle
QR reader'ı kullanarak yerine el ile bir hesap mobil uygulamaya eklemek istiyorsanız aşağıdaki adımları izleyin.

1. Seçin **hesabını el ile girin** düğmesi.  
2. Kod ve barkod gösteren aynı sayfada sağlanan URL'yi girin. Bu bilgileri girer **kod** ve **URL** mobil uygulamayı kutuları.

    ![Kurulum](./media/multi-factor-authentication-end-user-first-time/barcode2.png)
3. Etkinleştirme sona erdiğinde seçin **kişi benim**. Bu adım, telefonunuza bir bildirim veya bir doğrulama kodu gönderir. Seçin **doğrulayın**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>İletişim yönteminiz olarak cep telefonunuza kullanın
1. Seçin **kimlik doğrulama telefon** aşağı açılan listeden.  

    ![Kurulum](./media/multi-factor-authentication-end-user-first-time/phone.png)  
2. Aşağı açılan listeden ülkenizi seçin ve cep telefonu numaranızı girin.
3. Cep telefonunuz ile - metin veya arama kullanmayı tercih ettiğiniz yöntemi seçin.
4. Seçin **kişi benim** telefon numaranızı doğrulamak için. Şeklinde seçtiğiniz moda bağlı olarak bir metin göndermek veya çağırın. Ekrandaki yönergeleri izleyin ve ardından **doğrula**.
5. Bu noktada, Outlook 2010 gibi eski veya tarayıcı olmayan uygulamalar için uygulama parolaları veya Apple cihazlarda yerel e-posta uygulamasını ayarlamanız istenir. Bazı uygulamalar iki aşamalı doğrulamayı desteklemeyen olmasıdır. Bu uygulamaları kullanmıyorsanız tıklatın **Bitti** ve kalan adımları atlayın.
6. Bu uygulamalar kullanıyorsanız, uygulama parolası sağlanan kopyalayıp normal parolanız yerine uygulamanıza yapıştırın. Aynı uygulama parolasını birden çok uygulamaları için kullanabilirsiniz. Daha fazla bilgi için [Yardım ile uygulama parolaları].
7. **Bitti**’ye tıklayın.

## <a name="use-your-office-phone-as-the-contact-method"></a>İletişim yönteminiz olarak ofis telefonu kullanma
1. Seçin **ofis telefonu** açılan gelen  

    ![Kurulum](./media/multi-factor-authentication-end-user-first-time/office.png)  
2. Telefon numarası kutusu şirket iletişim bilgileri ile otomatik olarak doldurulur. Sayı eksik veya yanlış ise, değişiklik yapmak için yöneticinizden isteyin.
3. Seçin **kişi benim** telefonunuz doğrulamak için numarası ve biz numaranızı çağırır. Ekrandaki yönergeleri izleyin ve ardından **doğrula**.
4. Bu noktada, Outlook 2010 gibi eski veya tarayıcı olmayan uygulamalar için uygulama parolaları veya Apple cihazlarda yerel e-posta uygulamasını ayarlamanız istenir. Bazı uygulamalar iki aşamalı doğrulamayı desteklemeyen olmasıdır. Bu uygulamaları kullanmıyorsanız tıklatın **Bitti** ve kalan adımları atlayın.
5. Bu uygulamalar kullanıyorsanız, uygulama parolası sağlanan kopyalayıp normal parolanız yerine uygulamanıza yapıştırın. Aynı uygulama parolasını birden çok uygulamaları için kullanabilirsiniz. Daha fazla bilgi için bkz: [uygulama parolaları nedir](multi-factor-authentication-end-user-app-passwords.md).
6. **Bitti**’ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* Tercih edilen seçeneklerinizi değiştirin ve [iki aşamalı doğrulama için ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)
* Ayarlanan [uygulama parolaları](multi-factor-authentication-end-user-app-passwords.md) iki aşamalı doğrulamayı desteklemeyen yerel cihaz uygulamaları için.
* Kullanıma [Microsoft Authenticator uygulaması](microsoft-authenticator-app-how-to.md) bile hücre hizmeti olmadığında hızlı ve güvenli kimlik doğrulaması.
