---
title: Ne zaman ve nasıl bir Azure multi-Factor Auth sağlayıcısı kullanılır? - Azure Active Directory
description: Ne zaman bir Auth sağlayıcısı Azure MFA ile kullanmalısınız?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: dfababeae15ee18a140042d9a6ca10be40e41339
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60415812"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Azure multi-Factor Authentication sağlayıcısı kullanıldığı durumlar

İki adımlı doğrulama, Azure Active Directory’ye sahip genel yöneticiler ve Office 365 kullanıcıları için varsayılan olarak kullanılabilir durumdadır. Ancak, [gelişmiş özelliklerden](howto-mfa-mfasettings.md) yararlanmak isterseniz Azure Multi-Factor Authentication’ın (MFA) tam sürümünü satın almanız gerekir.

Kullanıcılar için Azure multi-Factor Authentication tarafından sağlanan özelliklerden yararlanmak için Azure multi-Factor Auth sağlayıcısı kullanılan kimin **lisansı olmayan**.

Ardından, kuruluşunuzdaki tüm kullanıcılar kapsayan lisanslarınız varsa, Azure multi-Factor Auth sağlayıcısı gerekmez. Yalnızca, ayrıca lisansınız yoksa kullanıcılarınız için iki aşamalı doğrulamayı sağlamanız gerekiyorsa, Azure multi-Factor Authentication sağlayıcısı oluşturun.

> [!NOTE]
> 1 Eylül 2018'e yeni etkin yetki sağlayıcılar artık oluşturulabilir. Mevcut kimlik doğrulama sağlayıcıları güncelleştirildi ve kullanılabilmesi devam edebilir. Çok faktörlü kimlik doğrulaması, Azure AD Premium lisansları bir özellik olarak kullanılabilir olmaya devam edecektir.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Azure MFA SDK'sı için ilgili uyarılar

Not SDK kullanım dışıdır ve yalnızca 14 Kasım 2018'e kadar çalışmaya devam eder. O tarihten sonra SDK çağrıları başarısız olacaktır.

## <a name="what-is-an-mfa-provider"></a>MFA Sağlayıcısı nedir?

İki tür kimlik doğrulama sağlayıcısı vardır ve Azure aboneliğinize nasıl ücretlendirilir etrafında ayrım ilgilidir. Kimlik doğrulaması başına seçeneğinde, kiracınızda bir ay içinde gerçekleştirilen kimlik doğrulaması sayısı hesaplanır. Bu, yalnızca gereken durumlarda kimlik doğrulamasını kullanan birkaç kullanıcınızın olması halinde en iyi seçenektir. Kullanıcı başına seçeneğinde, kiracınızda bir ayda iki aşamalı doğrulama gerçekleştiren kişi sayısı hesaplanır. Bu, lisansı bulunan bazı kullanıcılarınızın olması ancak MFA'yı, lisanslama sınırlarınızı aşacak sayıda kullanıcıya genişletmeniz gereken durumlarda en iyi seçenektir.

## <a name="manage-your-mfa-provider"></a>MFA Sağlayıcınızı yönetme

Bir MFA sağlayıcısı oluşturulduktan sonra kullanım modelini (etkin kullanıcı başına veya kimlik doğrulaması başına) değiştiremezsiniz.

MFA için etkinleştirilen tüm kullanıcıları kapsayacak yeterli sayıda lisans satın aldıysanız, MFA sağlayıcısını tamamen silebilirsiniz.

MFA sağlayıcınız bir Azure AD kiracısına bağlı değilse veya yeni MFA sağlayıcısını farklı bir Azure AD kiracısına bağlarsanız, kullanıcı ayarları ve yapılandırma seçenekleri aktarılmaz. Ayrıca, mevcut Azure MFA sunucularının MFA sağlayıcısı ile oluşturulan etkinleştirme kimlik bilgileri kullanılarak etkinleştirilmesi gerekir. Bunları MFA sağlayıcısına bağlamak için MFA sunucularını telefon aramasıyla kısa mesaj kimlik doğrulamasını etkilemez, ancak bunlar mobil uygulamayı yeniden kadar tüm kullanıcılar için mobil uygulama bildirimleri durdurun.

## <a name="next-steps"></a>Sonraki adımlar

[Multi-Factor Authentication ayarlarını yapılandırma](howto-mfa-mfasettings.md)
