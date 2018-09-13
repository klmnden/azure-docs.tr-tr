---
title: Microsoft Authenticator telefonla oturum açma - Azure ve Microsoft hesapları | Microsoft Docs
description: Parolanızı yazmak yerine Microsoft hesabınızda oturum açmak için telefonunuza kullanın. Bu makalede, bu özellik hakkında sık sorulan sorular yanıtlanmaktadır.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directoary
ms.workload: identity
ms.component: user-help
ms.topic: conceptual
ms.date: 08/12/2017
ms.author: lizross
ms.reviewer: librown
ms.openlocfilehash: 3536c5da7833c32b583f1a510be43864c9107068
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44717055"
---
# <a name="sign-in-with-your-phone-not-your-password"></a>Parolanızla değil telefonunuzla oturum açma
Microsoft Authenticator uygulamasını parolanızı girdikten sonra iki aşamalı doğrulamayı uygulayarak hesaplarınızın güvenliğini sağlamanıza yardımcı olur. Ancak bunu kişisel Microsoft hesabınızın parolasının yerine kullanabileceğinizi biliyor muydunuz?

Bu özellik, iOS ve Android cihazlarda kullanılabilir ve kişisel Microsoft hesapları ile çalışır.
 
## <a name="how-it-works"></a>Nasıl çalışır?
Microsoft hesabınızda oturum açtığınızda, birçoğu iki aşamalı doğrulama için Microsoft Authenticator uygulamasını kullanın. Parolanızı yazın, ardından bir bildirimi onaylayın veya bir doğrulama kodu almak için tıklayın veya dokunun. Telefonla oturum açma ile parolayı atlamak ve tüm kimlik doğrulamanızı telefonunuzda yapın. Telefonla oturum açma iki aşamalı doğrulamayı bir tür olduğundan, yine de bildiğiniz bir şey ve, kimliğinizi doğrulamak için bir şey sağlamanız gerekir. Telefon yine de sahip olduğunuz şey ve telefonunuzun PIN veya biyometrik anahtarı bildiğiniz bir şey.

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım
Telefonunuz, kişisel Microsoft hesabınızla oturum açmak için şu adımları izleyin:

1. Telefonla oturum açma hesabınız için bu seçeneği etkinleştirin.

    - Microsoft Authenticator uygulamasını henüz, yükleme ve adımlara göre kişisel Microsoft hesabınızı ekleyin yoksa [Microsoft Authenticator sayfasına](microsoft-authenticator-app-how-to.md). Dokunmanız böylece yeni eklenen hesapları otomatik olarak etkinleştirilir.

    - İki aşamalı doğrulama için Microsoft Authenticator zaten kullanıyorsanız, hesabınızı uygulama giriş sayfasından seçip **etkinleştirme telefonla oturum açma** aşağı açılan menüden.

    >[!NOTE]
    >Hesabınızı korumak için bir PIN veya biyometrik kilit Cihazınızda gerekli. Uygulama kilidi telefonunuzu tutarsanız, telefonla oturum açma etkinleştirmeden önce bir kilidi ayarlanacak isteyen bir istek görüntüler.

2. Çoğu sayfaları normalde girdiğiniz Microsoft hesabı parolanızı bildiren bir bağlantıya sahip **uygulama kullanmanız**. Oturum açmak için bu bağlantıyı seçin.
 
3. Microsoft, telefonunuza bir bildirim gönderir. Hesabınızda oturum açmak için bildirimi onaylayın.   
 
## <a name="faq"></a>SSS

### <a name="how-is-signing-in-with-my-phone-more-secure-than-typing-a-password"></a>Nasıl daha güvenli bir parola yazmak daha telefonumu oturum imzalama?  
Bugün çoğu kişi web siteleri veya uygulamaları bir kullanıcı adı ve parola kullanarak oturum açın.  Ne yazık ki, parolaları genellikle kayıp, çalınmış veya saldırganlar tarafından tahmin. Oturum açmak için Microsoft Authenticator uygulamasını ayarlama, biz, hesabınızın kilidini açmak telefonunuza bir anahtar oluşturun. Biz bu anahtar PIN veya biyometrik telefonunuzda zaten kullandığınız ile koruyun.  Telefonunuzla oturum açtığınızda, bu anahtarı güvenli bir şekilde iki faktör – telefon ve kilidini olanağı ile kimliğinizi ispatlamak üzere kullanılır.
 
Windows Hello ve FIDO Alliance UAF belirtimleri kullanılan anahtarlar için kullanılan anahtarı benzerdir. Yalnızca verilerdir biyografiniz anahtar yerel olarak korumak için kullanılan ve hiçbir zaman gönderilen veya bulutta depolanan. 
 
