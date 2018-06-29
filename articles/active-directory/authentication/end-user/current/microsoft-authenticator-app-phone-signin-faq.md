---
title: Microsoft Authenticator Azure ve Microsoft hesaplarının oturum açma - telefon | Microsoft Docs
description: Parolanızı yazmak yerine Microsoft hesabınızda oturum açmak için telefonunuz kullanın. Bu makalede, bu özellik hakkında sık sorulan sorular yanıtlanmaktadır.
services: multi-factor-authentication
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.assetid: ''
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/12/2017
ms.author: lizross
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: 063a97fc8f5b0746517919c73094525942418719
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102674"
---
# <a name="sign-in-with-your-phone-not-your-password"></a>Telefonunuz oturum, parolanızı değil oturum

Microsoft Authenticator uygulamasını parolanızı girdikten sonra iki aşamalı doğrulama gerçekleştirerek hesaplarınızın güvenliğini sağlamanıza yardımcı olur. Ancak bunu Kişisel Microsoft hesabınızın parolasını tamamen değiştirebilir olduğunu biliyor muydunuz?

Bu özellik, iOS ve Android cihazlarda kullanılabilir ve kişisel Microsoft hesaplarıyla çalışır.

## <a name="how-it-works"></a>Nasıl çalışır?

Microsoft hesabınızda oturum açtığınızda, birçoğu için iki aşamalı doğrulamayı Microsoft Authenticator uygulamasını kullanın. Parolanızı yazın ve sonra bir bildirim onaylayabilir veya bir doğrulama kodu almak için uygulamasına gidin. İle telefon oturum açma, parola atlayın ve tüm kimlik doğrulama telefonunuza yapın. Telefon oturum açma iki aşamalı doğrulamayı bir tür olduğundan, hala bildiğiniz bir şey ve kimliğinizi doğrulamak için sahip bir şey sağlamanız gerekir. Telefon hala elinizde şeydir ve telefonunuzun PIN veya biyometrik anahtarı bildiğiniz şeydir.

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

Telefonunuz kişisel Microsoft hesabınızla oturum açmak için şu adımları izleyin:

1. Telefon oturum açma hesabınızın etkinleştirin.

  - Henüz, yükleme ve kişisel Microsoft hesabınızı adımları göre eklemek Microsoft Authenticator uygulamasını yoksa [Microsoft Authenticator sayfasına](../../../../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md). Git iyi böylece yeni eklenen hesapları otomatik olarak etkinleştirilir.

  - İki aşamalı doğrulama için Microsoft Authenticator zaten kullanıyorsanız, uygulama giriş sayfasından hesabınızı seçin ve Seç **etkinleştirme telefon oturum açma** açılır menüsünden.

  >[!NOTE]
  >Hesabınızı korumak için size bir PIN veya biyometrik kilit aygıtınızda gerektirir. Telefonunuz kilidi tutarsanız, uygulama telefon oturum açma etkinleştirmeden önce bir Kilidi'ni ayarlamak için isteyen bir istek görüntüler.

3. Çoğu sayfaları normalde girdiğiniz Microsoft hesabı parolanızı belirten bir bağlantı sahip **bir uygulama kullanın**. Telefonunuz oturum imzalamak için bu bağlantıyı seçin.

4. Microsoft, telefonunuza bir bildirim gönderir. Hesabınızda oturum açmak için bildirim onaylayın.   

## <a name="faq"></a>SSS

### <a name="how-is-signing-in-with-my-phone-more-secure-than-typing-a-password"></a>Nasıl daha güvenli bir parola yazarak daha telefonum oturum imzalama?  

Günümüzde çoğu kişi web siteleri veya uygulamaları bir kullanıcı adı ve parola kullanarak oturum açın.  Ne yazık ki, parolaları genellikle kaybolduğunda, çalındığında veya saldırganlar tarafından tahmin. Oturum açmak için Microsoft Authenticator uygulamasını ayarladığınızda, biz telefonunuzda hesabınızın kilidini açmak bir anahtar oluşturun. Biz PIN veya biyometrik zaten telefonunuzda kullanıyorsanız bu anahtarla koruyun.  Telefonunuz oturum oturum açtığınızda, bu anahtar iki faktörleri – telefon ve kilidini açmak için yeteneğinizi ile güvenli, kimliğini kanıtlamak için kullanılır. 

Kullanılan anahtar, Windows Hello ve FIDO Alliance UAF belirtimleri kullanılan anahtarları benzerdir. Yalnızca verileri değil, biyografisi anahtarı yerel olarak korumak için kullanılan ve hiçbir zaman gönderilen veya bulutta depolanan. 
 
### <a name="where-can-i-use-my-phone-to-replace-my-password-and-where-would-i-still-need-the-password"></a>Burada telefonum parolamı değiştirmek için kullanabilir miyim ve burada hala parola ihtiyacım?  

