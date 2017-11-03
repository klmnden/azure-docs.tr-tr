---
title: "Azure Active Directory B2B işbirliği davet e-posta öğelerini | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği davet e-posta şablonu"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 458a2cab13b7e83f120e0926a95d454070181dfb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email"></a>B2B işbirliği davet e-posta öğeleri

Davet e-postaları karttaki ortaklar B2B işbirliği kullanıcılar olarak Azure AD içinde getirmek için kritik bir bileşen var. Alıcının güven artırmak için bunları kullanabilirsiniz. yasallığı ekleyebilir ve alıcı emin olmak için e-posta, sosyal kanıt hissi seçme ile rahat **Get Started** daveti kabul düğmesi. Bu güven dir paylaşım uyuşmazlık azaltmak bir anahtar anlamına gelir. Ve ayrıca görünümlü e-posta yapmak istediğiniz!

![Azure AD B2b davet e-postası](media/active-directory-b2b-invitation-email/invitation-email.png)

## <a name="explaining-the-email"></a>E-posta açıklayan
En iyi şekilde nasıl yeteneklerini kullanmak için bilmesi e-postanın birkaç öğeler bakalım.

### <a name="subject"></a>Konu
E-postanın konu şu deseni izler: davet ettiğiniz &lt;tenantname&gt; kuruluş

### <a name="from-address"></a>Adresinden
Başlangıç adresi için bir LinkedIn desen kullanırız.  Davet eden olan temizleyin ve hangi şirket ve ayrıca bir Microsoft e-posta geliyor açıklamak e-posta adresi gerekir. Biçim: &lt;davet eden görünen adını&gt; gelen &lt;tenantname&gt; (aracılığıyla Microsoft) <invites@microsoft.com&gt;

### <a name="reply-to"></a>Yanıtla
E-posta yanıtlama geri davet eden bir e-posta gönderir Yanıtla e-posta davet eden'ın e-posta için kullanılabilir olduğunda ayarlanır.

### <a name="branding"></a>Markalama
Kiracı kullanımınız gelen davet e-postaları, marka şirket kiracınız için ayarladığınız. Bu özellikten yararlanabilir istiyorsanız [burada](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) nasıl yapılandırılacağı hakkında bilgi. Başlık logosu e-postasında görüntülenir. Görüntü boyutu izleyin ve kalite yönergeleri [burada](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) en iyi sonuçlar için. Ayrıca, şirket adı da eylem çağrısında görüntülenir.

### <a name="call-to-action"></a>Arama eylemi için
Eylem çağrısı iki bölümden oluşur: alıcı posta neden aldı ve ne alıcı ne yapmanız isteniyor açıklayan.
- "Neden" bölümünde şu biçimi kullanarak çözülebilir: uygulamalarında erişmeye davet &lt;tenantname&gt; kuruluş

- Ve "ne, yapmanız isteniyor" bölümü varlığını tarafından gösterilen **Get Started** düğmesi. Alıcı davetleri gerek kalmadan eklendiğinde bu düğme göstermez.

### <a name="inviters-information"></a>Davet eden'ın bilgi
Davet eden'ın görünen adı e-postasında dahil edilir. Ve Azure AD hesabınız için bir profil resmi oluşturmadıysanız, ayrıca, davet e-posta o resmi de içerir. Her ikisi de, e-posta alıcının güveni artırmak için tasarlanmıştır.

Profil resminizi henüz ayarlamadıysanız, resmi yerine davet eden'ın baş harflerini içeren bir simge gösterilir:

  ![Davet eden'ın baş harflerini görüntüleme](media/active-directory-b2b-invitation-email/inviters-initials.png)

### <a name="body"></a>Gövde
Davet eden oluşturur veya API davet yoluyla geçirilen ileti gövdesi içerir. Güvenlik nedenleriyle HTML etiketlerini işlemek için bir metin alanı var.

### <a name="footer-section"></a>Altbilgi bölümü
Altbilgi Microsoft şirketinizin markasını içeren ve e-posta izlenmeyen bir diğer ad tarafından gönderildiğini bilmeniz alıcı olanak tanır. Özel durumlar:

- Davet eden davet kiralama ile bir e-posta adresi yok

  ![davet eden resmini davet kiralama ile bir e-posta adresi yok](media/active-directory-b2b-invitation-email/inviter-no-email.png)


- Alıcı daveti kullanmak gerekmez

  ![ne zaman alıcı davet kullanmak gerekmez](media/active-directory-b2b-invitation-email/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Azure Active Directory yöneticileri B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-admin-add-users.md)
* [Bilgi çalışanları B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-iw-add-users.md)
* [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B işbirliği lisanslama](active-directory-b2b-licensing.md)
* [Azure Active Directory B2B işbirliği sorunlarını giderme](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B işbirliği sık sorulan sorular (SSS)](active-directory-b2b-faq.md)
* [Azure Active Directory B2B işbirliği API ve özelleştirme](active-directory-b2b-api.md)
* [B2B işbirliği kullanıcıları için çok faktörlü kimlik doğrulaması](active-directory-b2b-mfa-instructions.md)
* [B2B işbirliği kullanıcıları davet olmadan ekleme](active-directory-b2b-add-user-without-invite.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
