---
title: Ne zaman ve nasıl bir Azure multi-Factor Auth sağlayıcısı kullanılır?
description: Ne zaman bir Auth sağlayıcısı Azure MFA ile kullanmalısınız?
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 8e77a33667bd6794f667348958e0edb9c6a8fb0d
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094986"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Azure multi-Factor Authentication sağlayıcısı kullanıldığı durumlar

İki adımlı doğrulama, Azure Active Directory’ye sahip genel yöneticiler ve Office 365 kullanıcıları için varsayılan olarak kullanılabilir durumdadır. Ancak, [gelişmiş özelliklerden](howto-mfa-mfasettings.md) yararlanmak isterseniz Azure Multi-Factor Authentication’ın (MFA) tam sürümünü satın almanız gerekir.

Kullanıcılar için Azure multi-Factor Authentication tarafından sağlanan özelliklerden yararlanmak için Azure multi-Factor Auth sağlayıcısı kullanılan kimin **lisansı olmayan**.

Ardından, kuruluşunuzdaki tüm kullanıcılar kapsayan lisanslarınız varsa, Azure multi-Factor Auth sağlayıcısı gerekmez. Yalnızca, ayrıca lisansınız yoksa kullanıcılarınız için iki aşamalı doğrulamayı sağlamanız gerekiyorsa, Azure multi-Factor Authentication sağlayıcısı oluşturun.

> [!NOTE]
> 1 Eylül 2018'e yeni etkin yetki sağlayıcılar artık oluşturulabilir. Mevcut kimlik doğrulama sağlayıcıları güncelleştirildi ve kullanılabilmesi devam edebilir. Çok faktörlü kimlik doğrulaması, Azure AD Premium lisansınız kullanılabilir bir özellik olmaya devam edecektir.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Azure MFA SDK'sı için ilgili uyarılar

SDK’yı indirmek için Azure Multi-Factor Auth sağlayıcısı gerekir. SDK, kullanım dışı bırakıldı ve yeni müşteriler için artık desteklenmiyor ve yalnızca 14 Kasım 2018'e kadar çalışmaya devam edecektir unutmayın. O tarihten sonra SDK çağrıları başarısız olacaktır.

Azure MFA, AAD Premium veya ile birlikte gelen diğer lisans olsa bile SDK'yı indirmek için Azure multi-Factor Auth sağlayıcısı oluşturun. Bu amaçla Azure Multi-Factor Auth Sağlayıcısı oluşturursanız ve zaten lisanslarınız varsa Sağlayıcıyı **Etkin Kullanıcı Başına** modeliyle oluşturduğunuzdan emin olun. Ardından, sağlayıcıyı Azure MFA, Azure AD Premium veya ile birlikte gelen diğer lisans içeren dizine bağlayın. Bu yapılandırma, sizden yalnızca iki aşamalı doğrulama kullanan benzersiz kullanıcılarınızın sayısı sahip olduğunuz lisanslardan daha fazlaysa ücret alınmasını sağlar.

## <a name="what-is-an-mfa-provider"></a>MFA Sağlayıcısı nedir?

İki tür kimlik doğrulama sağlayıcısı vardır ve Azure aboneliğinize nasıl ücretlendirilir etrafında ayrım ilgilidir. Kimlik doğrulaması başına seçeneğinde, kiracınızda bir ay içinde gerçekleştirilen kimlik doğrulaması sayısı hesaplanır. Bu, yalnızca gereken durumlarda kimlik doğrulamasını kullanan birkaç kullanıcınızın olması halinde en iyi seçenektir. Kullanıcı başına seçeneğinde, kiracınızda bir ayda iki aşamalı doğrulama gerçekleştiren kişi sayısı hesaplanır. Bu, lisansı bulunan bazı kullanıcılarınızın olması ancak MFA'yı, lisanslama sınırlarınızı aşacak sayıda kullanıcıya genişletmeniz gereken durumlarda en iyi seçenektir.

## <a name="manage-your-mfa-provider"></a>MFA Sağlayıcınızı yönetme

Bir MFA sağlayıcısı oluşturulduktan sonra kullanım modelini (etkin kullanıcı başına veya kimlik doğrulaması başına) değiştiremezsiniz.

MFA için etkinleştirilen tüm kullanıcıları kapsayacak yeterli sayıda lisans satın aldıysanız, MFA sağlayıcısını tamamen silebilirsiniz.

MFA sağlayıcınız bir Azure AD kiracısına bağlı değilse veya yeni MFA sağlayıcısını farklı bir Azure AD kiracısına bağlarsanız, kullanıcı ayarları ve yapılandırma seçenekleri aktarılmaz. Ayrıca, mevcut Azure MFA sunucularının MFA sağlayıcısı ile oluşturulan etkinleştirme kimlik bilgileri kullanılarak etkinleştirilmesi gerekir. Bunları MFA sağlayıcısına bağlamak için MFA sunucularını telefon aramasıyla kısa mesaj kimlik doğrulamasını etkilemez, ancak bunlar mobil uygulamayı yeniden kadar tüm kullanıcılar için mobil uygulama bildirimleri durdurun.

## <a name="next-steps"></a>Sonraki adımlar

[Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md)
