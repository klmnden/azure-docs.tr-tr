---
title: Azure Active Directory koşullu erişim ile ilgili SSS | Microsoft Docs
description: Azure Active Directory'de koşullu erişim hakkında sık sorulan soruların yanıtlarını alın.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad0494868c494b488a238a81e504c58552813907
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508982"
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Azure Active Directory koşullu erişim ile ilgili SSS

## <a name="which-applications-work-with-conditional-access-policies"></a>Hangi uygulamaların koşullu erişim ilkeleriyle birlikte çalışır?

Koşullu erişim ilkeleriyle birlikte çalışma uygulamaları hakkında daha fazla bilgi için bkz: [uygulamaları ve Azure Active Directory'de koşullu erişim kuralları kullanan tarayıcılar](technical-reference.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>B2B işbirliği ve Konuk kullanıcılar için koşullu erişim ilkeleri uygulanır?

İlkeleri, işletmeler arası (B2B) işbirliği kullanıcıları için uygulanır. Ancak, bazı durumlarda, bir kullanıcı ilke gereksinimlerini karşılamak mümkün olmayabilir. Örneğin, bir konuk kullanıcının kuruluşunun çok faktörlü kimlik doğrulamasını desteklemiyor olabilir. 

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Bir SharePoint Online İlkesi, OneDrive iş için de geçerli midir?

Evet. Bir SharePoint Online İlkesi, OneDrive iş için de geçerlidir.

## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Bir ilke Word veya Outlook gibi istemci uygulamalarını neden ayarlayamıyorum?

Koşullu erişim ilkesi, bir hizmete erişim gereksinimleri ayarlar. Bu hizmet için kimlik doğrulaması oluştuğunda zorlanır. Doğrudan bir istemci uygulaması İlkesi ayarlanmadı. Bunun yerine, bir istemci bir hizmet çağırdığında uygulanır. Örneğin, SharePoint üzerinde ayarlanan bir ilke, SharePoint arama istemciler için geçerlidir. Bir ilke değişikliği kümesi Outlook'a uygulanır.

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Koşullu erişim ilkesi, hizmet hesapları için geçerli midir?

Koşullu erişim ilkeleri, tüm kullanıcı hesaplarına uygulanır. Bu hizmet hesapları olarak kullanılan kullanıcı hesaplarını içerir. Genellikle, katılımsız çalışan bir hizmet hesabı, bir koşullu erişim ilkesi gereksinimlerini gerçekleştiremiyor. Örneğin, çok faktörlü kimlik doğrulaması gerekli olabilir. Hizmet hesapları, koşullu erişim ilkesi yönetim ayarları'nı kullanarak bir ilkeden tutulabilir. 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a>Graph API'leri, koşullu erişim ilkeleri yapılandırmak için kullanılabilir mi?

Şu anda yok. 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a>Desteklenmeyen cihaz platformları için varsayılan dışlama İlkesi nedir?

Şu anda koşullu erişim ilkeleri, iOS ve Android cihazlarının kullanıcıları seçmeli olarak uygulanır. Diğer cihaz platformlarında uygulamalar varsayılan olarak, iOS ve Android cihazlar için koşullu erişim ilkesi tarafından etkilenmez. Bir kiracı Yöneticisi desteklenmeyen platformlardaki kullanıcılara erişimi engellemek için genel ilke geçersiz kılmayı seçebilirsiniz.

## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Microsoft Teams için koşullu erişim ilkeleri nasıl çalışır?

Microsoft Teams yoğun olarak Exchange Online ve SharePoint Online üzerinde toplantıları, takvimler ve dosya paylaşımı gibi temel üretkenlik senaryoları için kullanır. Bu bulut uygulamaları için ayarlanan koşullu erişim ilkeleri, Microsoft Teams kullanıcı işaretleri, doğrudan Microsoft Teams içinde geçerlidir.

Microsoft Teams ayrıca Azure Active Directory koşullu erişim ilkeleri, bulut uygulaması olarak ayrı olarak desteklenir. Bir kullanıcı oturum açtığında bir bulut uygulaması için ayarlanan koşullu erişim ilkeleri, Microsoft Teams için geçerlidir. Bununla birlikte, Exchange Online ve SharePoint Online gibi diğer uygulamalarda doğru ilkeleri olmadan kullanıcılar yine de bu kaynakları doğrudan erişmek mümkün olabilir.

Windows ve Mac için Microsoft Teams Masaüstü istemcileri, modern kimlik doğrulamasını destekler. Modern kimlik doğrulaması, oturum üzerinde Azure Active Directory kimlik doğrulama kitaplığı (ADAL) Microsoft Office istemci uygulamaları için platformlar arasında açma getirir.

## <a name="next-steps"></a>Sonraki adımlar

- Ortamınız için koşullu erişim ilkeleri yapılandırmak için bkz: [Azure Active Directory'de koşullu erişim için en iyi uygulamalar](best-practices.md). 
