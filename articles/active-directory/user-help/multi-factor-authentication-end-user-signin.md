---
title: Azure MFA ile oturum açma iki aşamalı doğrulama | Microsoft Docs
description: Bu sayfa üzerinde çeşitli oturum Azure MFA ile kullanılabilecek yöntemleri görmek için yapılması gerekenler Kılavuzu sağlayacaktır.
keywords: Kullanıcı kimlik doğrulaması, oturum açma deneyimi, cep telefonu ile oturum ofis telefonu ile oturum açın
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/02/2017
ms.author: lizross
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: 2bba40d48b855b2cfa5c607e63a49e2bb79b6b23
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059455"
---
# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication ile oturum açma deneyimi
> [!NOTE]
> Bu makalenin amacı, bir normal oturum açma deneyimi boyunca size yol sağlamaktır. Oturum açma sorunlarını gidermek için veya Yardım için bkz: [Azure multi-Factor Authentication sorununuz](multi-factor-authentication-end-user-troubleshoot.md).

## <a name="what-will-your-sign-in-experience-be"></a>Ne oturum açma deneyiminiz olacaktır?
Oturum açma deneyimini, ikinci bir faktör olarak kullanmak istediğinize bağlı olarak farklılık gösterir: telefon araması, bir kimlik doğrulama uygulaması veya metinleri. En iyi ne yaptığınızı açıklayan bir seçenek belirleyin:

| Nasıl oturum? |
| --- |
| [Mobil veya ofis telefonumu için bir telefon çağrısı ile](#signing-in-with-a-phone-call) |
| [Cep telefonuma metin ile](#signing-in-with-a-text-message)
| [Microsoft Authenticator uygulamasından ile bildirimleri](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [Microsoft Authenticator uygulaması ile doğrulama kodları](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [Alternatif bir yöntem ile tercih edilen yöntemimi şu anda kullanamazsınız çünkü](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a>Telefonla Oturum imzalama
Aşağıdaki bilgiler, mobil veya ofis telefonunuza bir çağrı iki aşamalı doğrulama deneyimine açıklar.

1. Bir uygulama veya hizmet, kullanıcı adı ve parolanızı kullanarak Office 365 gibi oturum açın.  
2. Microsoft, çağırır.  
3. Telefonu yanıtlayın ve # tuşuna basın.  

## <a name="signing-in-with-a-text-message"></a>Kısa mesaj ile imzalama
Aşağıdaki bilgiler, cep telefonunuza bir kısa mesaj iki aşamalı doğrulama deneyimine açıklar:

1. Bir uygulama veya hizmet, kullanıcı adı ve parolanızı kullanarak Office 365 gibi oturum açın.
2. Microsoft, size bir numara kodu içeren bir kısa mesaj gönderir.
3. Oturum açma sayfada sağlanan kutuya kodu girin.

## <a name="signing-in-with-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasını açın imzalama
Aşağıdaki bilgiler, iki aşamalı doğrulama işlemleri için Microsoft Authenticator uygulamasını kullanma deneyimi açıklanmıştır. Uygulamayı kullanmak için iki farklı yolu vardır. Cihazınızda anında iletme bildirimleri alabilir veya ve uygulamayı bir doğrulama kodu açabilirsiniz.

### <a name="to-sign-in-with-a-notification-from-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasından bir bildirim oturum açmanız
1. Bir uygulama veya hizmet, kullanıcı adı ve parolanızı kullanarak Office 365 gibi oturum açın.
2. Microsoft, Cihazınızda Microsoft Authenticator uygulamasına bir bildirim gönderir.

  ![Microsoft, bildirim gönderir.](./media/multi-factor-authentication-end-user-signin/notify.png)

3. Telefon ve seçim bildirimi açın **doğrulama** anahtarı. Şirketiniz bir PIN gerektiriyorsa, buraya girin.
4. Artık oturum açmanız.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması ile doğrulama kodunu kullanarak oturum açmanız

Doğrulama kodları almak için Microsoft Authenticator uygulamasını kullanın, uygulamayı açtığınızda sonra birkaç hesap adınızın altında görürsünüz. Böylece iki kere aynı sayıda kullanmayın, bu sayı her 30 saniyede değiştirir. İçin bir doğrulama kodu sorulduğunda, uygulamayı açın ve şu anda görüntülenen sayıyı kullanın.

1. Bir uygulama veya hizmet, kullanıcı adı ve parolanızı kullanarak Office 365 gibi oturum açın.
2. Microsoft için bir doğrulama kodu ister.

  ![Doğrulama kodunu girin](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. Telefonunuza Microsoft Authenticator uygulamasını açın ve kodu nerede açtığınız kutuya girin.

## <a name="signing-in-with-an-alternate-method"></a>Alternatif bir yöntem ile imzalama
Bazı durumlarda telefon veya, tercih edilen doğrulama yöntemi olarak ayarladığınız cihaz yok. Hesabınız için yedekleme yöntemleri ayarlamanızı öneririz neden bu durumda. Aşağıdaki bölümde, birincil yöntemi kullanılamayabilir oturum alternatif bir yöntem oturum işlemini göstermektedir.

1. Bir uygulama veya hizmet, kullanıcı adı ve parolanızı kullanarak Office 365 gibi oturum açın.
2. Seçin **farklı bir doğrulama seçeneği kullanma**. Kaç, Kurulum olmadığına göre farklı bir kimlik doğrulama seçenekleri görürsünüz.
3. Alternatif bir yöntem seçin ve oturum açın.

  ![Alternatif yöntemi kullanın](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a>Sonraki adımlar

İki aşamalı doğrulaması ile oturum açma sorunları varsa, daha fazla bilgi edinin [Azure multi-Factor Authentication sorununuz](multi-factor-authentication-end-user-troubleshoot.md).

Bilgi edinmek için nasıl [iki adımlı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md).

Hakkında bilgi edinin [Microsoft Authenticator uygulaması ile çalışmaya başlama](microsoft-authenticator-app-how-to.md) böylece bildirimleri, telefon görüşmeleri ve metinler yerine oturum açmak için kullanabilirsiniz.