Bugün, oturum açma bu telefon özelliği yalnızca web uygulamaları ve kişisel Microsoft hesapları, iOS veya kişisel bir Microsoft hesabı kullanmanız Android uygulamalarını ve Windows 10 kişisel bir Microsoft hesabı kullanan uygulamalar tarafından desteklenen Hizmetleri ile çalışır. Bu web siteleri veya uygulamaları, birine oturum açtığınızda, genellikle girdiğiniz parolanızı sayfasında yoktur belirten bir bağlantı **bir uygulama kullanın**. 

Telefon oturum açma şu anda bir Windows bilgisayarı, XBOX veya Office uygulamaları gibi Microsoft uygulamalarına Masaüstü sürümlerini kilidini açmak için kullanılamaz.
 
### <a name="does-this-replace-two-step-verification-should-i-turn-it-off"></a>Bu, iki aşamalı doğrulamayı yerini almaz? I kapatmak?   

Bazen. Telefon oturum açma kapsamı genişletme üzerinde çalışıyoruz, ancak şu an için hala desteklemeyen yer vardır Microsoft ekosistemindeki. Bu yerde biz yine iki aşamalı doğrulamayı güvenli oturum açma için kullanıyorsunuz. Bu nedenle, Hayır, iki basamaklı doğrulamayı kapatmak hesabınız için etkinleştirmeniz gerekir.
 
### <a name="okay-if-i-keep-two-step-verification-turned-on-for-my-account-do-i-have-to-approve-two-notifications"></a>İki aşamalı doğrulamayı hesabımın açık tutarsanız, Tamam, ı iki bildirim onaylamanız gerekiyor mu?

Hayır, olmaz. Telefonunuz Microsoft hesabınızla oturum açma iki aşamalı doğrulamayı sayılır. Parolanızı yazarak, yerine daha sonra bir bildirim onaylama, kimliğinizi telefonunuz kilidini açma bilerek ve bildirim onaylama kanıtlamak. Biz onaylamak için ikinci bir bildirim göndermez.

### <a name="what-if-i-lose-my-phone-or-dont-have-it-with-me-how-can-i-access-my-account"></a>Ne telefonum kaybeder veya sahip değilseniz, benimle, nasıl Hesabımı erişebilir mi?  

Her zaman tıklayabilirsiniz **bir parolasını kullanmanız** parolanızı kullanmaya geçiş yapmak için oturum açma sayfasında. İki aşamalı doğrulama kullanırsanız, yine oturum açma işleminiz doğrulamak için ikinci bir yöntem gerektiğini göz önünde bulundurun. İşte bu nedenle ek, güncel güvenlik bilgileri hesabınıza olduğundan emin olmak için kesinlikle öneririz. Adresinden güvenlik bilgilerinizi yönetebilirsiniz https://account.live.com/proofs/manage.
 
### <a name="how-do-i-stop-using-this-feature-and-go-back-to-entering-my-password"></a>Nasıl bu özelliği kullanarak durdurmak ve parolamı girmek için geri dönün?

Tıklatın **bir parolasını kullanmanız** oturum açtığınızda. Biz en son tercih ettiğiniz unutmayın ve varsayılan olarak, oturum açtığınızda sunar. Şimdiye kadar istiyorsanız, telefonunuzun oturum imzalama için geri dönün, tıklatın **bir uygulama kullanın**. 
 
### <a name="can-i-use-the-app-to-sign-in-to-all-my-accounts-with-microsoft"></a>My tüm Microsoft hesaplarıyla oturum açmak için uygulama kullanabilir miyim?   
Bu işlevsellik şu anda yalnızca kişisel Microsoft hesapları için kullanılabilir. 
 
### <a name="can-i-sign-into-my-pc-with-my-phone"></a>Bilgisayarımda telefonum ile oturum açarak?  
Bilgisayarınıza için oturum Windows Hello Windows 10 imzalama, yüz, parmak izi veya bir PIN kullanmanızı öneririz.   
 
### <a name="can-i-sign-in-with-my-windows-phone"></a>My Windows Phone oturum oturum?  
Şu anda Biz bu işlevselliği Windows Phone üzerinde Microsoft Authenticator için geliştirdiğiniz değil. 

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Authenticator uygulamasını yüklemediyseniz atın. Uygulama için kullanılabilir [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), ve telefon oturum açma için Microsoft Authenticator uygulaması üzerinde kullanılabilir [Android](http://go.microsoft.com/fwlink/?Linkid=825072) ve [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Genel uygulama hakkında sorularınız varsa, bir göz atalım [Microsoft Doğrulayıcı SSS](../../../../multi-factor-authentication/end-user/microsoft-authenticator-app-faq.md)
