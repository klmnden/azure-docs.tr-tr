---
title: Öğeleri B2B davet e-posta - Azure Active Directory | Microsoft Docs
description: Azure Active Directory B2B işbirliği davet e-posta şablonu
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/06/2019
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: f701642c76446049ad2868ae59a03370e798b6a2
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57453703"
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email---azure-active-directory"></a>B2B işbirliği davet e-- Azure Active Directory öğeleri

Davet e-postalarına, iş ortakları karttaki B2B işbirliği kullanıcıları Azure AD'de getirmek için önemli bir bileşendir. Alıcının güveni artırmak için bunları kullanabilirsiniz. yasallığı ekleyebilirsiniz ve alıcı emin olmak için e-posta, sosyal kanıt hissettirir seçme ile rahat **Başlarken** davetiyeyi kabul etmek için düğme. Bu güven, paylaşım uyuşmazlıkları azaltmak bir anahtar anlamına gelir. Ve ayrıca e-posta harika görünecek yapmak istediğiniz!

![Azure AD B2b davet e-postası](media/invitation-email-elements/invitation-email.png)

## <a name="explaining-the-email"></a>E-posta açıklayan
En iyi şekilde nasıl yeteneklerini kullanılacak bilmesi e-postanın bazı öğeler bakalım.

### <a name="subject"></a>Özne
E-postanın şu deseni izler: Davet ettiğiniz &lt;kiracıadı&gt; kuruluş

### <a name="from-address"></a>Gönderici adresi
Bir LinkedIn desen Kimden adresi için kullanırız.  Davet eden olan temizleyin ve şirket, ve ayrıca bir Microsoft e-posta geldiğini açıklamak e-posta adresi gerekir. Biçimi şu şekildedir: &lt;Davet eden görünen adını&gt; gelen &lt;kiracıadı&gt; (Microsoft aracılığıyla) <invites@microsoft.com>

### <a name="reply-to"></a>Yanıtla
Yanıt için e-posta, davet eden'ın e-posta için kullanılabilir olduğunda, yeniden davet eden e-posta göndermesi için e-posta yanıtlama ayarlanır.

### <a name="branding"></a>Markalama
Kiracı kullanım davet e-postaları şirket markası, kiracınız için ayarlamış. Bu özellikten faydalanmak istiyorsanız [burada](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) nasıl yapılandırılacağı hakkında ayrıntılar bulunur. Başlık logosu, e-postada görünür. Görüntü boyutu izleyin ve kalite yönergeler [burada](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) en iyi sonuçlar için. Ayrıca, şirket adı da eylem çağrısı içinde gösterilir.

### <a name="call-to-action"></a>Eylemini çağırın
Eylem çağrısı iki bölümden oluşur: alıcı, e-posta neden aldı ve bununla ilgili alıcı ne sorulmaktadır açıklayan.
- "Neden" bölümünde şu biçimi kullanarak çözülebilir: Uygulamalara erişmek üzere davet edildiniz &lt;kiracıadı&gt; kuruluş

- Ve "ne, yapmak istenir" bölümünde varlığını tarafından belirtilen **Başlarken** düğmesi. Alıcı davetleri gerek kalmadan eklendiğinde, bu düğmeyi göstermez.

### <a name="inviters-information"></a>Davet eden'ın bilgi
Davet eden'ın görünen ad, e-postada dahil edilir. Ve ayrıca, Azure AD hesabınız için bir profil resmi oluşturan ayarladıysanız, davet eden e-posta o resmi de içerecektir. Her ikisi de, e-posta, alıcının ellerde artırmak için tasarlanmıştır.

Profil resminizi henüz ayarlamadıysanız, davet eden'ın baş resmi yerine bir simgeyle gösterilir:

  ![Davet eden'ın baş görüntüleme](media/invitation-email-elements/inviters-initials.png)

### <a name="body"></a>Gövde
Davet eden ne zaman ölçeklemesini ileti gövdesinde [directory, Grup veya uygulama için Konuk kullanıcı davet](add-users-administrator.md) veya [davet API kullanarak](customize-invitation-api.md). Güvenlik nedenleriyle HTML etiketlerini işlemek için bir metin alanı var.

### <a name="footer-section"></a>Alt bilgi bölümü
Alt bilgi, Microsoft şirket markası içerir ve e-posta izlenmeyen bir diğer addan gönderildiğini bilmeniz alıcı olanak tanır. Özel durumlar:

- Davet eden, davet eden Kiracı içinde bir e-posta adresi yok

  ![Resim davet eden, davet eden Kiracı bir e-posta adresi yok](media/invitation-email-elements/inviter-no-email.png)


- Alıcı davetini gerekmez

  ![ne zaman alıcı davetini gerekmez](media/invitation-email-elements/when-recipient-doesnt-redeem.png)

## <a name="how-the-language-is-determined"></a>Dil nasıl belirlenir
Konuk kullanıcı davet e-posta olarak sunulan dili, aşağıdaki ayarları tarafından belirlenir. Bu ayarlar, öncelik sırasına göre listelenir. Bir ayar yapılandırılmamışsa, listedeki sonraki ayarı dili belirler. 
- **MessageLanguage** özelliği [invitedUserMessageInfo](https://docs.microsoft.com/graph/api/resources/invitedusermessageinfo?view=graph-rest-1.0) API oluşturma davet kullanılıp kullanılmadığını nesnesi
-   **PreferredLanguage** konuğun içinde belirtilen özellik [kullanıcı nesnesi](https://docs.microsoft.com/graph/api/resources/user?view=graph-rest-1.0)
-   **Bildirim dili** (yalnızca Azure AD kiracılar için) ana Kiracı Konuk kullanıcının özelliklerini ayarlama
-   **Bildirim dil** kaynak Kiracı özelliklerinde ayarlayın

Bu ayarların hiçbiri yapılandırıldıysa, İngilizce (ABD) dili varsayılandır.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği hakkında aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir](what-is-b2b.md)
- [Azure Active Directory yöneticileri B2B işbirliği kullanıcılarını nasıl ekleyebilir?](add-users-administrator.md)
- [Bilgi çalışanları B2B işbirliği kullanıcılarını nasıl ekleyebilir?](add-users-information-worker.md)
- [B2B işbirliği Davetiyesi kullanımı](redemption-experience.md)
- [B2B işbirliği kullanıcıları davet etmeden ekleme](add-user-without-invite.md)
