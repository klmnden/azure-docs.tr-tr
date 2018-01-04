---
title: "Azure MFA ile oturum açma iki aşamalı doğrulama | Microsoft Docs"
description: "Bu sayfa, oturum açma Azure MFA ile kullanılabilen yöntemleri görmek için yapılması gerekenler hakkında kılavuzluk sağlar."
keywords: "Kullanıcı kimlik doğrulaması, oturum açma deneyimi, cep telefonu ile oturum aç ofis telefonu ile oturum açma"
services: multi-factor-authentication
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: barlan
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: c47356b7b84e38a1db9259304c2a975958b1977c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication ile oturum açma deneyimi
> [!NOTE]
> Bu makalede amacı tipik oturum deneyimi boyunca size yol olmaktır. Oturum açma ile veya sorunlarını gidermek için Yardım için bkz: [Azure multi-Factor Authentication sorununuz](multi-factor-authentication-end-user-troubleshoot.md).

## <a name="what-will-your-sign-in-experience-be"></a>Oturum açma deneyimini ne olacak?
Oturum açma deneyimini ikinci faktörü olarak kullanmak seçtiğiniz öğeye bağlı olarak farklılık gösterir: telefon araması, bir kimlik doğrulama uygulaması veya metinleri. En iyi ne yaptığınızı açıklayan bir seçenek belirleyin:

| Nasıl oturum? |
| --- |
| [Cep telefonu numarası veya office telefonum için bir telefon çağrısı ile](#signing-in-with-a-phone-call) |
| [Bir metin ile Cep telefonum](#signing-in-with-a-text-message)
| [Microsoft Authenticator uygulaması ile bildirim](#signing-in-with-the-microsoft-authenticator-app-using-notification) |
| [Microsoft Authenticator uygulaması ile doğrulama kodları](#signing-in-with-the-microsoft-authenticator-app-using-verification-code) |
| [Alternatif bir yöntem ile my tercih edilen yöntem şu anda kullanamazsınız çünkü](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a>Bir telefon araması oturum imzalama
Aşağıdaki bilgiler, cep telefonu numarası veya office telefonunuza bir arama iki aşamalı doğrulama deneyimine açıklar.

1. Bir uygulama veya hizmet kullanıcı adı ve parola kullanarak Office 365 gibi oturum açın.  
2. Microsoft, çağırır.  
3. Telefonu yanıtlayın ve # tuşuna basın.  

## <a name="signing-in-with-a-text-message"></a>SMS mesajı oturum imzalama
Aşağıdaki bilgiler, cep telefonunuza bir kısa mesaj iki aşamalı doğrulama deneyimine açıklar:

1. Bir uygulama veya hizmet kullanıcı adı ve parola kullanarak Office 365 gibi oturum açın.
2. Microsoft bir numara kodu içeren bir kısa mesaj gönderir.
3. Oturum açma sayfada sağlanan kutuya kodu girin.

## <a name="signing-in-with-the-microsoft-authenticator-app"></a>Microsoft Kimlik Doğrulayıcı uygulama ile oturum açılmasını
Aşağıdaki bilgiler iki aşamalı Doğrulamalar için Microsoft Authenticator uygulamasını kullanma deneyimi açıklanır. Uygulamayı kullanmak için iki farklı yolu vardır. Cihazınızda anında iletme bildirimleri alabilir veya bir doğrulama kodu almak için uygulama açabilirsiniz.

### <a name="to-sign-in-with-a-notification-from-the-microsoft-authenticator-app"></a>Bir bildirim Microsoft Authenticator uygulama üzerinden oturum için
1. Bir uygulama veya hizmet kullanıcı adı ve parola kullanarak Office 365 gibi oturum açın.
2. Microsoft, Microsoft Authenticator uygulaması için bir bildirim gönderir.

  ![Microsoft bildirim gönderir](./media/multi-factor-authentication-end-user-signin/notify.png)

3. Telefon ve seçim bildirimi açın **doğrula** anahtarı. Şirketiniz bir PIN gerektiriyorsa, buraya girin.
4. Artık oturum açmanız.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasıyla doğrulama kodunu kullanarak oturum

Doğrulama kodları almak için Microsoft Authenticator uygulamasını kullanırsanız uygulamasını açın, sonra bir sayı, hesap adı altında görürsünüz. Bu sayı her 30 saniyede değiştirir, böylece aynı numarasını iki kez kullanmayın. İçin bir doğrulama kodu sorulduğunda, uygulamayı açın ve sayıyı görüntülenmekte kullanın.

1. Bir uygulama veya hizmet kullanıcı adı ve parola kullanarak Office 365 gibi oturum açın.
2. Microsoft için bir doğrulama kodu ister.

  ![Doğrulama kodunu girin](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. Telefonunuzda Microsoft Authenticator uygulamasını açın ve burada oturum kutusunda kodu girin.

## <a name="signing-in-with-an-alternate-method"></a>Alternatif bir yöntem ile imzalama
Bazen telefon veya tercih edilen doğrulama yöntemi olarak ayarladığınız cihaz yok. Hesabınız için yedekleme yöntemleri ayarlamak öneririz neden bu durumda. Aşağıdaki bölümde birincil yönteminiz kullanılamayabilir oturum alternatif bir yöntem oturum gösterilmiştir.

1. Bir uygulama veya hizmet kullanıcı adı ve parola kullanarak Office 365 gibi oturum açın.
2. Seçin **farklı bir doğrulama seçeneği kullanma**. Kaç tane Kurulum sahip bağlı olarak farklı bir doğrulama seçeneklerini bakın.
3. Alternatif bir yöntem seçin ve oturum açın.

  ![Alternatif yöntemi kullanın.](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a>Sonraki adımlar

İki aşamalı doğrulamayı oturum imzalama sorunlarınız varsa, daha fazla bilgi almak [Azure multi-Factor Authentication sorununuz](multi-factor-authentication-end-user-troubleshoot.md).

Bilgi edinmek için nasıl [iki basamaklı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md).

İçin öğrenin [Microsoft Authenticator uygulaması ile çalışmaya başlama](microsoft-authenticator-app-how-to.md) böylece bildirimleri, telefon aramaları ve metinler yerine imzalamak için kullanabilirsiniz.