### <a name="where-can-i-use-my-phone-to-replace-my-password-and-where-would-i-still-need-the-password"></a>Parolamı değiştirmek için telefonumu nerede kullanabilirim ve burada parolayı hala değiştirmeliyim?  
Bugün, oturum açma bu telefon özelliği, web uygulamaları ve kişisel Microsoft hesapları, iOS veya kişisel Microsoft hesabı kullanan Android uygulamaları ve Windows 10 kişisel bir Microsoft hesabı'nı kullanan uygulamalar tarafından desteklenen hizmetler ile yalnızca çalışır. Bu web siteleri veya uygulamaları birine oturum açtığınızda, genellikle girebileceğiniz parolanızı sayfasında şunu bağlantısı bulunur **uygulama kullanmanız**. 

Telefonla oturum açma şu anda bir Windows PC, XBOX veya Office uygulamaları gibi Microsoft uygulamaları masaüstü sürümlerini kilidini açmak için kullanılamaz.
 
### <a name="does-this-replace-two-step-verification-should-i-turn-it-off"></a>Bu, iki aşamalı doğrulama yerini mi aldı? Ben bunu kapatmanız gerekir?   
Bazen. Telefonla oturum açma kapsamını genişleterek üzerinde çalışıyoruz, ancak şimdilik hala var. desteklemeyen Microsoft ekosistemi yerde. Bu yerlerde yine de iki aşamalı doğrulama güvenli oturum açma için kullanıyoruz. Bu nedenle, Hayır, iki aşamalı doğrulamayı kapatırsanız, hesabınız için özelliğini olmamalıdır.
 
### <a name="okay-if-i-keep-two-step-verification-turned-on-for-my-account-do-i-have-to-approve-two-notifications"></a>Tamam, ben iki aşamalı doğrulama hesabım için açık tutarsanız, ben iki bildirim onaylamak zorunda mıyım?
Hayır, gerekmez. Telefonunuz ile Microsoft hesabınızda oturum açarken iki aşamalı doğrulama sayar. Parolanızı yazmak, yerine daha sonra bir bildirim onaylama kimliğinizi telefonunuzun kilidini açma bilmek ve sonra bir bildirim onaylama kanıtlayacağınızı. Onaylamak için ikinci bir bildirim göndereceğiz olmaz.

### <a name="what-if-i-lose-my-phone-or-dont-have-it-with-me-how-can-i-access-my-account"></a>Ne telefonumu kaybeder veya yok benimle, Hesabımı nasıl erişebilirim?  
Her zaman tıklayabilirsiniz **bir parolasını kullanmanız** parolanızı kullanmaya geçmek için oturum açma sayfasında. İki aşamalı doğrulama kullanıyorsanız yine de oturum açma işleminizi doğrulamak için ikinci yöntem gerektiğini aklınızda bulundurun. İşte bu ek, güncel güvenlik bilgileri hesabınıza olduğundan emin olmak için önemle öneririz. Adresinden güvenlik bilgilerinizi yönetebileceğiniz https://account.live.com/proofs/manage.
 
### <a name="how-do-i-stop-using-this-feature-and-go-back-to-entering-my-password"></a>Nasıl bu özelliği kullanmayı bırakmak ve parolamı girmeye dönün?
Tıklayın **bir parolasını kullanmanız** oturum açtığınızda. Biz en son seçtiğiniz unutmayın ve, varsayılan olarak bir sonraki oturum açışınızda sunar. Şimdiye kadar istiyorsanız telefonunuzla oturum için geri dönün, tıklayın **uygulama kullanmanız**. 
 
### <a name="can-i-use-the-app-to-sign-in-to-all-my-accounts-with-microsoft"></a>My tüm Microsoft hesaplarıyla oturum açmak için uygulamayı kullanabilir miyim?   
Bu işlevsellik şu anda yalnızca kişisel Microsoft hesapları için kullanılabilir. 
 
### <a name="can-i-sign-into-my-pc-with-my-phone"></a>Bilgisayarımda telefonumu ile imzalayabilir mi?  
İçin bilgisayarınızın oturum Windows Hello Windows 10 imzalama, yüz tanıma, parmak izi veya PIN kullanmanızı öneririz.   
 
### <a name="can-i-sign-in-with-my-windows-phone"></a>My Windows Phone ile oturum açmak?  
Şu anda, biz bu işlev üzerinde Windows Phone için Microsoft Authenticator geliştirmekte olduğunuz değil. 

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Authenticator uygulamasını yüklemediyseniz, gözden geçirin. Uygulama için kullanılabilir [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), ve telefonla oturum açma için Microsoft Authenticator uygulamasının kullanılabilir [Android](http://go.microsoft.com/fwlink/?Linkid=825072) ve [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Genel uygulama hakkında sorularınız varsa, bir göz atın [Microsoft Authenticator ile ilgili SSS](microsoft-authenticator-app-faq.md)