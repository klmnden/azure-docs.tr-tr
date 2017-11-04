---
title: "Office 365 Hizmetleri için Azure Active Directory koşullu erişim cihaz ilkeleri | Microsoft Docs"
description: "Şirket kaynaklarına daha güvenli, kullanıcının uyumluluk koruyarak ve hizmetlere erişim sağlamak için koşullu erişim cihaz ilkeleri sağlamasını yapma hakkında bilgi edinin."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8664c0bb-bba1-4012-b321-e9c8363080a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: d33babd331570624ca6a0fc967c79dbc467a1b40
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="active-directory-conditional-access-device-policies-for-office-365-services"></a>Office 365 Hizmetleri için Active Directory koşullu erişim cihaz ilkeleri

Koşullu erişim çalışmak için birden fazla parça gerektirir. Çok faktörlü içerir kimliği doğrulanmış kullanıcı, kimliği doğrulanmış cihaz ve diğer etkenlere bağlı arasında uyumlu bir cihaz. Bu makalede, biz öncelikle Office 365 hizmetlerine erişimi denetleme kuruluşunuz yardımcı olmak için kullanabileceğiniz cihaz tabanlı koşulları odaklanın. 

Şirket kullanıcıları hizmetlerine erişim Office 365 Exchange ve SharePoint Online gibi iş veya Okul kişisel cihazlarından istiyor. Güvenli olması için erişim istiyor. Şirket kaynaklarına uyumlu aygıtlarını kullanan kullanıcılar için hizmetlerine erişim verilirken daha güvenli olmasına yardımcı olmak için koşullu erişim cihaz ilkeleri sağlayabilirsiniz. Office 365 için Microsoft Intune koşullu erişim Portalı'nda koşullu erişim ilkeleri ayarlayabilirsiniz.

Azure Active Directory (Azure AD), Office 365 hizmetlerine güvenli erişimini sağlamak için koşullu erişim ilkeleri zorunlu tutar. Bir Office 365 hizmeti erişmesini uyumsuz bir cihazı kullanan bir kullanıcı engelleyen bir koşullu erişim ilkesi oluşturabilirsiniz. Hizmete erişim verilmeden önce kullanıcı için şirketin cihaz ilkelerini uygun olmalıdır. Alternatif olarak, kullanıcıların bir Office 365 hizmeti erişmek için cihazlarını kaydetmesine izin gerektiren bir ilke oluşturabilirsiniz. İlkeleri bir kuruluştaki tüm kullanıcılar için geçerli veya birkaç hedef grupları için sınırlı. Daha fazla hedef grupları zaman içinde bir ilkeye ekleyebilirsiniz.

Cihaz ilkelerini zorlama kullanıcıların cihazlarını Azure AD cihaz kaydı hizmeti ile kaydetmelisiniz önkoşuldur. Azure AD cihaz kaydı hizmeti ile kaydetme cihazları çok faktörlü kimlik doğrulamasını etkinleştirmek için tercih edebilirsiniz. Çok faktörlü kimlik doğrulaması Azure Active Directory cihaz kayıt hizmeti için önerilir. Çok faktörlü kimlik doğrulaması etkinken, kullanıcılar cihazlarını Azure AD cihaz kaydı hizmeti ile kaydetmek için ikinci faktör kimlik doğrulaması sayaçlara.

## <a name="how-does-a-conditional-access-policy-work"></a>Bir koşullu erişim ilkesi nasıl çalışır?

Bir kullanıcı istekleri erişimi bir Office 365 hizmeti desteklenen cihaz platformdan, Azure AD kullanıcı ve cihaz kimliğini doğrular. Azure AD Kullanıcı hizmet için ayarlanan İlkesi uyup uymadığını yalnızca hizmete erişimi verir. Kayıtlı olmayan cihazlardaki kullanıcılar kaydolma ve şirket Office 365 hizmetlerine erişmek için uyumlu hale konusunda yönergeler verilir. Kullanıcıların iOS ve Android cihazlarda Intune Şirket portalı uygulamasını kullanarak cihazlarını kaydetmek için gereklidir. Bir kullanıcı bir cihaz kaydediliyorsa, Azure AD ile cihaz kaydedilir ve aygıt yönetimi ve uyumluluk için kayıtlı. Office 365 Hizmetleri için mobil cihaz yönetimi için Microsoft Intune Azure AD cihaz kaydı hizmeti kullanmanız gerekir. Cihaz ilkelerini uygulanmadığında cihaz kaydı hizmetlerine erişim Office 365 kullanıcıları için gereklidir.

Bir kullanıcı bir cihaz başarıyla kaydediliyorsa cihaz güvenilir hale gelir. Azure AD şirket uygulamaları için oturum açma kimliği doğrulanmış kullanıcının tek erişim sağlar. Azure AD yalnızca kullanıcı erişim isteğinde ilk kez bir hizmete erişim vermek için koşullu erişim ilkesi zorunlu tutar, ancak her zaman kullanıcı erişim için bir istek yeniler. Kullanıcı oturum açma kimlik bilgileri değiştirilirken, cihaz kaybolur veya çalınırsa veya istek zaman yenileme için ilkenin koşullarıyla karşılanmıyor hizmetlerine erişim reddedildi.

## <a name="deployment-considerations"></a>Dağıtma konuları

Cihazları kaydetmek için Azure AD cihaz kaydı hizmeti kullanmanız gerekir.

Şirket içi kullanıcılar olduğunda ilgili olacak şekilde kimlik doğrulaması, Active Directory Federasyon Hizmetleri (AD FS) (sürüm 1.0 ve sonraki sürümler) gereklidir. Kimlik sağlayıcısı çok faktörlü kimlik doğrulaması özelliğine sahip olmadığında çalışma alanına katılma için çok faktörlü kimlik doğrulaması başarısız olur. Örneğin, AD FS 2.0 ile çok faktörlü kimlik doğrulamasını kullanamazsınız. Şirket içi emin AD FS çok faktörlü kimlik doğrulamasıyla çalışır ve çok faktörlü kimlik doğrulaması için Azure AD cihaz kaydı hizmeti etkinleştirmeden önce yerinde bir geçerli çok faktörlü kimlik doğrulama yöntemidir. Örneğin, Windows Server 2012 R2 AD FS çok faktörlü kimlik doğrulama özellikleri içerir. Çok faktörlü kimlik doğrulaması için Azure AD cihaz kaydı hizmeti etkinleştirmeden önce AD FS sunucusunda ek geçerli kimlik doğrulama (çok faktörlü kimlik doğrulaması) yöntemi de ayarlamanız gerekir. AD FS'de desteklenen çok faktörlü kimlik doğrulama yöntemleri hakkında daha fazla bilgi için bkz: [AD FS için ek kimlik doğrulama yöntemleri yapılandırma](/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs).

## <a name="next-steps"></a>Sonraki adımlar

*   Sık sorulan soruların yanıtları için bkz: [Azure Active Directory koşullu erişim ile ilgili SSS](active-directory-conditional-faqs.md).
